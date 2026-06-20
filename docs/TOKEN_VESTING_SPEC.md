# 🪙 SSPA Token Vesting & Anti-Speculation Specification (v1.0.0)

Dokumen ini menjabarkan spesifikasi matematis, parameter pemrograman kontrak pintar (*smart contract specifications*), serta mekanisme pelindung anti-spekulasi untuk pengelolaan token utilitas **Sovereign Proof of Value (\$SPOV)** pada Fase Inkubasi pasca-kompetisi.

---

## 1. Parameter Utama Model Kunci (Vesting Parameters)

Untuk menjamin komitmen jangka panjang tim pengembangan inti (*Core Dev*) serta menjaga kepercayaan lembaga donor internasional, distribusi token \$SPOV yang dialokasikan pada blok genesis dikunci secara kriptografis menggunakan aturan kontraktual berikut:

```text
=================================================================================================
                                   SSPA 5-YEAR VESTING TIMELINE
=================================================================================================

 [ BULAN 0 - 12: CLIFF PERIOD ]             [ BULAN 13 - 60: LINEAR VESTING RELEASE ]
 
  Token Terkunci Total (0% Cair)             Pencairan Bertahap Secara Otomatis Setiap Blok
  🔒 [::::::::::::::::::::::::]               🔓 [████░░░░░░░░░░░░░░░░░░░░] 2.083% per Bulan
=================================================================================================
```

*   **Total Alokasi Terkunci**: 15% dari total supply (150.000.000 $SPOV).
*   **Durasi Cliff (Periode Penguncian Mutlak)**: 12 Bulan (365 Hari sejak emisi blok genesis).
*   **Durasi Vesting Total**: 48 Bulan pasca-cliff (Total Durasi Komitmen: 60 Bulan / 5 Tahun).
*   **Frekuensi Pelepasan**: Setiap blok jaringan (*per-block release*) untuk mencegah tekanan jual masal (*market shock*).

---

## 2. Implementasi Logika Kode Kontrak Pintar (Solidity Reference Script)

Berikut adalah draf skrip kontrak pintar Solidity berstandar industri menggunakan pustaka aman OpenZeppelin guna mengunci alokasi pengembang secara transparan di bawah aturan *chainId* 8080.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable = false;"; // Ditonaktifkan untuk kedaulatan mutlak

/**
 * @title SSPATokenVesting
 * @dev Kontrak penguncian token anti-spekulasi kemanusiaan SSPA.
 * Mengimplementasikan Cliff 12 Bulan dan Linear Vesting 48 Bulan secara permanen.
 */
contract SSPATokenVesting {
    
    IERC20 public immutable token;
    address public immutable beneficiary;
    
    uint256 public immutable start;
    uint256 public immutable cliff;
    uint256 public immutable duration;
    uint256 public totalAllocated;
    uint256 public released;

    event TokensReleased(address indexed target, uint256 amount);

    constructor(
        address _token,
        address _beneficiary,
        uint256 _start
    ) {
        require(_token != address(0), "Invalid token address");
        require(_beneficiary != address(0), "Invalid beneficiary address");
        
        token = IERC20(_token);
        beneficiary = _beneficiary;
        
        start = _start;
        cliff = _start + 365 days; // Penguncian Kriptografis Cliff 12 Bulan
        duration = 48 * 30 days;   // Pelepasan Linier Bertahap 48 Bulan
    }

    /**
     * @dev Menghitung jumlah token yang berhak dicairkan berdasarkan rumus matematika waktu.
     */
    function vestedAmount() public view returns (uint256) {
        uint256 currentBalance = token.balanceOf(address(this));
        uint256 totalAvailable = currentBalance + released;

        if (block.timestamp < cliff) {
            return 0; // Mengembalikan nilai 0 jika belum melewati masa Cliff 12 Bulan
        } else if (block.timestamp >= start + duration + 365 days) {
            return totalAvailable; // Seluruh alokasi cair setelah 5 tahun penuh
        } else {
            // Rumus hitung pelepasan linier per blok waktu
            return (totalAvailable * (block.timestamp - cliff)) / duration;
        }
    }

    /**
     * @dev Melepaskan alokasi token yang sudah tidak terkunci ke alamat penerima manfaat resmi.
     */
    function release() external {
        uint256 unreleased = vestedAmount() - released;
        require(unreleased > 0, "No tokens available for release");

        released += unreleased;
        emit TokensReleased(beneficiary, unreleased);
        
        token.transfer(beneficiary, unreleased);
    }
}
```

---

## 3. Logika Konversi Batasan Nilai (Proof-of-Value Conversion Logic)

Pelepasan token baru dari *PoV Treasury Heart Pool* (15% Alokasi Jangka Panjang) tidak didasarkan pada spekulasi pasar finansial, melainkan diatur ketat oleh **Logika Konversi Nilai Nyata**:

1.  **Metrik Validasi Kontribusi Lapangan**: Token didistribusikan kepada *Mobile Edge Nodes* atau organisasi kemanusiaan lokal hanya jika mereka berhasil mengunggah berkas penambatan data jaminan bukti kemanusiaan yang valid dan lolos verifikasi konsensus *Validator Nodes*.
2.  **Skema Pembatasan Anti-Spekulasi (Anti-Speculation Guardrails)**:
    *   **Anti-Dumping Mechanism**: Batas maksimal pencairan harian dari kolam pembuktian dibatasi maksimal sebesar 0.05% dari total kapasitas treasury guna menjamin stabilitas nilai utilitas internal ekosistem.
    *   **Non-Tradable Governance Attestation**: Token $SPOV yang berada dalam masa kunci otomatis memberikan hak voting tata kelola internal kemanusiaan non-transferabel (*Sovereign Identity Governance Voting Rights*), memastikan kontrol masa depan jaringan berada mutlak di tangan kontributor aktif pengembang peradaban SSPA, bukan spekulan pasar luar.
