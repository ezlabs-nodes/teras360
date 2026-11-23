# PRD: Redesign Safety Interaction & Modul Baru "My Open Actions"

**Dokumen:** PRD-TERAS360-K3-SI-001
**Modul:** `modules/kpi/activityreport.php`, `modules/kpi/my_actions.php` (Baru), `modules/dashboard/index.php`, `includes/sidebar.php`
**Referensi:** SAI360 Safety Interaction & Action Management + SAI360_Modul IND_ENG.pdf (diadaptasi untuk PT. Teras Teknik Perdana)
**Status:** Menunggu Persetujuan User sebelum Eksekusi
**Tanggal:** 24 November 2026

---

## 1. Ringkasan Eksekutif & Tujuan

Meng-upgrade form **Safety Interaction** pada Dashboard K3 agar setara dengan standar enterprise SAI360, namun disesuaikan dengan realitas pekerjaan **PT. Teras Teknik Perdana** (Refractory, Insulation, Scaffolding, Painting & Blasting, Smelter/Refinery Maintenance), serta membangun modul baru **My Open Actions** di bawah KPI & Evaluation yang terhubung langsung ke Dashboard masing-masing pekerja.

---

## 2. Alur Kerja Sistem (End-to-End Flow)

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────────┐
│  User Login     │───▶│ Safety Interaction│───▶│ Generate SI Number  │
│  (K3 / HSE)     │    │ Form (activity)   │    │ (on Save)           │
└─────────────────┘    └──────────────────┘    └──────────┬──────────┘
                                                          │
                         ┌────────────────────────────────┘
                         ▼
              ┌──────────────────────┐
              │ Ada Less Safe        │
              │ Behaviors?           │
              └──────┬───────────────┘
                     │
        ┌────────────┴────────────┐
        │ Ya                      │ Tidak
        ▼                         ▼
┌──────────────────┐    ┌──────────────────┐
│ Section ACTIONS  │    │ Simpan SI        │
│ aktif (Add       │    │ selesai          │
│ Action)          │    └──────────────────┘
└────────┬─────────┘
         ▼
┌──────────────────┐    ┌──────────────────────┐
│ Action tersimpan │───▶│ My Open Actions      │
│ (ACT-XXXXXX)     │    │ (Dashboard & Sidebar)│
└──────────────────┘    └──────────┬───────────┘
                                   ▼
                        ┌──────────────────────┐
                        │ PIC Close Action     │
                        │ (upload evidence)    │
                        └──────────┬───────────┘
                                   ▼
                        ┌──────────────────────┐
                        │ Notifikasi ke        │
                        │ Assigned By          │
                        └──────────────────────┘
```

---

## 3. Spesifikasi Detail Form Safety Interaction
**File:** `modules/kpi/activityreport.php`

### 3.1 Header & System-Generated Fields

| Field | Perilaku & Aturan |
|---|---|
| **DATE ENTERED** (System Generated) | Read-only & ter-blur/greyed-out (disabled), otomatis mencatat tanggal/waktu sistem saat input agar tercatat kapan user menginput data. |
| **SAFETY INTERACTION NUMBER** | Ter-blur / kosong sebelum disimpan (tidak langsung menampilkan `SI-100286`). Nomor resmi (`SI-XXXXXX`) baru di-generate sistem pada saat klik **Save**. |
| **Link to Safety Interactions Tutorial** | Tautan di kanan atas form. Saat diklik membuka **Modal Panduan Ringkas & Visual** (mengadaptasi konsep SAI360_Modul IND_ENG.pdf: Tujuan Safety Interaction, alur 5 langkah input, perbedaan *More Safe* vs *Less Safe*, serta cara tindak lanjut Action di TERAS360). |

### 3.2 Area, Cascading Location & TTP Work Topic

**AREA / FACILITY \*** — Dropdown Proyek/Area TTP dari tabel proyek + Head Office. Perubahan pada field ini otomatis mengisi ulang opsi dropdown **LOCATION \*** yang sesuai:

| Area / Facility | Daftar Location |
|---|---|
| **Refractory Repair and Maintenance Services** (Sumbawa Barat / Maluk) | Smelter - FCF, Anode Casting Shop, Slag Concentrator, TTP Workshop, Refractory Warehouse, Laydown Yard, Office & Admin Area |
| **Manyar Smelter Project** (Gresik) | Flash Smelting Furnace, Anode Casting, Gas Cleaning Plant, TTP Site Workshop, Material Yard, Site Office |
| **Head Office - Jakarta** | Wisma TTP Office, Meeting Room, Parking & Common Area, Warehouse Pasar Kemis |
| **Proyek Lainnya** | Daftar lokasi standar operasional proyek + **Other** |

**SPECIFIC LOCATION \*** — Input detail lokasi spesifik (contoh: `Furnace Level 3`, `Bay A`).

**TITLE / TOPIC \*** — Diubah menjadi **Searchable Dropdown Aktivitas Pekerjaan TTP**:

- Refractory Installation & Brick Laying
- Refractory Demolition & Wrecking
- Castable Gunning, Casting & Ramming
- Thermal Insulation (Hot / Cold / Cryogenic)
- Tubular Scaffolding Erection, Modification & Dismantling
- Industrial Painting, Coating & Grit Blasting
- Working at Height & Fall Protection
- Confined Space Entry
- Lifting, Rigging & Material Handling
- Hot Work (Welding, Cutting, Grinding)
- Electrical Safety & LOTO (Isolation)
- Chemical & B3 Handling (MSDS)
- PPE Compliance & Respiratory Protection
- Tools, Equipment & Pre-Use Check
- Housekeeping & Material Stacking
- Manual Handling & Ergonomics
- Traffic Management & Mobile Equipment
- Environmental & Waste Management
- Emergency Response & First Aid Readiness
- Other Safety Topic

### 3.3 Less Safe Behaviors & More Safe Behaviors (Multi-Select Modal Picker)

Kedua field (**LESS SAFE BEHAVIORS** dan **MORE SAFE BEHAVIORS**) memiliki tombol pembuka **Modal Checklist Multi-Select** lengkap dengan:

- Kotak pencarian (Search)
- Clear All
- Filter **Full List** / **Selected Items Only**

**Daftar Opsi Identik untuk Keduanya (10 Kategori):**

1. Employee Competency / Licenses
2. Ergonomics
3. Housekeeping
4. Positions of People (Line of Fire)
5. PPE
6. Procedures
7. Permit System *(menggantikan Safe Check / JHA / ATW / Work Permit)*
8. Tools and Equipment
9. Work Environment / Confined Space
10. Lifting, Rigging & Material Handling

> **Validasi:** Minimal salah satu dari *Less Safe Behaviors* atau *More Safe Behaviors* wajib diisi.

### 3.4 Field Diskusi & Pembersihan Field Lama

- **DISCUSSION SUMMARY / KEY POINTS \*** — Wajib diisi (menggantikan *Other Topics Discussed*).
- **DIHAPUS:** Field `TOTAL PARTICIPANTS`.
- **DIHAPUS:** Field `FOLLOW-UP / COMMITMENT ACTION`.

### 3.5 PARTICIPANTS / PERSONS INVOLVED (Person Interacted With — Multi-Row HRIS)

Tabel daftar peserta interaksi dengan kolom:

| Kolom | Keterangan |
|---|---|
| Person Interacted With | Nama karyawan (HRIS search) |
| Role | Jabatan (auto-fill dari HRIS, editable) |
| Third Party Person | Nama pihak ketiga (opsional) |
| Third Party Company | Perusahaan pihak ketiga (opsional) |

Dilengkapi tombol **+ Add New** serta hapus baris (mendukung lebih dari 1 orang).

**Modal Safety Interaction > Person Interacted With:**

- Input **PERSON INTERACTED WITH**: Pencarian *privacy-first* ke database HRIS (`terasmyi_hris.karyawan_baru`) — ketik min. 2 karakter, **tanpa menampilkan NIK KTP**.
- Saat karyawan HRIS dipilih, field **ROLE** otomatis terisi Jabatan karyawan tersebut dari HRIS (tetap bisa disesuaikan bila perlu).
- Tersedia field opsional **THIRD PARTY PERSON INTERACTED WITH** dan **THIRD PARTY COMPANY** untuk interaksi dengan klien/subkontraktor.

### 3.6 PEOPLE TO NOTIFY

Mengadopsi **100% tampilan dan fungsi** dari form `hazardreport.php`:

- Pencarian karyawan HRIS *privacy-first* (bisa pilih lebih dari 1 orang).
- Pilihan kanal notifikasi: **Email**, **WhatsApp**, atau **Keduanya**.
- Otomatis mengirim notifikasi saat Safety Interaction disimpan.

### 3.7 ACTIONS (Conditional saat Ada Less Safe Behaviors)

Section **ACTIONS** otomatis muncul/aktif ketika user memilih minimal 1 item pada *Less Safe Behaviors* (dengan tabel ringkasan + tombol **+ Add Action**).

Form input di dalam setiap Action mengikuti **persis layout tab Actions pada `hazardreport.php`**:

| Field | Aturan |
|---|---|
| ACTION NUMBER (Generated on Save) | Ter-blur / kosong sebelum disimpan (baru di-generate saat Save, misal `ACT-XXXXXX`). |
| ACTION SOURCE (System Generated) | Ter-blur / read-only, otomatis terisi **Safety Interaction**. |
| ASSIGNED BY \* | Default user login / HRIS search. |
| DATE RAISED | Default hari ini. |
| ACTION TYPE \* | Corrective, Preventive, Immediate |
| SOLUTION TYPE \* | Permanent, Temporary |
| CURRENTLY ASSIGNED TO \* | HRIS search — PIC penanggung jawab tindakan. |
| DUE DATE \* | Tanggal jatuh tempo. |
| SECONDARY ASSIGNED TO | HRIS search (opsional). |
| PRIORITY CODE \* | A — Immediate / 24h · B — Within 7 Days · C — Within 30 Days |
| ACTION RELATE TO \* | Safety, Health, Environment, Quality |
| ACTION TITLE \* | Judul tindakan |
| ACTION DESCRIPTION | Deskripsi detail tindakan |

---

## 4. Spesifikasi Modul Baru: My Open Actions & Integrasi Dashboard

### 4.1 Perubahan Database (Zero Breaking Change)

**Tabel `kpi_evidence`** — Menambahkan kolom baru (jika belum ada):

| Kolom | Tipe | Default |
|---|---|---|
| `date_entered` | DATETIME | CURRENT_TIMESTAMP |
| `less_safe_behaviors` | JSON / TEXT | NULL |
| `more_safe_behaviors` | JSON / TEXT | NULL |
| `people_to_notify_json` | LONGTEXT | NULL |
| `notify_channel` | VARCHAR(20) | `'both'` |

**Tabel `hazard_actions`** (Diperluas menjadi tabel Action terpadu lintas sumber):

| Kolom | Tipe | Keterangan |
|---|---|---|
| `source_record_id` | INT | NULL — menyimpan `kpi_evidence.id` jika `action_source = 'Safety Interaction'`, atau `hazards.id` jika Hazard. |
| `assigned_to_user_id` | INT | NULL — untuk query cepat per user. |
| `assigned_to_karyawan_id` | INT | NULL — untuk query cepat per pekerja. |

### 4.2 Halaman Baru: `/modules/kpi/my_actions.php`

Tampilan bersih bergaya enterprise (seperti referensi My Actions):

- **View Dropdown:** My Open Actions (default), My Closed Actions, Assigned By Me, All Open Actions (Admin/HSE Manager).
- **Quick Filter Search:** Cari berdasarkan *Action Number*, *Title*, *Assigned By*, atau *Source*.
- **Kolom Tabel:** Due Date (*Overdue highlight*), Action Number, Source (Safety Interaction / Hazard), Assigned By, Action Title, Priority Code, Status, dan tombol **Detail** / **Close Action**.
- **Modal Penyelesaian Action:** PIC dapat mengisi *Completion Comments*, *Completion Date*, dan upload bukti foto/dokumen penyelesaian (`actions_evident`) untuk mengubah status menjadi **Closed** (sekaligus mengirim notifikasi ke *Assigned By*).

### 4.3 Integrasi Sidebar & Dashboard K3

**Sidebar** (`includes/sidebar.php`):

- Menambahkan submenu **My Open Actions** (dengan *live counter badge* & indikator alert merah bila ada action terbuka/overdue) di bawah grup **KPI & Evaluation**.

**Dashboard K3** (`modules/dashboard/index.php`):

- Menambahkan kartu/widget **My Open Actions** pada Dashboard masing-masing user sehingga pekerja langsung melihat jumlah & daftar Action terbuka yang ditugaskan kepadanya dari Safety Interaction maupun Hazard Report, dengan 1 klik menuju penyelesaian.

---

## 5. Rencana Eksekusi Setelah Persetujuan

| Step | Aktivitas | Cakupan |
|---|---|---|
| **Step 1** | Migrasi Schema | Tambah kolom pada `kpi_evidence` & `hazard_actions` (otomatis & idempotent). |
| **Step 2** | Redesign `modules/kpi/activityreport.php` | Terapkan seluruh field baru, cascading Area→Location, TTP Topic dropdown, Modal Less/More Safe Behaviors (Permit System), Multi-row HRIS Person Interacted With (auto-fill Jabatan), People to Notify (identik Hazard Report), Conditional Actions (blurred number & source), dan Modal Tutorial. |
| **Step 3** | Buat Modul `modules/kpi/my_actions.php` | Daftar & filter My Open Actions + modal update/close action dengan bukti. |
| **Step 4** | Integrasi Sidebar & Dashboard K3 | Tambahkan menu & live counter My Open Actions di `includes/sidebar.php` dan widget di `modules/dashboard/index.php`. |
| **Step 5** | Verifikasi | `php -l` seluruh file + uji query database & alur simpan Safety Interaction → Action → My Open Actions. |

---

## 6. Checklist Verifikasi

- [ ] Migrasi schema berjalan idempotent (aman dijalankan ulang)
- [ ] Form Safety Interaction: semua field baru tampil & validasi berjalan
- [ ] Cascading Area → Location berfungsi
- [ ] Modal Less/More Safe Behaviors (multi-select + search + clear all)
- [ ] Multi-row HRIS Person Interacted With dengan auto-fill Jabatan
- [ ] People to Notify identik dengan Hazard Report
- [ ] Conditional Actions muncul hanya saat ada Less Safe Behaviors
- [ ] Action Number ter-generate saat Save
- [ ] Modul My Open Actions: filter, tabel, modal close action berfungsi
- [ ] Sidebar: badge counter & indikator overdue tampil
- [ ] Dashboard: widget My Open Actions tampil per user
- [ ] `php -l` lolos untuk seluruh file yang diubah
- [ ] End-to-end: Safety Interaction → Action → My Open Actions → Close berhasil

---

*Dokumen ini menunggu persetujuan user sebelum eksekusi.*
