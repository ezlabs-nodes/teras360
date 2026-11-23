# PRD: Tata Kelola Pengajuan Akun TERAS360, Redesain Form, & Isolasi Data Multi-Proyek HRIS

**Dokumen:** PRD Org Chart Access Flow & HRIS Project Isolation
**Referensi Resmi:** Company Organization Chart PT. Teras Teknik Perdana (Doc No. QHSE/TTP/01, Date: January 01, 2025, Approved by Teddy Suryadi Hasan — President Director)
**Lingkup Sistem:** HRIS (`hris.localhost`), IT Desk (`itdesk.localhost`), Dashboard K3 (`k3.localhost`), Centralized SSO (`terasmyi_sso`)
**Status:** ⚠️ Menunggu Review & Persetujuan User

---

## 1. Masukan Strategis: Apakah Pengajuan Akun Harus dari HRIS?

> 💡 **Rekomendasi Arsitektur (Best Practice Enterprise): Model Hibrida 2 Pintu Masuk → 1 Antrean Terpusat (`terasmyi_sso.sso_access_requests`)**

Jika pengajuan hanya bisa dilakukan dari dalam HRIS yang mengharuskan login, akan muncul kendala operasional (*chicken-and-egg problem*) ketika seorang karyawan baru, Site Manager proyek baru, atau PIC yang belum memiliki akun HRIS/TERAS360 ingin mengajukan akses.

| Pintu Masuk | Siapa yang Menggunakan | Keunggulan |
|---|---|---|
| **Pintu 1: Dari Dalam HRIS** (`hris.localhost/pages/ess/access_request.php`) | HR & GA Proyek, HRD HO, Site Manager, atau Karyawan yang sudah login HRIS | Sangat ideal saat proses onboarding / penunjukan personel proyek; data proyek pemohon otomatis terkunci sesuai proyeknya. |
| **Pintu 2: Portal Pengajuan IT Desk** (`itdesk.localhost/request.php` — dapat diakses dari link "Ajukan Akses TERAS360" di halaman Login) | Personel Ditunjuk / Manager yang belum punya akun login atau langsung dari halaman login | Tidak menghambat operasional jika belum punya akun HRIS; tetap aman karena wajib verifikasi Badge ID + Email dari HRIS & tetap wajib melalui Approval berjenjang. |

**Kesimpulan:** Kedua pintu masuk menggunakan desain form yang **identik**, menulis ke **1 database antrean** (`sso_access_requests`), dan melewati **1 alur approval resmi yang sama** sesuai Org Chart QHSE/TTP/01.

---

## 2. Flowchart Profesional Pengajuan Akun TERAS360 (Sesuai Org Chart QHSE/TTP/01)

Alur dipisahkan secara jelas antara **Jalur Proyek (Site Project)** dan **Jalur Pusat (Head Office — Jakarta)** mengikuti garis komando resmi pada QHSE/TTP/01.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    PENGAJUAN AKSES TERAS360                          │
│         (Pintu 1: HRIS ESS  |  Pintu 2: IT Desk Portal)              │
└────────────────────────────────┬────────────────────────────────────┘
                                 ▼
                    ┌────────────────────────┐
                    │  Antrean Terpusat      │
                    │  sso_access_requests   │
                    └───────────┬────────────┘
                                ▼
              ┌─────────────────┴─────────────────┐
              │  Jalur Pemohon?                   │
              └──────┬──────────────────┬─────────┘
                     │                  │
              ┌──────▼──────┐    ┌──────▼──────────┐
              │ Jalur Proyek│    │ Jalur Pusat HO  │
              │ (Site)      │    │ (Jakarta)       │
              └──────┬──────┘    └──────┬──────────┘
                     ▼                  ▼
              ┌──────────────┐   ┌──────────────────┐
              │ Level 1:     │   │ Level 1:         │
              │ Site/Project │   │ Manager / GM     │
              │ Manager      │   │ Divisi terkait   │
              └──────┬───────┘   └──────┬───────────┘
                     ▼                  ▼
              ┌──────────────┐   ┌──────────────────┐
              │ Level 2:     │   │ Level 2:         │
              │ Construction │   │ Director terkait │
              │ GM / GA&Adm  │   │ (sesuai divisi)  │
              │ GM / HSE Corp│   └──────┬───────────┘
              │ / Director   │          │
              └──────┬───────┘          │
                     └────────┬─────────┘
                              ▼
                    ┌──────────────────┐
                    │  Eksekutor:      │
                    │  Tim IT Desk     │
                    │  (itdesk.local)  │
                    └──────────────────┘
```

### Matriks Kewenangan Approval Sesuai Org Chart QHSE/TTP/01

| Asal Pemohon | Level 1 Approver (Wajib) | Level 2 Approver (Khusus Akses Lintas Proyek / Admin / Sensitif) | Eksekutor |
|---|---|---|---|
| **Proyek Site** (Maluk Sumbawa Barat, Manyar Gresik, dll.) | Site Manager / Project Manager di area proyek masing-masing | Construction GM (Niarta Ginting) / GA & Adm. GM (Ferry Irawan) / HSE Corporate Manager (Rista Situngkir) / Director | Tim IT Desk (`itdesk.localhost`) |
| **Head Office — Divisi Marketing** | Marketing Manager / Marketing GM (David Saut Simatupang) | Marketing Director (Teddy Suryadi Hasan) | Tim IT Desk |
| **Head Office — Divisi Finance & Acct.** | Cost Control & Finance-Acct. Manager (Harsono) / Finance & Acct. GM (Rendy Pramundinta) | Finance / GA & Adm. Director (Tri Gunarso D.S.) | Tim IT Desk |
| **Head Office — Divisi GA, Adm. & HRD** | HRD Manager / GA & Adm. GM (Ferry Irawan) | Finance / GA & Adm. Director (Tri Gunarso D.S.) | Tim IT Desk |
| **Head Office — Divisi Construction & Support** | Maint. & Warehouse / Log. & Proc. / Estimator / Eng. & QC Manager / Construction GM (Niarta Ginting) | Construction Director (Tesna Rafinari Hasan) | Tim IT Desk |
| **Head Office — MR / HSE Corporate / PPD** | HSE Corporate Manager (Rista Situngkir) / Management Representative (David Saut Simatupang) | President Director (Teddy Suryadi Hasan) | Tim IT Desk |

---

## 3. Redesain UI Form Pengajuan Akses TERAS360

### 3.1 Masalah UX Saat Ini

Berdasarkan screenshot form saat ini (`hris.localhost/pages/ess/access_request.php`), ditemukan **5 masalah UX** yang akan diperbaiki tuntas:

1. ❌ Judul dobel ("Pengajuan Akses TERAS360" muncul 2 kali berurutan di header & body).
2. ❌ Terlalu padat dalam 1 kotak panjang tanpa pemisahan langkah visual.
3. ❌ 13 checkbox modul menumpuk datar sehingga sulit dibaca/dipilih.
4. ❌ Baris Approver miring/tidak sejajar karena panjang label 2 baris vs 1 baris.
5. ❌ Tidak ada kartu preview identitas karyawan (foto HRIS, badge, proyek, jabatan) setelah karyawan dipilih.

### 3.2 Rancangan Desain Baru (Clean, Scannable, Executive)

- **Header Ringkas Tunggal** + **Visual Flow Bar 4 Langkah** di bagian atas:
  `1. Pilih Personel → 2. Kebutuhan Modul → 3. Approver → 4. Proses IT Desk`
- **Layout 2 Kolom Terstruktur** (`max-w-6xl`):

#### Kolom Kiri (65%) — Formulir Bersih dalam 3 Section Card

| Section | Isi |
|---|---|
| **Section A — Personel yang Diajukan** | 1 kolom pencarian *privacy-first* (ketik nama/badge). Begitu dipilih, muncul **Employee Identity Card** elegan: Foto HRIS bulat + Nama + Badge ID + Jabatan HRIS + Proyek/Lokasi + Email (editable) + No. WA. |
| **Section B — Kebutuhan Akses & Scope** | • Pilihan Scope Proyek (*Khusus Proyek Sendiri* / *Semua Proyek — Khusus HO*)<br>• Jalur Login (3 kartu radio bersih: Email & Password, Microsoft Outlook, Google SSO)<br>• **Grid Kartu Modul 2–3 Kolom** yang rapi (dikelompokkan menjadi **Core Modules** & **Supply Chain / Support** dengan ikon minimalis, bukan checkbox datar menumpuk). |
| **Section C — Persetujuan & Justifikasi** | 1 baris sejajar sempurna (Ketik Nama Approver + Jabatan Approver otomatis + Email Approver) dengan validasi visual level minimal Site Manager, diikuti kotak Justifikasi Bisnis. |

#### Kolom Kanan (35%) — Ringkasan Tiket & Alur Persetujuan Live

Menampilkan **live preview alur approval** yang akan dilewati tiket ini (misal: *Jalur Proyek Maluk → Site Manager → IT Desk*) sehingga pemohon langsung paham ke mana tiket mengalir sebelum menekan tombol **Ajukan**.

---

## 4. Arsitektur Isolasi Data Multi-Proyek / Lokasi Area (HRIS & TERAS360)

Fitur keamanan data (**Multi-Tenant Row-Level Security**) agar HR & GA / Admin masing-masing proyek hanya dapat melihat & mengelola data proyek tempatnya bekerja, sementara Head Office (Pusat — Jakarta) dapat melihat seluruh proyek.

### 4.1 Klasifikasi Scope Data (`project_scope`)

| Tipe Scope | Siapa yang Mendapatkannya | Perilaku di HRIS & Dashboard K3 |
|---|---|---|
| **`PROJECT_ONLY`** (Default untuk seluruh user proyek) | HR & GA Proyek, Site Manager, HSE Project, Admin Proyek, Warehouse Proyek di `id_proyek = 1` (Maluk, Sumbawa Barat), `id_proyek = 2` (Manyar, Gresik), atau proyek site lainnya. | • Seluruh query otomatis dikunci `WHERE id_proyek = :user_id_proyek`.<br>• Dropdown proyek terkunci (*locked badge*: **Proyek Anda: Maluk, Sumbawa Barat**).<br>• Tidak bisa mencari, melihat, mengedit, atau mengunduh slip gaji/absensi/data karyawan proyek lain (**Anti-IDOR 403**). |
| **`ALL_PROJECTS`** (Khusus Head Office / Pusat) | Personel Head Office — Jakarta (`id_proyek = 4`) pada level manajemen/pusat: Direksi, General Managers, HRD Manager HO, HSE Corporate Manager, MR / PPD HO, Superadmin / IT Desk. | • Default menampilkan **Semua Proyek** (*All Projects*).<br>• Tersedia **Project Filter / Switcher** di header/filter untuk menyaring per proyek (Semua Proyek, Head Office - Jakarta, Maluk - Sumbawa Barat, Manyar - Gresik). |

### 4.2 Perubahan Skema Database & Helper Terpusat

**Kolom `access_scope`** di `terasmyi_sso.sso_users` & `terasmyi_hris.users`:

```sql
access_scope ENUM('project', 'all') NOT NULL DEFAULT 'project'
id_proyek INT NULL  -- tersinkronisasi otomatis dari terasmyi_hris.karyawan_baru.id_proyek
```

**Aturan Otomatis Saat Sync / Provisioning IT Desk:**

- Jika karyawan berada di `id_proyek = 4` (Head Office - Jakarta) **DAN** jabatannya termasuk manajemen pusat / HRD HO / Corporate HSE / Direksi / IT Desk → `access_scope = 'all'`.
- Selain itu (seluruh karyawan & HR & GA di site proyek) → `access_scope = 'project'`, terkunci ketat pada `id_proyek` masing-masing.
- Tim IT Desk dapat mengubah `access_scope` (*Project Only* vs *All Projects*) secara eksplisit di `itdesk.localhost` bila ada penugasan khusus.

**Helper Terpusat di HRIS** (`hris/includes/project_scope.php`):

| Fungsi | Deskripsi |
|---|---|
| `hrisUserScope()` | Mengembalikan `['scope' => 'project'|'all', 'id_proyek' => int, 'active_filter_proyek' => ?int, 'project_name' => string]`. |
| `hrisProjectWhereSql(string $alias = 'k')` | Menghasilkan klausa SQL aman (`AND k.id_proyek = ?`) untuk seluruh modul HRIS. |

**Cakupan Penerapan Filter Scope:**

| Modul | File | Perilaku |
|---|---|---|
| Dashboard HRIS | `index.php` | KPI total karyawan, absensi hari ini, payroll bulan ini hanya menghitung proyek user (kecuali HO). |
| Data Karyawan & Personalia | `pages/personalia/`, `modules/employees/` | Daftar karyawan & detail profil terkunci per proyek (+ guard 403 bila akses ID proyek lain). |
| Absensi | `pages/absensi/`, `modules/attendance/` | Log kehadiran, lembur, cuti terkunci per proyek. |
| Payroll | `pages/payroll/`, `modules/payroll/` | Daftar gaji, payroll run, slip gaji terkunci per proyek (**sangat krusial untuk kerahasiaan kompensasi antar proyek!**). |
| Pencarian Privacy API | `api/search_karyawan_privacy.php` | User proyek hanya bisa mencari karyawan di proyeknya sendiri; user HO bisa mencari lintas proyek. |
| Pengajuan Akses TERAS360 | `pages/ess/access_request.php` | HR & GA / Site Manager proyek hanya melihat & meng-approve tiket dari proyeknya; HO/IT Desk melihat semua proyek. |

---

## 5. Rencana Implementasi Bertahap

| Fase | Lingkup Pekerjaan | File Utama |
|---|---|---|
| **Fase 1: Core Scope & Schema** | Tambah kolom `access_scope` (`project`/`all`) + `project_scope_requested` di `TerasSSO.php` (`sso_users`, `sso_access_requests`) dan `terasmyi_hris.users`; tambahkan kontrol Project Scope di Console IT Desk (`itdesk.localhost`). | `sso/TerasSSO.php`, `it_desk/index.php` |
| **Fase 2: Isolasi Multi-Proyek HRIS** | Buat helper `hris/includes/project_scope.php`, tambahkan **Project Scope Indicator / Switcher** di header HRIS (*locked badge* untuk Proyek, dropdown switcher untuk HO), dan terapkan filter SQL + Anti-IDOR pada Dashboard, Personalia/Karyawan, Absensi, Payroll, dan API Pencarian. | `hris/includes/project_scope.php`, `hris/layout/header.php`, `hris/index.php`, `hris/pages/{personalia,absensi,payroll}/`, `hris/modules/{employees,attendance,payroll}/`, `hris/api/search_karyawan_privacy.php` |
| **Fase 3: Redesain Form Pengajuan & Portal Hibrida** | Redesain total `hris/pages/ess/access_request.php` (layout 2-kolom bersih, kartu foto HRIS, grid kartu modul, 1 baris Approver sejajar + preview alur Org Chart QHSE/TTP/01), dan buat **Pintu 2** (`it_desk/request.php` + link dari halaman Login `k3.localhost/login.php`). | `hris/pages/ess/access_request.php`, `it_desk/request.php`, `dashboard-k3/login.php` |
| **Fase 4: Verifikasi End-to-End** | `php -l` seluruh file, uji simulasi query scope Proyek 1 vs Proyek 2 vs HO (`id_proyek = 4`), dan verifikasi tampilan form pengajuan baru. | Seluruh modul terdampak |

---

## 6. Checklist Verifikasi

- [ ] Kolom `access_scope` & `id_proyek` ditambahkan ke `sso_users`, `sso_access_requests`, dan `hris.users`
- [ ] Helper `hrisUserScope()` & `hrisProjectWhereSql()` berfungsi
- [ ] Project Scope Indicator / Switcher tampil di header HRIS
- [ ] Filter SQL + Anti-IDOR (403) berjalan di Dashboard, Personalia, Absensi, Payroll, API Pencarian
- [ ] Form pengajuan akses redesain (2 kolom, kartu identitas HRIS, grid modul, Approver sejajar, preview alur)
- [ ] Pintu 2 (`it_desk/request.php`) dapat diakses dari halaman login
- [ ] Kedua pintu masuk menulis ke `sso_access_requests` yang sama
- [ ] Alur approval sesuai matriks Org Chart QHSE/TTP/01
- [ ] `php -l` lolos untuk seluruh file yang diubah
- [ ] Uji simulasi scope: Proyek 1 vs Proyek 2 vs HO (`id_proyek = 4`)

---

## 7. Konfirmasi Sebelum Eksekusi

> 📌 **Mohon tinjau PRD ini** (termasuk flowchart Org Chart QHSE/TTP/01, masukan pintu masuk hibrida HRIS + Portal IT Desk, redesain form 2-kolom, dan isolasi data proyek HRIS).
>
> Jika sudah sesuai, silakan klik **Proceed** (atau sampaikan penyesuaian) agar saya langsung mulai eksekusi dari **Fase 1 hingga selesai**.

---

*Dokumen ini menunggu review & persetujuan user sebelum eksekusi.*
