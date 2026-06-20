---

* pitch video
* live demo
* atau slide narasi teknis

Formatnya: **Attack → Failure → Recovery → Proof**

---

# 🎬 SSPA CALL FOR CODE DEMO SCRIPT

## 🧪 “Survivor Identity Resilience Under Adversarial Conditions”

---

# 🟡 SCENE 1 — NORMAL OPERATION (BASELINE STATE)

📍 *Lokasi: Field NGO Mobile Unit (offline-first device)*

Seorang petugas kemanusiaan mengakses SSPA Mobile Edge App.

```text
[STATE]
✔ Survivor Identity Vault ACTIVE
✔ Evidence Capture ENABLED
✔ Local Hash Store RUNNING
✔ Network: INTERMITTENT
```

📲 Event terjadi:

* Survivor testimony recorded
* Photo evidence captured
* Audio statement stored locally

```text
ACTION:
→ SHA-256 hash generated locally
→ Entry appended to local immutable log
→ Pending sync queue created
```

📌 Status:

> “Data secured locally. Awaiting ledger anchoring.”

---

# 🔴 SCENE 2 — ATTACK EVENT (DEVICE SEIZURE SIMULATION)

📍 *Checkpoint Border / Conflict Zone*

Simulasi ancaman:

```text
⚠️ THREAT DETECTED: DEVICE SEIZURE
Actor: Hostile authority
Action: Physical confiscation of device
```

Petugas dipaksa membuka aplikasi.

---

## 💥 SYSTEM RESPONSE (AUTOMATIC PROTECTION)

```text
SECURITY LAYER ACTIVATED:

✔ Duress Mode Triggered
✔ Vault encryption remains intact (AES-256-GCM)
✔ Private keys NOT exposed
✔ Identity metadata not accessible in plaintext
```

📲 UI yang muncul ke attacker:

* Data dummy / sanitized interface
* No real survivor data exposed

---

## 🧠 BACKGROUND SYSTEM ACTION

```text
→ Panic lock engaged
→ Encryption keys remain device-bound
→ No remote decryption possible
→ Silent integrity alert sent (if relay available)
```

📌 Outcome:

> Device compromised physically, but data remains cryptographically inaccessible.

---

# 🔴 SCENE 3 — NETWORK FAILURE (REGIONAL ISOLATION)

📍 *Area: Network blackout zone (conflict region)*

```text
⚠️ EVENT: FULL NETWORK PARTITION
Cause: Infrastructure shutdown / censorship
Status: OFFLINE REGION
```

---

## ⚙️ SYSTEM BEHAVIOR

SSPA continues operating in isolation mode:

```text
✔ Local log continues (append-only CRDT)
✔ Evidence still hashed locally
✔ No external dependency required
✔ No data rollback possible
```

📌 Key principle:

> “No internet ≠ no evidence capture”

---

# 🔵 SCENE 4 — RECOVERY PHASE (RECONNECTION EVENT)

📍 *3 days later — NGO relay node restored*

Connection re-established via:

* NGO Local Relay Hub
* Store-and-forward sync
* Mesh network bridge

---

## 🔄 SYNC PROCESS

```text
INITIATING RECONCILIATION ENGINE...

→ Local logs uploaded
→ Merkle tree constructed
→ Hash integrity verification started
→ Conflict-free resolution (CRDT + vector clock)
```

---

## 🧾 RESULT

```text
✔ 0 data loss
✔ 0 duplication conflict
✔ 100% integrity preserved
✔ Timeline preserved cryptographically
```

---

# 🟢 SCENE 5 — LEDGER ANCHOR CONFIRMATION

📍 *SSPA Core Ledger (EVM Settlement Layer)*

```text
BLOCK ANCHOR SUCCESSFUL
---------------------------------
Merkle Root: 0x8f3a9c7d...
Validator Signature: VALID
Batch Size: 150 records
Status: FINALIZED
```

---

## 🔐 FINAL STATE

* Survivor identity restored across devices
* Evidence permanently anchored
* Audit trail verifiable on-chain
* No PII exposed on-chain

---

# 🟣 FINAL SCENE — PROOF OF VALUE

```text
SSPA OUTCOME METRICS:
✔ Identity preserved under coercion
✔ Evidence survived offline destruction
✔ System recovered without central authority
✔ Legal-grade audit trail generated
```

---

# 🧠 CLOSING STATEMENT (FOR JURY)

> “Even under device seizure, network collapse, and hostile environments, SSPA preserves what matters most: identity, evidence, and the possibility of justice.”

---

# 🚀 WHY THIS DEMO WORKS (UNTUK JURI IBM)

Ini sengaja didesain untuk menunjukkan 4 hal yang juri Call for Code cari:

### 1. Real-world stress testing

* seizure
* censorship
* offline disaster

### 2. System resilience

* offline-first
* append-only logs
* CRDT reconciliation

### 3. Cryptographic credibility

* SHA-256
* AES-256-GCM
* Merkle root anchoring

### 4. Human impact framing

* survivor identity protection
* evidence preservation
* justice continuit

