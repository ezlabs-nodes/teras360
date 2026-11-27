# PRD: Pembaruan Direktori Personalia & Kartu Profil Pekerja Detail (`pages/personalia/index.php`)

**Dokumen:** Product Requirements Document (PRD)  
**Target URL:** `http://hris.localhost/pages/personalia/index.php`  
**Sumber Referensi Desain:** Kartu Profil Pekerja Detail + 5 Modal Interaktif (Lihat, Tambah/Edit, Cetak ID CR80, Block/Unblock, Hapus)  
**Basis Data:** `terasmyi_hris.karyawan_baru` (`906` data, `843` aktif, `63` non-aktif/demob), `blacklist_karyawan` (`2` data), `terasmyi_dashboard_k3.proyek` (`9` proyek)  
**Status:** Menunggu Persetujuan User

---

## 1. Latar Belakang & Tujuan Migrasi

Saat ini pengelolaan data karyawan di HRIS masih terpecah antara:
1. **Modul lama (`modules/employees/index.php`, `add.php`, `edit.php`, `blacklist.php`, `export*.php`)** yang memakai Bootstrap 5 legacy dan memuat seluruh fitur CRUD, filter MCU/Kontrak, Blacklist, serta Export Excel/PDF.
2. **Halaman modern (`pages/personalia/index.php`)** yang masih berupa tabel sederhana read-only tanpa tombol Tambah/Edit/Hapus, tanpa pratinjau kartu profil detail, tanpa Block/Unblock ID, dan tanpa Cetak ID Card.

**Tujuan Pembaruan:**
- Memigrasikan seluruh kapabilitas manajemen karyawan dari `modules/employees/` ke dalam **`pages/personalia/index.php`** di atas arsitektur modern **HR Connect Kolabo Teal (Tailwind CSS + Lucide + PDO)**.
- Mengadopsi tampilan **Grid Kartu Profil Pekerja Detail** sesuai desain HTML referensi user (dilengkapi opsi **View Mode Toggle: Grid Kartu ↔ Tabel Lengkap** dan **Pagination / Lazy Rendering** untuk 906 karyawan).
- Menyediakan **5 Modal Interaktif lengkap** yang langsung tersambung ke database `karyawan_baru`, `blacklist_karyawan`, dan foto karyawan (`uploads/foto_karyawan/` / `TerasSSO::resolveHrisPhotoUrl()`), dengan tetap menegakkan **3-Tier Multi-Project RBAC (`hrisGetScope()`)**.

---

## 2. Arsitektur Antarmuka & Komponen UI (`pages/personalia/index.php`)

Halaman tetap terpasang rapi di dalam shell standar HR Connect (`layout/header.php` + `layout/sidebar.php` + `layout/footer.php`) agar sidebar Kolabo Teal, Global Search (`Ctrl+K`), dan switcher proyek tetap berfungsi konsisten.

### 2.1 Header Direktori, Statistik Live & Tombol Aksi
- **Judul & Identitas Scope:**
  - Icon card (`id-card`), judul **Direktori & Kartu Profil Karyawan**, subjudul area aktif (`Akses Head Office: Menampilkan Semua 9 Proyek` atau `Akses Terisolasi Proyek: <Nama Proyek>`).
- **Statistik Pill Real-Time (Sesuai Scope Proyek):**
  - `Total`: jumlah karyawan dalam scope aktif.
  - `Aktif` (titik hijau): `employee_active = 'Active' AND status_aktif = 1`.
  - `ID Diblokir / Non-Aktif` (titik merah): `employee_active IN ('Non Active','Demob') OR status_aktif = 0` (+ penanda jika NIK terdaftar di `blacklist_karyawan`).
- **KPI Alert Mini Strip (Migrasi dari `modules/employees/index.php`):**
  - 4 kartu klik-cepat filter: **MCU Expired**, **MCU ≤ 30 Hari**, **Kontrak Expired**, dan **Kontrak ≤ 30 Hari**.
- **Tombol Aksi Utama:**
  - **View Switcher:** `Grid Kartu` (default) vs `Tabel` (untuk keperluan HR scan massal).
  - **Export Data:** Dropdown `Export Excel`, `Export Excel (Full)`, dan `Export PDF` (mengarahkan ke `modules/employees/export*.php` dengan filter scope aktif).
  - **Kelola Blacklist:** Pintasan cepat ke `modules/employees/blacklist.php` atau filter tab `ID Diblokir`.
  - **+ Tambah Karyawan** (tampil untuk role `superadmin` dan `hrd`): membuka Modal 2 (Tambah Data).

### 2.2 Bar Pencarian & Filter Multi-Dimensi
Menggabungkan filter desain referensi dengan kebutuhan proyek PT. Teras Teknik Perdana:
1. **Live Search Input:** Cari berdasarkan Nama Lengkap, NIP / Badge ID (`id_pegawai`), NIK/KTP (`nik`), Jabatan (`jabatan`), Divisi, atau Email.
2. **Tab Status Segmented:** `Semua` | `Aktif` | `ID Diblokir`.
3. **Dropdown Filter:**
   - **Divisi / Departemen:** Dinamis dari `SELECT DISTINCT divisi FROM karyawan_baru`.
   - **Area / Proyek (khusus HO / Tier 1):** Pilihan 9 site proyek aktif (`terasmyi_dashboard_k3.proyek`).
   - **Status Kerja & Kepatuhan:** `Semua`, `Permanent`, `Contract`, `Probation`, `MCU ≤ 30 Hari / Expired`, `Kontrak ≤ 30 Hari / Expired`.
4. **Pagination / Load More:** Menampilkan 24 kartu per halaman (atau opsi `24 / 48 / 96`) agar rendering UI tetap responsif saat memuat 906 karyawan.

### 2.3 Grid Kartu Profil Pekerja (`grid-cols-1 md:grid-cols-2 lg:grid-cols-3`)
Setiap kartu menampilkan:
- **Pita Indikator Atas:** Gradient Teal/Indigo (`Aktif`) atau Merah Rose (`ID Diblokir / Non-Aktif`).
- **Baris Atas:** Badge NIP/Badge ID monospaced (`id_pegawai` atau `#id_karyawan`), Nama Proyek singkat, serta Status Pill (`Aktif` hijau berdenyut / `ID Diblokir` merah berdenyut).
- **Identitas Utama:**
  - Foto karyawan (`64×64` `rounded-2xl`) via resolver foto terpadu (`uploads/foto_karyawan/` / `TerasSSO::resolveHrisPhotoUrl()`), otomatis grayscale + ikon gembok merah di sudut kanan bawah jika diblokir, atau avatar inisial bila belum ada foto.
  - **Nama Lengkap**, **Jabatan (`jabatan` / `present_position`)**, dan pill **Divisi (`divisi`)**.
- **Banner Alasan Blokir (kondisional jika `ID Diblokir`):**
  - Menampilkan teks `keterangan_demob` / alasan blokir / keterangan blacklist.
- **Ringkasan Info:** Email perusahaan, Nomor HP/WA, Tanggal Masuk (`tanggal_masuk`), serta indikator ringkas masa berlaku **MCU** & **Kontrak** (merah/amber jika ≤ 30 hari).
- **Footer Aksi Kartu:**
  - **Lihat** → Membuka Modal 1 (Detail Profil).
  - **Cetak ID** → Membuka Modal 3 (Pratinjau & Cetak ID Card CR80; dinonaktifkan jika ID berstatus Diblokir).
  - **Ikon Aksi Cepat (sesuai RBAC `superadmin` / `hrd`):**
    - `Edit` (pensil) → Modal 2 (Edit Data).
    - `Block / Unblock ID` (perisai) → Modal 4 (Blokir atau Aktifkan Kembali ID).
    - `Hapus` (tempat sampah, khusus `superadmin` / `hrd` dengan proteksi CSRF) → Modal 5 (Konfirmasi Hapus).

---

## 3. Spesifikasi 5 Modal Interaktif & Integrasi Database

Semua operasi tulis (Create, Update, Upload Foto, Block/Unblock, Delete) dieksekusi melalui endpoint API terproteksi auth + CSRF + 3-Tier RBAC: **`api/personalia/employees.php`**.

### 3.1 Modal 1 — Mode Lihat Detail Profil (`view-modal`)
- **Banner Header:** Gradient Teal/Biru (untuk Aktif) atau Merah (untuk ID Diblokir).
- **Header Foto & Aksi Cepat:** Foto besar (`96×96`), Nama Lengkap, Status Badge, Jabatan, serta tombol langsung **Cetak ID** dan **Edit**.
- **Alert Blokir (jika Diblokir):** Kotak peringatan merah menampilkan alasan blokir / demobilisasi & info apakah NIK terdaftar di `blacklist_karyawan`.
- **Grid Detail Terstruktur (3 Seksi Lengkap — Mapped ke `karyawan_baru`):**
  1. **Identitas, Kontak & Penempatan Proyek:** NIP (`id_pegawai`), ID Karyawan, PIN ADMS (`fingerprint_pin`), No. ID Card (`identity_card_number`), NIK (`nik`), KTP (`ktp`), TTL & Umur, Jenis Kelamin, Agama, Pendidikan (`highest_education` / `pendidikan`), Bahasa, Proyek (`nama_proyek`), Scope, Divisi, Jabatan & Present Position, Email, Handphone, Alamat, Domisili, Kontak Darurat, Status Kepegawaian, Tanggal Masuk, & Masa Berlaku Kontrak.
  2. **Data Keluarga & Administrasi:** No. Kartu Keluarga (`no_kk`), NPWP (`npwp`), BPJS Kesehatan (`bpjs_kes`), BPJS Ketenagakerjaan (`bpjs_tk`), Status Pernikahan/Pajak (`marital_status`: `TK/0`..`K/3`), Jumlah Anak (`jumlah_anak`), Nama Pasangan (`nama_pasangan`), Ibu Kandung (`ibu_kandung`), dan Ayah Kandung (`ayah_kandung`).
  3. **Sertifikasi, Kesehatan (MCU) & Pengalaman:** Tanggal MCU (`mcu_date`) & Kedaluwarsa MCU (`mcu_expired_date`) + indikator warna, SIO Validity (`sio_validity`), Informasi Medis (`medical_information`) & Gol. Darah (`golongan_darah`), Sertifikat Pelatihan (`training_certificates`), Perusahaan Sebelumnya (`previous_work_company`), dan Lama Pengalaman Kerja (`pengalaman_kerja`).

### 3.2 Modal 2 — Tambah & Edit Data Karyawan (`form-modal` — 4 Tab Terstruktur)
- **Tab 1 — Identitas, Pribadi & Kontak:** Seluruh identitas personal, kontak darurat, dan upload foto (`uploads/foto_karyawan/`).
- **Tab 2 — Pekerjaan, Posisi & Proyek:** Status kerja (`Permanent`, `Contract`, `Probation`, `Intern`), status aktif/demob, tanggal kontrak, scope, divisi, jabatan, dan proyek (terkunci untuk Tier 2 Site HR).
- **Tab 3 — Data Keluarga & Administrasi:** `no_kk`, `npwp`, `bpjs_kes`, `bpjs_tk`, `identity_card_number`, `marital_status`, `jumlah_anak`, `nama_pasangan`, `ibu_kandung`, `ayah_kandung`.
- **Tab 4 — Sertifikasi, Kesehatan (MCU) & Pengalaman:** `mcu_date`, `mcu_expired_date`, `sio_validity`, `medical_information`, `training_certificates`, `previous_work_company`, `pengalaman_kerja`.
- **Validasi Backend:** CSRF token, role `superadmin`/`hrd`, anti-blacklist `blacklist_karyawan`, anti-duplikat NIK, dan sinkronisasi ke `sso_users`.

### 3.3 Modal 3 — Pratinjau & Cetak ID Card CR80 (`print-modal`)
- **Validasi:** Menolak cetak jika status karyawan `blocked` / non-aktif dengan toast peringatan.
- **Kartu Sisi Depan (`260px × 410px` standar CR80):**
  - Header lengkung bertema korporat **PT. TERAS TEKNIK PERDANA** (menggunakan warna Kolabo Teal `#0D9488` / Deep Navy `#0F172A` TERAS360).
  - Foto karyawan bulat (`resolveHrisPhotoUrl`), Nama Lengkap, Jabatan, Divisi, dan Nama Proyek/Area.
  - Nomor Induk Karyawan (`id_pegawai` / `EMP-...`) dan simulasi barcode Code-128.
- **Kartu Sisi Belakang:**
  - Ketentuan Penggunaan Kartu resmi **PT. Teras Teknik Perdana**.
  - **QR Code Verifikasi Karyawan** (memuat kode verifikasi `TERAS360-EMP:{id_karyawan}:{badge}` — dapat di-render SVG QR deterministik dari ID karyawan).
  - Golongan Darah (`golongan_darah`), Tahun Bergabung (`tanggal_masuk`), dan alamat kantor pusat / proyek.
- **CSS `@media print`:** Mencetak area `#print-area` (Depan & Belakang berdampingan rapi) dengan `print-color-adjust: exact` sehingga warna header kartu ikut tercetak.

### 3.4 Modal 4 — Konfirmasi Block / Unblock ID (`block-modal`)
- **Mode Blokir (`status_aktif = 0`, `employee_active = 'Non Active'`):**
  - Pilihan alasan siap pakai (*Kartu ID hilang / dilaporkan dicuri*, *Cuti panjang*, *Proses resign / berakhir kontrak*, *Demobilisasi proyek*, *Audit keamanan / pelanggaran*, atau *Alasan lainnya...*).
  - Menyimpan alasan ke `karyawan_baru.keterangan_demob` dan `tanggal_keluar = CURDATE()` (serta opsional menandai di `blacklist_karyawan` bila dipilih alasan pelanggaran/blacklist).
  - Opsional: jika karyawan terhubung ke `terasmyi_sso.sso_users`, beri opsi atau sinkronkan status akses bila diblokir permanen.
- **Mode Buka Blokir / Aktifkan Kembali (`status_aktif = 1`, `employee_active = 'Active'`):**
  - Menghapus status blokir, mengosongkan `keterangan_demob` / `tanggal_keluar`, dan mengembalikan status kartu ke `Aktif`.

### 3.5 Modal 5 — Konfirmasi Hapus Data (`delete-modal`)
- Hanya tersedia untuk role berwenang (`superadmin` / `hrd`) dengan proteksi CSRF dan scope proyek.
- Melakukan pengecekan relasi penting (atau opsi soft-delete / penghapusan bersih jika tidak memiliki log krusial, dengan pesan jelas ke admin).

---

## 4. Kebutuhan Teknis & Pemetaan Database (`karyawan_baru`)

Tidak perlu merusak struktur tabel eksisting; seluruh field telah tersedia di `terasmyi_hris.karyawan_baru`:
- **ID, Identitas & Data Pribadi:** `id_karyawan`, `id_pegawai`, `fingerprint_pin`, `nik`, `ktp`, `nama_lengkap`, `tempat_lahir`, `tanggal_lahir`, `umur`, `jenis_kelamin`, `agama`, `golongan_darah`, `kewarganegaraan`, `highest_education`, `pendidikan`, `language_spoken`, `photo_url`
- **Alamat & Kontak:** `alamat`, `domisili`, `handphone`, `email`, `emergency_name`, `emergency_number`
- **Pekerjaan, Posisi & Proyek:** `employee_status` (`Permanent`/`Contract`/`Probation`/`Intern`), `employee_active` (`Active`/`Non Active`/`Demob`), `status_aktif` (`1`/`0`), `tanggal_masuk`, `tanggal_keluar`, `keterangan_demob`, `contract_signed`, `contract_expired`, `scope`, `divisi`, `jabatan`, `present_position`, `category`, `id_proyek`, `project_location`
- **Data Keluarga & Administrasi:** `no_kk`, `npwp`, `bpjs_kes`, `bpjs_tk`, `identity_card_number`, `marital_status`, `ibu_kandung`, `ayah_kandung`, `nama_pasangan`, `jumlah_anak`
- **Sertifikasi, MCU & Pengalaman:** `sio_validity`, `mcu_date`, `mcu_expired_date`, `medical_information`, `training_certificates`, `previous_work_company`, `pengalaman_kerja`

**Catatan Kompatibilitas Query & Export:**
- Query pencarian & filter pada `pages/personalia/index.php` dan `api/personalia/employees.php` memanfaatkan `hrisProjectWhere('k')` agar Tier 2 Site HR hanya melihat dan mengelola karyawan proyeknya sendiri.
- Halaman lama `modules/employees/index.php` akan disiapkan banner/link transisi menuju **`pages/personalia/index.php`** (atau rute menu sidebar `Data Karyawan Lengkap` diarahkan langsung ke `pages/personalia/index.php`), sementara `modules/employees/export.php`, `export_excel.php`, `export_pdf.php`, dan `blacklist.php` tetap berfungsi dan terhubung sebagai aksi lanjutan.

---

## 5. Rencana Implementasi (Daftar File)

| No | File Target | Peran Perubahan |
|---|---|---|
| 1 | `C:\laragon\www\teras360\hris\api\personalia\employees.php` *(Baru)* | REST API AJAX (List/Filter + Pagination, Detail, Create, Update + Upload Foto, Block/Unblock, Delete) dengan auth, CSRF & 3-Tier RBAC. |
| 2 | `C:\laragon\www\teras360\hris\pages\personalia\index.php` *(Upgrade Penuh)* | Mengganti tabel sederhana dengan KPI strip, Search/Filter bar, View Toggle (Grid Kartu Detail + Tabel), 5 Modal Interaktif, Print ID Card CR80, dan Toast notifikasi. |
| 3 | `C:\laragon\www\teras360\hris\layout\sidebar.php` & `C:\laragon\www\teras360\hris\modules\employees\index.php` | Mengarahkan link *Data Karyawan Lengkap* di sidebar ke `pages/personalia/index.php` (serta menampilkan banner akses cepat di modul lama bila diakses via URL lama). |

---

## 6. Rencana Verifikasi

1. **Syntax & Endpoint Security Check (`php -l`):** Memastikan 0 syntax error pada semua file yang dibuat/diubah.
2. **RBAC & Data Integrity Test:**
   - Memastikan data `906` karyawan (`843` Aktif, `63` Non-Aktif/Demob/Diblokir) tampil dengan badge, foto (`uploads/foto_karyawan/`), dan filter akurat.
   - Uji validasi scope Tier 1 (HO melihat 9 proyek) vs Tier 2 (terisolasi 1 proyek).
3. **End-to-End CRUD & Modal Flow Test (via API & Browser/HTTP):**
   - Uji `http://hris.localhost/pages/personalia/index.php` mengembalikan `HTTP 200 OK`.
   - Uji API Create → View → Edit → Block ID → Unblock ID → Delete pada data uji sementara (lalu dibersihkan kembali), serta verifikasi tampilan Print ID Card CR80 Depan & Belakang.
