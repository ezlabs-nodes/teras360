PRD v5: Pemisahan Periodic vs Continuous Inspection, Normalisasi DB Divisi & Badge ID, Buku Panduan KPI & Interactive HSE Intelligence pada Grafik Trend

Versi: 5.0
Status: Draft untuk Review & Persetujuan
Modul Terdampak:

Database kpi_inspections (normalisasi department_responsible & entered_by_formatted pada data seed 2025–2026)
modules/kpi/my_inspection.php (pemisahan Periodic vs Continuous / Daily Theme Inspection)
modules/indicator/_kpi_live_engine.php (metrik kualitas HSE, deteksi duplikat/bot read-only, rasio pencegahan)
modules/indicator/index.php (Penjelasan YoY, Link Buku Panduan KPI, dan Panel Solusi Interaktif di bawah grafik HSE Activity Trend)
1. Perbaikan Database kpi_inspections (Divisi Resmi & Format ENTERED BY)
1.1 Temuan Saat Ini

Data historis/seed 2025–2026 di kpi_inspections sebelumnya masih menyimpan:

department_responsible: 'HSE / Safety', 'HSE / Operations', 'Management / HSE' (padahal data asli di sistem memakai nama divisi resmi: Divisi HSE, Management, Divisi HRD & Keuangan).
entered_by_formatted: masih menyertakan jabatan & proyek (padahal standar form inspectionreport.php adalah NAMA KAPITAL, BADGE_ID seperti WIRYADI SULASMANA, 30103086).
1.2 Normalisasi yang Akan Dijalankan

Memperbarui seluruh record seed (IN-2025%, IN-2026M%, IN-2026DT%) di kpi_inspections agar 100% seragam dengan data asli HRIS:

Inspektor	entered_by_formatted (Standar Baru)	department_responsible (Divisi Resmi)
Wiryadi Sulasmana	WIRYADI SULASMANA, 30103086	Divisi HSE
Muhammad Fachrul Yusrinullah	MUHAMMAD FACHRUL YUSRINULLAH, 30101469	Divisi HSE
Dirga Tantari Immanuel	DIRGA TANTARI IMMANUEL, 30106570	Divisi HSE
Sutarna	SUTARNA, 1041105	Management (untuk Management Inspection) / Divisi HSE
Muhammad Hadi Farhan	MUHAMMAD HADI FARHAN, 30106420	Divisi HRD & Keuangan
2. Pemisahan Periodic Inspection vs Continuous Inspection (Daily Theme) di My Inspections

Pada modules/kpi/my_inspection.php, pengguna saat ini melihat seluruh 441 inspeksi tercampur dalam satu daftar All Inspections.

Perubahan UI & Filter:
Dropdown View Diperluas:
My Inspections (N)
All Inspections (441)
Periodic Inspections (247) (Workplace, PPE, 5R/5S, Safety, Hygiene, Management)
Continuous / Daily Theme Inspections (194) (22 Tema Harian Program Safety)
Open Inspections (N)
Closed Inspections (N)
Quick Filter Pill di Toolbar (All Types | Periodic | Continuous / Daily Theme) + Badge Tipe bersih di samping judul inspeksi agar mudah dibedakan saat review atau export Excel/CSV/PDF.
3. Analisis Strategis HSE: Menjawab Pertanyaan Efektivitas Kuota Hazard, Bot/Formalitas & Paradoks "Hazard Tinggi"

Bagian ini menjadi dasar logika Interactive HSE Intelligence Panel dan Buku Panduan KPI di /modules/indicator/:

3.1 Apa Itu YoY pada HSE Activity Trend?
YoY = Year-over-Year (Perbandingan Tahun-ke-Tahun) — membandingkan angka pada bulan yang sama antar tahun (2026 vs 2025 vs 2024).
Tujuannya melihat arah kematangan budaya K3 dari tahun ke tahun, bukan hanya angka sesaat.
3.2 Apakah Program Wajib 2 Hazard / Manpower / Bulan Efektif?
Aspek	Penilaian Objektif
Kelebihan (Fase Awal)	Efektif membangun kebiasaan (habit) pekerja agar aktif peduli dan tidak takut melapor.
Kelemahan (Fase Lanjut)	Jika KPI hanya mengejar kuantitas murni (2/orang/bulan), pekerja terdorong melakukan "Quota Gaming" — membuat laporan formalitas, duplikat, atau input by bot / copy-paste demi menggugurkan kewajiban. Kualitas temuan turun & supervisor kelelahan memverifikasi laporan semu.
3.3 Paradoks Klasik K3: "Semakin Banyak Hazard Report, Apakah Area Kerja Makin Buruk?"
TIDAK otomatis buruk! Menurut DuPont Bradley Curve & Bird's Safety Pyramid:
Hazard Report TINGGI + Closure Rate >95% + Insiden 0 (Zero Accident) = TANDA POSITIF (Proactive Culture). Artinya pekerja berani melapor dan bahaya berhasil ditangkap sebelum berubah menjadi kecelakaan.
Hazard Report TINGGI menjadi TANDA BURUK hanya jika:
Open Hazard menumpuk (tidak ditindaklanjuti),
High / Extreme Risk terus naik (bukan sekadar temuan ringan), atau
Temuan berulang di lokasi & Energy Source yang sama (pengendalian tidak menyentuh akar masalah).
Fase Kematangan Ideal (Target Berikutnya): Ketika HSE Activities (Periodic Inspection, Daily Theme, Pre-Use, Toolbox Talk, Safety Interaction) terus ditingkatkan, maka High/Extreme Risk & Recurring Hazard akan MENURUN, bukan karena orang berhenti melapor, melainkan karena kondisi lapangan memang semakin aman.
3.4 Solusi Konkret untuk Perusahaan (Rekomendasi Sistem)
Ubah Fokus dari "Kuantitas Hazard" → "Kualitas & Partisipasi K3":
Evolusi program wajib bulanan: dari "Wajib 2 Hazard Report" menjadi "Wajib 2 Safety Participation / Bulan" (bisa kombinasi Hazard Report berkualitas, Safety Interaction, atau Pre-Use Equipment Check) → menghilangkan insentif pekerja memakai bot/laporan palsu ketika area kerja memang sudah aman!
Pisahkan Metrik "Volume" vs "Risiko & Kualitas":
Presentasikan ke Manajemen/Klien 3 angka berdampingan: Total Activities Pencegahan, Closure Rate (%), dan Tren High/Extreme Risk — bukan total jumlah hazard mentah semata.
Deteksi Dini Laporan Duplikat / Bot (Read-Only Quality Guardrail):
Sistem otomatis mendeteksi (secara read-only, tanpa menghapus data) jika ada lonjakan laporan berdeskripsi identik/serupa agar tim HSE bisa melakukan coaching kualitas pelaporan.
4. Fitur Baru di /modules/indicator/ (KPI Dashboard)
Link 📖 Buku Panduan KPI & Interpretasi Grafik (gaya persis Link to Inspection Tutorial):
Diletakkan di header atas / di dalam kartu HSE Activity Trend & menu Wrench (⚙).
Membuka modal panduan lengkap yang berisi:
Arti YoY (Year-over-Year) & garis putus-putus Cut-Off,
Penjelasan rumus seluruh KPI (Safe Man-Hours, Hazard Quota, Toolbox × Shift, Smart Hybrid, On-Demand Continuous Inspection),
Panduan Menjelaskan Paradoks Hazard Report ke Manajemen / Klien (mengapa Hazard tinggi + Zero Accident + Closure >95% berarti budaya K3 sehat, dan kapan harus waspada).
Interactive HSE Intelligence & Solution Panel (Langsung di Bawah Grafik HSE Activity Trend):
Menghitung otomatis secara real-time sesuai proyek & tahun terpilih:
Prevention Ratio (HSE Activities : Hazards) — menunjukkan seberapa kuat program pencegahan (Inspections + Pre-Use + Meetings + Interactions) dibanding jumlah bahaya yang ditemukan.
High / Extreme Risk Share (%) — metrik utama apakah kondisi fisik lapangan makin aman atau makin berisiko.
Closure Effectiveness (%) — efektivitas penutupan bahaya (>95% = Sangat Baik).
Reporting Quality Check (Anti-Bot / Duplicate Indicator) — mendeteksi persentase laporan unik vs deskripsi berulang/identik, disertai Kotak Solusi Interaktif yang memberi rekomendasi konkret sesuai kondisi data proyek saat itu (bisa diklik untuk membuka detail rekomendasi & panduan).
Konfirmasi Singkat Sebelum Eksekusi

Apakah seluruh 4 bagian di PRD v5 ini (normalisasi DB Divisi & NAMA, BADGE_ID, pemisahan Periodic vs Continuous di My Inspections, Buku Panduan KPI, serta Interactive HSE Intelligence & Solution Panel di grafik HSE Activity Trend) sudah sesuai untuk langsung saya eksekusi?
