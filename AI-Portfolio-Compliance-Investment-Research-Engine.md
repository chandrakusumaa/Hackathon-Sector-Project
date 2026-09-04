# Analisis Komprehensif Ide: AI Portfolio Compliance & Investment Research Engine

*Engine Analis Kepatuhan Portofolio & Riset Investasi Otomatis*

---

## 1. Lanskap Permasalahan & Ringkasan Eksekutif

### Permasalahan Bisnis Nyata (*Enterprise Problem Statement*)

Institusi finansial, pengelola dana pensiun, dan manajer investasi di kawasan Asia Tenggara (khususnya pasar BEI/IDX dan SGX) menghadapi tiga tantangan operasional utama:

- **Lambatnya Audit Kepatuhan Regulasi** — Pemantauan batasan investasi (seperti batas maksimum alokasi 10% per emiten, rasio likuiditas minimum, atau standar ESG) terhadap ratusan emiten dilakukan secara manual dan terpisah-pisah, memakan waktu berhari-hari.
- **Keterlambatan Membaca Arus Modal (*Institutional vs Retail Flow*)** — Saat harga saham berfluktuasi, analis sulit membedakan secara instan apakah pergerakan tersebut didorong oleh akumulasi pemodal asing institusional atau spekulasi ritel domestik.
- **Ketidaksigapan Optimasi Risiko & Pajak** — Peluang *Tax-Loss Harvesting* (pemanenan kerugian pajak untuk efisiensi beban fiskal portofolio) dan kalkulasi *Value at Risk* (VaR) sering kali terlewatkan saat volatilitas pasar melonjak tinggi.

### Ringkasan Solusi

Sistem ini dirancang sebagai **Asisten Agen Kecerdasan Buatan Otonom Berbasis Multi-Agen** yang mengintegrasikan *Model Context Protocol* (MCP) untuk terhubung langsung ke Sectors Financial API. Agen beroperasi secara independen untuk melakukan penyaringan instrumen, evaluasi risiko, kalkulasi kepatuhan, serta penyusunan rekomendasi penyeimbangan kembali (*rebalancing*) portofolio.

---

## 2. Arsitektur Pola Multi-Agen (*Agentic Depth*)

Untuk memenuhi kriteria tertinggi pada pilar *Agentic Depth* (25%–30% bobot penilaian), sistem menggunakan arsitektur graf multi-agen asinkron yang menerapkan empat pola agentik utama:

```
                                +---------------------------+
                                |     Pengguna / Input      |
                                +---------------------------+
                                              |
                                              v
                                +---------------------------+
                                |  Orkestrator / Planner    |
                                +---------------------------+
                                              |
                     +------------------------+------------------------+
                     |                                                 |
                     v                                                 v
      +------------------------------+                  +------------------------------+
      | Quantitative Research Agent  |                  | Compliance & Risk Specialist |
      +------------------------------+                  +------------------------------+
                     |                                                 |
                     +------------------------+------------------------+
                                              |
                                              v
                                +---------------------------+
                                |   Critic / Judge Agent    |
                                |  (Self-Correction Loop)   |
                                +---------------------------+
                                              |
                                              v
                                +---------------------------+
                                |  Human-in-the-Loop (HITL) |
                                |     Circuit Breaker       |
                                +---------------------------+
                                              |
                                              v
                                +---------------------------+
                                |   Eksekusi & Push Output  |
                                +---------------------------+
```

### Rincian Peran dan Tugas Masing-Masing Agen

#### A. Agen Orkestrator / Planner (*Triage Agent*)

- **Fungsi:** Menerima instruksi bisnis abstrak dari pengguna (contoh: *"Evaluasi portofolio dana pensiun kami di IDX/SGX terhadap batasan regulasi, cari peluang tax-loss harvesting, dan periksa apakah emiten yang tertekan mengalami akumulasi asing"*).
- **Mekanisme:** Dekomposisi tugas menjadi *Directed Acyclic Graph* (DAG) dan membagi sub-tugas secara paralel ke agen spesialis.

#### B. Agen Riset Kuantitatif (*Quantitative Research Agent*)

- **Fungsi:** Bertindak sebagai pencari dan pengolah data finansial mentah.
- **Mekanisme:** Berinteraksi langsung dengan MCP Server Sectors Financial API untuk mengeksekusi kueri bahasa alami, mengambil data *Broker Registry*, dan menyaring emiten.

#### C. Agen Kepatuhan & Risiko (*Compliance & Risk Specialist Agent*)

- **Fungsi:** Menganalisis kalkulasi finansial tingkat lanjut.
- **Mekanisme:**
  - Memeriksa persentase bobot portofolio terhadap aturan kepatuhan hukum.
  - Menghitung potensi simulasi *Tax-Loss Harvesting*.
  - Menghitung matriks *Value at Risk* (VaR) menggunakan formula:

    ```
    VaR(α) = μ + z(α) · σ
    ```

    di mana **μ** adalah rata-rata imbal hasil, **σ** adalah deviasi standar portofolio, dan **z(α)** adalah skor-z pada tingkat kepercayaan α.

#### D. Agen Kritikus (*Critic / Judge Agent*)

- **Fungsi:** Menjalankan mekanisme *Self-Correction Loop*.
- **Mekanisme:** Memeriksa draf laporan dari agen spesialis sebelum disajikan. Jika ditemukan ketidaksesuaian data (misal: total bobot portofolio tidak senilai 100%, atau kesalahan format JSON), draf dikembalikan ke agen pengeksekusi untuk direvisi secara mandiri.

---

## 3. Integrasi MCP & Ekosistem Sectors Financial API

Sistem memisahkan logika kecerdasan buatan dari lapisan data menggunakan **Model Context Protocol (MCP) Server** berbasis JSON-RPC 2.0.

### Pemanfaatan Titik Akhir (*Endpoints*) Sectors API

1. **Natural Language Query (NLQ) Screener** — Menerjemahkan instruksi pengguna menjadi parameter saringan teknis untuk memfilter saham BEI dan SGX.
2. **Broker Registry** — Menganalisis log transaksi bulanan/harian makelar untuk mendeteksi akumulasi modal asing (*foreign flow*) versus ritel.
3. **SGX News & Insider Filings** — Menarik rilis berita resmi SGX/IDX dan transaksi orang dalam (*insider trading*) guna analisis kepatuhan.
4. **Top Gainers / Losers & Aggregates** — Memantau anomali pergerakan sektoral secara *real-time*.

### Integrasi Omnichannel (n8n Webhook)

Hasil sintesis tidak hanya ditampilkan di antarmuka konsol, tetapi disalurkan melalui platform otomatisasi n8n:

- **Telegram / Slack Bot** — Mengirimkan ringkasan eksekutif dan notifikasi otorisasi.
- **Notion / Postgres Chat Memory** — Menyimpan log jejak keputusan agen (*trace logs*) dan memori analisis sejarah portofolio.

---

## 4. Keamanan Zero Trust & Protokol Human-in-the-Loop (HITL)

Untuk memastikan keandalan tingkat perusahaan dan mengamankan skor pada metrik *Usability* dan *Execution Quality*, sistem menerapkan dua pilar keamanan:

### Postur Keamanan Zero Trust

- **Security Token Service (STS) & JWT** — Pemanggilan antar-agen tidak menggunakan kunci API statis, melainkan tiket otorisasi JWT (*JSON Web Tokens*) berumur pendek (kurang dari 40 milidetik).
- **Validasi Skema Pydantic** — Setiap data masukan dan keluaran dari LLM ke fungsi alat MCP divalidasi menggunakan Pydantic untuk mencegah serangan *Command Injection* dan *Tool Poisoning*.

### Protokol Human-in-the-Loop (HITL)

Agen diberikan otonomi penuh untuk melakukan operasi pembacaan dan analisis (*read-only operations*). Namun, ketika agen menyusun usulan tindakan yang memutasi data (*write operations* — seperti mereset portofolio atau mengubah data di basis data), sistem mengaktifkan **Circuit Breaker**:

1. Eksekusi otomatis dibekukan sementara.
2. Webhook otorisasi dikirimkan ke manajer investasi melalui Telegram/Slack.
3. Tindakan baru dilanjutkan setelah penyelia manusia menekan tombol *"Approve"*.

---

## 5. Simulasi Alur Kerja End-to-End (Skenario Riil)

| Langkah | Komponen Pengeksekusi | Aksi Operasional | Output Sistem |
|---|---|---|---|
| **Langkah 1** | Pengguna | Menginput kueri: *"Audit portofolio saham BEI & SGX kami. Cari saham tertekan yang mengalami akumulasi asing, dan susun usulan tax-loss harvesting."* | Pesan masukan terdaftar di sistem. |
| **Langkah 2** | Agen Orkestrator | Memecah kueri menjadi 3 sub-tugas: (1) Screener Sectors, (2) Broker Registry, (3) Analisis Pajak. | Rencana eksekusi DAG. |
| **Langkah 3** | Agen Kuantitatif | Memanggil MCP Tool `nlq_screener` dan `broker_registry` Sectors API secara paralel. | Payload JSON data emiten terfilter dan log akumulasi broker. |
| **Langkah 4** | Agen Kepatuhan | Menghitung rasio VaR dan potensi penghematan pajak melalui *Tax-Loss Harvesting*. | Draf kalkulasi kuantitatif. |
| **Langkah 5** | Agen Kritikus | Memeriksa validitas angka dan kepatuhan format draf. Jika valid, diteruskan. | Draf laporan terverifikasi. |
| **Langkah 6** | Sirkuit HITL | Menghentikan otomatisasi; mengirimkan pesan persetujuan ke Telegram Manajer Investasi. | Notifikasi tombol *"Approve/Reject"*. |
| **Langkah 7** | Manajer Manusia | Menekan tombol *"Approve"* pada Telegram. | Otorisasi dikirim via Webhook n8n. |
| **Langkah 8** | Agen Eksekusi | Memperbarui basis data portofolio persisten dan menerbitkan laporan PDF/Notion. | Laporan akhir terpublikasi. |

---

## 6. Pemetaan Terhadap Rubrik Penilaian Hackathon

| Kriteria Penilaian | Bobot | Bukti Penerapan Pada Ide Ini |
|---|---|---|
| **Agentic Depth** | 25% – 30% | Memilih arsitektur 4 agen terspesialisasi (Planner, Quant, Risk, Critic) dengan alur komunikasi asinkron dan *self-correction loop*. |
| **Kualitas Eksekusi & Reliabilitas Teknis** | 20% – 25% | Menggunakan standar MCP berbasis JSON-RPC 2.0, validasi Pydantic, dan arsitektur hibrida (SLM untuk perutean, LLM untuk sintesis) untuk latensi sub-detik. |
| **Dampak Bisnis & Keselarasan Misi** | 20% | Menyelesaikan masalah nyata institusi keuangan (otomatisasi audit kepatuhan & optimasi pajak di BEI/SGX) dengan estimasi penghematan waktu operasional hingga 90%. |
| **Usability & Human-in-the-Loop** | 15% – 20% | Menyediakan visibilitas *trace logs* secara interaktif dan menerapkan *Circuit Breaker* HITL berbasis otorisasi Webhook. |
| **Inovasi & Presentasi** | 10% – 15% | Eksplorasi kombinasi data *Broker Registry* dan *NLQ Screener* Sectors API yang dipresentasikan melalui *live demo execution* dinamis. |

---

## 7. Kesimpulan dan Nilai Unggul

Ide **AI Portfolio Compliance & Investment Research Engine** merupakan pilihan paling strategis untuk dikerjakan dalam Sectors Hackathon. Ide ini tidak hanya menonjolkan kecanggihan teknis di ruang mesin (multi-agen, MCP, *Zero Trust*), tetapi juga mengeksploitasi keunggulan utama Sectors Financial API sebagai penyedia data pasar finansial kawasan regional secara menyeluruh. Dengan menyelesaikan alur kerja bisnis secara utuh dari hulu ke hilir, solusi ini siap memikat dewan juri kelas perusahaan.
