# FestPass — Docker & Container Security Assessment

## Student Details
- **Registration Number:** 202401100100200
- **Application:** FestPass 2026
- **Docker Hub Namespace:** `ramsingh161`
- **Frontend Port:** `8000`

## Project Overview
FestPass is a three-tier application comprising:
- **Frontend:** HTML served by Nginx.
- **Backend:** Python Flask REST API.
- **Database:** PostgreSQL.

Docker Compose orchestrates the services using separate networks and a named volume for persistent database storage.

## Docker Hub Images

- **Repository:** https://hub.docker.com/r/ramsingh161/fest-backend
- **Release tag:** `1.0.0`
- **Signed image digest:** `sha256:db708e65443e8c01672f0192eb80500a7bb80c515cca1989a1487e856f262778`
- **Unsigned test image:** `1.0.1`
- **Unsigned test digest:** `sha256:4412c03fe7b8b133d5b9a75bcced77e0e6b010911571de3a3379532b61e22933`

## Container Security

### Hardening
- Backend runs as a non-root user (`appuser`, UID 999).
- `.dockerignore` excludes environment files and unnecessary build context.
- Compose separates frontend/backend traffic from backend/database traffic.
- PostgreSQL data persists in the named volume `festpass-data`.
- Environment configuration is stored locally in `.env`; `.env` is excluded from Git.

### Trivy Scanning
- Vulnerability reports are stored in `reports/trivy-before.txt` and `reports/trivy-after.txt`.
- The final Trivy gate used `--severity CRITICAL --ignore-unfixed --exit-code 1`.
- The gate reported zero matching vulnerabilities for the Debian OS and Python packages.
- The full scan still reported HIGH and MEDIUM findings; passing the CRITICAL gate does not mean the image is vulnerability-free.

### Cosign Signing and Verification
- The `1.0.0` image was signed using Cosign and a locally generated key pair.
- Verification against `cosign.pub` succeeded for the published image digest.
- A distinct `1.0.1` image digest was published without signing.
- Cosign verification of the unsigned digest returned `no signatures found`, as expected.
- `cosign.pub` is safe to share; `cosign.key` and its password must remain private and must not be committed.

## Screenshots and Reports
- `screenshots/` contains practical assessment evidence.
- `reports/` contains Trivy scan reports.

## AI Tools Used
- **ChatGPT:** Assisted with step-by-step Docker commands, troubleshooting, vulnerability scan interpretation, and Cosign signing/verification workflow.
- **GitHub Copilot / other tools:** Add only if actually used during this assessment.

## Local Execution

1. Create a local `.env` based on `.env.example`.
2. Start the stack:

   ```powershell
   docker compose up -d --build
   ```

3. Open `http://localhost:8000`.
4. Check service status:

   ```powershell
   docker compose ps
   ```

5. Stop the stack while preserving database data:

   ```powershell
   docker compose down
   ```

## Secret Handling
Never commit `.env`, `cosign.key`, or any passwords or access tokens. Only public signing material should be included in the repository.
