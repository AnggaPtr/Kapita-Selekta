# 6.5.4 Audit & Kritik Keras: Kesesuaian, Kelayakan & Nilai Portofolio (CRITIQUE-HABIT-001)

> **[Catatan Audit Kritis — Rev. 1.2]**
> Dokumen **Bagian 6.5.4 (`CRITIQUE-HABIT-001 Rev. 1.2`)** berperan sebagai *adversarial reviewer* (penguji sidang Sempro & *Senior Engineering Reviewer*) yang menguji tanpa kompromi tiga dimensi penentu keberhasilan Tugas Akhir: **(1) Kesesuaian Akademik & Lintas Dokumen**, **(2) Kelayakan Teknis, Statistik, Waktu & Etika**, serta **(3) Nilai Portofolio Rekayasa di Industri**. Setiap kritik tajam disertai vonis risiko dan rencana mitigasi wajib sebelum pelaksanaan TA-2.

---

## Informasi Dokumen

| Field | Keterangan |
| :--- | :--- |
| **Kode & Bagian Dokumen** | `CRITIQUE-HABIT-001` — **Bagian 6.5.4 (Kritik Keras: Kesesuaian, Kelayakan & Nilai Portofolio)** |
| **Versi / Status** | `Rev. 1.2 — Adversarial Readiness Review` |
| **Objek yang Diaudit** | Judul Final, `PICOS-HABIT-001`, `BRD-HABIT-001`, `SLR-HABIT-001`, `EVIDENCE-MATRIX-HABIT-001`, Proposal Bab 1–5 |
| **Penyusun / NIM** | Angga Kurnia Putra / `20230801133` — Teknik Informatika FASILKOM UEU (2026) |
| **Perspektif Penilai** | Dewan Penguji Skripsi FASILKOM (RPL & Metodologi) + *Senior Mobile/Product Engineer* |
| **Vonis Umum Kesiapan** | **LAYAK DENGAN CATATAN MITIGASI WAJIB** — Fondasi `BRD`/`SLR` kuat, namun **4 celah kritis wajib dikawal ketat** saat implementasi & uji lapangan |

---

## 1. Ringkasan Eksekutif & Kartu Skor Audit (*Scorecard*)

| Pilar Evaluasi | Skor Audit | Status Vonis | Temuan Kritis Teratas |
| :--- | :---: | :---: | :--- |
| **Pilar A: Kesesuaian Akademik, RPL & Lintas Artefak** | **88 / 100** | **Baik (Waspada Inkonsistensi)** | Batas antara *habit tracking tool* vs *behavior modification* sudah terkunci di `BRD Rev. 1.2`, tetapi konsistensi detail katalog aturan `R-01..R-05` antar dokumen turunan wajib diaudit ketat agar tidak bergeser dari `BRD` |
| **Pilar B: Kelayakan Teknis, Statistik & Operasional** | **82 / 100** | **Layak Bersyarat** | Sampel `N = 30` (15 vs 15) berisiko *underpowered* untuk efek kecil dan rentan *drop-out* responden 14 hari; mekanisme *clock drift* & sinkronisasi LWW menuntut pengujian *edge-case* ekstra |
| **Pilar C: Nilai Portofolio Rekayasa & Karier** | **90 / 100** | **Tinggi (Jika Dieksekusi Bersih)** | Jika sekadar CRUD + poin statis, nilai jual nol; namun dengan **Data-Driven Rule Engine (`NFR-21`), Offline-First Sync + LWW (`FR-16/21`), UU PDP Compliance, & Feature-Flag A/B Testing**, nilai portofolio sangat kompetitif |

---

## 2. Pilar A — Kritik Keras Kesesuaian (Academic Fit & Alignment Audit)

### A.1 Serangan Penguji: *"Ini Skripsi Teknik Informatika atau Psikologi Perubahan Perilaku?"*
* **Akar Kritik:** Istilah *"habit tracker"* dan *"meningkatkan retensi"* kerap memancing pertanyaan dosen penguji apakah mahasiswa mengklaim mampu mengubah kebiasaan permanen manusia (yang secara literatur psikologi membutuhkan puluhan hari).
* **Audit Kesiapan:** Dokumen `BRD-HABIT-001 (Rev. 1.2)` sudah memagari klaim ini pada **Out-of-Scope 3.2**: sistem adalah *habit tracking tool* dan yang diukur adalah **retensi penggunaan aplikasi & keterlibatan pencatatan jangka pendek (14 hari)**, bukan otomatisasi perilaku klinis (*habit automaticity*).
* **Celah Tersisa & Mitigasi Wajib:** Mahasiswa wajib konsisten menggunakan diksi **retensi penggunaan aplikasi (*app retention / study completion*)** dan menghindari kalimat over-klaim seperti *"membentuk kebiasaan seumur hidup"* saat presentasi Sempro maupun penulisan abstrak.

### A.2 Serangan Penguji: *"Kenapa Pakai Aturan IF-THEN Sederhana, Bukan Machine Learning atau LLM?"*
* **Akar Kritik:** Di tahun 2026, memakai hanya 5 aturan IF-THEN (`R-01..R-05`) bisa dicap "kurang canggih" atau terlalu sederhana untuk skripsi Teknik Informatika.
* **Audit Kesiapan:** Justifikasi berbasis literatur sudah solid: `K08` (Ren & Mu, 2025) membuktikan *Reinforcement Learning* gagal efektif di fase awal karena *cold-start*; `K09` (Manjula, 2025) & `K12` menyoroti isu *explainability*; `K23` (Jali, 2026) menegaskan beban latensi/halusinasi LLM; serta `K16` (Bucchiarone, 2023) memvalidasi pemisahan tabel aturan deklaratif (`rules`).
* **Celah Tersisa & Mitigasi Wajib:** Kekuatan teknisnya tidak terletak pada "banyaknya `if/else` di kode", melainkan pada **arsitektur Data-Driven Declarative Rule Engine** (`rules` table di Supabase/SQLite dengan `priority` `10–100`, `rule_version` freeze, conflict resolution `FR-31`, dan `<= 500 ms` offline execution sesuai `NFR-02/NFR-21`). Jika aturan di-*hardcode* di UI Flutter, kritik "terlalu dangkal" menjadi benar!

### A.3 Audit Temuan Sinkronisasi Lintas Dokumen (*Real Document Audit Finding*)
* **Temuan Kritik Keras:** Dalam pemeriksaan silang korpus proyek ditemukan kerentanan inkonsistensi apabila dokumen penelusuran atau naskah turunan masih mencampurkan rancangan konseptual lama (misal variasi nama aturan HEXAD/Streak Shield atau penomoran FR sebelum revisi) dengan spesifikasi terkunci pada **`BRD-HABIT-001 (Rev. 1.2)`**:
  1. Katalog resmi `BRD Rev. 1.2` menetapkan **32 FR (`FR-01..FR-32`)** dan **5 aturan konkret berbasis `current_streak` & `success_rate_7d`** (`R-01: Puncak 14 Hari`, `R-02: Konsistensi Mingguan`, `R-03: Scaffolding/Pemulihan`, `R-04: Konsistensi Awal`, `R-05: Baseline Statis`), **tanpa** kuesioner kepribadian HEXAD saat *onboarding* (`FR-04` di `BRD Rev. 1.2` adalah *Pencatatan Check-in*).
  2. Referensi `K16` & `K19` memiliki varian *proceeding/journal* di `Literatur.ris` vs ringkasan awal `SLR` (sudah dicatat pada kotak rekonsiliasi `EVIDENCE-MATRIX`).
* **Vonis Audit:** Sebelum masuk TA-2 (*coding* & sidang), **seluruh tim/penulis wajib menjadikan `BRD-HABIT-001 Rev. 1.2` sebagai *Single Source of Truth*** untuk kode `FR-01..FR-32`, `NFR-01..NFR-23`, 7 tabel DB, dan 5 aturan `R-01..R-05`.

---

## 3. Pilar B — Kritik Keras Kelayakan (Technical, Statistical, Operational & Ethical)

| Dimensi Kelayakan | Kritik Keras & Skenario Terburuk (*Worst-Case*) | Tingkat Risiko | Mitigasi Teknis & Metodologis Wajib |
| :--- | :--- | :---: | :--- |
| **B.1 Kekuatan Statistik (`N = 30`, 15 vs 15)** | Ukuran sampel `n = 15` per grup hanya memiliki *statistical power* memadai untuk efek besar (*large effect size*). Jika 3–4 peserta *drop out* di tengah jalan, varians meningkat dan uji beda (`t-Test`) berisiko gagal mencapai `p < 0,05`. | **TINGGI** | Rekrut **18–20 orang per grup** (*over-sampling 20%*) untuk berjaga terhadap *drop-out*, siapkan uji non-parametrik **Mann-Whitney U**, dan laporkan **Effect Size (Cohen's d / r)** + analisis deskriptif lengkap agar tetap ilmiah meski tidak signifikan ekstrem (*BRD BO-02/03 netral*). |
| **B.2 Manipulasi Waktu (*Clock Drift* & *Time Cheating*)** | Aplikasi mengizinkan operasional 100% offline (`NFR-16`) dengan batas hari lokal `00:00–23:59` (`FR-25`). Responden yang memundurkan jam HP dapat mengakali *streak* menjadi 14 hari instan. | **SEDANG–TINGGI** | Catat pasangan **`tanggal_lokal` DAN `waktu_checkin_utc` (`FR-05`)**, terapkan batas validasi selisih LWW **24 jam (`FR-21`)**, dan audit anomali *timestamp* mundur pada `activity_logs` saat data disinkronkan ke Supabase. |
| **B.3 Ketahanan Sinkronisasi Offline-Online (`SQLite` ↔ `Supabase`)** | Data gagal tersinkron (*silent drop*) saat mahasiswa memakai HP di area kampus berjaringan lemah, atau `local_rewards` ganda menyebabkan XP terhitung dua kali (*double counting*). | **TINGGI** | Gunakan UUID deterministik dari sisi klien (`log_id`, `reward_id`), flag `is_synced`, idempotensi *upsert* di PostgreSQL, serta **retry otomatis 3x jeda 30 detik (`NFR-18`)**. Uji paksa skenario *airplane mode 3 hari -> online*. |
| **B.4 Kontaminasi *Single-Blind* Antar-Mahasiswa** | Teman satu kelas di kelompok `adaptif` dan `statis` saling melihat HP dan sadar sistem reward mereka berbeda, memicu *Hawthorne / John Henry effect*. | **SEDANG** | Pastikan antarmuka (UI) identik tanpa badge pembeda mode (`FR-20`), acak penugasan lintas angkatan/komunitas yang tidak saling berinteraksi harian, dan sembunyikan detail mekanisme reward antar-peserta saat *briefing*. |
| **B.5 Kepatuhan Hukum Privasi (UU PDP 27/2022)** | Pengumpulan *log* kebiasaan harian mahasiswa dikategorikan data perilaku pribadi; kelalaian hak hapus akun/persetujuan melanggar etika riset & UU PDP. | **WAJIB** | Jangan pernah melewatkan `FR-29` (*Delete Account* kaskade SQLite+Supabase), `FR-30` (*Informed Consent* digital sebelum login pertama), RLS PostgreSQL (`NFR-06`), & skrip ekspor CSV anonim tanpa email/nama (`NFR-10`). |

---

## 4. Pilar C — Kritik Keras Nilai Portofolio (Engineering & Career Value)

### C.1 Realitas Pasar Kerja: *Mengapa Habit Tracker Biasa Tidak Menarik di Mata Tech Recruiter?*
Aplikasi *habit tracker* standar (CRUD daftar tugas, tombol centang, grafik mingguan, poin statis) adalah proyek tutorial tingkat pemula yang tersedia ribuan di GitHub. Jika proyek ini hanya dinilai sebagai aplikasi pencatat biasa, **nilai portofolionya rendah**.

### C.2 Empat Pembeda Teknikal yang Mengangkat Nilai Portofolio ke Level *Production-Grade*
Agar skripsi ini sekaligus menjadi **portofolio unggulan untuk peran Mobile Engineer, Backend/Full-Stack Engineer, atau Product Engineer**, implementasi pada TA-2 wajib menonjolkan 4 aset rekayasa berikut:

| Aset Rekayasa pada Proyek | Mengapa Bernilai Tinggi di Industri? | Bukti Kode & Dokumentasi yang Wajib Dipamerkan di GitHub / CV |
| :--- | :--- | :--- |
| **1. Declarative Data-Driven Rule Engine** (`rules`, `priority`, `rule_version`) | Di industri fintech/e-commerce/SaaS, aturan promo, *scoring*, dan *feature gating* dipisah dari kode biner agar dapat diubah tanpa rilis ulang aplikasi (`NFR-21`). | Modul terisolasi `RuleEvaluator` + **Unit Test 100% coverage** untuk 5 aturan `R-01..R-05` dan skenario konflik prioritas (`FR-31`). |
| **2. Offline-First Architecture & LWW Conflict Sync** (`SQLite` + `Supabase`) | Kemampuan membangun aplikasi mobile yang tetap responsif 0% jaringan (`<= 2 dtk`, `NFR-01`) dan sinkron aman tanpa duplikasi data adalah kompetensi inti *Mobile Engineer* profesional. | Diagram alur sinkronisasi, implementasi *sync queue* dengan *retry* (`NFR-18`), penanganan *clock drift* (`FR-21/25`), serta log pengujian jaringan putus-nyambung. |
| **3. Privacy-by-Design & Event Telemetry** (`activity_logs` + `RLS` + UU PDP) | Kepatuhan UU PDP No. 27/2022 (`FR-29/30`) + PostgreSQL *Row Level Security* (`NFR-06`) + telemetri terstruktur JSONB (`FR-18`) mencerminkan kesadaran keamanan & *data engineering* produksi. | Kebijakan SQL RLS Supabase, skrip anonimisasi ekspor riset (`NFR-10`), & dokumentasi skema event telemetry. |
| **4. Built-in A/B Experimentation & Data-Backed Product Impact** | *Engineer* terbaik bukan hanya menulis kode, tetapi membuktikan dampak fitur dengan metrik nyata (**Retensi 14 hari, SUS, HMSAM, uji statistik**). | Laporan eksperimen A/B `N = 30`, grafik kohort retensi hari ke-1 s.d. ke-14, & rangkuman *engineering trade-off* (Rule-Based vs ML/LLM). |

---

## 5. Daftar Pertanyaan "Pembunuh" Sidang & Strategi Pertahanan (*Defense Q&A*)

| No | Pertanyaan Tajam Dewan Penguji | Jawaban Pertahanan Defensif (Berbasis Bukti `BRD` & `SLR`) |
| :---: | :--- | :--- |
| **1** | *"Bagaimana Anda yakin perbedaan retensi disebabkan oleh rule engine IF-THEN, bukan karena tampilan atau jenis kebiasaannya?"* | Kedua kelompok memakai **UI dan kuota kebiasaan identik** (maks. 2 kebiasaan, 15–30 menit/hari via `FR-03`) dengan penugasan **Single-Blind (`FR-20`)** dan pembanding *Baseline Statis* (`R-05`), bukan tanpa gamifikasi—sehingga variabel pembedanya terisolasi murni pada adaptivitas aturan. |
| **2** | *"Apa yang terjadi jika hasil uji 14 hari nanti menunjukkan perbedaan kelompok adaptif vs statis ternyata tidak signifikan secara statistik (`p > 0,05`)?"* | Sesuai revisi netral **`BRD Rev. 1.2` (`BO-02/03/05`)**, penelitian mengukur *signifikansi perbedaan (comparative effect size)* secara objektif. Jika tidak signifikan pada `N = 30`, temuan tersebut tetap valid secara ilmiah (dibahas lewat *effect size*, keterbatasan durasi 14 hari, dan distribusi aturan `R-01..R-05` yang terpicu di `activity_logs`). |
| **3** | *"Kenapa tidak langsung memakai Machine Learning atau Gemini/LLM API agar lebih adaptif terhadap perilaku tiap pengguna?"* | Didukung `K08`, `K09`, dan `K23` (2025–2026): ML/RL butuh data latih ratusan hari (*cold-start* pada uji 14 hari), sedangkan LLM berisiko halusinasi pesan, mahal biaya/latensi, serta **gagal saat mode offline (`NFR-16`)**. Rule-based IF-THEN deterministik menjamin `<= 500 ms` offline & *auditability* 100%. |
| **4** | *"Bagaimana cara menghindari kecurangan pengguna yang hanya buka aplikasi lalu check-in palsu demi poin?"* | Kontrol pembatasan maks. 2 kebiasaan (`FR-03`), audit koreksi check-in hari berjalan (`FR-24`), pencatatan ganda `waktu_checkin_utc` vs `tanggal_lokal` (`FR-05/25`), serta korelasi telemetri pasif di `activity_logs` (`K20`, `K21`). |
| **5** | *"Di mana bukti bahwa aturan prioritas `R-01..R-05` tidak saling bertabrakan saat beberapa kondisi terpenuhi bersamaan?"* | Setiap aturan memiliki atribut integer **`priority` (`10–100`)** pada tabel `rules` (`C.4`) dan mekanisme resolusi konflik eksplisit **`FR-31`** (memilih prioritas tertinggi, misal `R-01 = 100` mengalahkan `R-02 = 90` dan `R-04 = 60`), yang diverifikasi 100% melalui *Black-Box & Unit Testing* (`BO-01`). |

---

## 6. Rencana Aksi Mitigasi Wajib Menuju TA-2 (*Action Checklist*)

| No | Tindakan Perbaikan / Pengawalan Wajib | Target Pemilik | Batas Waktu | Status Target |
| :---: | :--- | :--- | :---: | :--- |
| **1** | Kunci **`BRD-HABIT-001 Rev. 1.2`** sebagai satu-satunya rujukan kode `FR-01..32`, `NFR-01..23`, 7 tabel DB, & aturan `R-01..05` di implementasi Flutter/Supabase | Peneliti / Developer | Sebelum Sprint 1 TA-2 | **Wajib** |
| **2** | Tulis **Unit Test otomatis** untuk seluruh matriks `R-01..R-05`, batas `success_rate_7d`, dan resolusi konflik `priority` (`FR-31`) | Developer | Sprint 1 TA-2 | **Wajib** |
| **3** | Lakukan **Stress Test Sinkronisasi Offline** (3 hari *airplane mode* + perubahan jam perangkat + retry 3x/30s `NFR-18`) sebelum rekrutmen responden | Developer | Pra-Uji Lapangan | **Wajib** |
| **4** | Rekrut cadangan responden **+20% (18–20 orang/kelompok)** dengan verifikasi *Informed Consent* digital (`FR-30`) guna mencegah *underpower* akibat *drop-out* | Peneliti | Minggu ke-1 Uji Coba | **Wajib** |
| **5** | Susun repositori GitHub rapi (*clean architecture*, dokumentasi arsitektur 3-tier, diagram sistem, & skrip evaluasi CSV) sebagai **aset portofolio industri** | Peneliti / Developer | Pasca-Uji & Sidang | **Direkomendasikan** |

---

**— Akhir Dokumen `CRITIQUE-HABIT-001 (Bagian 6.5.4 — Rev. 1.2)` —**
