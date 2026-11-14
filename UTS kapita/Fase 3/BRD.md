# BUSINESS REQUIREMENTS DOCUMENT (BRD) — EDISI REVISI
## Rancang Bangun Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN untuk Meningkatkan Retensi Jangka Pendek pada Aplikasi Mobile Habit Tracker

> **Catatan Rilis Revisi (BRD-HABIT-001 Rev. 1.2):**  
> Dokumen ini dimutakhirkan untuk mempertegas batasan keilmuan Rekayasa Perangkat Lunak (FASILKOM):  
> 1. **Penyelarasan Judul Resmi:** *"Rancang Bangun Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN untuk Meningkatkan Retensi Jangka Pendek pada Aplikasi Mobile Habit Tracker"*.  
> 2. **Klarifikasi Domain Sistem (Tracking vs Modification):** Menegaskan bahwa sistem adalah aplikasi pencatat kebiasaan mandiri (*habit tracking tool*), bukan intervensi psikologis klinis perubahan kebiasaan permanen (*behavior modification*).  
> 3. **Netralitas Akademik Sasaran Bisnis (BO-02, BO-03, BO-05):** Mengubah target dari asumsi "kemenangan sepihak" menjadi pengukuran signifikansi perbedaan (*comparative effect size*) yang defensif secara metodologis.  
> 4. **Penegasan Desain Between-Subject:** Menjamin kontinuitas pencatatan habit tanpa *washout period*, dengan protokol *Single-Blind*.

---

## Informasi Dokumen
| Field | Keterangan |
| :--- | :--- |
| **Nama Proyek** | Rancang Bangun Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN untuk Meningkatkan Retensi Jangka Pendek pada Aplikasi Mobile Habit Tracker |
| **Kode Dokumen** | BRD-HABIT-001 (Rev. 1.2) |
| **Platform Target** | Aplikasi Mobile Android (Flutter) — Standalone Native Android |
| **Backend & Basis Data** | Supabase (PostgreSQL Cloud) & SQLite Lokal (Offline-First) |
| **Jumlah FR** | **32 Kebutuhan Fungsional** (FR-01 s.d. FR-32, Sekuensial Penuh) |
| **Jumlah NFR** | **23 Kebutuhan Non-Fungsional** (NFR-01 s.d. NFR-23, Terstruktur) |
| **Metodologi Evaluasi** | Parallel Between-Subject A/B Testing + Black-box Testing + SUS + HMSAM |

---

## 1. Executive Summary
### 1.1 Latar Bisnis & Akademik
Aplikasi pencatat kebiasaan (*habit tracker mobile*) berfungsi sebagai alat bantu digital bagi individu untuk memantau konsistensi aktivitas harian. Namun, data empiris dan tinjauan literatur 2021–2026 mengungkapkan tingginya angka *drop-off* pengguna dalam 14 hari pertama akibat *gamification fatigue*—kondisi di mana gamifikasi statis konvensional kehilangan daya tarik motivasional karena memberikan insentif seragam tanpa memedulikan dinamika konsistensi pengguna. Penelitian ini berfokus pada ranah Rekayasa Perangkat Lunak untuk mengatasi kelemahan tersebut melalui perancangan mesin aturan adaptif deterministik.

### 1.2 Masalah Nyata
* **Ketidaksesuaian Tantangan:** Gamifikasi statis memberlakukan reward yang sama untuk semua orang; pengguna berkinerja tinggi bosan, sedangkan pengguna yang sering gagal mengalami demotivasi tanpa *adaptive scaffolding*.
* **Ketiadaan Umpan Balik Real-Time:** Minimnya mesin aturan deterministik pada aplikasi mobile yang secara langsung menyesuaikan insentif (XP, lencana, tantangan) berdasarkan riwayat keberhasilan, streak, dan kegagalan pencatatan.
* **Kesenjangan Bukti Komparatif:** Terbatasnya riset empiris yang menguji efektivitas gamifikasi adaptif berbasis aturan (*rule-based*) secara transparan (*auditable*) terhadap retensi jangka pendek penggunaan aplikasi *habit tracker*.

### 1.3 Tujuan Sistem
Membangun dan mengevaluasi aplikasi *habit tracker* Android yang mengintegrasikan mesin adaptasi gamifikasi berbasis aturan IF–THEN deterministik. Sistem ini memproses parameter telemetri pencatatan pengguna untuk memodifikasi reward secara dinamis, serta mengukur dan membandingkan signifikansi perbedaannya terhadap keterlibatan pencatatan, retensi jangka pendek aplikasi (14 hari), usability (SUS), dan penerimaan pengguna (HMSAM) berbanding kelompok gamifikasi statis.

---

## 2. Business Objectives & Success Metrics (SMART)
| ID | Sasaran | Deskripsi Metrik | Target | Validasi |
| :--- | :--- | :--- | :--- | :--- |
| **BO-01** | Validitas Mesin Aturan | Kesesuaian eksekusi aturan IF–THEN lokal & cloud | 100% lulus skenario | Black-box testing |
| **BO-02** | Keterlibatan Pengguna | Rata-rata persentase hari aktif check-in pencatatan selama 14 hari | Mengukur signifikansi perbedaan (Adaptif vs Statis) via uji beda rata-rata | Log aktivitas (FR-18) |
| **BO-03** | Retensi Penyelesaian | Proporsi responden yang konsisten aktif mencatat hingga mengisi kuesioner akhir (*Study Completion Rate*) | Mengukur signifikansi rasio ketuntasan (Adaptif vs Statis) | Log aktivitas & Formulir (FR-28) |
| **BO-04** | Kelayakan Usability | Skor kuesioner System Usability Scale | SUS ≥ 68 (Ambang acuan industri mHealth) | Instrumen SUS (FR-28) |
| **BO-05** | Penerimaan Pengguna | Skor konstruk Behavioral Intention to Use (HMSAM) | Mengukur signifikansi perbedaan penerimaan (Adaptif vs Statis) | Kuesioner HMSAM (FR-28) |
| **BO-06** | Kesiapan Offline | Fungsi pencatatan dan reward lokal saat tanpa internet | 100% data tersinkron saat online | Pengujian offline/online |
| **BO-07** | Kepatuhan Privasi | Penyediaan informed consent dan hak hapus data pengguna | 100% patuh UU PDP 27/2022 | Verifikasi fitur (FR-29/30) |

---

## 3. Scope
### 3.1 In-Scope
* Aplikasi mobile native Android dikembangkan dengan Flutter.
* Fitur inti habit tracker: autentikasi, CRUD kebiasaan harian (maks. 2 kebiasaan aktif), pencatatan check-in harian (durasi 15–30 menit).
* Elemen gamifikasi: XP, level, badge, streak, dan tantangan adaptif.
* Mesin adaptasi rule-based IF–THEN deterministik dengan tabel aturan dinamis di database.
* Desain eksperimen baseline: pemisahan grup (Kelompok Adaptif vs Statis) via *feature flag*.
* Arsitektur offline-first: SQLite lokal dan cloud PostgreSQL (Supabase) dengan mitigasi clock drift.
* Evaluasi telemetri otomatis serta antarmuka kuesioner eksternal SUS dan HMSAM.

### 3.2 Out-of-Scope
* **Intervensi Perubahan Perilaku Klinis / Pembentukan Kebiasaan Jangka Panjang (*Long-term Behavior Modification*):** Penelitian ini berfokus murni pada ranah rekayasa perangkat lunak pencatat (*habit tracking tool*) dan evaluasi retensi penggunaan aplikasi jangka pendek (14 hari), bukan evaluasi otomatisitas psikologis kebiasaan permanen (*habit automaticity*).
* Versi iOS, Web, atau Desktop (terkontrol murni di Android OS ≥ 8.0).
* Machine Learning / Deep Learning / LLM (fokus pada aturan *expert rule-based* deterministik yang auditable).
* Integrasi sensor wearable (smartwatch/fitness band).
* Fitur media sosial lintas-pengguna (leaderboard publik, feed sosial).
* Sistem pembayaran, in-app purchase, atau monetisasi iklan.
* Pembanding "tanpa gamifikasi" (dikecualikan agar tidak mengaburkan isolasi variabel komparasi).

---

## 4. Functional Requirements (32 FR Sekuensial Penuh)
| ID Baru | Nama Kebutuhan | Deskripsi Spesifikasi | Prioritas | RQ | Ref. Lama |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **FR-01** | Registrasi Akun | Pendaftaran akun baru menggunakan email dan password (min. 8 karakter). | Must | RQ 1.1 | FR-01 |
| **FR-02** | Autentikasi Sesi | Fasilitas login, logout, dan manajemen token sesi aman. | Must | RQ 1.1 | FR-02 |
| **FR-03** | Manajemen Kebiasaan | CRUD kebiasaan harian (judul, target waktu, kategori umum), dibatasi **maks. 2 kebiasaan aktif** per akun selama eksperimen guna mengontrol variasi tingkat kesulitan habit. | Must | RQ 1.1 | FR-03 (Kuota Maks 2) |
| **FR-04** | Pencatatan Check-in | Centang status penyelesaian kebiasaan harian (selesai/belum). | Must | RQ 1.1 | FR-04 |
| **FR-05** | Logging Data Harian | Pencatatan atribut log lengkap: ID, status, waktu UTC, timestamp perangkat asli, mode eksperimen, dan status koreksi. | Must | RQ 1.1 | FR-05 |
| **FR-06** | Perhitungan Streak | Penghitungan runtutan hari sukses berturut-turut secara otomatis tanpa pembekuan washout yang merusak rutinitas. | Must | RQ 1.1 | FR-06 |
| **FR-07** | Tingkat Keberhasilan | Rasio performa: (check-in sukses 7 hari ÷ 7) × 100%, dihitung tiap 00:00 lokal. | Must | RQ 1.1 | FR-07 |
| **FR-08** | Kalkulasi XP & Level | Pemberian XP dan level dinamis via aturan IF–THEN (atau nilai statis konstan pada grup kontrol). | Must | RQ 1.2 | FR-08 |
| **FR-09** | Sistem Lencana (Badge) | Pemberian badge berdasarkan kondisi aturan perilaku (streak, rasio sukses, dsb). | Must | RQ 1.2 | FR-09 |
| **FR-10** | Tantangan Dinamis | Target tantangan adaptif mingguan yang tingkat kesulitannya disesuaikan otomatis dengan performa pengguna. | Must | RQ 1.2 | FR-10 |
| **FR-11** | Mesin Eksekusi IF–THEN | Engine evaluasi aturan yang mengeksekusi konsekuensi reward saat check-in terjadi. | Must | RQ 1.2 | FR-11 |
| **FR-12** | Manajemen Tabel Aturan | Penyimpanan aturan dalam tabel database terpisah (`rules`) dengan atribut bobot prioritas. | Must | RQ 1.2 | FR-12 |
| **FR-13** | Visualisasi Progres | Dasbor progres menampilkan XP, level, status streak aktif, badge, dan grafik check-in. | Must | RQ 1.2 | FR-13 |
| **FR-14** | Notifikasi Pengingat | Notifikasi lokal terjadwal pada smartphone untuk mengingatkan check-in kebiasaan. | Should | RQ 1.2 | FR-14 |
| **FR-15** | Pesan Motivasi Kontekstual | Umpan balik pesan motivasi yang dipilih aturan dari tabel `messages` sesuai status performa pengguna. | Should | RQ 1.2 | FR-15 |
| **FR-16** | Sinkronisasi Otomatis | Sinkronisasi dua arah latar belakang antara SQLite dan Supabase tiap 15 menit dan saat aplikasi dibuka. | Must | RQ 1.2 | FR-16 |
| **FR-17** | Operasional Mode Offline | Pencatatan check-in dan evaluasi aturan gamifikasi lokal saat perangkat tanpa koneksi internet. | Should | RQ 1.2 | FR-17 |
| **FR-18** | Telemetri & Audit Log | Pencatatan aktivitas: sesi buka, eksekusi aturan, perubahan mode, event konflik sinkronisasi. | Must | RQ 1.3 | FR-21 |
| **FR-19** | Pengaturan Profil | Pengelolaan nama profil, preferensi notifikasi, dan jam pengingat harian. | Could | RQ 1.1 | FR-22 |
| **FR-20** | Konfigurasi Grup Eksperimen | Penugasan mode (`adaptif` vs `statis`) dikontrol peneliti via Supabase Studio (persisten di akun & SQLite lokal, tidak berubah saat logout/login kembali, Single-Blind). | Must | RQ 1.3 | FR-23 (Revisi) |
| **FR-21** | Resolusi Konflik Sinkronisasi | Penanganan konflik data multi-perangkat berbasis Last-Write-Wins (LWW) dengan batas validasi 24 jam. | Must | RQ 1.2 | FR-24 |
| **FR-22** | Pembekuan Versi Aturan | Pemberian label versi aturan (`rule_version`); aturan dibekukan selama periode pengumpulan data riset. | Must | RQ 1.2 | FR-25 |
| **FR-23** | Fleksibilitas Frekuensi | Dukungan pengaturan target kebiasaan fleksibel (misal: 3× per minggu). | Could | RQ 1.1 | FR-26 |
| **FR-24** | Koreksi Check-in | Fitur pembatalan/koreksi check-in pada hari berjalan hingga 23:59 lokal dengan penanda `is_corrected`. | Should | RQ 1.1 | FR-27 |
| **FR-25** | Normalisasi Zona Waktu | Batas harian menggunakan waktu lokal pengguna (00:00–23:59), sedangkan timestamp telemetri dalam UTC. | Must | RQ 1.1 | FR-28 |
| **FR-26** | Panduan Onboarding | Layar orientasi awal yang menjelaskan cara check-in dan sistem reward saat pertama kali aplikasi dibuka. | Could | RQ 1.1 | FR-29 |
| **FR-27** | Lokalisasi Bahasa | Seluruh antarmuka, label, dan narasi pesan disajikan dalam Bahasa Indonesia baku. | Should | RQ 1.1 | FR-30 |
| **FR-28** | Gerbang Kuesioner Evaluasi | Pintu akses instrumen SUS dan HMSAM (Google Forms) di akhir uji coba dengan pencatatan status pengisian. | Should | RQ 1.3 | FR-31 |
| **FR-29** | Penghapusan Akun & Data | Fasilitas pengguna untuk menghapus akun dan seluruh riwayatnya sesuai amanat UU PDP No. 27/2022. | Must | Non-RQ | FR-32 |
| **FR-30** | Informed Consent Digital | Lembar penjelasan penelitian dan formulir persetujuan digital sebelum data pengguna dikumpulkan. | Must | Non-RQ | FR-33 |
| **FR-31** | Resolusi Konflik Aturan | Mesin aturan menyelesaikan multi-kondisi dengan memilih aturan dengan atribut `priority` (INT) tertinggi. | Must | RQ 1.2 | FR-34 (Revisi) |
| **FR-32** | Reset Password | Fitur pemulihan kata sandi melalui tautan yang dikirimkan ke email terdaftar. | Could | Non-RQ | FR-36 |

---

## 5. Non-Functional Requirements (23 NFR Terstruktur)
### 5.1 Performance (Performa)
* **NFR-01:** Waktu respons antarmuka saat check-in ≤ 2 detik (online maupun offline).
* **NFR-02:** Waktu komputasi evaluasi aturan IF–THEN ≤ 500 ms di lingkungan smartphone.
* **NFR-03:** Waktu render halaman dasbor progres ≤ 3 detik.
* **NFR-04:** Ukuran paket instalasi file APK Android ≤ 50 MB.

### 5.2 Security & Privacy (Keamanan & Privasi)
* **NFR-05:** Autentikasi dilindungi token JWT dengan kedaluwarsa aman.
* **NFR-06:** Isolasi data antar-pengguna menggunakan Row Level Security (RLS) di basis data cloud.
* **NFR-07:** Kata sandi di-hash menggunakan algoritma standar industri (bcrypt/Argon2).
* **NFR-08:** Seluruh saluran transmisi data terenkripsi melalui HTTPS/TLS 1.3.
* **NFR-09:** Kepatuhan penuh regulasi privasi UU PDP No. 27/2022 (Privacy Notice, Informed Consent, Delete Account).
* **NFR-10:** Anonimisasi data: nama dan email dihapus saat data diekspor untuk analisis akademik skripsi.

### 5.3 Usability & Accessibility (Kegunaan & Aksesibilitas)
* **NFR-11:** Operasi check-in harian dapat diselesaikan dalam maksimal 2 ketukan (*taps*).
* **NFR-12:** Rata-rata skor evaluasi System Usability Scale (SUS) ≥ 68 (*Acceptable / Grade C+*).
* **NFR-13:** Hierarki tipografi jelas dengan ukuran font teks body minimal 12pt.
* **NFR-14:** Desain antarmuka konsisten mengikuti pedoman Material Design 3.
* **NFR-15:** Rasio kontras teks terhadap latar belakang memenuhi standar WCAG AA (minimal 4.5:1).

### 5.4 Reliability & Maintainability (Keandalan & Pemeliharaan)
* **NFR-16:** Operasional pencatatan dan reward offline berfungsi 100% tanpa internet.
* **NFR-17:** Ketahanan data SQLite lokal terjamin, tidak korup saat aplikasi ditutup paksa.
* **NFR-18:** Mekanisme retry otomatis 3 kali dengan jeda 30 detik saat sinkronisasi cloud gagal.
* **NFR-19:** Penanganan kesalahan menampilkan pesan ramah tanpa *crash unhandled*.
* **NFR-20:** Desentralisasi layanan: fungsi pencatatan tidak bergantung ketersediaan realtime server.
* **NFR-21:** Aturan adaptif dapat diperbarui pada tabel database tanpa mengompilasi ulang APK.

### 5.5 Compatibility (Kompatibilitas)
* **NFR-22:** Mendukung versi sistem operasi Android minimal Android 8.0 (API Level 26) ke atas.
* **NFR-23:** Tata letak antarmuka responsif terhadap rasio layar smartphone dari 16:9 hingga 20:9.

---

## 6. Desain Metodologi Eksperimen Komparatif
Desain pengujian menggunakan **Parallel Two-Group Between-Subject Design**:
1. **Kelompok A (Grup Adaptif):** Menjalankan aplikasi dengan mesin IF–THEN adaptif aktif selama 14 hari penuh.
2. **Kelompok B (Grup Statis Baseline):** Menjalankan aplikasi dengan reward gamifikasi statis konstan selama 14 hari penuh.

### 6.1 Protokol Sampling & Kontrol Variabel
* **Ukuran Sampel:** Target total **N = 30 responden** (15 responden Kelompok Adaptif, 15 responden Kelompok Statis) dengan teknik *Purposive Sampling* (Android OS ≥ 8.0, komitmen 14 hari).
* **Alokasi Kelompok Terkontrol (Researcher-Controlled Single-Blind):** Penugasan mode `adaptif` (15 responden) vs `statis` (15 responden) diatur langsung oleh peneliti via Supabase Studio. Status mode bersifat persisten di cloud & SQLite lokal (tetap terkunci saat logout/login kembali). Antarmuka kedua kelompok identik tanpa label pembeda.
* **Kontrol Variabel Kebiasaan:** Sistem membatasi **maksimal 2 kebiasaan aktif** per akun dengan durasi terukur setara (15–30 menit/hari) pada kategori umum (Belajar, Olahraga/Kebugaran, atau Rutinitas Harian) agar tingkat kesulitan antar-kelompok setara dan bebas bias.
* **Retensi Penyelesaian Eksperimen (Study Completion Rate):** `Retensi Akhir = (Jumlah responden yang tuntas hingga kuesioner akhir ÷ 15 responden awal per kelompok) × 100%`.
* **Metode Uji Statistik:** Komparasi nilai rata-rata keterlibatan hari aktif, skor SUS, dan skor HMSAM (BIU) kedua kelompok menggunakan uji beda rata-rata (*Independent Samples t-Test* / *Mann-Whitney U Test*), serta verifikasi rata-rata skor SUS terhadap standar industri ≥ 68.

### 6.2 Katalog Aturan IF–THEN Adaptif Konkret (Base XP = 10 Poin)
Setiap check-in habit yang selesai memiliki nilai dasar **Base XP = 10 Poin**. Mesin aturan mengevaluasi kondisi perilaku berikut:

| Rule ID | Kategori Aturan | Kondisi Pemicu (IF) | Konsekuensi Reward (THEN) | Priority |
| :--- | :--- | :--- | :--- | :--- |
| **R-01** | **Puncak 14 Hari (Grand Finale)** | `current_streak >= 14 AND success_rate_7d >= 0.85` | Multiplier = **2.0× (20 Poin)**, unlock Badge **'Juara 14 Hari'** + Tautan Kuesioner Akhir (FR-28) | **100** |
| **R-02** | Konsistensi Mingguan | `current_streak >= 7 AND success_rate_7d >= 0.85` | Multiplier = **1.5× (15 Poin)**, unlock Badge 'Streak Master' | **90** |
| **R-03** | Scaffolding / Pemulihan | `success_rate_7d < 0.50` (indikasi demotivasi) | Multiplier = **1.25× (13 Poin)** (Bonus Dorongan) + Pesan motivasi penyemangat | **80** |
| **R-04** | Konsistensi Awal | `current_streak >= 3 AND success_rate_7d >= 0.60` | Multiplier = **1.15× (12 Poin)**, unlock Badge 'Konsisten 3 Hari' | **60** |
| **R-05** | Baseline Statis | `mode == 'statis'` (Grup Kontrol) atau hari normal | Multiplier = **1.0× (10 Poin)** flat konstan tanpa penyesuaian | **10** |

---

## 7. Kamus Data Singkat (Database Schema)
* **`users` (C.1):** `user_id` (PK, UUID), `email`, `nama_tampilan`, `mode_eksperimen` (ENUM: 'adaptif' / 'statis'), `created_at`.
* **`habits` (C.2):** `habit_id` (PK, UUID), `user_id` (FK), `judul`, `kategori` (Belajar, Olahraga, Rutinitas), `target_waktu`, `is_active`.
* **`habit_logs` (C.3):** `log_id` (PK, UUID), `habit_id` (FK), `user_id` (FK), `tanggal_lokal`, `status`, `waktu_checkin_utc`, `mode_saat_checkin`, `is_corrected`.
* **`rules` (C.4):** `rule_id` (PK, Serial), `nama_aturan`, `kondisi_streak_min`, `kondisi_rasio_min`, `kondisi_rasio_max`, `aksi_xp_multiplier`, `aksi_badge_id`, **`priority` (INT, 1-100)**, `rule_version`.
* **`activity_logs` (C.5):** `activity_id` (PK, BigSerial), `user_id` (FK), `tipe_event`, `payload_json`, `timestamp_utc`.
* **`user_rewards` (C.6):** `reward_id` (PK, UUID), `user_id` (FK), `rule_id`, `xp_awarded`, `badge_id`, `created_at`.
* **`local_rewards` (C.7):** `local_id` (PK, Autoincrement), `user_id`, `rule_id`, `xp_awarded`, `is_synced` (BOOLEAN), `timestamp_utc`.

---
*Dokumen ini merupakan artefak resmi yang telah diselaraskan dengan Kerangka PICOS, SLR-HABIT-001, dan Panduan Master TA FASILKOM Universitas Esa Unggul.*
