# ⚛️ Quantum Experimentation

A hands-on repository for learning and experimenting with **Quantum Computing, Quantum Circuits, and Post-Quantum Cryptography (PQC)** through implementation.

The goal is to move beyond theory by building concepts from mathematical foundations upward, experimenting with established quantum-computing frameworks, running circuits on simulators and real IBM Quantum hardware, and exploring quantum-safe cryptographic primitives with Open Quantum Safe (OQS).

> **Learn → Implement → Experiment → Measure → Understand → Build**

---

## 📌 What is in this repository?

The repository currently contains four Jupyter notebooks covering three connected areas:

- **Qiskit** — constructing, simulating, visualizing, and executing quantum circuits.
- **Quantum Computer Builder** — implementing basic quantum-state and circuit operations from scratch with NumPy.
- **Post-Quantum Cryptography** — exploring KEMs and digital-signature schemes through liboqs.
- **Hybrid Cryptography** — combining ML-KEM, ML-DSA, and AES-256-GCM into an end-to-end experimental communication flow.

## 📂 Repository Structure

```text
Quantum-Experimentation/
│
├── Qiskit Implementations/
│   └── HelloQuantum.ipynb
│
├── Quantum Computer Builder/
│   └── Quantum Computer Builder.ipynb
│
├── PQC Algorithms/
│   ├── Basic PQC Algorithms.ipynb
│   └── Hybrid Cryptography.ipynb
│
└── README.md
```

---

# ⚛️ 1. Qiskit Implementations

## `HelloQuantum.ipynb`

This notebook is a practical introduction to quantum-circuit programming with **Qiskit**.

### Concepts explored

- Quantum and classical registers
- Quantum circuits
- Hadamard gate
- Controlled-X / CNOT gate
- Measurement
- Statevectors
- Measurement probabilities
- Circuit visualization
- Q-sphere visualization
- Histogram visualization
- Circuit transpilation
- Local simulation with Qiskit Aer
- Execution on IBM Quantum hardware

### Circuit experiment

```text
q0 ── H ──●── M
          │
q1 ───────X── M
```

The circuit is simulated with **Qiskit Aer** using 1024 shots, with the resulting measurement distribution visualized as a histogram. The notebook also obtains a statevector and visualizes it using a Q-sphere.

### IBM Quantum execution

The notebook demonstrates the workflow for:

1. Configuring an IBM Quantum Runtime account.
2. Discovering available backends.
3. Selecting an operational non-simulator backend.
4. Transpiling the circuit for that backend.
5. Submitting the circuit with the sampler.
6. Waiting for the job result.
7. Plotting the measured distribution.

The notebook intentionally contains placeholders rather than credentials:

```python
token='ENTER YOUR OWN API KEY'
instance='ENTER YOUR OWN INSTANCE NAME'
```

**Do not replace these placeholders with credentials before committing the notebook to a public repository.** Use environment variables or another secret-management mechanism instead.

---

# 🧮 2. Quantum Computer Builder

## `Quantum Computer Builder.ipynb`

This notebook experiments with building a small quantum-computing abstraction **from scratch using NumPy**, rather than relying entirely on Qiskit's circuit engine.

### Implemented concepts

- Computational-basis states
- Superposition
- State normalization
- Unitary-operator validation
- Pauli-X, Pauli-Y, Pauli-Z and Hadamard gates
- Measurement by probability sampling
- Tensor products
- Two-qubit basis states
- Bell states
- Inner products
- Two-qubit separability / entanglement testing
- Single-qubit operators on arbitrary qubits
- Controlled gates
- Multi-qubit circuit execution
- Repeated measurement experiments
- Manual SVG circuit visualization

The computational basis is represented as:

```text
|0⟩ = [1, 0]
|1⟩ = [0, 1]
```

A superposition is constructed as:

```text
|+⟩ = (|0⟩ + |1⟩) / √2
```

Measurement samples according to `P(i) = |αᵢ|²`.

The notebook explicitly constructs the four Bell states and includes a separability test based on the determinant of the reshaped two-qubit state.

### Custom circuit abstraction

The `QuantumCircuit` class currently supports:

- Arbitrary numbers of qubits.
- Single-qubit gates.
- `h()`, `x()`, `y()` and `z()` convenience methods.
- Controlled gates.
- Circuit execution.
- Repeated measurements.
- SVG circuit visualization.

The execution flow is:

```text
Circuit definition
       ↓
Operation list
       ↓
Operator construction
       ↓
Matrix application
       ↓
Final quantum state
       ↓
Measurement
```

> This is an educational simulator, not a replacement for a full-featured quantum SDK or hardware simulator.

---

# 🔐 3. Post-Quantum Cryptography

## `PQC Algorithms/Basic PQC Algorithms.ipynb`

This notebook explores **Post-Quantum Cryptography using liboqs-python**, backed by the Open Quantum Safe ecosystem.

The experiments inspect algorithm availability and parameters, test KEM and signature correctness, exercise tamper detection, and measure selected operations.

### OQS exploration

The recorded environment exposed **29 KEM mechanisms and 221 signature mechanisms**. These counts are installation/version dependent and should not be interpreted as permanent liboqs totals.

### KEM experiments

Families explored include:

- Classic McEliece
- Kyber
- ML-KEM
- NTRU
- NTRU-HRSS
- Streamlined NTRU Prime
- FrodoKEM

### ML-KEM

The notebook explicitly tests:

| Algorithm | Public Key | Ciphertext | Shared Secret |
|---|---:|---:|---:|
| ML-KEM-512 | 800 B | 768 B | 32 B |
| ML-KEM-768 | 1184 B | 1088 B | 32 B |
| ML-KEM-1024 | 1568 B | 1568 B | 32 B |

Each experiment performs key generation, encapsulation, decapsulation, and shared-secret comparison.

### Digital signatures

Signature families explored include:

- **ML-DSA**
- **SLH-DSA / SPHINCS+**
- **Falcon**
- Other signature mechanisms exposed by the installed liboqs version

ML-DSA experiments cover ML-DSA-44, ML-DSA-65 and ML-DSA-87, including signing, verification, tamper detection and benchmarking.

The SLH-DSA / SPHINCS+ experiments cover SHA-2 variants and test valid signatures, tampered messages, forged signatures and wrong verification keys. One detailed experiment uses `SPHINCS+-SHA2-256f-simple` and records a 64-byte public key, 128-byte private key and 49,856-byte signature.

Falcon experiments include Falcon-512 and Falcon-1024 with key generation, signing, verification, tamper testing and timing measurements.

### KEM performance experiment

The notebook includes a 1000-iteration timing experiment for ML-KEM-512, ML-KEM-768, ML-KEM-1024, Kyber512 and FrodoKEM-640-AES.

One recorded run produced:

| Algorithm | KeyGen | Encap | Decap | Total |
|---|---:|---:|---:|---:|
| ML-KEM-512 | 0.309 ms | 0.376 ms | 0.463 ms | 1.148 ms |
| ML-KEM-768 | 0.342 ms | 0.390 ms | 0.478 ms | 1.210 ms |
| ML-KEM-1024 | 0.493 ms | 0.548 ms | 0.658 ms | 1.699 ms |
| Kyber512 | 0.177 ms | 0.232 ms | 0.250 ms | 0.659 ms |
| FrodoKEM-640-AES | 7.070 ms | 6.590 ms | 6.543 ms | 20.203 ms |

These are measurements from the notebook's execution environment, not universal performance specifications.

One exploratory ML-DSA benchmark cell currently depends on `time.perf_counter()` but the recorded execution did not import `time` in that cell, so its displayed failure should not be treated as benchmark data.

---

# 🔄 4. Hybrid Cryptography

## `PQC Algorithms/Hybrid Cryptography.ipynb`

This notebook combines post-quantum key establishment and signatures with classical authenticated encryption.

```text
ML-KEM-768
     ↓
Shared Secret
     ↓
AES-256-GCM
     ↓
Ciphertext
     ↓
ML-DSA-65 signature
```

### Protocol roles

**Alice — Receiver**
- ML-KEM-768 key pair

**Bob — Sender**
- ML-DSA-65 signing key pair

### Bob's side

1. Use Alice's ML-KEM public key.
2. Encapsulate a shared secret.
3. Use the 32-byte shared secret as the AES-256 key.
4. Generate a random 12-byte AES-GCM nonce.
5. Encrypt the message with AES-256-GCM.
6. Sign `KEM ciphertext || nonce || AES ciphertext` using ML-DSA-65.
7. Package the KEM ciphertext, nonce, AES ciphertext and signature.

### Alice's side

1. Verify the ML-DSA-65 signature.
2. Decapsulate the ML-KEM ciphertext.
3. Confirm both sides derived the same shared secret.
4. Recover the AES-256 key.
5. Decrypt the AES-GCM ciphertext.

### Tamper detection

The notebook intentionally modifies the ciphertext after signing. The modified package fails ML-DSA signature verification and is rejected.

This demonstrates the composition of:

- **ML-KEM** for key establishment.
- **AES-256-GCM** for authenticated symmetric encryption.
- **ML-DSA** for a post-quantum digital-signature layer.

> This is an educational protocol experiment, not a production-ready secure messaging protocol. A real protocol would also need careful key lifecycle management, identity binding, replay protection, serialization/domain separation, nonce management and formal protocol analysis.

---

# 🧰 Technologies Used

| Technology | Role |
|---|---|
| **Python** | Main implementation language |
| **Jupyter Notebook** | Interactive experimentation |
| **NumPy** | Quantum-state mathematics and custom simulator |
| **Qiskit** | Quantum circuit construction and execution |
| **Qiskit Aer** | Local quantum simulation |
| **IBM Quantum Runtime** | Real quantum-hardware execution |
| **liboqs-python** | PQC experimentation |
| **Open Quantum Safe (OQS)** | PQC implementations |
| **cryptography** | AES-256-GCM |
| **SVG** | Custom circuit visualization |
| **Git / GitHub** | Version control |

---

# 🧠 Learning Areas

### Quantum Computing
- Qubit representation
- Quantum states
- Probability amplitudes
- Superposition
- Quantum gates
- Unitary operators
- Controlled operations
- Tensor products
- Bell states
- Entanglement
- Measurement
- Circuit simulation
- Quantum hardware execution

### Post-Quantum Cryptography
- Key Encapsulation Mechanisms
- Digital signatures
- ML-KEM
- ML-DSA
- SLH-DSA / SPHINCS+
- Falcon
- KEM correctness testing
- Signature verification
- Tamper detection
- Cryptographic performance measurement
- Hybrid cryptographic construction

---

# 🔬 Experimentation Philosophy

```text
Learn the concept
       ↓
Understand the mathematics
       ↓
Implement it
       ↓
Run the experiment
       ↓
Measure the result
       ↓
Inspect what happened
       ↓
Iterate
```

The repository intentionally includes lower-level implementations alongside framework-based experiments. The custom quantum circuit builder is an example: it reconstructs state, gate, measurement, controlled-operation and circuit concepts using linear algebra before using Qiskit for higher-level workflows.

---

# 🚧 Current Status

**Active learning and experimentation**

The repository is deliberately a work-in-progress. Some notebooks contain exploratory cells, experimental benchmarks and partially developed functionality. Results shown here are tied to the recorded notebook executions and local library versions.

---

# 🛣️ Possible Next Experiments

### Quantum Computing
- Quantum teleportation
- Deutsch-Jozsa
- Grover's algorithm
- Quantum Fourier Transform
- Shor's algorithm
- Quantum error correction
- Variational quantum algorithms
- Quantum machine learning
- Quantum optimization
- Quantum networking
- More complete custom quantum simulation

### Quantum Security
- Quantum Key Distribution
- BB84
- E91
- Quantum-safe communication models
- Hybrid classical/PQC protocols
- Broader PQC benchmarking
- PQC-based PKI
- X.509 and post-quantum certificate experimentation
- Quantum-safe IoT security

---

# 🔐 Security Note

The current tracked files were checked for common credential patterns including API keys, access tokens, bearer tokens, GitHub tokens, common service-key formats, private-key PEM headers and secret-key assignments.

**No actual API key, access token or private-key material was found in the current tracked files.**

The IBM Quantum notebook contains only the literal placeholders:

```text
ENTER YOUR OWN API KEY
ENTER YOUR OWN INSTANCE NAME
```

The notebooks also contain generated hexadecimal cryptographic values in stored Jupyter outputs. These are experimental key/signature/ciphertext material produced during notebook execution and are not service API credentials.

### Recommended practice

Do not commit credentials directly into notebooks. Prefer environment variables or a secret manager:

```python
import os

token = os.environ['IBM_QUANTUM_TOKEN']
```

---

# 👨‍💻 Author

**Krish Gupta**

Computer Science & Engineering

Interests:
- Quantum Computing
- Post-Quantum Cryptography
- Cybersecurity
- Artificial Intelligence
- Quantum-Classical Systems

---

## ⭐ Philosophy

> **Don't just learn how quantum computers work. Build the pieces, test them, break them, and understand why they work.**

This repository documents that journey.