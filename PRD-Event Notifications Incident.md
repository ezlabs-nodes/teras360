# Product Requirements Document (PRD v2.1)
## Penyempurnaan UI/UX Event Notification (Standar Enterprise), Aturan Kategori & Vital Behaviour, serta Pelengkapan Data Investigasi Database

| Metadata | Detail |
| :--- | :--- |
| **Versi Dokumen** | v2.1 (UI/UX Polish, Enterprise Form Alignment & DB Investigation Backfill) |
| **Modul Target** | `modules/event-notification/` (`index.php`, `_incident_form.php`, `input.php`, `edit.php`, `export_pdf.php`, `setup.php`, `api/`) |
| **Referensi Visual** | 11 Screenshot Referensi Enterprise (`Notification : <New Record>`, `Category Accident`, `Risk Review / Current Risk Level`, `Persons to Notify`) |
| **Data Aktual DB** | 3 record existing (`IN-168695`, `IN-509648`, `IN-434632`) |

---

## 1. Evaluasi Masalah Saat Ini (Berdasarkan Audit Tampilan & Database)

1. **Form Input/Edit Belum Jelas & Belum Responsif:**
   - Input field terlihat abu-abu datar tanpa batas tegas karena konflik CSS global `layout.css` (`.form-control`).
   - Checkbox di bagian bawah (*Vital Behaviour*, *HIPO*, *DEMR*) tertarik memanjang secara vertikal.
   - Bahasa label masih campur aduk (*Title / Judul Kejadian*, *Jenis / Klasifikasi K3*, *LOCATION*).
2. **Tabel Event Notification List:**
   - Tombol aksi (`Buka` & ikon PDF) bentuknya terlalu tinggi/tidak proporsional.
   - **Isu Privasi:** Kolom `REPORTED BY` menampilkan 16 digit NIK KTP (`3604302904960004`) — harus diganti dengan Departemen/Area & Badge/Local ID (tidak pernah menampilkan NIK KTP).
   - Record `IN-168695` (*8 Vital Behaviours Point Nomor 1*) tampil sebagai kategori `Lainnya` karena aturan klasifikasi Vital Behaviour belum dipisahkan dari `Category Accident`.
3. **Kesenjangan Data Investigasi di Database:**
   - Seluruh 3 record lama di tabel `insiden` (`IN-168695`, `IN-509648`, `IN-434632`) masih menyimpan hasil ICAM/Fishbone sebagai teks mentah di kolom `penyebab_awal` dengan karakter rusak (*mojibake*: `â€”`, `Ã°Å¸â€œÅ’`, `â€¢`, `ðŸ‘¤`), sehingga tab **Efek Domino**, **Investigasi Terstruktur**, **Tim Investigasi**, **Risk Review**, dan **CAPA** tampak kosong saat dibuka.

---

## 2. Desain Baru Form Input & Edit (`Notification : <New Record>`)

Mengikuti struktur bersih & responsif sesuai **Gambar Referensi 3–11**:

### 2.1 Header & Navigasi Tab
- **Header Bar:** Judul **`Notification : <New Record>`** (atau **`Notification : IN-XXXXXX`** pada mode edit) dengan tombol kanan atas yang rapi & sejajar:
  - **`Save`** (Primary `#0f172a`)
  - **`Save to PDF`** (mode edit)
  - **`Salin WA`** (mode edit)
  - **`Close`** (kembali ke daftar)
- **5 Tab Garis Bawah (Clean Enterprise Tabs):**
  1. **`Notification`** — Informasi Kejadian, Risk Review (*Current Risk Level*), & *Persons to Notify*
  2. **`Investigation & Domino`** — Tahap 1–5 (Pengamanan Korban & Area, Tim Investigasi, Efek Domino 5 Blok, 6 Metode Terstruktur, Penanganan Pihak Eksternal)
  3. **`Actions (CAPA)`** — Daftar Tindakan Perbaikan & Pencegahan (`Containment` / `Corrective` / `Preventive`) + PIC + Due Date + Status
  4. **`Communication & Approval`** — Thread Komunikasi Live (`Internal`, `Escalation`, `External`) + Approval berjenjang (`HSE Manager` & `Site Manager`)
  5. **`File Attachments`** — Upload & galeri lampiran foto TKP / dokumen bukti

### 2.2 Standar Field Tab `Notification` (2 Kolom Responsif + Outlined Inputs)
Setiap field menggunakan desain *crisp outlined input* (border `#cbd5e1`, fokus `#0f172a`, indikator wajib `*` merah lembut jika belum diisi, 100% responsif menjadi 1 kolom pada mobile):

| Baris | Kolom Kiri | Kolom Kanan |
| :---: | :--- | :--- |
| **1** | `EVENT NUMBER (SYSTEM GENERATED)` — Readonly (`IN-XXXXXX`) | `TOTAL APPROXIMATE COST (SYSTEM GENERATED)` — Readonly (Rp auto-sum) |
| **2** | `REPORTED BY *` — Searchable HRIS Picker (`Nama, Area, Badge` — tanpa NIK KTP) | `CLIENT / SITE EVENT NUMBER` — Opsional (nomor referensi klien) |
| **3** | `REPORTED DATE` — Otomatis tanggal lapor | `REPORTED TIME` — Otomatis jam lapor |
| **4** | `TYPE (INCIDENT = NEAR MISS) *` — Radio: `( ) Accident` `( ) Incident` | `CATEGORY ACCIDENT *` — **14 Kategori Kanonik** (lihat §2.3, **tanpa VB**) |
| **5** | `IS IT VITAL BEHAVIOR VIOLATION?` — Select (`No` / `Yes`) *(lihat aturan VB §2.3)* | `IS IT DEMR / ESDM ACCIDENT CATEGORY?` — Select (`No` / `Yes`) |
| **6** | `EVENT DATE *` — Date picker | `EVENT TIME (HH:MM 24H) *` — Time picker |
| **7** | `AREA / FACILITY *` — Full-path Proyek (`PT. Teras Teknik Perdana\Indonesia\...`) | *(Full width 2 kolom)* |
| **8** | `LOCATION *` — Lokasi spesifik di site | `ENERGY SOURCE` — Sumber energi utama |
| **9** | `DEPARTMENT RESPONSIBLE *` — Departemen penanggung jawab | *(Full width 2 kolom)* |
| **10** | `TITLE OF EVENT *` — Judul singkat kejadian | *(Full width 2 kolom)* |
| **11** | `DESCRIPTION *` — Kronologi kejadian (UTF-8 bersih, bebas mojibake) | *(Full width 2 kolom)* |
| **12** | `IMMEDIATE ACTIONS TAKEN` — Tindakan pengamanan langsung (sinkron ke Tahap 1 & 2) | *(Full width 2 kolom)* |
| **13** | `CONSEQUENCES / IMPACTS *` — Dampak utama kejadian | `CATEGORY *` — Kategori dampak (*Injury/Illness, Property Damage, Process Loss, Spills, Near Miss, Security, dll.*) |

### 2.3 Standar 14 Kategori `Category Accident` & Aturan Khusus Vital Behaviour (VB)
Sesuai **Gambar 4 & 11**, dropdown **`CATEGORY ACCIDENT *`** berisi tepat **14 klasifikasi standar** (tanpa `VB`):
1. `Fatality`
2. `Lost Time Injury (LTI)`
3. `Serious Injuries (SI)`
4. `Restricted Work Injury (RWI)`
5. `Medical Treatment Injury (MTI)`
6. `First Aid Injury (FAI)`
7. `Property Damage (PD)`
8. `Process Loss (PL)`
9. `Non work related case (NWR)`
10. `Food Incident (FI)`
11. `Incident (INC)`
12. `Environmental (ENV)`
13. `Illness (ILLN)`
14. `Security (SEC)`

> [!IMPORTANT]
> **Aturan Vital Behaviour (VB):**
> `VB` **tidak dimasukkan** ke dalam daftar `Category Accident`. Sebagai gantinya, jika user memilih **`Is It Vital Behavior Violation? = Yes`**, sistem otomatis mengklasifikasikan & menampilkan badge **`Vital Behaviours / LSR / Golden Rules`** pada Event Notification List, filter view `Vital Behaviour / LSR`, serta masuk ke lapisan bayangan **VB** pada **Lagging Indicator Pyramid** (`modules/indicator/lagging.php`).

### 2.4 Section `Risk Review` (`Current Risk Level` — Gambar 5–9)
Dipisahkan dengan garis header amber tipis (`Risk Review`):
- **Sub-tabel `CURRENT RISK LEVEL`** (`IMPACTED AREA` | `ACTUAL CONSEQUENCE` | `POTENTIAL CONSEQUENCE`) + tombol **`+ Add New`** & **`Delete`**:
  - **Impacted Area (7 opsi persis Gambar 6):** `HSLP`, `Operational`, `Environmental`, `Social Responsibility`, `Security`, `Legal Compliance`, `Financial`.
  - **Actual Consequence (5 opsi persis Gambar 7):** `1 - Insignificant`, `2 - Minor`, `3 - Moderate`, `4 - Major`, `5 - Catastrophic`.
  - **Potential Consequence (5 opsi persis Gambar 8):** `1 - Insignificant`, `2 - Minor`, `3 - Moderate`, `4 - Major`, `5 - Catastrophic`.
- **`EVENT CONSEQUENCE LEVEL (SYSTEM GENERATED)`:** Otomatis menghitung nilai tertinggi (`1`–`5`) dari seluruh baris `Current Risk Level` (dan menentukan SLA eskalasi).
- Checkbox **`IS THIS A FATALITY?`** + tombol **`Link to Incident Level Matrix & SOP`** (membuka modal matriks L1–L5).

### 2.5 Section `PERSONS TO NOTIFY` (Gambar 9–10)
- Tabel daftar penerima notifikasi (`LAST NAME` | `FIRST NAME` | `AREA` | `LOCAL ID`) + tombol **`Selection List`** & **`Remove`**.
- Modal **`Persons to Notify`** memuat data karyawan dari `terasmyi_hris.karyawan_baru` dengan:
  - Pencarian cepat per kolom (*Name*, *Area*, *Local ID/Badge*)
  - Checkbox multi-pilih + tombol `Clear All`
  - Radio filter `Full List` / `Selected Items Only`
  - Saat Event Notification disimpan, seluruh *Persons to Notify* otomatis menerima notifikasi di TERAS360 Notification Center & tercatat di tab Komunikasi.

---

## 3. Perbaikan Tabel Event Notification List (`index.php`)

- **Tombol Aksi Rapi:** Tombol `Buka` dan `PDF` dibuat sejajar horizontal dengan tinggi `28px`, padding ringkas, tidak melar/menumpuk.
- **Privasi Data Pelapor:** Kolom `REPORTED BY` hanya menampilkan **Nama Lengkap** + **Area/Proyek** (tanpa 16-digit NIK KTP).
- **Kategori & Badge VB Jelas:**
  - Jika `Is Vital Behavior Violation = Yes`, kolom `CATEGORY` otomatis menampilkan **`Vital Behaviours / LSR / Golden Rules`** (dengan pill khusus), tidak lagi `Lainnya`.
  - Kolom `CONSEQUENCE` menampilkan ringkasan skor (`Level 1–5` / `A2 • P4`).
- **Konsistensi Bahasa:** Seluruh header tabel, filter, dan modal menggunakan bahasa profesional yang seragam dan mudah dipahami.

---

## 4. Pembersihan & Pelengkapan Data Investigasi 3 Record Database

Migrasi satu-kali yang aman (`setup.php` — hanya mengisi field yang masih kosong/rusak tanpa menghapus histori) akan merapikan seluruh 3 kejadian aktual di database:

| Event No | Judul Kejadian | Penyesuaian Klasifikasi | Data Investigasi yang Dilengkapi |
| :--- | :--- | :--- | :--- |
| **`IN-168695`** | *8 Vital Behaviours Point Nomor 1* | Type: `Incident`, Category Accident: `Incident (INC)`, **Vital Behavior: `Yes`** (`Vital Behaviours / LSR / Golden Rules`), Risk Review: `HSLP` (Actual `1 - Insignificant`, Potential `5 - Catastrophic`, HIPO) | Sanitasi mojibake deskripsi & ICAM (`Individual Actions`, `Task/Conditions`, `Defences`, `Org Factors`), isi 5 blok **Efek Domino**, 3 **CAPA Actions** dari `tindakan_perbaikan`, status Korban (`Tidak Ada Korban`) & Area (`Ya`), serta susunan Tim Investigasi. |
| **`IN-509648`** | *Finger hit by Lip Refractory Brick* | Type: `Accident`, Category Accident: `First Aid Injury (FAI)`, Category: `Injury/Illness`, Risk Review: `HSLP` (Actual `2 - Minor`, Potential `3 - Moderate`) | Sanitasi mojibake ICAM, isi 5 blok **Efek Domino**, 3 **CAPA Actions** (tidak bekerja sendirian, penempatan material, JSA sebelum kerja), status Korban (`Ya`) & Area (`Ya`), serta Tim Investigasi. |
| **`IN-434632`** | *Fall from Launder and Hit by Jack Hammer 60kg* | Type: `Accident`, Category Accident: `First Aid Injury (FAI)`, Category: `Injury/Illness`, Risk Review: `HSLP` (Actual `2 - Minor`, Potential `4 - Major`) | Sanitasi mojibake **Fishbone 6M** (Man, Machine 60kg, Method, Environment), isi 5 blok **Efek Domino**, 3 **CAPA Actions** (posisi kerja launder, kesesuaian breaker, identifikasi bahaya), status Korban (`Ya`) & Area (`Ya`), serta Tim Investigasi. |

---

## 5. Rencana Eksekusi

1. **Database & Backfill (`setup.php` & `api/save_incident.php` & `api/employees_lookup.php`):**
   - Tambah kolom `event_type`, `client_event_number`, `reported_date`, `reported_time`, `immediate_actions`, `consequences_impacts`, `current_risk_json`, `persons_to_notify_json`, `is_fatality`.
   - Endpoint pencarian karyawan HRIS (`api/employees_lookup.php`) untuk *Reported By* & *Persons to Notify* (tanpa NIK KTP).
   - Sanitasi UTF-8 mojibake & backfill terstruktur 3 record existing (`IN-168695`, `IN-509648`, `IN-434632`).
2. **Redesign Form Input & Edit (`_incident_form.php`):**
   - Tampilan persis referensi enterprise (Screenshots 3–11) dengan CSS terisolasi (tanpa konflik border/checkbox), 14 `Category Accident`, aturan VB otomatis, sub-tabel `Current Risk Level`, modal `Persons to Notify`, serta tab `Investigation & Domino`, `Actions (CAPA)`, `Communication & Approval`, dan `File Attachments`.
3. **Polish Tabel List (`index.php`) & PDF (`export_pdf.php`):**
   - Perbaikan tampilan tombol aksi, privasi `Reported By`, tampilan kategori `Vital Behaviours / LSR / Golden Rules`, dan sinkronisasi kolom baru ke PDF.

---

> [!IMPORTANT]
> Silakan tinjau PRD v2.1 ini. Klik **Proceed** atau beri masukan jika ada penyesuaian sebelum saya mengeksekusi perubahan.
