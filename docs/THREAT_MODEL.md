# 🛡️ SSPA Security Threat Modeling & Mitigation Analysis (v1.0.0)

Dokumen ini memetakan permukaan serangan (*threat surface*), skenario eksploitasi musuh di lapangan krisis, serta kontrol mitigasi taktis yang diterapkan untuk mengamankan data kemanusiaan pada sistem **SSPA Human Rights Ledger**.

---

## 1. Matriks Penilaian Risiko & Prioritas Ancaman

Analisis ini menggunakan metodologi penilaian berbasis dampak dampak operasional nyata di zona konflik dan krisis kemanusiaan.

| ID Ancaman | Deskripsi Ancaman | Kemungkinan (Likelihood) | Dampak (Impact) | Tingkat Risiko | Kontrol Mitigasi Utama |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **THR-001** | Penyitaan perangkat fisik (*Device Seizure*) & intimidasi untuk membuka kunci enkripsi. | Tinggi | Kritis | **SANGAT TINGGI** | Enkripsi end-to-end sisi klien + Penghancuran Kunci Darurat (*Panic Shredding*). |
| **THR-002** | Isolasi wilayah jaringan (*Network Partition*) oleh rezim lokal untuk menghambat sinkronisasi bukti. | Tinggi | Medium | **TINGGI** | Protokol replikasi oportunistik *store-and-forward* via *mesh network*. |
| **THR-003** | Manipulasi data log lokal (*Local Rollback Attack*) sebelum penambatan jangkar kriptografis. | Medium | Tinggi | **TINGGI** | Desain arsitektur log *append-only* menggunakan algoritma pembuktian CRDT. |
| **THR-004** | Pengambilalihan node validator (*Validator Capture*) oleh entitas jahat untuk melakukan sensor bukti. | Rendah | Kritis | **TINGGI** | Syarat konsensus hibrida multi-jurisdiksi global & sanksi pemotongan hak (*slashing*). |

---

## 2. Skenario Serangan Detail & Mitigasi Teknis

### 2.1 Skenario Serangan THR-001: Device Seizure + Coercion
*   **Deskripsi Skenario**: Otoritas lokal atau musuh di perbatasan menyita ponsel milik penyintas secara paksa dan memaksa pemilik perangkat untuk membuka data SIV (Survivor Identity Vault) demi melacak riwayat pelaporan bukti kemanusiaan.
*   **Mitigasi Teknis & Operasional**:
    *   **Pemisahan PII**: Tidak ada data mentah demografis korban yang disimpan dalam bentuk *plaintext*. Seluruh payload dienkripsi secara asimetris menggunakan kunci `X25519`.
    *   **Fitur Duress Password**: Aplikasi menyediakan fitur kata sandi palsu darurat (*duress credential*). Jika dimasukkan, aplikasi akan menampilkan antarmuka data simulasi umum yang aman, sementara di latar belakang sistem langsung mengunci total vault asli dan mengirimkan sinyal distres ke *Local Relay Node* terdekat jika terdeteksi jaringan intermiten.

### 2.2 Skenario Serangan THR-002: Partitioned Region Isolation
*   **Deskripsi Skenario**: Suatu wilayah konflik mengalami pemadaman internet total secara sengaja. Musuh mencoba memanipulasi riwayat kejadian lokal dan memaksa *Relay Node* di wilayah tersebut menerima data palsu sebelum terhubung kembali ke jaringan internet global.
*   **Mitigasi Teknis & Operasional**:
    *   Setiap entri bukti kemanusiaan yang dicatat oleh *Edge Node* lapangan wajib menyertakan stempel waktu bertingkat yang ditandatangani secara kriptografis oleh perangkat asal.
    *   Saat terjadi isolasi jaringan, sistem menerapkan status *Local Sync State*. Ketika koneksi internet global pulih (baik melalui jaringan kurir fisik perangkat keras atau tautan satelit orbit rendah LEO), node global akan menjalankan rekonsiliasi berbasis pohon Merkle (*anti-entropy reconciliation*) untuk mendeteksi dan menolak data log palsu yang tidak sesuai dengan tanda tangan kronologis perangkat asal.

---

## 3. Playbook Respon Insiden Teknis (Incident Runbook)

### 3.1 Runbook Mass-Validator Censorship (Deteksi Intervensi Mayoritas Node)
Jika terdeteksi bahwa lebih dari 33% *Validator Node* menolak untuk memvalidasi *Merkle Root Anchor* kemanusiaan baru dari wilayah tertentu tanpa alasan teknis yang valid:

```text
 [ DETEKSI ] ──► Adanya penurunan drastis pada tingkat konfirmasi blok SSPA.
                       │
                       ▼
 [ ISOLASI ]  ──► Smart contract otomatis memicu status pembekuan darurat (Circuit Breaker).
                       │
                       ▼
 [ RESPON ]   ──► Mengaktifkan mekanisme konsensus darurat berbasis penunjukan 
                  Validator Cadangan non-terdampak (Multi-Jurisdictional NGOs Alliance).
                       │
                       ▼
 [ FORENSIK ] ──► Mengunggah log tanda tangan penolakan validator nakal ke GitHub 
                  sebagai bukti audit terbuka, diikuti pemotongan hak validator (Slashing).
```

---

## 4. Kebijakan Forensik & Rencana Pengumpulan Bukti Struktural

Untuk memastikan keabsahan data kemanusiaan yang dikumpulkan SSPA di hadapan pengadilan internasional, ekosistem menetapkan aturan pengumpulan bukti digital (*Forensic Evidence Collection Plan*) sebagai berikut:

1.  **Artefak Kunci yang Wajib Dikoleksi**:
    *   Hash penambatan jangkar global (`On-Chain Merkle Root`).
    *   Log transaksi lokal terenkripsi bersertifikat tanda tangan kurva `Ed25519`.
    *   Catatan riwayat commit SHA repositori GitHub yang mengikat versi logika smart contract saat pembentukan blok genesis dilakukan.
2.  **Kebijakan Retensi (Data Retention Policy)**:
    *   Metadata pembuktian (Hash dan Anchor) disimpan secara **abadi** di dalam struktur rantai terdistribusi.
    *   Payload data demografis personal sensitif penyintas dapat diatur untuk hancur secara otomatis (*automatic cryptographic expiration*) setelah status hukum pengungsi disahkan oleh badan hukum internasional PBB atau atas permintaan mandiri penyintas demi keselamatan jangka panjang mereka.


# 🛡️ SSPA Security Threat Modeling & Mitigation Analysis (v1.0.0)

Dokumen ini memetakan permukaan serangan (*threat surface*), skenario eksploitasi musuh di lapangan krisis, serta kontrol mitigasi taktis yang diterapkan untuk mengamankan data kemanusiaan dan aset treasury pada sistem **SSPA Human Rights Ledger**.

---

## 1. Matriks Penilaian Risiko & Prioritas Ancaman

Analisis ini menggunakan metodologi penilaian berbasis dampak operasional nyata di zona konflik dan krisis kemanusiaan berskala global.

| ID Ancaman | Deskripsi Ancaman | Kemungkinan (Likelihood) | Dampak (Impact) | Tingkat Risiko | Kontrol Mitigasi Utama |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **THR-001** | Penyitaan perangkat fisik (*Device Seizure*) & intimidasi untuk membuka kunci enkripsi. | Tinggi | Kritis | **SANGAT TINGGI** | Enkripsi end-to-end sisi klien + Penghancuran Kunci Darurat (*Panic Shredding*). |
| **THR-002** | Isolasi wilayah jaringan (*Network Partition*) oleh rezim lokal untuk menghambat sinkronisasi bukti. | Tinggi | Medium | **TINGGI** | Protokol replikasi oportunistik *store-and-forward* via *mesh network* dengan sinkronisasi hibrida *vector clocks*. |
| **THR-003** | Manipulasi data log lokal (*Local Rollback Attack*) sebelum penambatan jangkar kriptografis. | Medium | Tinggi | **TINGGI** | Desain arsitektur log *append-only* menggunakan algoritma pembuktian CRDT dan *Lamport timestamps*. |
| **THR-004** | Pengambilalihan node validator (*Byzantine Validator Compromise*) oleh entitas jahat untuk melakukan sensor bukti. | Rendah | Kritis | **TINGGI** | Syarat konsensus hibrida multi-jurisdiksi global & sanksi pemotongan hak (*slashing*). |

---

## 2. Skenario Serangan Detail & Mitigasi Teknis

### 2.1 Skenario Serangan THR-001: Device Seizure + Coercion
*   **Deskripsi Skenario**: Otoritas lokal atau musuh di perbatasan menyita ponsel milik penyintas atau agen lapangan secara paksa dan memaksa pemilik perangkat untuk membuka data SIV (Survivor Identity Vault) demi melacak riwayat pelaporan bukti kemanusiaan.
*   **Asumsi Keamanan (*Security Assumption*)**: Sistem berasumsi bahwa penyerang memiliki akses fisik penuh ke perangkat (*Assumes adversary may have full physical device access*).
*   **Mitigasi Teknis & Operasional**:
    *   **Pemisahan PII**: Tidak ada data mentah demografis korban yang disimpan dalam bentuk *plaintext*. Seluruh payload dienkripsi secara asimetris menggunakan kunci `X25519`.
    *   **Fitur Duress Password**: Aplikasi menyediakan fitur kata sandi palsu darurat (*duress credential*). Jika dimasukkan, aplikasi akan menampilkan antarmuka data simulasi umum yang aman, sementara di latar belakang sistem langsung mengunci total vault asli dan mengeksekusi *Cryptographic Shredding* yang menghancurkan kunci dekripsi lokal tanpa merusak integritas ledger global.

### 2.2 Skenario Serangan THR-002: Partitioned Region Isolation (Internet Blackout)
*   **Deskripsi Skenario**: Suatu wilayah konflik mengalami pemadaman internet total secara sengaja. Musuh mencoba memanipulasi riwayat kejadian lokal dan memaksa *Relay Node* di wilayah tersebut menerima data palsu sebelum terhubung kembali ke jaringan internet global.
*   **Mitigasi Teknis & Operasional**:
    *   Setiap entri bukti kemanusiaan yang dicatat oleh *Edge Node* lapangan wajib menyertakan stempel waktu bertingkat yang ditandatangani secara kriptografis oleh perangkat asal.
    *   Saat terjadi isolasi jaringan, sistem menerapkan status *Local Sync State*. Untuk menjamin konsistensi urutan data tanpa bergantung pada jam internet, sistem menggunakan rekonsiliasi berbasis pohon Merkle (*anti-entropy synchronization*) dengan urutan *vector clocks* bertingkat untuk mendeteksi dan menolak data log palsu yang disisipkan oleh penyusup.

---

## 3. Playbook Respon Insiden Teknis (Incident Runbook)

### 3.1 Runbook Byzantine Validator Compromise (Deteksi Intervensi Mayoritas Node)
Jika terdeteksi bahwa lebih dari 33% *Validator Node* melakukan kolusi atau menolak untuk memvalidasi *Merkle Root Anchor* kemanusiaan baru dari wilayah tertentu tanpa alasan teknis yang valid:

```text
 [ DETEKSI ] ──► Adanya penurunan drastis pada tingkat konfirmasi blok SSPA.
                       │
                       ▼
 [ ISOLASI ]  ──► Smart contract otomatis memicu status pembekuan darurat (Circuit Breaker).
                       │
                       ▼
 [ RESPON ]   ──► Mengaktifkan mekanisme konsensus darurat berbasis penunjukan 
                  Validator Cadangan non-terdampak (Multi-Jurisdictional NGOs Alliance).
                       │
                       ▼
 [ FORENSIK ] ──► Mengunggah log tanda tangan penolakan validator nakal ke GitHub 
                  sebagai bukti audit terbuka, diikuti pemotongan hak validator (Slashing).
```

---

## 4. Kebijakan Forensik & Rencana Pengumpulan Bukti Struktural

Untuk memastikan keabsahan data kemanusiaan yang dikumpulkan SSPA di hadapan pengadilan internasional atau audit hukum korporasi, ekosistem menetapkan aturan pengumpulan bukti digital sebagai berikut:

1.  **Artefak Kunci yang Wajib Dikoleksi**:
    *   Hash penambatan jangkar global (`On-Chain Merkle Root`).
    *   Log transaksi lokal terenkripsi bersertifikat tanda tangan kurva `Ed25519`.
    *   Catatan riwayat commit SHA repositori GitHub yang mengikat versi logika smart contract saat pembentukan blok genesis dilakukan.
2.  **Kebijakan Retensi (Data Retention Policy)**:
    *   Metadata pembuktian (Hash dan Anchor) disimpan secara **abadi** di dalam struktur rantai terdistribusi.
    *   **Ethical Redaction Workflow**: Sesuai prinsip kedaulatan data penyintas, payload data demografis personal sensitif penyintas akan hancur secara otomatis (*automatic cryptographic expiration*) melalui mekanisme penghancuran kunci jika penyintas memicu hak untuk dilupakan, mengubah isi payload menjadi *unreadable random noise* demi keselamatan jangka panjang mereka tanpa merusak validitas rantai blok.

---
*© 2026 QuorumState International Network & SSPA Project. Dokumen keamanan tingkat tinggi untuk perlindungan aset dan infrastruktur kemanusiaan global.*

