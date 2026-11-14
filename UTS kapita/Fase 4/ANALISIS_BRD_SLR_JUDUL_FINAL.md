# 6.4 Analisis Sintesis BRD + SLR → Penetapan Judul Final (SYNTHESIS-HABIT-001)

> **[Catatan Rilis & Keselarasan — Rev. 1.2]**
> Dokumen **Bagian 6.4 — Analisis Sintesis BRD + SLR → Judul Final (`SYNTHESIS-HABIT-001 Rev. 1.2`)** memadukan spesifikasi kebutuhan bisnis-teknis pada `BRD-HABIT-001 (Rev. 1.2)` dengan temuan empiris 30 studi mutakhir (2021–2026) pada `SLR-HABIT-001 (Rev. 1.2)` serta `EVIDENCE-MATRIX-HABIT-001 (Rev. 1.2)`. Dokumen ini mengunci justifikasi **Judul Final Tugas Akhir**, mengevaluasi kandidat alternatif judul, serta memetakan dekonstruksi setiap unsur judul ke artefak implementasi Proposal Bab 1–5.

---

## Informasi Dokumen

| Field | Keterangan |
| :--- | :--- |
| **Kode & Bagian Dokumen** | `SYNTHESIS-HABIT-001` — **Bagian 6.4 (Analisis BRD + SLR → Judul Final)** |
| **Versi / Status** | `Rev. 1.2 — Final Selaras (Mandatori Sempro & TA-1)` |
| **Judul Final Terpilih** | **Rancang Bangun Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN untuk Meningkatkan Retensi Jangka Pendek pada Aplikasi Mobile Habit Tracker** |
| **Penyusun / NIM** | Angga Kurnia Putra / `20230801133` |
| **Program Studi / Fakultas** | Teknik Informatika / Fakultas Ilmu Komputer — Universitas Esa Unggul (2026) |
| **Dokumen Masukan Utama** | `PICOS-HABIT-001 (Rev. 1.2)`, `BRD-HABIT-001 (Rev. 1.2)`, `SLR-HABIT-001 (Rev. 1.2)`, `EVIDENCE-MATRIX-HABIT-001 (Rev. 1.2)` |
| **Target Luaran Sintesis** | Justifikasi Judul Final, Matriks Konvergensi BRD×SLR, Komparasi Kandidat Judul, & *Scope Lock* Bab 1–5 |

---

## 1. Sintesis Kebutuhan Bisnis & Rekayasa (`BRD-HABIT-001 Rev. 1.2`)

Berdasarkan dokumen **`BRD-HABIT-001 (Rev. 1.2)`**, permasalahan utama aplikasi *habit tracker mobile* berada pada **ranah Rekayasa Perangkat Lunak (FASILKOM)** — bukan intervensi modifikasi perilaku klinis jangka panjang — dengan tiga masalah nyata yang ditangani sistem:
1. **Ketidaksesuaian Tantangan (*One-Size-Fits-All Fatigue*):** Gamifikasi statis memberikan insentif seragam bagi semua pengguna; pengguna berkonsistensi tinggi cepat jenuh, sedangkan pengguna yang mengalami penurunan performa kehilangan motivasi karena tidak adanya *adaptive scaffolding*.
2. **Ketiadaan Umpan Balik Aturan *Real-Time* & *Offline-First*:** Minimnya mesin aturan deterministik pada perangkat mobile Android yang memodifikasi insentif (XP, lencana, tantangan, pesan motivasi) secara lokal dan tersinkronisasi aman ke *cloud*.
3. **Kesenjangan Bukti Komparatif yang Terukur:** Kurangnya pengujian terkontrol *Between-Subject Single-Blind* yang mengisolasi efek mesin IF-THEN adaptif terhadap retensi jangka pendek 14 hari, *usability* (SUS), dan penerimaan pengguna (HMSAM).

| Dimensi Spesifikasi `BRD` | Cakupan Terkunci | Kode & Parameter Kritis | Target / Kriteria Verifikasi |
| :--- | :--- | :--- | :--- |
| **Sasaran Bisnis (`BO-01..BO-07`)** | 7 Sasaran SMART berfokus netral pada signifikansi perbedaan (*comparative effect size*) | `BO-01` s.d. `BO-07` | 100% lulus Black-Box, uji beda retensi & HMSAM, `SUS >= 68`, 100% offline-ready, patuh UU PDP 27/2022 |
| **Kebutuhan Fungsional (`FR`)** | **32 FR Sekuensial** (Auth, Habit ≤ 2 aktif, Check-in, IF-THEN Engine, Sync LWW, Telemetry, Consent/Delete) | `FR-01` s.d. `FR-32` | Terpetakan ke `RQ 1.1` (11 FR), `RQ 1.2` (13 FR), `RQ 1.3` (3 FR), dan `Non-RQ` (5 FR) |
| **Kebutuhan Non-Fungsional (`NFR`)** | **23 NFR Terstruktur** (Performa, Keamanan/RLS, Usability, Reliabilitas Offline, Kompatibilitas) | `NFR-01` s.d. `NFR-23` | Check-in `<= 2 dtk`, evaluasi rule `<= 500 ms`, APK `<= 50 MB`, check-in `<= 2 ketukan`, Android `>= 8.0` |
| **Katalog Aturan IF-THEN** | **5 Aturan Konkret** berprioritas eksplisit (`10–100`) & *Base XP = 10 Poin* | `R-01` s.d. `R-05` | Evaluasi berbasis `current_streak`, `success_rate_7d`, dan `mode` (`adaptif` vs `statis`) |
| **Arsitektur & Skema Data** | **Flutter Android + SQLite + Supabase PostgreSQL (RLS & JWT Auth)** dengan **7 Tabel** | `users`, `habits`, `habit_logs`, `rules`, `activity_logs`, `user_rewards`, `local_rewards` | *Offline-first*, mitigasi *clock drift* (lokal vs UTC), resolusi konflik LWW 24 jam, `rule_version` freeze |

---

## 2. Sintesis Temuan Literatur Sistematis (`SLR-HABIT-001 Rev. 1.2`)

Dari **122 rekaman unik Scopus** (`scopus (11) 1–3.ris`) yang disaring menjadi **81 kandidat** (`Literatur.ris`) hingga **30 studi terpilih final** (**5 Studi Inti `S1–S5`** + **25 Studi Kontekstual `K01–K25`**), seluruhnya terbit pada **2021–2026** (0 studi > 5 tahun; rata-rata **QA = 4,07 / 5,00** dengan `76,7%` studi berskor ≥ 4). Lima tema sintesis dan empat celah riset utama dirangkum sebagai berikut:

| Tema / Gap `SLR` | Sintesis Temuan Kunci | Studi Rujukan Utama | Implikasi Langsung ke Desain & Judul |
| :--- | :--- | :--- | :--- |
| **Tema 1: Dinamika Gamifikasi** | Gamifikasi statis mengalami peluruhan kebaruan (*gamification fatigue*) dalam 2 minggu pertama; adaptasi aturan menjaga motivasi jika transparan | `S1` (Gosetto, 2025), `S4` (Sami & Matcham, 2026), `K11` (Huang, 2024), `K13` (Rodrigues, 2021) | Membenarkan frasa **"Mesin Gamifikasi Adaptif"** dan fokus durasi **"Retensi Jangka Pendek" (14 hari)** |
| **Tema 2: Arsitektur Aturan Deklaratif** | Logika adaptasi wajib dipisah dalam tabel deklaratif (*decoupled*), mendukung pembekuan versi aturan & auditabilitas tanpa *black-box AI* | `K16` (Bucchiarone, 2023), `K17` (Dalmina, 2024), `K08` (Ren & Mu, 2025), `K09` (Manjula, 2025), `K23` (Jali, 2026) | Membenarkan pilihan spesifik **"Berbasis Aturan IF-THEN"** (bukan ML/RL/LLM yang berat & *cold-start*) |
| **Tema 3: Pemicu Adaptasi Minimal** | Tiga parameter perilaku minimal untuk evaluasi adaptasi yang stabil adalah `current_streak`, rasio sukses 7 hari, dan status *check-in* harian | `S2` (Gosetto, 2026), `K01` (Zhu, 2024), `K03` (Weathers, 2024), `K05` (Shahnawaz, 2026), `K06` (Aguiar, 2022) | Memvalidasi parameter pemicu konkret aturan `R-01..R-05` dan `FR-04..FR-07` |
| **Tema 4: Evaluasi 4 Lapis Terpadu** | Keberhasilan rekayasa sistem diukur melalui validitas logika (Black-Box), telemetri aktivitas nyata, *usability* (`SUS >= 68`), & penerimaan (`HMSAM`) | `S5` (Yang, 2026), `K18` (Hyzy, 2022), `K19` (Ma, 2025), `K20` (Bijkerk, 2023), `K21` (Neiva, 2025) | Membenarkan pendekatan **Rancang Bangun + Evaluasi Komparatif** yang objektif dan multi-metrik |
| **Tema 5: Desain Baseline & Etika** | *Parallel Two-Group Between-Subject* tanpa *washout period* mencegah bias interupsi rutinitas; kepatuhan privasi meningkatkan kepercayaan | `S5` (Yang, 2026), `K14` (Bai & Liu, 2024), `K22` (Ofosu-Ampong, 2026), `K24` (Kapoor, 2025) | Mengunci desain uji `N = 30` (15 Adaptif vs 15 Statis) selama 14 hari + kepatuhan UU PDP No. 27/2022 |

---

## 3. Matriks Konvergensi Silang: `BRD` × `SLR` → Konstruk Judul Final

Konvergensi antara kebutuhan sistem (`BRD`) dan celah penelitian (`SLR`) menghasilkan lima pilar yang membentuk struktur **Judul Final**:

| Unsur / Frasa pada Judul Final | Kebutuhan Bisnis & Teknis (`BRD-HABIT-001`) | Landasan & Celah Empiris (`SLR-HABIT-001`) | Wujud Artefak & Bukti Pembuktian |
| :--- | :--- | :--- | :--- |
| **1. "Rancang Bangun"** | Pengembangan perangkat lunak *end-to-end* Android (`Flutter`), arsitektur *offline-first* (`SQLite` + `Supabase`), 32 FR & 23 NFR | `S3` (Kondaveeti, 2021) baru membangun tracker statis; `S1/S2` baru sebatas model konseptual tanpa prototipe berjalan | Aplikasi APK Android (`<= 50 MB`), skema 7 tabel, Black-Box Testing (`100%` skenario lolos) |
| **2. "Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN"** | Modul rule engine terpisah (`rules`, `priority 10–100`, `rule_version`), `FR-08..12`, `FR-31`, `R-01..R-05`, latensi `<= 500 ms` | Gap 2 `SLR`: AI/RL/LLM (`K08/K09/K23`) mengalami *cold-start* & kotak-hitam; `K16/K17` menegaskan *decoupled rule engine* | Katalog aturan deterministik `R-01..R-05` + `activity_logs` yang dapat diaudit penuh |
| **3. "untuk Meningkatkan Retensi Jangka Pendek"** | Mengukur *Study Completion Rate* 14 hari (`BO-03`), persentase hari aktif (`BO-02`), `SUS >= 68` (`BO-04`), & `HMSAM BIU` (`BO-05`) | `S4` & `K11` membuktikan *drop-off* kritis terjadi pada 14 hari pertama akibat kejenuhan fitur statis | Data telemetri 14 hari (`N = 30`), uji beda t-Test / Mann-Whitney U (`Adaptif vs Statis`) |
| **4. "pada Aplikasi Mobile Habit Tracker"** | Domain *habit tracking tool* mandiri (`max 2 kebiasaan`, 15–30 menit/hari), check-in `<= 2 ketukan`, Bahasa Indonesia, patuh UU PDP 27/2022 | `K05`, `K06`, `K21`, `K25` menunjukkan konsistensi check-in rendah friksi lebih krusial daripada kompleksitas fitur sosial | Aplikasi Flutter standalone Android (`>= OS 8.0`) siap dipakai responden uji coba |

---

## 4. Evaluasi Komparatif Alternatif Judul → Penetapan Judul Final

Sebelum mengunci Judul Final, tiga alternatif formulasi judul dievaluasi secara ketat terhadap **5 kriteria kelayakan TA FASILKOM**: (1) Ketepatan Ranah RPL (*Software Engineering Fit*), (2) Ketajaman Mekanisme Teknis, (3) Kelayakan Uji 14 Hari (`N = 30`), (4) Keterlacakan `BRD` & `SLR`, dan (5) Netralitas Ilmiah.

| Kandidat Judul | Evaluasi Kekuatan | Kelemahan / Risiko Kritis | Status |
| :--- | :--- | :--- | :---: |
| **Alt-1:** *Aplikasi Mobile Pembentuk Kebiasaan Jangka Panjang Berbasis Kecerdasan Buatan untuk Mahasiswa* | Terdengar populer karena menyebut AI dan pembentukan kebiasaan | **Ditolak:** Melanggar batas ranah RPL (*behavior modification* klinis butuh >= 66 hari), klaim AI tidak cocok untuk `N = 30` / 14 hari (*cold-start*), & tidak mencerminkan `BRD` | **Ditolak** |
| **Alt-2:** *Rancang Bangun Aplikasi Habit Tracker Android dengan Fitur Gamifikasi untuk Meningkatkan Motivasi Pengguna* | Jelas sebagai proyek rekayasa aplikasi Android | **Ditolak:** Terlalu generik/deskriptif seperti proyek CRUD biasa; tidak menonjolkan kebaruan mesin aturan adaptif IF-THEN maupun desain komparatif | **Ditolak** |
| **Alt-3:** *Implementasi Sistem Pakar IF-THEN untuk Penentuan Reward Poin pada Aplikasi Pencatat Aktivitas Harian* | Menonjolkan logika aturan IF-THEN secara spesifik | **Ditolak:** Istilah "Sistem Pakar" kurang tepat karena tidak memakai basis pengetahuan ahli klinis/forward chaining kompleks, melainkan *adaptive gamification engine* berprioritas | **Ditolak** |
| **JUDUL FINAL:** **Rancang Bangun Mesin Gamifikasi Adaptif Berbasis Aturan IF-THEN untuk Meningkatkan Retensi Jangka Pendek pada Aplikasi Mobile Habit Tracker** | **100% Presisi:** Menonjolkan objek rekayasa (*Mesin Gamifikasi Adaptif IF-THEN*), terukur dalam batas waktu TA (*Retensi Jangka Pendek 14 hari*), berbasis domain aplikasi nyata (*Mobile Habit Tracker*), & selaras penuh dengan `PICOS`, `BRD`, `SLR`, dan `Evidence Matrix` | Membutuhkan disiplin ketat menjaga fokus pada **retensi penggunaan aplikasi (tracking)** dan pembuktian **komparatif Single-Blind**, yang telah diatasi melalui protokol `BRD Rev. 1.2` | **DITETAPKAN (FINAL)** |

---

## 5. Penguncian Ruang Lingkup (*Scope Lock*) & Peta Bab Proposal

Agar implementasi TA-2 tidak melebar (*scope creep*), seluruh unsur Judul Final dikunci ke dalam batas operasional berikut:

| Aspek Batasan (*Scope Lock*) | Keputusan Terkunci (Sesuai `BRD` & `SLR`) | Alasan Metodologis & Teknis | Posisi di Proposal |
| :--- | :--- | :--- | :--- |
| **Ranah Keilmuan Sistem** | **Habit Tracking Tool** (pencatatan & keterlibatan aplikasi), bukan terapi psikologis perubahan kebiasaan permanen | Menghindari bias klaim domain Psikologi/Kesehatan Klinis pada sidang Teknik Informatika | **Bab 1** (Batasan), **Bab 2** |
| **Mekanisme Adaptasi** | **5 Aturan IF-THEN Deterministik (`R-01..R-05`)** dengan `priority` (`10–100`) & `rule_version` | Bebas *cold-start*, eksekusi lokal `<= 500 ms`, transparan diaudit via `activity_logs`, tanpa ML/LLM | **Bab 2**, **Bab 3** (Desain Engine) |
| **Platform & Arsitektur** | **Standalone Native Android (`Flutter`) + `SQLite` Lokal + `Supabase` Cloud (RLS & JWT)** | Menjamin operasi **100% offline-first** dan resolusi konflik LWW 24 jam tanpa bias koneksi | **Bab 3** (Arsitektur 3-Tier & DB) |
| **Desain & Durasi Uji** | **Parallel Two-Group Between-Subject Single-Blind (`N = 30`: 15 Adaptif vs 15 Statis)** selama **14 hari penuh**, maks **2 kebiasaan aktif** | Menghindari efek *carry-over/washout* dan mengisolasi perbedaan variabel adaptif vs statis secara adil | **Bab 3** (Metodologi Eksperimen) |
| **Metrik & Instrumen Final** | 4 Lapis: **Black-Box (100%)**, **Telemetri Retensi 14 Hari & Hari Aktif**, **SUS (`>= 68`)**, & **HMSAM (`BIU`)** | Membuktikan validitas fungsional, keterlibatan objektif, kelayakan usability, dan persepsi penerimaan | **Bab 3**, **Bab 4** (Rencana Hasil) |
| **Kepatuhan Privasi** | Implementasi penuh **UU PDP No. 27/2022** (*Informed Consent*, *Delete Account*, Anonimisasi ekspor) | Melindungi data perilaku responden mahasiswa & memenuhi standar etik riset digital (`K22`, `K24`) | **Bab 3** (`FR-29`, `FR-30`, `NFR-09..10`) |

---

## 6. Checklist Kesiapan Penetapan Judul Final (Bagian 6.4)

| No | Item Audit Sintesis 6.4 | Status | Keterangan Verifikasi |
| :---: | :--- | :---: | :--- |
| 1 | Seluruh unsur Judul Final memiliki bukti pendukung dari `SLR` (30 studi 2021–2026) | **Sesuai** | Terpetakan lengkap ke 5 Tema Sintesis & 4 Research Gap `SLR-HABIT-001` |
| 2 | Judul Final didukung spesifikasi teknis terukur pada `BRD-HABIT-001` (`32 FR`, `23 NFR`) | **Sesuai** | Seluruh elemen judul tertaut ke `BO-01..07`, `FR-01..32`, & `R-01..R-05` |
| 3 | Pemilihan metode *Rule-Based IF-THEN* memiliki justifikasi unggul dibanding ML/LLM | **Sesuai** | Bebas *cold-start* 14 hari, deterministik, offline `<= 500 ms`, & dapat diaudit |
| 4 | Alternatif judul lain telah dievaluasi dan alasan penolakannya terdokumentasi | **Sesuai** | 3 alternatif judul dibandingkan secara objektif pada Tabel Bagian 4 |
| 5 | Ruang lingkup (`Scope Lock`) aman diselesaikan dalam batas waktu TA mahasiswa S1 | **Sesuai** | Terkendali: Android-only, `N = 30`, uji 14 hari, maks 2 kebiasaan, arsitektur Flutter+Supabase |

---

**— Akhir Dokumen `SYNTHESIS-HABIT-001 (Bagian 6.4 — Rev. 1.2)` —**
