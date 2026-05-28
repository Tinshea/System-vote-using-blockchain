# Blockchain-Based Electoral Voting System

> Academic project developed as part of my Data Structures course at Sorbonne Université — Campus Pierre et Marie Curie

A two-round electoral voting system implemented in C, secured end-to-end with RSA encryption and backed by a blockchain. Each voter generates an RSA key pair, signs their vote declaration, and submissions are grouped into blocks forming a tamper-evident chain. A decentralized winner-computation function resolves elections by trusting the longest chain, mirroring real-world blockchain consensus.

## 🛠️ Tech Stack

- **Core Technologies**: C (C99), RSA encryption (modular exponentiation, Miller-Rabin primality), SHA-256 hashing (via `openssl`)
- **Tools & Environment**: GCC, Make, Gnuplot (performance curve generation)

## 📦 Installation & Setup

### Prerequisites

- GCC — GNU C Compiler
- Make
- OpenSSL (`libssl-dev`) — for SHA-256 hashing

```bash
# Debian / Ubuntu
sudo apt install gcc make libssl-dev
```

### Instructions

```bash
# 1. Clone the repository
git clone https://github.com/Tinshea/System-vote-using-blockchain.git
cd System-vote-using-blockchain

# 2. Compile
make

# 3. Run
./exec
```

The `main` function demonstrates all project components: RSA protocol, hash tables, signed declarations, blockchain construction, and winner computation.

## 🔐 System Architecture

```
Voter generates RSA key pair (public + private)
          │
          ▼
    Signs vote declaration → Protected { public_key, message, signature }
          │
          ▼
  Declarations grouped into Blocks (10 per block)
  Each Block stores: votes, assessor key, SHA-256 hash, previous hash, nonce (PoW)
          │
          ▼
    Blocks form a blockchain tree (CellTree)
          │
          ▼
  compute_winner_BT() → resolves election via longest-chain consensus
```

## 🧱 Core Data Structures

| Structure       | Description                                                                                  |
|-----------------|----------------------------------------------------------------------------------------------|
| `Key`           | RSA key — stores `val` and modulus `n` (both `long`)                                        |
| `Signature`     | Array of `long` values with known size — RSA-signed message                                 |
| `Protected`     | Voter's public key + plaintext vote message + associated signature                          |
| `CellKey`       | Singly linked list of `Key` (voter/candidate registry)                                      |
| `CellProtected` | Singly linked list of `Protected` (vote declaration log)                                    |
| `HashCell`      | `(Key*, int)` pair for hash table entries                                                   |
| `HashTable`     | Array of `HashCell` with linear probing for collision resolution                            |
| `Block`         | 10 vote declarations + assessor key + SHA-256 hash + previous hash + nonce (proof-of-work) |
| `CellTree`      | N-ary tree cell: block, father pointer, first child, next sibling, height                   |

## ⚙️ Key Functions

| Function                  | Description                                                                          |
|---------------------------|--------------------------------------------------------------------------------------|
| `int_is_prime_naive`      | Naïve primality test — O(p). Largest prime tested in < 2ms: **188,935**             |
| `modpow_naive`            | Naïve modular exponentiation — O(m)                                                 |
| `modpow`                  | Fast modular exponentiation (square-and-multiply) — O(log m)                       |
| `miller_rabin`            | Probabilistic primality test — error probability ≤ 0.25^k for k rounds             |
| `generate_random_data`    | Generates random voters, candidates, and vote declarations; writes them to files    |
| `compute_winner`          | Determines the election winner from linked lists using hash table lookups in O(1)  |
| `compute_proof_of_work`   | Finds a nonce such that the block hash starts with `d` zero bits. Exceeds 1s at **d = 3** |
| `compute_winner_BT`       | Resolves the election from the blockchain tree using longest-chain consensus        |

## 📊 Experimental Results

| Experiment                           | Finding                                                    |
|--------------------------------------|------------------------------------------------------------|
| `modpow_naive` vs `modpow`           | `modpow_naive` grows linearly with m; `modpow` stays near 0ms for all tested values |
| Proof-of-work time vs difficulty `d` | Execution time exceeds 1 second starting at **d = 3**      |

Performance curves were generated with Gnuplot and are stored in the `GRAPH/` directory.

## 🗂️ Project Structure

```
System-vote-using-blockchain/
├── Blockchain/          # Blockchain logic (block creation, chaining, PoW)
├── Lib/                 # RSA, hash table, signature, and linked list modules
├── Bin/                 # Compiled object files
├── GRAPH/               # Gnuplot performance graphs
├── keys.txt             # Generated voter/candidate RSA keys
├── candidates.txt       # Candidate declarations
├── declarations.txt     # Signed vote declarations
├── block.txt / Block.txt# Blockchain block data
├── Comparaison2.txt     # modpow benchmark comparison data
├── Makefile             # Build configuration
└── Compte_rendu_final_BOUZARKOUNA_AUDIGIER.pdf
```

## 👤 Authors

**Malek Bouzarkouna** & ** Roman Audigier** — Sorbonne Université, Data Structures course

## 📄 License

No license specified.
