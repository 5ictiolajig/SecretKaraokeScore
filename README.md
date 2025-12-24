# 🎤 SecretKaraokeScore

Secret Karaoke Score lets a user compute a local “voice score” off-chain, encrypt it, and submit it to a confidential smart contract. 
The contract compares the encrypted score against Bronze, Silver, and Gold thresholds, which are handled homomorphically on-chain. 
Only the resulting tier (None/Bronze/Silver/Gold) is exposed to the user, never the raw score. 
This demonstrates privacy-preserving scoring and tiering for user performance metrics.

---
## Contract
- **Contract name:** `SecretKaraokeScore`
- **Network:** Sepolia
- **Contract address:** `0x81551aaE3390D72D2B7D8aD016c25EFc9fFBdD0d` 
- **Relayer SDK:** `@zama-fhe/relayer-sdk` (v0.3.x required)

---

## Features
Encrypted karaoke score submission per user key (with ability to update score).

Homomorphic comparison against hardcoded Bronze/Silver/Gold thresholds.

Encrypted level code (0–3) that can be made publicly decryptable and read via relayer.

Minimal UI flow: submit score, compute level, make level public, decrypt human-readable tier.

---

Modern glassmorphic UI built with pure HTML + CSS.  
Powered by Zama Relayer SDK v0.3.0-5 and Ethers.js v6.15.


Zero knowledge of inputs — full privacy preserved

Modern dual-column glassmorphic UI built with pure HTML + CSS

Powered by Zama Relayer SDK v0.3.0 and Ethers.js v6

🛠 Quick Start
Prerequisites

Node.js ≥ 20

npm / yarn / pnpm

MetaMask or any injected Ethereum-compatible wallet

## Installation (development)
1. Clone repo  
```bash
git clone <repo-url>
cd health-metric-zone
Install dependencies (example)

npm install
# or
yarn install

Install Zama Relayer SDK on frontend

npm install @zama-fhe/relayer-sdk @fhevm/solidity ethers

Build & deploy (Hardhat)

npx hardhat clean
npx hardhat compile
npx hardhat deploy --network sepolia
Make sure your hardhat.config.js includes the Zama config and the Solidity version ^0.8.27.


Make sure `hardhat.config.js` has the fhEVM config and Solidity `^0.8.27`.

---

## Frontend (Quickstart)

Set CONFIG.CONTRACT_ADDRESS in frontend/index.html to the deployed SecretKaraokeScore address.

Serve the frontend (npx serve frontend or similar).

User (submit/update score):

Derive userId = keccak256(utf8(userKey)) on the frontend.

Encrypt the local karaoke score via add16(score) and call:
submitScore(bytes32 userId, bytes32 encScore, bytes attestation)

The same userKey can be reused to update the score (only by the owner).

Compute level:

Encrypt zero via add16(0) (used internally as a base) and call:
computeLevel(bytes32 userId, bytes32 encZero, bytes attestation)

Read levelHandle(bytes32 userId) to get the encrypted level code (0–3).

Reveal level:

Call makeLevelPublic(bytes32 userId).

Decrypt the level on the frontend:

const out = await relayer.publicDecrypt([handle]);
const v = out.clearValues[handle] ?? out.clearValues[handle.toLowerCase()];
const levelCode = Number(v); // 0=None, 1=Bronze, 2=Silver, 3=Gold;

---

# Security & Privacy
The contract never stores plain health data.
FHE.allow and FHE.allowThis are used so only authorized parties (owner + contract) can decrypt.
Users must protect their wallets and local attestation proofs — if lost, privacy is still preserved (attestations are on inputs).

# Common Commands:

Compile: npx hardhat compile
Deploy: npx hardhat deploy --network sepolia
Serve frontend: npx serve frontend or any static server

Troubleshooting
If publicDecrypt returns undefined: ensure you passed a clean bytes32 handle and that the contract used FHE.makePubliclyDecryptable(...).
If Relayer worker fails in browser: ensure server sends Cross-Origin-Opener-Policy: same-origin and Cross-Origin-Embedder-Policy: require-corp headers.

## 📁 Project Structure
tinderdao-private-match/
├── contracts/
│   └── SecretKaraokeScore.sol               # Main FHE-enabled matchmaking contract
├── deploy/                                  # Deployment scripts
├── frontend/                                # Web UI (FHE Relayer integration)
│   └── index.html
├── hardhat.config.js                        # Hardhat + FHEVM config
└── package.json                             # Dependencies and npm scripts

📜 Available Scripts
Command	Description
npm run compile	Compile all smart contracts
npm run test	Run unit tests
npm run clean	Clean build artifacts
npm run start	Launch frontend locally
npx hardhat deploy --network sepolia	Deploy to FHEVM Sepolia testnet
npx hardhat verify	Verify contract on Etherscan

🔗 Frontend Integration

The frontend (pure HTML + vanilla JS) uses:

@zama-fhe/relayer-sdk v0.3.0

ethers.js v6.13

Web3 wallet (MetaMask) connection

Workflow:

Connect wallet

Encrypt & Submit a preference query (desired criteria)

Compute match handle via computeMatchHandle()

Make public the result using makeMatchPublic()

Publicly decrypt → get final result (MATCH ✅ / NO MATCH ❌)

🧩 FHEVM Highlights

Encrypted types: euint8, euint16

Homomorphic operations: FHE.eq, FHE.and, FHE.or, FHE.gt, FHE.lt

Secure access control using FHE.allow & FHE.allowThis

Public decryption enabled with FHE.makePubliclyDecryptable

Frontend encryption/decryption handled via Relayer SDK proofs

📚 Documentation

Zama FHEVM Overview

Relayer SDK Guide

Solidity Library: FHE.sol

Ethers.js v6 Documentation

🆘 Support

🐛 GitHub Issues: Report bugs or feature requests

💬 Zama Discord: discord.gg/zama-ai
 — community help

📄 License

BSD-3-Clause-Clear License
