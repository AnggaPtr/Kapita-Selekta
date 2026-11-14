# FASE 1 — FORMULASI PICOS & RESEARCH QUESTIONS (EDISI REVISI)
## Rancang Bangun Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN untuk Meningkatkan Retensi Jangka Pendek pada Aplikasi Mobile Habit Tracker

> **Catatan Rilis Revisi (PICOS-HABIT-001 Rev. 1.2):**  
> Dokumen ini dimutakhirkan untuk mempertegas batasan keilmuan Rekayasa Perangkat Lunak (FASILKOM):  
> 1. **Penyelarasan Judul Resmi:** *"Rancang Bangun Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN untuk Meningkatkan Retensi Jangka Pendek pada Aplikasi Mobile Habit Tracker"*.  
> 2. **Klarifikasi Domain Sistem:** Aplikasi adalah instrumen pencatat (*habit tracking tool*), bukan terapi klinis perubahan perilaku psikologis (*behavior modification*).  
> 3. **Netralitas Target Evaluasi (Outcome):** Mengukur dan menganalisis signifikansi perbedaan keterlibatan dan retensi aplikasi (*effect size*) antara Kelompok Adaptif dan Kelompok Statis tanpa praduga sepihak.

---

## Informasi Dokumen
| Field | Keterangan |
| :--- | :--- |
| **Topik Penelitian** | Rancang Bangun Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN untuk Meningkatkan Retensi Jangka Pendek pada Aplikasi Mobile Habit Tracker |
| **Kode Dokumen** | PICOS-HABIT-001 (Rev. 1.2) |
| **Platform Sasaran** | Aplikasi Mobile Native Android (Flutter, SQLite Lokal, Supabase Cloud) |
| **Tujuan Fase 1** | Memformulasikan kerangka PICOS yang operasional sebagai landasan Bab I s.d. Bab III Skripsi |
| **Status Sinkronisasi** | **Tersinkronisasi 100%** dengan BRD Rev. 1.2 dan SLR Rev. 1.2 |

---

## 1. Kerangka PICOS
* **P — Population / Platform / Target User:**  
  * **Target Populasi & Profil Responden:** Mahasiswa aktif (Fakultas Ilmu Komputer Universitas Esa Unggul dan/atau usia produktif 18–30 tahun) yang berkomitmen mencatat rutinitas harian konsisten (studi/olahraga/manajemen waktu).  
  * **Ukuran Sampel (Sample Size):** Definitif **N = 30 responden** (dibagi seimbang: **15 responden** Kelompok Eksperimen Adaptif, **15 responden** Kelompok Kontrol Statis) yang memenuhi ambang kelayakan uji komparatif skripsi rancang bangun perangkat lunak.  
  * **Metode Sampling:** *Purposive Sampling* dengan kriteria:  
    * *Inklusi:* Smartphone Android OS ≥ 8.0, bersedia menyetujui *informed consent*, berkomitmen mencatat 1–2 kebiasaan selama 14 hari penuh, dan mengisi kuesioner evaluasi akhir (SUS & HMSAM).  
    * *Eksklusi:* Tidak memiliki perangkat Android, tidak menyelesaikan onboarding, atau menolak informed consent.  
  * **Alokasi Grup:** Alokasi terkontrol peneliti via *Feature Flag* Supabase Studio (15 Adaptif : 15 Statis), persisten pada profil akun & SQLite lokal (tidak berubah saat logout/login), dengan protokol *Single-Blind* (UI identik).  
  * **Batasan Platform:** Mobile native Android murni (Flutter, SQLite lokal, Supabase backend) tanpa versi iOS/web guna mengontrol variabel platform.
* **I — Intervention:**  
  Aplikasi mobile *habit tracker* dengan sistem gamifikasi adaptif berbasis aturan IF–THEN deterministik (membaca parameter: *current streak*, rasio keberhasilan 7 hari, dan status check-in) untuk menyesuaikan Base XP (10 poin) dengan multiplier dinamis (1.15×, 1.25×, 1.5×, hingga puncak 2.0× di hari ke-14), pembukaan lencana bertingkat, dan pesan motivasi kontekstual secara transparan (*auditable*) tanpa Machine Learning/LLM.
* **C — Comparison:**  
  Aplikasi habit tracker dengan **gamifikasi statis konstan** sebagai baseline pembanding.  
  *Desain Uji:* **Parallel Two-Group Between-Subject Design** (Grup A: Adaptif vs Grup B: Statis) selama 14 hari penuh. Bebas dari periode *washout* yang mematikan tombol check-in, menjamin pencatatan kebiasaan berjalan natural.
* **O — Outcome:**  
  Kerangka evaluasi 4 lapis terpadu:  
  1. *Validitas Logika Aturan:* Black-box testing eksekusi aturan IF–THEN (Target: 100% valid).  
  2. *Keterlibatan & Retensi Jangka Pendek:* Persentase hari aktif pencatatan (check-in) dan retensi ketuntasan partisipan hingga kuesioner akhir (*Study Completion Rate*), dianalisis menggunakan uji signifikansi komparatif (*t-Test / Mann-Whitney U*) guna mengukur besaran perbedaan (*effect size*) kedua kelompok secara objektif.  
  3. *Usability Antarmuka:* Skor System Usability Scale dengan target acuan industri mHealth **SUS ≥ 68** (Hyzy et al., 2022).  
  4. *Penerimaan Pengguna:* Skor konstruk *Behavioral Intention to Use* (BIU) pada model HMSAM untuk membandingkan kecenderungan keberlanjutan pemakaian aplikasi antar-kelompok.
* **S — Setting / Domain:**  
  Domain aplikasi pencatat kebiasaan harian mandiri (*habit tracking tool*) berdurasi terukur (15–30 menit/hari), dibatasi maks. 2 kebiasaan aktif per akun dalam kategori netral: Belajar, Olahraga/Kebugaran, atau Rutinitas Harian. *(Catatan Metodologi: Penelitian ini berfokus pada evaluasi retensi penggunaan aplikasi digital selama periode uji 14 hari, bukan intervensi klinis pembentukan kebiasaan otomatis permanen).*

---

## 2. Tiga Research Questions (RQ Penelitian)
* **RQ 1.1 (Kebutuhan & Parameter Data):**  
  *Apa saja kebutuhan fungsional dan non-fungsional aplikasi habit tracker gamifikasi adaptif, serta data perilaku pengguna apa (streak, rasio sukses 7 hari, status check-in) yang diperlukan untuk menyusun aturan adaptasi yang relevan dan dapat dioperasionalkan?*  
  &rarr; **Implementasi:** FR-01 s.d. FR-07, FR-19, FR-23 s.d. FR-27, NFR-01 s.d. NFR-23 (Bab I & Bab III).
* **RQ 1.2 (Implementasi Teknis & Arsitektur Aturan):**  
  *Bagaimana formulasi aturan IF–THEN untuk adaptasi gamifikasi dirumuskan ke dalam tabel aturan terbuka (auditable rules) dan diimplementasikan ke dalam arsitektur aplikasi mobile offline-first dari sisi klien (Flutter/SQLite) hingga backend (Supabase)?*  
  &rarr; **Implementasi:** FR-08 s.d. FR-17, FR-21, FR-22, FR-31 (Rules + Priority) (Bab III & Bab IV).
* **RQ 1.3 (Evaluasi Empiris, Usability, & Penerimaan):**  
  *Sejauh mana aplikasi habit tracker gamifikasi adaptif berbasis aturan mampu berjalan valid secara logika, meningkatkan keterlibatan dan retensi pengguna, serta memperoleh skor usability (SUS ≥ 68) dan penerimaan (HMSAM BIU) yang lebih unggul dibandingkan baseline gamifikasi statis?*  
  &rarr; **Implementasi:** FR-18 (Telemetri), FR-20 (Mode Between-Subject), FR-28 (Kuesioner Google Forms) (Bab IV & Bab V).

---

## 3. Matriks Keselarasan PICOS &rarr; BRD &rarr; SLR
| Komponen PICOS | RQ Terkait | Implementasi BRD Revisi | Bukti Literatur SLR Revisi |
| :--- | :--- | :--- | :--- |
| **P (Platform)** | RQ 1.1 | FR-01 s.d. FR-05, NFR-22 (Android API 26+) | Kondaveeti (2021) [S3], Sami (2026) [S4] |
| **I (Intervensi)** | RQ 1.2 | FR-08 s.d. FR-17, FR-31 (Rules + Priority) | Bucchiarone (2023) [K16], Dalmina (2024) [K17] |
| **C (Comparison)** | RQ 1.3 | FR-20 (Between-Subject A/B Testing) | Bai & Liu (2024) [K14], Shahnawaz (2026) [K05] |
| **O (Outcome)** | RQ 1.3 | FR-18 (Telemetri), FR-28 (SUS & HMSAM), NFR-12 | Hyzy (2022) [K18], Ma (2025) [K19], Bijkerk (2023) [K20] |
| **S (Setting)** | RQ 1.1 | FR-03 (CRUD Kebiasaan Harian Umum) | Zhu (2024) [K01], Huang (2024) [K11] |

---

## 4. Checklist Fase 1 (100% Selesai)
* [x] Topik spesifik (Mobile Android Habit Tracker Gamifikasi Adaptif Rule-Based)
* [x] Definisi PICOS operasional dan terukur
* [x] 3 Research Questions terpetakan ke bab skripsi
* [x] Platform native Android Flutter & SQLite
* [x] Baseline komparasi Parallel Between-Subject (Bebas Washout)
* [x] Metrik evaluasi 4 lapis objektif (Validitas, Telemetri, SUS ≥ 68, HMSAM)
* [x] Sinkronisasi 100% dengan BRD Revisi dan SLR Revisi
