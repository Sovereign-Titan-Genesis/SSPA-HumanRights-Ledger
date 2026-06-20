------------------------------
## ARCHITECTURE.md## Overview
Project: SSPA Human Rights Ledger
Purpose: Provide a concise, audit-ready technical specification that maps the survivor-centered mission to implementable system components. This document is intentionally pragmatic: minimal on-chain footprint, offline-first resilience, cryptographic auditability, and operational playbooks for field recovery [3.2].
Design Principles

* Survivor-Centered: PII is never stored on-chain; recovery flows prioritize survivor consent and physical safety [3.2].
* Minimal On-Chain Footprint: Only Merkle anchors and governance metadata are settled on the EVM-compatible L2 layer (chainId: 8080) [3.2].
* Offline-First Resilience: Employs store-and-forward replication, opportunistic mesh transport, and anti-entropy reconciliation [3.2].
* Audit-Ready: Integrates commit SHAs, cryptographic timestamps, and Merkle proofs to link field artifacts to the repository [3.2].

------------------------------
## Architecture Diagram (Mermaid)

flowchart TD
    classDef edgeNode fill:#050816,stroke:#ff3366,stroke-width:2px,color:#d7f9ff;
    classDef relayNode fill:#081b3a,stroke:#0f62fe,stroke-width:2px,color:#d7f9ff;
    classDef validatorNode fill:#050816,stroke:#00e5ff,stroke-width:2px,color:#d7f9ff;
    classDef storageLayer fill:#03050f,stroke:#00ff99,stroke-width:2px,stroke-dasharray: 5 5,color:#00ff99;

    subgraph Layer_1 [Layer 1: Mobile Edge Node - Bandwidth 0 Kbps]
        A[Field Agent / Incident Capture] -->|Raw Evidence| B(Client-Side Hashing Engine)
        B -->|SHA-256 Hash| C{Survivor Identity Vault}
        C -->|Duress Credential Pin| D[Cryptographic Shredding Protocol]
        C -->|X25519 Encrypted Payload| E[Local Append-Only Log]
        D -->|Destroy Encryption Keys| F[Unreadable Random Noise Blob]
    end
    class A,B,C,D,E,F edgeNode;

    subgraph Layer_2 [Layer 2: Local Relay Node - Intermittent Transport]
        E -->|Store-&-Forward Queue| G[Wi-Fi Direct / Bluetooth 5.0 Mesh]
        G --> H[Local Relay Hub / NGO Field Office]
        H -->|Lamport Timestamps| I[Vector Clock Sync Database]
    end
    class G,H,I relayNode;

    subgraph Layer_3 [Layer 3: SSPA Settlement & Validator Network]
        I -->|LEO Satellite Link / Web Access| J(Anti-Entropy Reconciliation)
        J --> K[Validator Nodes Consortium]
        K -->|Groth16 / PLONK Abstraction| L{ZK Proof Identity Verification}
        K -->|Multi-Jurisdictional Agreement| M[Threshold Signature Identity Rebind]
    end
    class J,K,L,M validatorNode;

    subgraph Layer_4 [Layer 4: Persistence & Settlement Layer]
        M -->|1. On-Chain Anchoring| N[EVM L2 Settlement Layer]
        N -->|Periodic State Emissions| O[Merkle Root Hash Anchor Store]
        M -->|2. Off-Chain Decentralized Storage| P[Encrypted Sharded Blob Storage]
        P -->|Survivor Multi-Sig Consent| Q[International Court Judicial API]
    end
    class N,O,P,Q storageLayer;

    style Layer_1 fill:#050816,stroke:#ff3366,stroke-width:1px;
    style Layer_2 fill:#050816,stroke:#0f62fe,stroke-width:1px;
    style Layer_3 fill:#050816,stroke:#00e5ff,stroke-width:1px;
    style Layer_4 fill:#03050f,stroke:#00ff99,stroke-width:1px;

------------------------------
## Components Specification## 1. Survivor Identity Vault (SIV)

* Purpose: Client-side encrypted vault isolating private keys, identity metadata pointers, and encrypted pointers to evidence artifacts [3.2].
* Crypto Engine: X25519 / XChaCha20-Poly1305 for payload encryption; Ed25519 for signatures [3.2].
* Duress System: Ephemeral credentials trigger a local Cryptographic Shredding Protocol, immediately invalidating local decryptability while preserving external ledger integrity [3.2].
* Interface:
* siv.createIdentity(metadata, entropy) $\rightarrow$ identityId, publicKey
   * siv.sign(identityId, payload) $\rightarrow$ signature
   * siv.exportRebindShare(identityId, guardianList) $\rightarrow$ encryptedShares

## 2. Local Evidence Hash Store (EHS)

* Purpose: Appends per-entry SHA-256 hashes and metadata to produce tamper-evident Merkle leaves [3.2].
* Sync Strategy: Leverages Lamport timestamps and vector clock hybrid ordering to prevent local rollback and history rewriting during network isolation [3.2].
* Interface:
* ehs.append(entry) $\rightarrow$ leafHash
   * ehs.buildMerkleRoot() $\rightarrow$ merkleRoot, proofs

## 3. Local Relay Node (LR)

* Purpose: Local aggregator and store-and-forward routing hub for offline crisis zones [3.2].
* Security: Encrypted-at-rest state using zero-dependency vector clocks to handle opportunistic replication over multi-jurisdictional edge devices [3.2].

## 4. Anchor Layer (EVM-Compatible L2)

* Purpose: Settlement engine for batching roots to minimize transaction fees and maintain a zero-PII on-chain footprint [3.2].
* Format Structure:

{
  "$schema": "https://quorumstate.international",
  "chainId": 8080,
  "merkleRoot": "0x8f3a9c7d4b2e1f6d9a0c5b7e2d4f8a9b3c7d1e5f6a8b9c0d1e2f3a4b5c6d7e8f",
  "timestamp": 1774056000,
  "commitSHA": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0",
  "version": "v1.0-stable"
}

## 5. Validator Nodes & Threshold Signing

* Purpose: Multi-jurisdictional validators executing threshold signatures ($t$-of-$n$) for critical actions like identity binding [3.2].
* Identity Verification: Integrates a ZK-proof system abstraction layer (Groth16/PLONK compatible) to check identity ownership without revealing sensitive demographics [3.2].

## 6. Governance & Treasury Module

* Purpose: Transparent management of genesis allocations, community treasury funds, and anti-speculation vesting structures [3.2].
* Vesting Guardrails: Employs the SSPATokenVesting smart contract, relying on static allocation calculations (totalAllocation) to eliminate dynamic balance vulnerabilities [3.2].

------------------------------
## Operational Runbook (Attack $\rightarrow$ Failure $\rightarrow$ Recovery)## Phase 1: The Attack (Device Seizure)

* Field Scenario: An field agent's device is physically seized by an adversary in a disconnected territory [3.2].
* System Action: Entering the Duress Credential Pin fires the cryptographic shredding mechanism, destroying local decryption keys [3.2]. The adversary gains access to nothing but unreadable random noise blobs [3.2].

## Phase 2: The Failure (Network Partition)

* Field Scenario: A region is subjected to a complete internet blackout for 72 hours [3.2].
* System Action: Jumps into Local Sync State [3.2]. Local edge nodes safely sign and register hash logs over the opportunistic ad-hoc mesh transport, keeping data cached cleanly on local relays [3.2].

## Phase 3: The Recovery (Rebind & Settlement)

* Field Scenario: Relays secure a 60-second satellite link; simultaneously, a survivor requests identity recovery [3.2].
* System Action:
1. Relays complete an anti-entropy handshake, flushing batched Merkle roots directly onto the L2 settlement layer [3.2].
   2. The survivor passes partial mnemonics and ZK-proof certificates to the local node [3.2].
   3. Multi-jurisdictional validator nodes verify attestations and issue threshold signatures, safely binding the identity to a new device while revoking the compromised instance globally [3.2].

------------------------------
## Threats Mitigation Surface

* Physical Seizure & Coercion: Mitigated by client-side asymmetric key isolation, panic shredding, and guardian attestation patterns [3.2].
* Censorship & Disconnection: Countered by store-and-forward mesh replication and vector clock anti-entropy handshakes [3.2].
* Data Tampering: Prevented by continuous Ed25519 payload signatures, localized append-only logs, and on-chain ledger anchors [3.2].
* Byzantine Validator Compromise: Managed by multi-jurisdictional consensus design paired with objective automated slashing contract rule-checking [3.2].

------------------------------
## Ethical & Legal Safeguards Layer
While structural data remains cryptographically immutable to thwart malicious erasure by adversaries, individual privacy control is strictly preserved [3.2]. If a survivor exercises their right to be forgotten (complying with international legal frameworks like GDPR), the system triggers Cryptographic Shredding on the user's specific access components [3.2]. This invalidates decryptability across all storage relays, rendering the payload unreadable random noise while maintaining the holistic structural integrity of the public chain logs [3.2].
------------------------------

