# Product Requirements Document (PRD)
## **HR Connect — HRIS System & TERAS360 Dashboard (Kolabo Teal UI & Mobile ESS Companion)**

| Atribut | Detail |
| :--- | :--- |
| **Nama Produk** | HR Connect — HRIS System & TERAS360 Dashboard |
| **Versi Dokumen** | **v3.0 (Clean Kolabo Teal UI + Single-Column Accordion Sidebar + Mobile ESS Companion)** |
| **Status** | Draft — Menunggu Approval User Sebelum Implementasi PHP |
| **Target Aplikasi** | `C:\laragon\www\teras360\hris\` (`http://hris.localhost/`) |
| **Design System** | Tailwind CSS (`kolabo` Teal Palette `#0D9488`–`#0B4646`), `Plus Jakarta Sans` + `Inter`, **Lucide Icons**, Light/Dark Class Mode |
| **Backend & Auth** | PHP 8.3 (PDO), MySQL (`terasmyi_hris`, `terasmyi_dashboard_k3`, `terasmyi_sso`), Single-Door SUMMIT SSO + 3-Tier RBAC |

---

## 1. Ringkasan Eksekutif & Tujuan Desain

PRD ini mendefinisikan spesifikasi desain ulang antarmuka dan struktur navigasi **HR Connect (`TERAS HRIS`)** agar selaras penuh dengan *blueprint* HTML **Kolabo Teal Enterprise**, sekaligus terhubung **100% secara live** ke database MySQL PT. Teras Teknik Perdana (`9 proyek aktif`, `karyawan_baru`, `attendance`, `attendance_devices`, `hris_employee_rosters`, `hris_calendar_reminders`, `hris_credit_score_ledger`, `lowongan/lamaran`).

### Prinsip Utama UI/UX
1. **Clean Single-Column Left Sidebar (`w-64`)** dengan *segmented tab* **`Menu` | `Settings`**, *accordion submenu* yang rapi (hanya 1 accordion terbuka dalam satu waktu), badge notifikasi kontekstual (`67` dokumen MCU/Kontrak, `12` lowongan/rekrutmen, `6` reminder kalender, `9` proyek), serta responsif off-canvas di mobile.
2. **Design Tokens `kolabo` (Teal Enterprise) + Dark Mode (`html.dark`):**
   - Latar `#F8FAFC` (Light) / `slate-950` (Dark), kartu putih / `slate-900` dengan border tipis, `.stat-card` hover lift (`-2px`) + aksen `#99F6E4`.
   - Ikonografi vektor seragam menggunakan **Lucide Icons**.
3. **Interaktivitas Cepat:** Command Palette Global Search (**`Ctrl + K`**), Filter Proyek langsung memperbarui KPI & Donut Chart, *Toast Notification Banner*, dan **Floating View Switcher (`Web HRIS` ↔ `Mobile ESS`)**.

---

## 2. Arsitektur Navigasi: Clean Single-Column Sidebar (`w-64`)

### 2.1 Header Brand & Switcher
- Logo monogram `///` (`bg-gradient-to-br from-kolabo-700 to-kolabo-900`) + judul **`HRIS SYSTEM`** dan subjudul **`HR CONNECT • TERAS360`**.
- **Segmented Switcher (`Menu` | `Settings`)** di bawah header sidebar.

### 2.2 Tab 1 — `Menu` (5 Kelompok Hirarki Resmi & Pemetaan Rute PHP)

| Kelompok | Menu & Ikon Lucide | Submenu Accordion & Badge Live | Target Rute PHP |
| :--- | :--- | :--- | :--- |
| **1. OVERVIEW & SHORTCUT** | • **Summit** (`compass`)<br>• **Dashboard** (`layout-grid`, *active default*) | Langsung menuju Portal SUMMIT (dengan SSO transfer ticket) atau Dashboard HR Connect | `summit.localhost/`<br>`index.php` |
| **2. EMPLOYEE & COMPANY** | • **Company Management** (`building-2`)<br>• **Employee Management** (`users`, badge merah dinamis)<br>• **Recruitment Management** (`graduation-cap`, badge teal) | **Employee Management:**<br>1. *Direktori 9 Proyek* (badge total karyawan aktif)<br>2. *Status Kerja & Probation* (badge merah MCU/Kontrak ≤ 30 hr)<br>3. *Data Karyawan Lengkap*<br>4. *Blacklist Karyawan*<br>**Recruitment Management:**<br>1. *Recruitment Hub* (badge lowongan aktif)<br>2. *Kelola Lowongan*<br>3. *Onboarding Pelamar*<br>4. *Public Career Portal* (`career.domain.co.id`) | `pages/settings/index.php?tab=access`<br>`pages/personalia/index.php`<br>`pages/performance/index.php?tab=probation`<br>`modules/employees/index.php`<br>`modules/employees/blacklist.php`<br>`modules/recruitment/index.php`<br>`modules/recruitment/lowongan.php`<br>`modules/recruitment/onboarding.php`<br>`career/index.php` |
| **3. TIME & ATTENDANCE** | • **Shift Management** (`clock`)<br>• **Attendance Management** (`clipboard-check`, accordion)<br>• **Visit Attendance** (`map-pin`)<br>• **Team Meeting** (`video`)<br>• **Holidays Calendar** (`calendar-days`, badge merah)<br>• **Leave** (`file-minus`)<br>• **WFH Request** (`home`)<br>• **Credit Score & Awards** (`shield-check`, accordion) | **Attendance Management:**<br>1. *Log Kehadiran Harian*<br>2. *Mesin ADMS (X609 / X105)*<br>3. *Face & QR Kiosk* + *Face Enrollment*<br>4. *Import USB Fingerprint*<br>**Credit Score & Awards:**<br>1. *Credit Score (0–100)*<br>2. *Surat Peringatan (SP1–3)*<br>3. *Awards & Recognition*<br>4. *KPI & Evaluasi Tim* | `pages/absensi/index.php?tab=roster`<br>`pages/absensi/index.php?tab=overview`<br>`pages/absensi/index.php?tab=adms`<br>`modules/attendance/kiosk.php` & `enroll.php`<br>`modules/attendance/import_fingerprint.php`<br>`pages/calendar/index.php`<br>`pages/absensi/index.php?tab=leave`<br>`pages/performance/index.php?tab=credit_score`<br>`modules/punishment/index.php`<br>`pages/performance/index.php?tab=awards`<br>`modules/performance/index.php` |
| **4. ASSETS & BENEFITS** | • **Assets Management** (`laptop`) | Inventaris perangkat kerja, serah terima aset & fasilitas karyawan | `pages/performance/index.php?tab=tasks` |
| **5. PROJECT MANAGEMENT & TOOLS** | • **Project Management** (`folder-kanban`, badge `9`)<br>• **Clients** (`briefcase`) | Monitoring 9 site proyek aktif, alokasi SDM, Payroll & klien mitra | `pages/personalia/index.php` / `pages/payroll/index.php` |

### 2.3 Tab 2 — `Settings` Panel (Dalam Sidebar)
1. **My Profile** → `pages/settings/index.php?tab=profile`
2. **Security & Google 2FA** → `pages/settings/index.php?tab=security`
3. **Notifications** → `pages/settings/index.php?tab=notifications`
4. **Appearance Card (Terbuka):**
   - **Theme Mode:** `☀ Light` | `☾ Dark` (mengubah `document.documentElement.classList` ke `dark` dan menyimpan ke `localStorage`)
   - **Language:** `ID` | `EN` | `中文` (sinkron dengan pill `#header-lang-pill` di Topbar)
5. **SSO & Module Access** → `pages/settings/index.php?tab=access` (khusus Superadmin/IT Desk untuk `hris_access_grants`)
6. **Help Center** → Tautan ke `http://itdesk.localhost/help.php?from=hris`

---

## 3. Spesifikasi Top Header Bar & Command Palette (`Ctrl + K`)

- **Kiri Topbar:**
  - Tombol hamburger mobile + **Global Search Trigger** bertuliskan *"Cari karyawan, absensi, slip gaji..."* dengan badge shortcut **`Ctrl K`**.
  - Klik bar pencarian atau tekan `Ctrl + K` membuka **Command Palette Modal (`#search-modal`)** yang menampilkan *Quick Navigation* ber-badge langsung serta pencarian live ke `api/search.php` (karyawan, absensi, slip gaji, menu).
- **Kanan Topbar:**
  - **Live Sync Status Indicator:** Titik hijau berdenyut (`.live-dot`) + teks **`ADMS X609 Synced • 9 Proyek`** dan subjudul `hris.localhost • v3.60`.
  - Tombol **`Switch to TERAS360`** (`bg-kolabo-600`) → langsung menuju Portal SUMMIT TERAS360 membawa transfer ticket SSO aktif.
  - **Language Pill (`ID` / `EN` / `中文`)**, **Notification Bell** (badge merah peringatan dokumen MCU/Kontrak & cuti pending), serta **Avatar / Inisial Profil** + menu Logout.

---

## 4. Spesifikasi Main Canvas: Web HRIS View (`#web-dashboard-view`)

### 4.1 Header Halaman & Filter Scope Proyek
- Judul **`HR Connect`** dengan badge `HRIS System` + deskripsi *"Ringkasan SDM, kehadiran mesin ADMS X609, roster & kepatuhan 9 proyek aktif"*.
- **Project Scope Dropdown (`hrisGetScope()`):**
  - **Tier 1 (HO / Direktur / Finance / Superadmin):** Dropdown **`All Projects (9 Proyek • <N>)`** beserta seluruh **9 Proyek Resmi** (`Head Officer - Jakarta`, `Sumbawa Barat`, `Manyar Gresik` ×3, `Balikpapan`, `Cilegon`, `Batu Bara`, `Tangerang`) — mengganti dropdown memfilter seluruh KPI, Donut Chart, dan tabel.
  - **Tier 2 (HR/Admin/Finance Proyek):** Terkunci pada proyek penugasannya.
- **Pill Tanggal Aktif:** Menampilkan tanggal berjalan (klik langsung membuka **HR Calendar & To-Do Reminder** `pages/calendar/index.php`).

### 4.2 Top Section: 8 Stat Cards Grid (8 Kolom) + Attendance & Roster Donut Widget (4 Kolom)

#### A. 8 Kartu KPI Eksekutif (2 Baris × 4 Kolom — 100% Query MySQL Live)
| # | Label Kartu | Nilai Utama | Subteks / Detail | Aksi Klik |
| :-: | :--- | :--- | :--- | :--- |
| 1 | **TOTAL USERS** | Jumlah karyawan aktif (`karyawan_baru`) | `Perm: <X> • Contract: <Y>` | Toast rincian / ke Personalia |
| 2 | **ACTIVE PROJECTS** | Jumlah proyek berjalan (`9` atau `1` jika terfilter) | `Site & HQ units` | Ke Direktori 9 Proyek |
| 3 | **MCU & KONTRAK** | Jumlah dokumen habis/kadaluarsa ≤ 30 hari | `Dokumen MCU / Kontrak` (teks rose) | Ke tab Probation & MCU |
| 4 | **ON ROSTER OFF** | Jumlah karyawan sedang jadwal off / cuti (`hris_employee_rosters` + `leave_requests`) | `Pola 4:2, 6:2, 10:2` | Ke Tab Roster & Fatigue |
| 5 | **CHECK-IN TODAY** | Jumlah check-in hari ini (`attendance` `CURDATE()`) | `ADMS X609 & Mobile Face` | Ke Log Kehadiran Harian |
| 6 | **RECRUITMENT** | Jumlah lowongan terbuka / kandidat aktif (`lowongan` & `lamaran`) | `Kandidat dalam proses` | Ke Recruitment Hub |
| 7 | **CREDIT SCORE** | Rata-rata **Employee Credit Score** (`0–100` dari `hris_credit_score_ledger`) | `Skala 0–100 (Sangat Baik)` | Ke Leaderboard Credit Score |
| 8 | **HR REMINDERS** | Jumlah To-Do & reminder pending (`hris_calendar_reminders` + `hris_tasks`) | `To-do & kalender HR` | Ke HR Calendar & To-Do |

#### B. Attendance & Roster SVG Donut Widget (4 Kolom Kanan)
- Header **Attendance & Roster** + subjudul *Mesin ADMS (X609) & Face/QR Kiosk* + pill indikator **`Today`**.
- **SVG Donut Interaktif** (`stroke-dasharray="289"`, transisi halus) menampilkan rasio **`Check-In / Total Karyawan Aktif`** di tengah lingkaran.
- 3 kartu ringkasan di bawah donut: **Hadir** (kolabo teal), **Roster Off / Cuti** (amber), dan **Belum Check-In / Shift Malam** (slate).

### 4.3 Bottom Section: Tabel Direktori 9 Proyek & Status Kepatuhan
- Header dengan badge **`ADMS X609 Active`** dan tombol **`+ Tambah Karyawan`** (`modules/employees/add.php`).
- Kolom tabel dari query *real-time* 9 Proyek (`terasmyi_dashboard_k3.proyek` + `karyawan_baru` + `attendance_devices` + `hris_employee_rosters`):
  1. **Nama Proyek / Site** (Nama resmi proyek & lokasi)
  2. **Pola Roster** (Pola dominan proyek: `Roster 10:2`, `Roster 6:2`, `Roster 4:2`, `Reguler Office (12:12)`)
  3. **Personel** (Total karyawan aktif di proyek tersebut, mis. `142 SDM`)
  4. **Kepatuhan MCU & Kontrak** (Progress bar persentase + angka `%` patuh)
  5. **Perangkat Absensi** (Nama unit ADMS dari `attendance_devices`, mis. `ADMS X609`, `Solution X105`, `Face & QR Kiosk`)
  6. **Status** (Badge: `Optimal`, `N MCU Renewal`, `Synced`, `Active`)

---

## 5. Spesifikasi Mobile ESS Companion View (`#mobile-preview-view`) & Floating Switcher

### 5.1 Floating View Switcher (Pojok Kanan Bawah)
- Pill mengambang gelap (`bg-kolabo-900/95`) berisi tombol **`Web HRIS`** (`monitor`) dan **`Mobile ESS`** (`smartphone`).
- Memungkinkan HR/Admin/Karyawan beralih seketika antara tampilan desktop HRIS dan preview interaktif **Mobile ESS Companion** (juga menyediakan tombol langsung ke mode fungsional penuh `pages/ess/index.php`).

### 5.2 Dual-Phone Mobile ESS Screen
1. **Phone 1 — Clock-In / Absensi Mobile & Geofencing:**
   - Header sapaan nama pekerja + lokasi proyek + badge pola Roster (`Roster 6:2` / `10:2`).
   - Jam digital *real-time* (`#mobile-live-clock`), shift kerja, dan tombol bulat **`CLOCK IN` / `CLOCK OUT`** terhubung ke endpoint absensi (`api/attendance/face-checkin.php`).
   - Kartu 3 kolom: **Clock In**, **Clock Out**, dan **Credit Score (`XX / 100`)**.
   - Indikator **Geofencing Site Proyek & ADMS X609** (`Terverifikasi`).
2. **Phone 2 — HRIS ESS Menu Grid (8 Modul Mandiri) & Status Dokumen:**
   - Grid 8 ikon cepat: **Absensi**, **Tabung Cuti**, **Roster**, **Slip Gaji**, **MCU**, **Credit**, **QR Kiosk**, dan **TERAS360** (masing-masing menaut ke rute `pages/ess/index.php?view=...` yang relevan).
   - Kartu status **Kontrak & MCU Aktif** serta **Penempatan Proyek & Roster** milik pekerja.

---

## 6. Rencana Implementasi File PHP (Saat Disetujui)

| File | Perubahan yang Akan Dilakukan |
| :--- | :--- |
| `layout/header.php` | Menggunakan Tailwind `kolabo` palette + `Plus Jakarta Sans` + **Lucide Icons** + `darkMode: 'class'`, Topbar pencarian `Ctrl+K` Command Palette Modal (`#search-modal`), live status `ADMS X609 Synced • 9 Proyek`, tombol `Switch to TERAS360`, `#header-lang-pill`, lonceng notifikasi & Toast Notification Banner. |
| `layout/sidebar.php` | Konversi ke **Clean Single-Column Left Sidebar (`w-64`)** gaya `kolabo` dengan header `/// HRIS SYSTEM`, tab `Menu` \| `Settings`, 5 kelompok menu resmi + accordion submenu (dengan badge live `843`, `67`, `12`, `6`, `9`), dan panel `Settings` (**Light/Dark Mode** & **ID/EN/中文**). |
| `index.php` | Mengimplementasikan layout **HR Connect**: Page title + Project Scope Dropdown (`9 Proyek`), 8 Stat Cards Grid (live MySQL), Attendance & Roster SVG Donut Widget, Tabel Direktori 9 Proyek & Status Kepatuhan MCU/ADMS, serta **Floating View Switcher** & **Dual-Phone Mobile ESS Companion** terintegrasi data karyawan & endpoint absensi. |

---

## 7. Kriteria Verifikasi Pasca-Eksekusi
1. `php -l` pada `layout/header.php`, `layout/sidebar.php`, dan `index.php` bersih tanpa error.
2. Seluruh halaman lain (`pages/absensi/index.php`, `pages/calendar/index.php`, `pages/ess/index.php`, `pages/payroll/index.php`, `pages/performance/index.php`, `pages/personalia/index.php`, `pages/settings/index.php`) tetap merender normal (`HTTP 200 OK`) dengan Clean Single-Column Sidebar baru tanpa layout bentrok.
3. Seluruh fitur interaktif berfungsi: `Ctrl+K` Search Modal, Accordion Submenu, Tab `Menu ↔ Settings`, `Light ↔ Dark Mode`, Filter Proyek, dan Switch `Web HRIS ↔ Mobile ESS`.
