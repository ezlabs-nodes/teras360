===FILE: README.md===
# HR Connect — TERAS HRIS

![Status](https://img.shields.io/badge/status-Phase%202%20Ready-green)
![PHP](https://img.shields.io/badge/PHP-8.3%2B-777BB4?logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0%2B-4479A1?logo=mysql&logoColor=white)
![License](https://img.shields.io/badge/license-Proprietary-red)

> **Product Requirements Document (PRD) v2.2**
> Platform HRIS multi-proyek untuk **PT. Teras Teknik Perdana (TTP)**
> *Powered by [zeroharm.tech](https://zeroharm.tech)*

---

## 📌 Ringkasan Produk

**HR Connect (TERAS HRIS)** adalah platform *Human Resource Management System* berbasis **PHP Native 8.3+ & MySQL/MariaDB** yang dirancang khusus untuk karakteristik bisnis multi-proyek TTP (9 lokasi resmi: HO Jakarta, Sumbawa Barat, Manyar Gresik, Balikpapan, Cilegon, Batu Bara, Tangerang).

### Fitur Unggulan
- 🔐 **Single-Door SSO SUMMIT** (`TerasSSO.php` + Transfer Ticket `?sso_ticket=`)
- 🏢 **3-Tier RBAC** dengan isolasi data ketat (`ALL_PROJECTS`, `PROJECT_ONLY`, `SELF_ONLY`)
- 🖐️ **Face Enrollment + Kiosk Face/QR** terintegrasi
- 📡 **ADMS Push Protocol** untuk mesin fingerprint Solution X609/X105
- 📅 **Roster Engine** (4:2, 6:2, 10:2 + Fatigue 13:1, 12:12)
- 💰 **Tabung Cuti (Leave Banking)** otomatis
- 📱 **Mobile-First PWA ESS** dengan Anti Fake GPS multi-layer
- ⭐ **Employee Credit Score** (skala 0–100)
- 🌐 **Public Career Portal** di subdomain `career.domain.co.id`

---

## 📚 Dokumentasi

| Dokumen | Deskripsi |
| :--- | :--- |
| [📄 PRD Lengkap v2.2](docs/PRD-HR-Connect-v2.2.md) | Product Requirements Document |
| [🏗️ Arsitektur Modul](docs/arsitektur-modul.md) | Struktur folder & dependensi |
| [🗺️ Roadmap](docs/roadmap-implementasi.md) | Phase 1 → 3 |

---

## 🗺️ Status Roadmap

| Fase | Lingkup | Status |
| :--- | :--- | :--- |
| **Phase 1** | SSO, RBAC, Dashboard v4 | ✅ **Live** |
| **Phase 2A** | ADMS + Face Enroll + Kiosk | 🟡 Siap Eksekusi |
| **Phase 2B** | Roster + Fatigue + Leave Banking | 🟡 Siap Eksekusi |
| **Phase 2C** | Mobile ESS + Career Sync | 🟡 Siap Eksekusi |
| **Phase 3** | Kolaborasi & Enterprise | ⏳ Terjadwal |

---

## 🛠️ Tech Stack

| Layer | Teknologi |
| :--- | :--- |
| **Backend** | PHP 8.3+ (PDO) |
| **Database** | MySQL/MariaDB (`terasmyi_hris`, `terasmyi_sso`, `terasmyi_dashboard_k3`) |
| **Frontend** | Tailwind CSS, Chart.js 4, FontAwesome 6, SweetAlert2 |
| **Library** | PhpSpreadsheet, Face-Service |
| **Auth** | SUMMIT SSO (`?sso_ticket=`) |
| **Deployment** | `https://hris.teras360.my.id/` + `career.domain.co.id` |

---

## 📂 Struktur Aplikasi

```text
hris/
├── index.php                  # Dashboard HR Connect v4
├── login.php                  # SUMMIT SSO Gateway
├── includes/                  # bootstrap, auth, helpers, project_scope
├── api/
│   ├── adms/iclock/           # ADMS Push Receiver
│   ├── attendance/            # Face, GPS, QR, Kiosk API
│   ├── roster/                # Roster & Fatigue Engine
│   └── ess/                   # Mobile ESS API
├── pages/
│   ├── personalia/            # Direktori Karyawan
│   ├── absensi/               # Attendance Hub
│   ├── payroll/               # Payroll Cycle
│   ├── performance/           # KPI & Credit Score
│   └── ess/                   # Mobile PWA
├── modules/
│   ├── attendance/            # kiosk.php, enroll.php
│   ├── employees/ payroll/ recruitment/ punishment/ training/ analytics/
└── career/index.php           # Public Career Portal
```

---

## 🔒 Konfidensialitas

Dokumen ini bersifat **CONFIDENTIAL**. Dilarang mendistribusikan ke pihak luar tanpa izin tertulis **PT. Teras Teknik Perdana**.

---

## 📞 Kontak & Ownership

- **Product Owner:** PT. Teras Teknik Perdana
- **Powered by:** [zeroharm.tech](https://zeroharm.tech)
- **Dokumen Versi:** v2.2 (Final — Face Enrollment & Career Subdomain)

---

© 2025 PT. Teras Teknik Perdana. All rights reserved.


===FILE: CHANGELOG.md===
# Changelog — TERAS HRIS

Format berdasarkan [Keep a Changelog](https://keepachangelog.com/id/1.1.0/).

## [v2.2] — Final — 2025-XX-XX

### Added
- **Face Enrollment** (`modules/attendance/enroll.php`) untuk Tier 1 & 2
- **Self Face Enrollment** via Mobile ESS untuk Tier 3
- **Subdomain Career Portal** `career.domain.co.id` (UI tidak diubah)
- **ADMS Push Protocol Receiver** (`api/adms/iclock/*`)
- **Roster Engine** 4:2, 6:2, 10:2 (Fatigue 13:1), 12:12
- **Leave Banking** (`hris_leave_bank_ledger`)
- **Employee Credit Score** 0–100 (`hris_credit_score_ledger`)
- **Anti Fake GPS** 3-layer (`attendance_security_logs`)
- **Panel Manajemen Mesin Fingerprint ADMS** per proyek

### Changed
- Mobile ESS dipertajam sebagai **Mobile-First PWA** dengan bottom nav
- Dashboard v4: **Fatigue Alert** untuk roster `10:2` (≥13 hari kerja berturut)
- Validasi cuti roster `6:2`: min 6 bulan / max 12 bulan

### Removed
- Login lokal (100% single-door SSO)

---

## [v2.1] — 2024-XX-XX

### Added
- Integrasi penuh Kiosk & Mesin Fingerprint ADMS
- Engine Roster Proyek TTP & Tabung Cuti
- Mobile-First ESS (`SELF_ONLY`)

---

## [v2.0] — 2024-XX-XX

### Added
- 9 Proyek Full-Path (`ALL_PROJECTS`)
- 3-Tier RBAC (`hris_access_grants`)
- Pembersihan Bank Data (906 karyawan, 0 duplikasi NIK)


===FILE: LICENSE===
PROPRIETARY LICENSE

Copyright (c) 2025 PT. Teras Teknik Perdana
Powered by zeroharm.tech

All rights reserved.

This document and the information contained herein are the
confidential and proprietary property of PT. Teras Teknik Perdana.

No part of this document may be reproduced, distributed,
transmitted, disclosed, or used in any form or by any means
without the prior written permission of PT. Teras Teknik Perdana.

Unauthorized use, disclosure, or distribution of this document
is strictly prohibited and may result in legal action.

For inquiries, contact: legal@teras360.my.id


===FILE: .gitignore===
# Environment
.env
.env.*
!.env.example

# Logs
*.log
logs/
error_log

# OS
.DS_Store
Thumbs.db
desktop.ini

# IDE
.vscode/
.idea/
*.sublime-*

# Dependencies
/vendor/
composer.lock

# Uploads & Cache
/uploads/
/cache/
/tmp/

# Backup & Sensitive
*.bak
*.sql.gz
*.zip
*.rar
backup/

# Kredensial
config/database.php
config/sso.php


===FILE: docs/PRD-HR-Connect-v2.2.md===
---
title: "PRD — HR Connect: TERAS HRIS Platform"
version: "v2.2 (Final)"
status: "Ready for Approval"
owner: "PT. Teras Teknik Perdana"
powered_by: "zeroharm.tech"
last_updated: 2025-XX-XX
---

# Product Requirements Document (PRD) — Versi PHP
## **HR Connect — TERAS HRIS Platform (Powered by zeroharm.tech)**

> ⚠️ **CONFIDENTIAL** — Dilarang mendistribusikan ke pihak luar tanpa izin tertulis PT. Teras Teknik Perdana.

| Atribut | Detail |
| :--- | :--- |
| **Nama Produk** | HR Connect — TERAS HRIS (*Powered by zeroharm.tech*) |
| **Versi Dokumen** | **v2.2 (Final — Tambahan Face Enrollment & Subdomain Career Portal)** |
| **Status** | Ready for Approval — Siap Eksekusi Phase 2 |
| **Target Platform** | Web Application (Desktop-first untuk HR/Admin/HO) + **Mobile-First PWA** (khusus *Self-Only / ESS*) + **Public Career Subdomain (`career.domain.co.id`)** |
| **Tech Stack** | PHP 8.3+ (PDO), MySQL/MariaDB (`terasmyi_hris`, `terasmyi_sso`, `terasmyi_dashboard_k3`), Tailwind CSS, Chart.js 4, FontAwesome 6, SweetAlert2, PhpSpreadsheet, Face-Service |
| **Autentikasi** | Single-Door SSO melalui **SUMMIT** (`TerasSSO.php` + Transfer Ticket `?sso_ticket=`) |
| **URL Produksi / Lokal** | HRIS: `http://hris.localhost/` (`https://hris.teras360.my.id/`) • Career: `career.domain.co.id` (`/career/index.php`) |

---

## 1. Ringkasan Eksekutif & Penyesuaian Operasional PT. Teras Teknik Perdana

**HR Connect (TERAS HRIS)** adalah platform *Human Resource Management System* berbasis **PHP Native + MySQL** yang dirancang khusus untuk karakteristik bisnis **PT. Teras Teknik Perdana (TTP)** yang beroperasi secara **multi-proyek (9 lokasi resmi: HO Jakarta, Sumbawa Barat, Manyar Gresik, Balikpapan, Cilegon, Batu Bara, dan Tangerang)**.

### Pembaruan Utama pada Revisi v2.1
1. **Integrasi Penuh Kiosk & Mesin Fingerprint ADMS (`Solution X609`, `X105`, dll.):** Menggabungkan `modules/attendance/kiosk.php` (Face/QR Kiosk) dengan **ADMS Push Protocol Receiver (PHP)** agar seluruh mesin absensi fingerprint di site langsung mengirim log *real-time* ke database HRIS tanpa ekspor USB manual.
2. **Engine Roster Proyek TTP, Sistem Fatigue `13:1`, & Tabung Cuti:** Mendukung penuh pola kerja proyek **`4:2`**, **`6:2`** (tanpa fatigue, cuti min 6 bulan / max 12 bulan, plus **Tabung Cuti** bila masuk di hari libur/Sabtu-Minggu), **`10:2`** (dengan **Sistem Fatigue `13:1`** fleksibel hari off-nya), dan **`12:12`** (cuti tahunan).
3. **Mobile-First Employee Self-Service (`SELF_ONLY`):** Dirancang khusus untuk layar *smartphone* dengan fitur Slip Gaji, Izin Cuti/Tidak Masuk/Sakit, Absensi Online **Face Recognition + Live GPS Akurat + Anti Fake GPS**, serta **Employee Credit Score** (transparansi poin disiplin, pelanggaran/SP, dan ketepatan kehadiran).

---

## 2. Target Pengguna & Matriks RBAC 3-Tier (`includes/project_scope.php`)

Isolasi data ditegakkan di sisi server PHP melalui `hrisGetScope()` dan `hrisProjectWhere('k')` pada `project_scope.php`:

| Tier Scope | Role / Jabatan | Cakupan Data | Tampilan & Hak Akses Utama |
| :--- | :--- | :--- | :--- |
| **Tier 1 — `ALL_PROJECTS`** | IT Desk / Superadmin / Superuser, Direktur, GM Keuangan, GM ADM & Umum, Finance Staff HO, serta Staff HO yang memiliki approval di `hris_access_grants` | Seluruh **9 Proyek Resmi** (switcher Full-Path aktif) | Desktop Full Access (Dashboard, Direktori, Attendance/ADMS, **Face Enroll**, Roster, Payroll, Recruitment & Career Sync, Reports, Access Grants) |
| **Tier 2 — `PROJECT_ONLY`** | HR & GA Proyek, Admin Proyek, Finance Proyek, Site Manager | **Terkunci ketat** hanya pada `id_proyek` penugasannya (0 kebocoran lintas proyek) | Desktop Operasional Proyek (Karyawan proyeknya, Absensi & Kiosk/ADMS site, **Face Enroll Karyawan Proyek**, Approval Cuti/Roster/Fatigue, Payroll proyeknya) |
| **Tier 3 — `SELF_ONLY`** | Seluruh Karyawan / Pekerja Lapangan & HO | Hanya data pribadi (`id_karyawan` sendiri) | **Khusus Tampilan Mobile Phone (PWA ESS):** Slip Gaji, Izin Cuti/Sakit/Tidak Masuk, **Self Face Enroll (Daftar Wajah)**, Absensi Face + Live GPS Anti Fake GPS, **Employee Credit Score**, Jadwal Roster/Fatigue, Tabungan Cuti, ID Card QR |

---

## 3. Arsitektur Aplikasi PHP & Struktur Modul

```text
C:\laragon\www\teras360\hris\
├── index.php                          # Dashboard HR Connect v4 (LIVE — 5 KPI, Progress, Work Hours, Weekly Att, ZeroHarm AI, CSV)
├── login.php                          # Single-Door SUMMIT SSO Gateway (zero local login)
├── includes/
│   ├── bootstrap.php                  # Entry point tunggal (session, DB PDO, auth, helpers, project_scope)
│   ├── auth.php                       # requireLogin() -> konsumsi ?sso_ticket= via TerasSSO::currentUser()
│   ├── helpers.php                    # requireAuth(), url(), formatRupiah(), percent(), e(), navActive()
│   └── project_scope.php              # 9 Full-Path Projects + 3-Tier RBAC + hris_access_grants
├── api/
│   ├── adms/                          # [NEW] ADMS Push Protocol Receiver (Solution X609, X105, ZKTeco iClock)
│   │   ├── iclock/cdata.php           # Terima ATTLOG / OPERLOG / HEARTBEAT real-time dari mesin fingerprint
│   │   ├── iclock/getrequest.php      # Kirim perintah sinkronisasi user/jam ke mesin
│   │   └── iclock/devicecmd.php       # Konfirmasi eksekusi command mesin
│   ├── attendance/                    # Face check-in, Live GPS + Anti-Fake-GPS validator, QR scan, Kiosk API
│   ├── roster/                        # Kalkulator siklus Roster (4:2, 6:2, 10:2, 12:12), Fatigue 13:1 & Tabung Cuti
│   └── ess/                           # API Mobile ESS (Credit Score, Slip Gaji PDF, Izin/Sakit/Cuti)
├── pages/
│   ├── personalia/                    # Direktori 906 Karyawan, Multi-Area Assignments, Status Kerja
│   ├── absensi/                       # Dashboard Absensi, Manajemen Mesin ADMS, Roster & Fatigue, Cuti & Tabung Cuti
│   ├── payroll/                       # Siklus Payroll, Rincian Gaji, Slip Gaji PDF
│   ├── performance/                   # KPI, Probation Tracking, Awards & Employee Credit Score Rules
│   └── ess/                           # Mobile-First PWA Self-Service (Tier 3 Self-Only)
├── modules/
│   ├── attendance/kiosk.php           # Terminal Face/QR Kiosk Pos/Site (terintegrasi penuh ke attendance)
│   ├── attendance/enroll.php          # Face Enrollment (Registrasi/Update Wajah Karyawan -> face_encodings)
│   └── employees/, payroll/, recruitment/, punishment/, training/, analytics/
└── career/                            # [SUBDOMAIN KHUSUS: career.domain.co.id]
    └── index.php                      # Public Career Portal (Desain UI TIDAK diubah — integrasi penuh ke HRIS Recruitment)
```

---

## 4. Spesifikasi Fungsional Detail per Modul

### 4.1 Dashboard & ZeroHarm AI Insights (`index.php`) — **LIVE (Phase 1)**
- **Control Bar:** Switcher **9 Proyek Resmi Full-Path** (Tier 1) / *Project-Locked* (Tier 2), filter periode (`Today`, `7D`, `30D`, `90D`, `Custom`), dan **Export Report (CSV)**.
- **5 Executive KPI Cards:** *Total Users*, *Total Salary* (RBAC-protected), *Today Present*, *Today On Leave*, dan *Today Absent*.
- **3 Progress Insight Cards:** *Performance & Compliance* (MCU & Kontrak ≤ 30 hari), *Recruitment Efficiency*, dan *Leave Requests* (badge *Urgent*).
- **Work Hours Summary (Collapsible):** *Actual vs Planned vs Overtime*, *Daily Bar Chart*, dan *Department Performance*.
- **Weekly Attendance Overview (Area Chart) & 9-Project Distribution (Stacked Bar Chart)**.
- **ZeroHarm AI Insights (Live PHP/SQL Engine) + Recent Activity Live Feed** (termasuk *Fatigue Alert* untuk pekerja > 13 hari kerja berturut-turut pada roster `10:2`).

---

### 4.2 Attendance Hub: Kiosk (`kiosk.php`), Mesin Fingerprint ADMS & Absensi Mobile

Seluruh sumber absensi bermuara ke satu tabel terpadu `terasmyi_hris.attendance` dengan kolom `source` (`kiosk_face`, `kiosk_qr`, `adms_fingerprint`, `mobile_gps_face`, `manual_import`):

#### A. Face & QR Kiosk (`modules/attendance/kiosk.php`) & Face Enrollment (`modules/attendance/enroll.php`) — **TERINTEGRASI**
- **Face & QR Kiosk (`kiosk.php`):** Kanal resmi absensi di lobi HO, pos security, dan site office (mode layar penuh tablet/PC) dengan **Face Recognition** (`face-service` + `face_encodings`) dan fallback **QR ID Card** (`qr_scanner.php`).
- **Face Enrollment (`enroll.php` & `api/attendance/face-enroll.php`):**
  - **Admin / HR Proyek (Tier 1 & 2):** Mendaftarkan, memperbarui, atau me-reset sampel wajah karyawan dalam scope proyeknya langsung dari kamera webcam/tablet ke tabel `face_encodings` (dilengkapi indikator kualitas deteksi & status *Enrolled / Belum Enrolled*).
  - **Self Face Enrollment (Tier 3 Mobile ESS):** Pekerja dapat mendaftarkan wajahnya sendiri melalui kamera depan HP pada aktivasi awal (atau mengajukan *re-enroll* bila perubahan penampilan/perangkat dengan approval HR) sehingga langsung aktif untuk absensi Mobile & Kiosk.
- Otomatis mendeteksi Check-In / Check-Out, mencatat `face_confidence`, status terlambat, dan langsung muncul di *Live Feed* Dashboard serta rekap proyek terkait.

#### B. Integrasi Mesin Fingerprint ADMS (Solution Seri X609, X105, X100-C, ZKTeco)
- **Protokol ADMS Push (HTTP/HTTPS):** Mesin fingerprint di setiap lokasi proyek diarahkan ke URL server HRIS (`http(s)://<domain-hris>/api/adms/iclock/`).
- **Alur Kerja Otomatis:**
  1. Mesin mengirim *heartbeat* & `ATTLOG` (`SN`, `PIN`, `DateTime`, `Status`, `Verify`) secara *real-time* (atau *batch* otomatis saat internet site kembali online — *store-and-forward* bawaan mesin Solution).
  2. `api/adms/iclock/cdata.php` memvalidasi `serial_number` pada tabel `attendance_devices`, memetakan `PIN` ke `karyawan_baru.id_karyawan` (via `fingerprint_pin` / `id_pegawai`), mencegah duplikasi (*idempotent*), lalu menulis/memperbarui `check_in` & `check_out` di tabel `attendance`.
  3. Mengembalikan respons standar `OK` ke mesin agar mesin menandai log telah tersinkron.
- **Panel Manajemen Perangkat ADMS (di `pages/absensi/`):**
  - Daftar seluruh mesin fingerprint per proyek (Model: `Solution X609`, `Solution X105`, dll., Serial Number, Lokasi Proyek, status **Online / Offline** berdasarkan `last_heartbeat_at`, jumlah log hari ini, serta tombol *Sync Time / Reboot / Pull Users*).
  - Fitur `import_fingerprint.php` tetap dipertahankan sebagai cadangan apabila ada mesin lama tanpa ADMS.

---

### 4.3 Engine Roster Proyek TTP, Sistem Fatigue `13:1`, & Tabung Cuti (Leave Banking)

Karena operasional PT. Teras Teknik Perdana berbasis proyek, pengaturan jadwal kerja & hak cuti menggunakan **4 Pola Roster Resmi**:

| Kode Roster | Siklus Kerja & Off | Sistem Fatigue | Aturan Pengambilan Cuti | Tabung Cuti (*Leave Banking*) |
| :--- | :--- | :--- | :--- | :--- |
| **`4:2`** | **4 Minggu Kerja, 2 Minggu Off** | Tidak ada | Mengikuti jadwal *Field Break* 2 minggu off | Opsional bila masuk saat periode off atas surat perintah |
| **`6:2`** | **6 Hari Kerja, 2 Hari Off** | **Tidak ada sistem fatigue** | Hanya bisa ambil cuti **minimal per 6 bulan**, **maksimal 12 bulan** | **Aktif:** Jika masuk di hari libur/off (Sabtu/Minggu/Libur Nasional), otomatis menambah **Tabungan Cuti / Jatah Cuti Tahunan** |
| **`10:2`** | **10 Minggu Kerja, 2 Minggu Off** | **Aktif: Fatigue `13:1`** (13 hari kerja, 1 hari off) | Mengikuti siklus *Field Break* 2 minggu + jadwal Fatigue | **Aktif:** Hari libur yang tetap masuk kerja dapat ditabung ke saldo cuti |
| **`12:12`** | **Kerja Cuti Tahunan** (12 bulan kerja — hak cuti tahunan) | Tidak ada (mengikuti hari kerja standar / libur nasional) | Hak cuti tahunan 12 hari/tahun | **Aktif:** Masuk di hari libur resmi dapat ditabung sebagai cuti pengganti |

#### Detail Aturan Sistem Fatigue `13:1` (Roster `10:2`)
- **Mekanisme Standar:** Pekerja bekerja selama **13 hari berturut-turut** dan **wajib mendapatkan 1 hari Fatigue Off** di hari ke-14.
  - *Contoh:* Mulai kerja Senin minggu ke-1 → Minggu ke-1 tetap masuk kerja → **Minggu ke-2 mendapatkan Fatigue Off** (total 13 hari kerja + 1 hari off).
- **Fleksibilitas Hari Fatigue (`fatigue_day_override`):** Hari *Fatigue Off* **tidak harus selalu hari Minggu** — HR/Site Manager dapat menyesuaikan hari Fatigue Off ke hari kerja lain (misal Rabu atau Jumat) agar operasional site tidak kosong bersamaan (*staggered fatigue scheduling*).
- **Peringatan Otomatis (*Fatigue Guard*):** Jika pekerja pada roster `10:2` tercatat hadir ≥ 13 hari berturut-turut tanpa hari off, sistem memunculkan **Fatigue Alert** di Dashboard HRIS dan meneruskan indikator risiko fatigue ke modul K3.

#### Detail Aturan Tabung Cuti (*Leave Banking*)
- Setiap kali karyawan (terutama pada roster `6:2`, `10:2`, `12:12`) tercatat hadir di hari libur mingguan (Sabtu/Minggu) atau hari libur nasional (`hris_holidays`) dengan persetujuan atasan/HR, sistem otomatis mencatat kredit `+1 hari` ke buku besar **`hris_leave_bank_ledger`**.
- Saldo Tabungan Cuti terakumulasi ke jatah cuti tahunan dan dapat diajukan pada periode pengambilan cuti (untuk roster `6:2`: validasi sistem memastikan pengajuan cuti hanya terbuka setelah masa kerja/siklus **≥ 6 bulan** dan otomatis mengingatkan sebelum batas **12 bulan**).

---

### 4.4 Mobile-First Employee Self-Service (`TIER 3: SELF_ONLY` — Khusus Layar HP)

Modul `pages/ess/` dirancang **khusus tampilan Mobile Phone (Mobile-First PWA)** dengan *bottom navigation bar* (`Home`, `Absen`, `Izin/Cuti`, `Slip Gaji`, `Skor & Profil`), mendukung install ke *Home Screen* HP:

1. **Absensi Online Mobile (Face Recognition + Live GPS Akurat + Anti Fake GPS):**
   - **Face Recognition:** Verifikasi wajah langsung via kamera depan HP terhadap `face_encodings` milik pekerja + *liveness check* (anti foto layar/kertas).
   - **Live GPS Akurat & Geofencing:** Membaca koordinat `latitude`, `longitude`, dan `accuracy` secara *real-time* lalu memvalidasi jarak (Haversine) terhadap radius lokasi proyek (`proyek.latitude`, `proyek.longitude`, `radius_meter`).
   - **Proteksi Multi-Layer Anti Fake GPS:**
     - **Layer 1 (Client Telemetry):** Menolak lokasi bila `accuracy == 0` (indikasi *mock*), `accuracy > 100m` (sinyal lemah/spoof), atau koordinat statis identik 100% pada 3 *sampling* beruntun (*watchPosition* selama 2 detik).
     - **Layer 2 (Velocity / Impossible Travel Check di Server PHP):** Membandingkan jarak & waktu dari ping/absen sebelumnya — jika kecepatan perpindahan tidak masuk akal (> 150 km/jam dalam hitungan menit), absensi otomatis **ditolak** dan dicatat ke log anomali.
     - **Layer 3 (Timestamp & Environment Audit):** Validasi selisih jam perangkat vs server (`clock skew`), konsistensi *IP Geolocation*, serta deteksi flag `isFromMockProvider` (jika dibuka via Android WebView/Wrapper).
     - Semua percobaan manipulasi GPS dicatat ke `attendance_security_logs` dan langsung mengurangi **Employee Credit Score** setelah verifikasi HR.
2. **Slip Gaji Pribadi (`payslips`):**
   - Daftar riwayat slip gaji bulanan milik sendiri, rincian komponen (*Gaji Pokok, Tunjangan, Lembur, Potongan BPJS/PPh21, Net Pay*), dan tombol **Download PDF** (dilindungi verifikasi PIN/SSO).
3. **Pengajuan Izin & Cuti Mandiri:**
   - **Izin Cuti** (Tahunan, Roster/Field Break, Tabung Cuti — dengan validasi otomatis aturan roster `6:2` min 6 bln / max 12 bln).
   - **Izin Tidak Masuk** (keperluan keluarga/darurat dengan lampiran foto dari kamera HP).
   - **Izin Sakit** (wajib lampirkan foto Surat Keterangan Dokter dari kamera HP).
   - Pelacakan status persetujuan secara *real-time* (`Pending → Approved / Rejected` + catatan atasan).
4. **Employee Credit Score (Kartu Skor Disiplin & Kepatuhan Pribadi):**
   - Pekerja dapat melihat **Skor Kredit Pribadi (skala 0–100, default awal 100)** beserta rincian histori penambahan/pengurangan poin secara transparan:
     - **Pengurangan Poin (Contoh Bobot Dapat Dikonfigurasi HR):**
       - Terlambat masuk (`is_late = 1`): `-2` poin per kejadian.
       - Alpha / Mangkir tanpa keterangan: `-10` poin per hari.
       - Pelanggaran SP1 / SP2 / SP3 (dari `catatan_punishment_sp`): `-15` / `-25` / `-40` poin.
       - Pelanggaran **Vital Behavior / Life Saving Rules** (terintegrasi dari modul K3 `event-notification`): `-20` poin.
       - Percobaan **Fake GPS** terverifikasi: `-15` poin.
     - **Penambahan / Pemulihan Poin:**
       - 1 bulan penuh tanpa terlambat & tanpa alpha (*Zero Late Streak*): `+5` poin (maks. 100).
       - Penerima *Employee Award* atau pelapor *Hazard Report* aktif di K3: `+5` s/d `+10` poin.
   - Kategori status visual bersih: **90–100 (Excellent)**, **75–89 (Good)**, **60–74 (Perlu Pembinaan)**, **< 60 (Critical / Review HR)**.
5. **Fitur Pendukung Mobile ESS Lainnya:**
   - **Kalender Roster & Hitung Mundur Fatigue Off / Field Break** (pekerja tahu persis kapan jadwal off & kapan boleh ambil cuti).
   - **ID Card Digital + QR Code** (untuk scan di Kiosk/Pos Security).
   - **Pengumuman Perusahaan & Notifikasi Push** (approval cuti, slip gaji terbit, pengingat MCU/kontrak habis).
   - **Request Akses TERAS360** (`pages/ess/access_request.php`).

---

### 4.5 Portal Karir Subdomain Khusus (`career.domain.co.id` ← `/career/index.php`) & Modul Inti Lainnya

#### A. Portal Karir Publik (`career/index.php`) — **Dedicated Subdomain, Desain UI Tetap Utuh**
- **Deployment Subdomain:** Folder `/career/` dipetakan ke domain khusus **`career.domain.co.id`** (VirtualHost / DocumentRoot terpisah untuk akses publik tanpa perlu login SUMMIT SSO).
- **Aturan Desain:** **Tampilan dan desain UI yang sudah dibangun pada `career/index.php` TIDAK diotak-atik** (100% dipertahankan).
- **Integrasi Penuh dengan HRIS (`terasmyi_hris`):**
  - **Lowongan Kerja (`lowongan`):** Lowongan yang dibuat/diaktifkan dari HRIS (`modules/recruitment/lowongan.php`) otomatis tayang di `career.domain.co.id`.
  - **Pelamar & Dokumen (`kandidat`, `lamaran`, `kandidat_*`, `uploads/kandidat/`):** Lamaran yang masuk lewat `career.domain.co.id` otomatis tercatat di Recruitment Hub HRIS dan langsung memperbarui KPI *Recruitment Efficiency* di Dashboard HRIS (`index.php`).
  - **URL & Path Aware:** Konfigurasi base URL/asset & path upload lintas subdomain (`career.domain.co.id` ↔ `hris.domain.co.id`) dipastikan konsisten agar file CV/sertifikat/foto kandidat dapat dibuka lancar dari kedua sisi.
  - **Seamless Onboarding:** Kandidat yang lolos seleksi di HRIS (`modules/recruitment/onboarding.php`) dapat langsung dikonversi menjadi karyawan aktif pada `karyawan_baru` sesuai penempatan proyeknya.

#### B. Modul Inti & Ekspansi Lainnya
- **Direktori Karyawan (`pages/personalia/`):** 906 karyawan terverifikasi (0 duplikasi NIK), 61 penugasan multi-area (`karyawan_project_assignments`), mode Cards & Table, ekspor Excel/PDF, cetak ID Card.
- **Payroll (`pages/payroll/`, `modules/payroll/`):** Pengaturan gaji, eksekusi `payroll_runs` & `payroll_details` (termasuk hitung lembur & potongan absensi dari ADMS/Mobile/Kiosk), 5-tahap *Payroll Cycle*, dan generate `payslips`.
- **Modul Phase 3:** **Holiday Management** (`hris_holidays`), **Company Announcements** (`hris_announcements`), **Employee Awards & Leaderboard** (`hris_awards`), **Task Management** (`hris_tasks`), **Media Library** (`hris_media_files`), dan **Organizational Chart**.

---

## 5. Skema Tabel MySQL Tambahan (Migrasi Phase 2)

```sql
-- 1. Manajemen Mesin Fingerprint ADMS (Solution X609, X105, dll.)
CREATE TABLE attendance_devices (
    id_device INT AUTO_INCREMENT PRIMARY KEY,
    serial_number VARCHAR(64) NOT NULL UNIQUE,
    device_name VARCHAR(120) NOT NULL,
    device_model VARCHAR(80) DEFAULT 'Solution X609',
    id_proyek INT NOT NULL,
    ip_address VARCHAR(45) NULL,
    last_heartbeat_at DATETIME NULL,
    last_attlog_at DATETIME NULL,
    is_active TINYINT(1) NOT NULL DEFAULT 1,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 2. Master Pola Roster Proyek TTP (4:2, 6:2, 10:2 + Fatigue 13:1, 12:12)
CREATE TABLE hris_roster_patterns (
    id INT AUTO_INCREMENT PRIMARY KEY,
    code VARCHAR(20) NOT NULL UNIQUE,
    name VARCHAR(100) NOT NULL,
    work_units INT NOT NULL,
    off_units INT NOT NULL,
    unit_type ENUM('days','weeks','months') NOT NULL,
    has_fatigue TINYINT(1) NOT NULL DEFAULT 0,
    fatigue_work_days INT NULL DEFAULT 13,
    fatigue_off_days INT NULL DEFAULT 1,
    min_leave_months INT NULL DEFAULT 0,
    max_leave_months INT NULL DEFAULT 12,
    allow_leave_banking TINYINT(1) NOT NULL DEFAULT 1
);

-- 3. Penugasan Roster & Jadwal Fatigue per Karyawan
CREATE TABLE hris_employee_rosters (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_karyawan INT NOT NULL,
    id_proyek INT NOT NULL,
    roster_code VARCHAR(20) NOT NULL,
    cycle_start_date DATE NOT NULL,
    fatigue_day_of_week TINYINT NULL,
    is_active TINYINT(1) NOT NULL DEFAULT 1,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_emp_roster (id_karyawan, is_active)
);

-- 4. Buku Besar Tabung Cuti (Leave Banking)
CREATE TABLE hris_leave_bank_ledger (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_karyawan INT NOT NULL,
    source_date DATE NOT NULL,
    entry_type ENUM('credit_holiday_work','credit_weekend_work','debit_leave_used','adjustment') NOT NULL,
    days_delta DECIMAL(5,2) NOT NULL,
    notes VARCHAR(255) NULL,
    approved_by INT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_emp_bank (id_karyawan, source_date)
);

-- 5. Buku Besar Employee Credit Score
CREATE TABLE hris_credit_score_ledger (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_karyawan INT NOT NULL,
    category ENUM('attendance_late','attendance_alpha','punishment_sp','k3_vital_behavior','fake_gps_attempt','award_bonus','zero_late_streak','manual_adjustment') NOT NULL,
    points_delta INT NOT NULL,
    reference_id INT NULL,
    description VARCHAR(255) NOT NULL,
    recorded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_emp_score (id_karyawan, recorded_at)
);

-- 6. Log Keamanan Absensi Mobile (Anti Fake GPS Audit)
CREATE TABLE attendance_security_logs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_karyawan INT NOT NULL,
    event_type ENUM('mock_accuracy_zero','impossible_travel','static_coords','clock_skew','out_of_geofence','mock_provider') NOT NULL,
    lat DECIMAL(10,7) NULL,
    lng DECIMAL(10,7) NULL,
    accuracy_m DECIMAL(8,2) NULL,
    device_info VARCHAR(255) NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_sec_emp (id_karyawan, created_at)
);
```

---

## 6. Roadmap Implementasi Bertahap (PHP TERAS HRIS)

| Fase | Lingkup Pekerjaan | Status |
| :--- | :--- | :--- |
| **Phase 1 (Selesai)** | Single-Door SUMMIT SSO, 9 Proyek Full-Path, 3-Tier RBAC, Pembersihan Bank Data (906 karyawan, 0 duplikasi NIK), dan **Dashboard HR Connect v4 (`index.php`)**. | **Live** |
| **Phase 2A — Attendance, Face Enroll & ADMS** | Integrasi **ADMS Push Receiver** (`Solution X609`, `X105`, dll.) + Panel Manajemen Perangkat ADMS + Integrasi penuh `modules/attendance/kiosk.php` & **Face Enrollment (`enroll.php`)**. | **Siap Eksekusi** |
| **Phase 2B — Roster, Fatigue & Tabung Cuti** | Engine Roster Proyek (`4:2`, `6:2`, `10:2` + **Fatigue `13:1` fleksibel**, `12:12`), validasi cuti min 6 / max 12 bulan, dan **Tabung Cuti (*Leave Banking*)**. | **Siap Eksekusi** |
| **Phase 2C — Mobile-First ESS & Career Sync** | Redesign Mobile PWA `pages/ess/` (**Self Face Enroll**, Absensi Face + Live GPS + Anti Fake GPS, Slip Gaji PDF, Izin Cuti/Sakit/Tidak Masuk, **Employee Credit Score 0–100**) + Integrasi penuh subdomain **`career.domain.co.id`** (`/career/index.php` tanpa mengubah desain UI). | **Siap Eksekusi** |
| **Phase 3 — Kolaborasi & Enterprise** | Holiday Calendar, Company Announcements, Employee Awards & Leaderboard, Task Management, Media Library, Org Chart, dan AI Candidate Matching. | Terjadwal |


===FILE: docs/arsitektur-modul.md===
# Arsitektur Modul — TERAS HRIS

## Pohon Direktori

```text
hris/
├── index.php                  # Dashboard HR Connect v4
├── login.php                  # SUMMIT SSO Gateway
├── includes/
│   ├── bootstrap.php          # Entry point tunggal
│   ├── auth.php               # requireLogin() -> konsumsi ?sso_ticket=
│   ├── helpers.php            # requireAuth, url, formatRupiah, e
│   └── project_scope.php      # 3-Tier RBAC + hris_access_grants
├── api/
│   ├── adms/iclock/           # ADMS Push Receiver
│   │   ├── cdata.php          # ATTLOG/OPERLOG/HEARTBEAT
│   │   ├── getrequest.php     # Sync user/jam
│   │   └── devicecmd.php      # Command confirmation
│   ├── attendance/            # Face, GPS, QR, Kiosk
│   ├── roster/                # Roster & Fatigue Engine
│   └── ess/                   # Mobile ESS API
├── pages/
│   ├── personalia/            # Direktori Karyawan
│   ├── absensi/               # Attendance Hub
│   ├── payroll/               # Payroll Cycle
│   ├── performance/           # KPI & Credit Score
│   └── ess/                   # Mobile PWA
├── modules/
│   ├── attendance/
│   │   ├── kiosk.php          # Face/QR Kiosk
│   │   └── enroll.php         # Face Enrollment
│   ├── employees/
│   ├── payroll/
│   ├── recruitment/
│   ├── punishment/
│   ├── training/
│   └── analytics/
└── career/
    └── index.php              # Public Career Portal
```

## Dependensi Antar-Modul

| Modul | Bergantung pada | Menghasilkan |
| :--- | :--- | :--- |
| `modules/attendance/kiosk.php` | `api/attendance/*`, `face_encodings` | `attendance.source = kiosk_face/kiosk_qr` |
| `api/adms/iclock/cdata.php` | `attendance_devices`, `fingerprint_pin` | `attendance.source = adms_fingerprint` |
| `api/attendance/mobile.php` | `face_encodings`, `proyek`, `attendance_security_logs` | `attendance.source = mobile_gps_face` |
| `api/roster/*` | `hris_roster_patterns`, `hris_employee_rosters` | Jadwal Fatigue & Tabung Cuti |
| `api/ess/credit-score.php` | `hris_credit_score_ledger` | Skor 0–100 |
| `career/index.php` | `lowongan`, `kandidat`, `lamaran` | Recruitment Efficiency KPI |

## Alur Data Absensi (Unified)

```text
[Kiosk Face/QR]  ─┐
[ADMS Fingerprint] ─┼──▶ terasmyi_hris.attendance ──▶ Dashboard Live Feed
[Mobile GPS+Face] ─┘       (source column)         ──▶ Payroll Deduction
                                                   ──▶ Credit Score Ledger
                                                   ──▶ Fatigue Guard
```


===FILE: docs/roadmap-implementasi.md===
# Roadmap Implementasi — TERAS HRIS

## Phase 1 — Foundation ✅ Live
- Single-Door SUMMIT SSO
- 9 Proyek Full-Path (`ALL_PROJECTS`)
- 3-Tier RBAC + `hris_access_grants`
- Pembersihan Bank Data (906 karyawan, 0 duplikasi NIK)
- Dashboard HR Connect v4 (`index.php`)

## Phase 2A — Attendance, Face Enroll & ADMS
- [ ] ADMS Push Receiver (`api/adms/iclock/cdata.php`)
- [ ] Panel Manajemen Mesin Fingerprint
- [ ] Integrasi Kiosk (`modules/attendance/kiosk.php`)
- [ ] Face Enrollment (`modules/attendance/enroll.php`)
- [ ] Self Face Enrollment (ESS)

## Phase 2B — Roster, Fatigue & Tabung Cuti
- [ ] Engine Roster 4:2, 6:2, 10:2, 12:12
- [ ] Sistem Fatigue 13:1 (fleksibel)
- [ ] Leave Banking Ledger
- [ ] Validasi cuti 6:2 (min 6 bln / max 12 bln)
- [ ] Fatigue Alert di Dashboard

## Phase 2C — Mobile ESS & Career Sync
- [ ] PWA Mobile-First (`pages/ess/`)
- [ ] Absensi Face + Live GPS + Anti Fake GPS 3-Layer
- [ ] Slip Gaji PDF
- [ ] Izin Cuti/Sakit/Tidak Masuk
- [ ] Employee Credit Score (0–100)
- [ ] Career Portal `career.domain.co.id`

## Phase 3 — Kolaborasi & Enterprise
- [ ] Holiday Calendar
- [ ] Company Announcements
- [ ] Employee Awards & Leaderboard
- [ ] Task Management
- [ ] Media Library
- [ ] Organizational Chart
- [ ] AI Candidate Matching


===FILE: sql/phase2-attendance-devices.sql===
-- =========================================================
-- Phase 2A: ADMS Attendance Devices
-- Database: terasmyi_hris
-- =========================================================

CREATE TABLE IF NOT EXISTS attendance_devices (
    id_device         INT AUTO_INCREMENT PRIMARY KEY,
    serial_number     VARCHAR(64)  NOT NULL UNIQUE,
    device_name       VARCHAR(120) NOT NULL,
    device_model      VARCHAR(80)  DEFAULT 'Solution X609',
    id_proyek         INT          NOT NULL,
    ip_address        VARCHAR(45)  NULL,
    last_heartbeat_at DATETIME     NULL,
    last_attlog_at    DATETIME     NULL,
    is_active         TINYINT(1)   NOT NULL DEFAULT 1,
    created_at        TIMESTAMP    DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_device_proyek (id_proyek, is_active)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


===FILE: sql/phase2-roster-patterns.sql===
-- =========================================================
-- Phase 2B: Roster Patterns & Employee Rosters
-- =========================================================

CREATE TABLE IF NOT EXISTS hris_roster_patterns (
    id                  INT AUTO_INCREMENT PRIMARY KEY,
    code                VARCHAR(20)  NOT NULL UNIQUE,
    name                VARCHAR(100) NOT NULL,
    work_units          INT          NOT NULL,
    off_units           INT          NOT NULL,
    unit_type           ENUM('days','weeks','months') NOT NULL,
    has_fatigue         TINYINT(1)   NOT NULL DEFAULT 0,
    fatigue_work_days   INT          NULL DEFAULT 13,
    fatigue_off_days    INT          NULL DEFAULT 1,
    min_leave_months    INT          NULL DEFAULT 0,
    max_leave_months    INT          NULL DEFAULT 12,
    allow_leave_banking TINYINT(1)   NOT NULL DEFAULT 1
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

INSERT IGNORE INTO hris_roster_patterns
(code, name, work_units, off_units, unit_type, has_fatigue, fatigue_work_days, fatigue_off_days, min_leave_months, max_leave_months, allow_leave_banking)
VALUES
('4:2',   '4 Minggu Kerja, 2 Minggu Off',  4,  2,  'weeks',  0, NULL, NULL, 0, 12, 1),
('6:2',   '6 Hari Kerja, 2 Hari Off',      6,  2,  'days',   0, NULL, NULL, 6, 12, 1),
('10:2',  '10 Minggu Kerja, 2 Minggu Off', 10, 2,  'weeks',  1, 13,   1,    0, 12, 1),
('12:12', 'Kerja Cuti Tahunan',            12, 12, 'months', 0, NULL, NULL, 0, 12, 1);

CREATE TABLE IF NOT EXISTS hris_employee_rosters (
    id                  INT AUTO_INCREMENT PRIMARY KEY,
    id_karyawan         INT          NOT NULL,
    id_proyek           INT          NOT NULL,
    roster_code         VARCHAR(20)  NOT NULL,
    cycle_start_date    DATE         NOT NULL,
    fatigue_day_of_week TINYINT      NULL,
    is_active           TINYINT(1)   NOT NULL DEFAULT 1,
    updated_at          TIMESTAMP    DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_emp_roster (id_karyawan, is_active)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


===FILE: sql/phase2-leave-banking-credit-score.sql===
-- =========================================================
-- Phase 2B/2C: Leave Banking, Credit Score, Security Logs
-- =========================================================

CREATE TABLE IF NOT EXISTS hris_leave_bank_ledger (
    id           INT AUTO_INCREMENT PRIMARY KEY,
    id_karyawan  INT          NOT NULL,
    source_date  DATE         NOT NULL,
    entry_type   ENUM('credit_holiday_work','credit_weekend_work','debit_leave_used','adjustment') NOT NULL,
    days_delta   DECIMAL(5,2) NOT NULL,
    notes        VARCHAR(255) NULL,
    approved_by  INT          NULL,
    created_at   TIMESTAMP    DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_emp_bank (id_karyawan, source_date)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE IF NOT EXISTS hris_credit_score_ledger (
    id           INT AUTO_INCREMENT PRIMARY KEY,
    id_karyawan  INT          NOT NULL,
    category     ENUM('attendance_late','attendance_alpha','punishment_sp','k3_vital_behavior','fake_gps_attempt','award_bonus','zero_late_streak','manual_adjustment') NOT NULL,
    points_delta INT          NOT NULL,
    reference_id INT          NULL,
    description  VARCHAR(255) NOT NULL,
    recorded_at  TIMESTAMP    DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_emp_score (id_karyawan, recorded_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE IF NOT EXISTS attendance_security_logs (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    id_karyawan INT          NOT NULL,
    event_type  ENUM('mock_accuracy_zero','impossible_travel','static_coords','clock_skew','out_of_geofence','mock_provider') NOT NULL,
    lat         DECIMAL(10,7) NULL,
    lng         DECIMAL(10,7) NULL,
    accuracy_m  DECIMAL(8,2)  NULL,
    device_info VARCHAR(255)  NULL,
    created_at  TIMESTAMP     DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_sec_emp (id_karyawan, created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
