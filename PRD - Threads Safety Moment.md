# Product Requirements Document (PRD)
## Safety Moment — Media Sosial & Diskusi Budaya K3 (Konsep Instagram Threads)

| Metadata | Detail |
| :--- | :--- |
| **Versi Dokumen** | v1.1 (Universal Employee Feed + Rekomendasi Nama Pengganti `Department and Teams`) |
| **Modul Target** | `modules/safety-moment/` (`setup.php`, `api.php`, `index.php`) & `includes/topbar.php` |
| **URL Modul** | `http://k3.localhost/modules/safety-moment/` |
| **Konsep UI/UX** | **Instagram Threads-Style HSE Feed** — Feed bersih, cepat, *mobile-first*, foto lapangan, *thread* berantai (*vertical connector line*), dan interaksi sosial real-time lintas proyek. |
| **Akses Pengguna** | **Terbuka untuk SEMUA User** (Karyawan, Operator, Teknisi, Safetyman, Supervisor, HSE, Manager) agar setiap orang dapat membagikan *Safety Moment* langsung dari tempat kerjanya masing-masing. |

---

## 1. Rekomendasi Nama Baru Pengganti `Department and Teams` (Topbar)

Menu **`Department and Teams`** pada topbar TERAS360 (`includes/topbar.php`) akan diganti menjadi pintu masuk utama ke modul *Safety Moment* bergaya Instagram Threads ini. Berikut **5 rekomendasi nama** yang modern, singkat, dan kuat secara branding:

| No | Rekomendasi Nama | Nuansa & Keunggulan | Kecocokan |
| :---: | :--- | :--- | :--- |
| **1** | **`SafeThreads`** *(Rekomendasi Utama)* | Langsung membawa identitas **Threads** yang akrab di pekerja, dipadukan dengan kata *Safe* — singkat, modern, dan 100% mencerminkan K3. | Sangat cocok untuk budaya keselamatan modern & mudah diingat di HP maupun desktop. |
| **2** | **`TerasThreads`** | Menggabungkan identitas perusahaan (**PT. Teras Teknik Perdana / TERAS360**) + **Threads** (mirip *Meta Threads* → *TerasThreads*). | Sangat kuat untuk *internal corporate branding* lintas proyek. |
| **3** | **`HSE Threads`** | Paling jelas, lugas, dan profesional untuk konteks *HSE Management System*. | Cocok jika ingin penamaan formal dan langsung dipahami auditor/klien. |
| **4** | **`SafetyPulse`** | Menggambarkan denyut aktivitas & kesadaran keselamatan real-time dari seluruh site proyek. | Cocok jika ingin nuansa *live engagement* tanpa meniru kata "Threads". |
| **5** | **`SafeFeed`** | Singkat, fokus pada *live social feed* foto & momen keselamatan harian lapangan. | Ringkas untuk tampilan topbar dan mobile. |

> [!TIP]
> **Rekomendasi Terbaik:** **`SafeThreads`** atau **`TerasThreads`** — ditempatkan di posisi `Department and Teams` pada topbar (`SafeThreads | Ecosystem | Company Information`) dengan indikator titik/angka *live post* hari ini.

---

## 2. Visi Produk: "Instagram Threads untuk Budaya K3 Lapangan"

Setiap karyawan di lokasi proyek manapun (Smelter Sumbawa Barat, Manyar Gresik, Head Office Jakarta, Workshop, dll.) dapat dalam hitungan detik:
1. Membagikan **Safety Moment harian** di tempat kerjanya (foto kondisi aman, *Stop Work Authority*, *Toolbox Talk*, pengingat APD, *Lesson Learned*, atau apresiasi rekan kerja).
2. Mengikuti dan menanggapi *Safety Moment* dari rekan kerja di proyek lain lewat **balasan berantai (*nested replies*)**, **Like**, **Repost / Quote**, dan **Save (Bookmark)**.
3. Menjadikan *Safety Moment* terbaik sebagai materi siap tampil untuk **Morning Safety Talk / Toolbox Meeting (TBM Present Mode)**.

---

## 3. Tata Letak & Fitur Inti (Threads-Style Experience)

### 3.1 Layout Responsif
- **Desktop (`>= 1080px`):**
  - **Center Feed (`max-width: 640px`):** Kolom feed tunggal terpusat khas Instagram Threads — latar putih bersih, tipografi tajam, garis vertikal penghubung (*thread connector line*) untuk *thread* berantai & balasan.
  - **Right Rail Ringkas (`280px`):** Pencarian cepat, Filter Proyek/Site, **Trending Safety Topics** (`#LSR`, `#StopWorkAuthority`, `#HeatStress`, `#ZeroAccident`, `#APD`), dan **Top Safety Contributors** bulan ini.
- **Mobile & Tablet (`< 1080px`):**
  - Otomatis *full-width* 1 kolom (*mobile-first*) dengan filter horizontal *swipeable* dan tombol *floating* `+ Post` untuk pekerja di lapangan.

### 3.2 Tab Navigasi Feed
1. **`For You` (Semua Site):** Seluruh *Safety Moment* dari seluruh proyek PT. Teras Teknik Perdana (*Pinned Official* selalu di atas).
2. **`My Site / Project`:** Khusus *Safety Moment* dari tempat kerja/proyek user saat ini.
3. **`Official / Pinned`:** *Safety Moment of the Week* & buletin keselamatan resmi dari HSE Manager / Manajemen.
4. **`Saved`:** Daftar *Safety Moment* yang di-bookmark pribadi oleh user.

### 3.3 Quick Composer ("Bagikan Safety Moment dari tempat kerjamu...")
Tersedia langsung di bagian atas feed untuk **semua user**:
- **Teks & `#Hashtag` Otomatis:** Maks. 1.000 karakter per kartu; `#hashtag` otomatis menjadi tautan filter topik.
- **Multi-Foto / Kamera HP (hingga 4 foto):** Ditampilkan sebagai **horizontal swipe carousel** khas Instagram Threads lengkap dengan **Lightbox Zoom**.
- **Kategori Momen:** `Safety Moment`, `Lesson Learned`, `Stop Work Authority (SWA)`, `Toolbox Talk`, `Best Practice`, `Zero Accident Milestone`, `Health & Wellness`.
- **Lokasi Kerja & Proyek:** Otomatis terisi proyek user + input singkat area kerja (mis. *Area Furnace Bay 2*, *Workshop TTP*, *Office Lt. 3*).
- **Multi-Post Thread (`+ Add to thread`):** Memungkinkan user membuat cerita berantai (mis. *1/3 Kondisi Awal → 2/3 Tindakan Aman → 3/3 Pesan Keselamatan*) yang tampil tersambung dengan garis vertikal khas Threads.

### 3.4 Interaksi Sosial Khas Instagram Threads
Setiap post menampilkan Avatar foto profil HRIS, Nama Lengkap, **Verified HSE Badge** (untuk Tim HSE & Manajemen), Nama Proyek & Lokasi Kerja, waktu relatif (`2m`, `1h`, `3d`), serta tombol aksi instan (AJAX tanpa reload):
- **Like (`♡` / `♥`):** Toggle suka dengan counter real-time.
- **Reply (`💬`):** Membuka thread percakapan penuh dengan garis vertikal penghubung, mendukung balasan teks + 1 foto.
- **Repost / Quote (`↻`):** Bagikan ulang langsung atau *Quote* dengan menambahkan pandangan/pembelajaran untuk area kerja sendiri (tampil sebagai *embedded quote card*).
- **Save / Bookmark (`🔖`):** Simpan ke tab `Saved` untuk dibawakan di *Toolbox Meeting*.
- **TBM Present Mode (`📺`):** Tampilan layar penuh bersih 1-klik untuk dibacakan saat *Morning Safety Talk*.
- **Pin Official (`📌`):** Khusus HSE Supervisor / HSE Manager / Corporate / Superadmin untuk menyematkan *Safety Moment of the Week*.

### 3.5 Notifikasi Otomatis
- Setiap `Reply` atau `Repost/Quote` otomatis mengirim notifikasi ke tabel `notifications` TERAS360 milik penulis post agar percakapan tetap aktif.

---

## 4. Skema Database (`modules/safety-moment/setup.php`)

Dibuat otomatis secara idempoten (`CREATE TABLE IF NOT EXISTS`):

1. **`safety_moment_posts`**: `id`, `parent_id`, `root_id`, `repost_of_id`, `user_id`, `author_name`, `author_role`, `author_photo`, `id_proyek`, `project_name`, `location_text`, `category`, `content`, `hashtags`, `is_pinned`, `likes_count`, `replies_count`, `reposts_count`, `created_at`
2. **`safety_moment_media`**: `id`, `post_id`, `file_path`, `original_name`, `mime_type`, `sort_order`
3. **`safety_moment_likes`**: `post_id`, `user_id`, `created_at` (`PK: post_id, user_id`)
4. **`safety_moment_bookmarks`**: `post_id`, `user_id`, `created_at` (`PK: post_id, user_id`)

**Auto-Seed Awal:** 4 *Safety Moment* aktual lintas proyek + contoh balasan diskusi agar feed langsung aktif saat pertama kali dibuka.

---

## 5. Rencana Implementasi

| Fase | Ruang Lingkup | File Target |
| :--- | :--- | :--- |
| **Fase 1** | Skema 4 tabel + auto-seed + REST API lengkap (`feed`, `detail`, `create_post` multi-chain & multi-foto, `reply`, `like`, `repost`, `bookmark`, `pin`, `delete`) | `modules/safety-moment/setup.php`, `modules/safety-moment/api.php` |
| **Fase 2** | Antarmuka **Instagram Threads-Style Safety Moment** (Center Feed 640px, Quick Composer + Add-to-Thread, Photo Carousel + Lightbox, Nested Replies + Connector Line, Quote Card, Right Insights Rail, TBM Present Mode) | `modules/safety-moment/index.php` |
| **Fase 3** | Mengganti **`Department and Teams`** pada topbar menjadi nama pilihan Anda (mis. **`SafeThreads`**) menuju `/modules/safety-moment/` + verifikasi `php -l` & HTTP 200 | `includes/topbar.php` |

---

> [!IMPORTANT]
> **Konfirmasi Sebelum Eksekusi:**
> Dari 5 rekomendasi nama di atas (**`SafeThreads`**, **`TerasThreads`**, **`HSE Threads`**, **`SafetyPulse`**, **`SafeFeed`**), nama mana yang ingin Anda gunakan untuk mengganti **`Department and Teams`** di topbar? Silakan sebutkan pilihan Anda atau klik **Proceed** (default: **`SafeThreads`**) agar saya langsung mulai implementasi.
