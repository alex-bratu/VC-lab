# Verifiable Credentials Demo with walt.id

This project is a **teaching demo** for Verifiable Credentials (VCs) using the [walt.id](https://walt.id) open-source identity framework.  
It allows students to experience the **Issuer → Holder → Verifier** flow in a classroom environment.

---

## Requirements

- Ubuntu 22.04+ Server
- Docker & Docker Compose (v2+)
- Git

Check versions:
```bash
docker --version
docker compose version
```

---

## Getting Started

1. **Clone this repository**:
   ```bash
   git clone https://github.com/RalucaaP/VC-lab.git
   cd VC-lab/waltid-identity/docker-compose
   ```

2. **Start all services**:
   ```bash
   docker compose up --build
   ```

3. **Check running containers**:
   ```bash
   docker compose ps
   ```

---

## Services & Endpoints

Once running, the following services are available:

| Service                 | Role             | URL                          | Port  |
|-------------------------|------------------|------------------------------|-------|
| Wallet API              | Holder backend   | http://localhost:7001        | 7001  |
| Issuer API              | Issue VCs        | http://localhost:7002        | 7002  |
| Verifier API            | Verify VCs       | http://localhost:7003        | 7003  |
| Demo Wallet (UI)        | Holder wallet    | http://localhost:7101        | 7101  |
| Web Portal (UI)         | Issuer/Verifier  | http://localhost:7102        | 7102  |
| VC Repository           | Credential store | http://localhost:7103        | 7103  |
| Dev Wallet (optional UI)| Holder wallet    | http://localhost:7104        | 7104  |

---

## Classroom Demo Flow

### 1. Holder: Create a DID
- Open **Demo Wallet** → [http://localhost:7101](http://localhost:7101)  
- Click **Create new identity**  
- Copy your DID (e.g., `did:key:z6…`)

### 2. Issuer: Issue a credential
- Open **Web Portal** → [http://localhost:7102](http://localhost:7102)  
- Select **Issuer** tab  
- Choose a credential type (e.g., **StudentCard**)  
- Paste the student’s DID as **subject**  
- Fill in claims (`name`, `studentId`, `program`, …)  
- Click **Issue**

### 3. Holder: Store credential
- In **Demo Wallet** → Import credential (automatically via link, or paste JSON if provided)

### 4. Verifier: Request proof & verify
- In **Web Portal** → **Verifier** tab  
- Create a verification request (e.g., prove `isStudent = true`)  
- Student approves in **Demo Wallet**  
- Result appears in Verifier: ✅ Valid / ❌ Invalid

---

## Useful Commands

- Stop all services:
  ```bash
  docker compose down
  ```

- Rebuild images (after `.env` changes):
  ```bash
  docker compose build
  docker compose up
  ```

- Kill all containers (hard reset):
  ```bash
  docker kill $(docker ps -q)
  docker rm $(docker ps -aq)
  ```

---

## ⚠️ Troubleshooting

- **Demo Wallet shows `500: can't access property "id"`**  
  Ensure `.env` contains:
  ```env
  NUXT_PUBLIC_WALLET=http://localhost:7001
  NUXT_PUBLIC_ISSUER=http://localhost:7002
  NUXT_PUBLIC_VERIFIER=http://localhost:7003
  NUXT_PUBLIC_VC_REPO=http://localhost:7103
  ```
  Then rebuild:
  ```bash
  docker compose build waltid-demo-wallet
  docker compose up
  ```

- **Ports already in use** → Stop other apps using 7001–7104 or change ports in `.env`.

---

## Learning Objectives

- Understand the **roles**: Issuer, Holder, Verifier  
- Experience how **DIDs** replace centralized identifiers  
- See how **cryptographic verification** works without contacting the issuer  
- Discuss **privacy-preserving proofs** (selective disclosure, minimal disclosure)

---

With this setup, every student group can play all three roles and complete the trust triangle!