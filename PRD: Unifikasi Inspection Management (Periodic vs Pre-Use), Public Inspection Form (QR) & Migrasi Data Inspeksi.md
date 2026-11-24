PRD: Unifikasi Inspection Management (Periodic vs Pre-Use), Public Inspection Form (QR) & Migrasi Data Inspeksi

PT. Teras Teknik Perdana — TERAS360 HSE Ecosystem
Tanggal: 24 November 2026 (Revisi 3 — Hapus Safety Tools & Redesign Tabel modules/inspection/index.php)
Status: Menunggu Review & Persetujuan Sebelum Eksekusi

1. Latar Belakang & Arsitektur Pemisahan

Agar pengelolaan inspeksi lebih terstruktur, modul Inspection Management dipisahkan menjadi 2 jalur utama yang jelas:

Periodic Inspection (Terpusat di kpi_inspections / modules/kpi/my_inspection?view=all)
Mencakup seluruh inspeksi berkala: Weekly HSE Inspection, 5R / Housekeeping Inspection, PPE / APD Inspection, Fire Extinguisher Inspection, First Aid Kit Inspection, serta Air Conditioner, Fire Alarm, dan Smoke Detector (jika ada data baru).
Dapat diinput melalui Internal Form (/modules/kpi/inspectionreport) maupun Public Inspection Form (QR) (/modules/kpi/input_inspection).
Pre-Use Inspection (Submenu Terpisah di bawah Inspection Management)
Mencakup 5 inspeksi pra-pakai peralatan & kendaraan:
Fall Protection Inspection (/modules/inspection/fall-protection/)
Lifting Gear Inspection (/modules/inspection/lifting-gear/)
Light / Heavy Vehicle Inspection (/modules/inspection/light-vehicle/)
Paddle Mixer Inspection (/modules/inspection/padle-mixer/)
Cutting Brick Inspection (/modules/inspection/cutting-brick/)
Data Pre-Use tetap berada di modul Pre-Use masing-masing (tidak digabung ke kpi_inspections).
2. Deliverable 1 — Struktur Sidebar Inspection Management & modules/inspection/
A. Submenu Inspection Management di includes/sidebar.php

Flyout menu Inspection Management dikelompokkan rapi dengan section divider:

PERIODIC INSPECTION
New Inspection Report → /modules/kpi/inspectionreport
All Inspections → /modules/kpi/my_inspection?view=all (live badge total data)
My Inspections → /modules/kpi/my_inspection?view=my (live badge)
Public Inspection Form (QR) → /modules/kpi/input_inspection (tab baru)
Inspection Dashboard → /modules/inspection/
PRE-USE INSPECTION
Fall Protection Inspection → /modules/inspection/fall-protection/
Lifting Gear Inspection → /modules/inspection/lifting-gear/
Light / Heavy Vehicle Inspection → /modules/inspection/light-vehicle/
Paddle Mixer Inspection → /modules/inspection/padle-mixer/
Cutting Brick Inspection → /modules/inspection/cutting-brick/

(Menu Safety Tools (/modules/safety-tools/) dihapus dari submenu Inspection Management.)

B. Redesign modules/inspection/index.php (Gaya Bersih & Berbasis Tabel)

Tampilan modules/inspection/index.php diperbarui agar selaras 1:1 dengan modules/hazard/index.php & modules/kpi/my_activity.php (bersih, minimalis, dan fokus pada tabel data):

Toolbar Atas: View switcher (All Inspections / Periodic Inspection / Pre-Use Inspection), Quick Filter pencarian cepat, tombol utama + New (mengarah ke /modules/kpi/inspectionreport atau pilihan Pre-Use), serta tombol shortcut Public QR Form.
Summary Bar / Filter Pills: Ringkasan ringkas jumlah Periodic Inspection (terhubung ke my_inspection?view=all) dan 5 Pre-Use Inspection (Fall Protection, Lifting Gear, Light / Heavy Vehicle, Paddle Mixer, Cutting Brick) dalam bentuk pill filter yang bisa diklik.
Tampilan Tabel Utama:
Tab / View Periodic Inspection: Menampilkan tabel terpusat dari kpi_inspections (kolom: INSPECTION DATE, INSPECTION NUMBER, CATEGORY, TITLE, AREA / FACILITY, FINDING RESULT, STATUS, dengan klik langsung ke inspectionreport?id=... dan tautan ke my_inspection?view=all).
Tab / View Pre-Use Inspection: Menampilkan tabel ringkas 5 modul Pre-Use (Module Name, Total Records, Last Inspection Date, Status, tombol Open Module & + New) serta daftar catatan Pre-Use terbaru (Paddle Mixer, Cutting Brick, Fall Protection, Lifting Gear, Light Vehicle).
3. Deliverable 2 — Public Inspection Form (QR) (/modules/kpi/input_inspection.php)

Desain & UX dibuat 1:1 selaras dengan /modules/hazard/input (mobile-first, tanpa login, responsif di HP & desktop):

Komponen	Spesifikasi
Header & Bahasa	Judul 📋 Report New Inspection, tombol 📖 Tutorial (modal panduan interaktif 3 bahasa + tombol Salin Link QR), switcher bahasa 🇮🇩 ID | 🇬🇧 EN | 🇨🇳 中文 (doc_translator.js), dan layout 2 baris rapi di layar HP (<= 600px).
Step 1 — Inspection Info	ENTERED BY * (pencarian HRIS privacy-first → tersimpan sebagai NAMA LENGKAP, BADGE_ID), TYPE OF INSPECTION * (Periodic Inspection / Continuous Inspection), CATEGORY OF INSPECTION * (8 kategori resmi), TITLE *, INSPECTION DATE & TIME *.
Step 2 — Location & Division	AREA / FACILITY INSPECTED * (3 proyek aktif proyek), cascading LOCATION * (koordinat_locations.location_name), SPECIFIC LOCATION *, DEPARTMENT RESPONSIBLE (DIVISION) * (8 Divisi standar Hazard Report), dan INSPECTION TEAM MEMBERS (multi-pilih HRIS).
Step 3 — Findings & Photo	FINDING RESULT * (Compliant / Positive Findings, Observation / OFI, Minor Non-Conformity, Major Non-Conformity), POSITIVE FINDINGS / SUMMARY, dan upload Photo Evidence (File Category + Description → tersimpan ke kpi_inspection_attachments).
Step 4 — Action & Notify	Opsional Negative Finding Action (jika OFI/NC → otomatis masuk ke hazard_actions & My Open Actions PIC terkait), PERSON(S) TO NOTIFY (modal HRIS + kanal Email/WhatsApp), lalu Submit → menampilkan nomor IN-XXXXXX & status terkirim Email/WA.
4. Deliverable 3 — Migrasi & Normalisasi Data Periodic Inspection ke kpi_inspections

IMPORTANT

Migrasi bersifat non-destruktif (menyalin & menormalisasi ke kpi_inspections tanpa menghapus tabel lama, serta idempotent agar aman jika dijalankan ulang). Data Pre-Use Inspection (cutting_brick_inspections, padle_mixer_inspections, fall_protection, lifting_gear, light_vehicle) tidak ikut dimigrasikan dan tetap di modul Pre-Use.

Peta Migrasi Periodic Inspection (Total 169 Baris)
Tabel Sumber	Jumlah	type_of_inspection	category_of_inspection	Mapping finding_result
weekly_hse_inspections	60	Periodic Inspection	Workplace Inspection	Sesuai jumlah temuan (weekly_hse_findings): 0 → Compliant / Positive Findings, 1–2 → Observation / Opportunity for Improvement (OFI), ≥3 → Minor Non-Conformity
5r_inspections	15	Periodic Inspection	Housekeeping Inspection / 5R / 5S	Sesuai skor/temuan 5r_findings: baik → Compliant / Positive Findings, ada temuan → Observation / OFI atau Minor Non-Conformity
inspeksi_apd (PPE)	75	Periodic Inspection	Safety Inspection	apd_status Lengkap/Sesuai → Compliant / Positive Findings, lainnya → Minor Non-Conformity
fire_extinguisher_inspections	12	Periodic Inspection	Safety Inspection	Kondisi baik → Compliant / Positive Findings, ada defect → Minor Non-Conformity
firstaidkit_inspections	7	Periodic Inspection	Hygiene and Sanitation Inspection	Kotak & isi lengkap → Compliant / Positive Findings, kurang → Observation / Opportunity for Improvement (OFI)
ac_inspections, fire_alarm_inspections, smoke_detector_inspections	0	Periodic Inspection	Safety Inspection / Workplace Inspection	(Saat ini 0 baris; ke depan diinput langsung lewat Periodic Inspection)
Standar Normalisasi Field Hasil Migrasi
inspection_number: IN-100001 s/d IN-100169 (kode asli seperti WIR-..., 5R-..., APD-..., APAR-..., P3K-... dicatat di awal title agar mudah ditelusuri).
entered_by_formatted: Dicocokkan ke terasmyi_hris.karyawan_baru menjadi format standar NAMA LENGKAP, BADGE_ID.
area_facility: Dinormalisasi ke path penuh PT. Teras Teknik Perdana\Indonesia\Sumbawa Barat\Refractory Repair and Maintenance Services (atau proyek sesuai id_proyek).
department_responsible: Divisi HSE (default) atau Divisi Konstruksi sesuai proyek.
team_members_json: Diisi dari weekly_hse_team_members / 5r_team_members dalam format [{"name":"...","role":"..."}].
status & close_record: Data historis yang sudah selesai ditandai Closed (close_record = 1), tetap aman dan dapat diedit ulang oleh Superadmin via 🔧 → Edit Inspection (Superadmin).
5. Rencana Eksekusi & Verifikasi
Sidebar & Dashboard: Perbarui includes/sidebar.php (section Periodic Inspection & Pre-Use Inspection, hapus Safety Tools) dan redesign modules/inspection/index.php berbasis tabel seperti hazard/index.php & my_activity.php.
Public Inspection Form (QR): Buat modules/kpi/input_inspection.php (wizard 4 langkah, 3 bahasa + Tutorial interaktif, upload lampiran, action sync, Email/WA notif).
Migrasi & Normalisasi 169 Data: Jalankan skrip migrasi idempotent untuk mengisi kpi_inspections dari 5 tabel Periodic Inspection.
Verifikasi: Cek sintaks php -l seluruh file dan verifikasi total baris (169), format NAMA, ID, kategori, serta link sidebar.
