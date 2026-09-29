# DevSecOps Pipeline — OWASP Juice Shop

**IE3142 DevOps Security — Building and Securing a DevSecOps Pipeline**
Faculty of Computing, SLIIT | BSc (Hons) in Information Technology | Year 3 Semester 1, 2026

## Group Members

| Student ID | Name | Role |
|---|---|---|
| IT24103839 | (your name) | Lead / all sections |
| | (member 2) | |
| | (member 3) | |
| | (member 4) | |

## Project Overview

This project selects **OWASP Juice Shop** (Node.js/Express backend, Angular frontend, SQLite database) as the target open-source application, and demonstrates the full DevSecOps lifecycle:

1. Architecture analysis and STRIDE threat modelling
2. Exploit-and-fix for 4 real vulnerabilities, each proven before and after
3. An automated CI/CD pipeline (GitHub Actions) with security gates
4. Secrets management via GitHub Actions encrypted secrets

## Vulnerabilities Found, Fixed, and Verified

| # | Vulnerability | Location | Fix |
|---|---|---|---|
| 1 | SQL Injection — authentication bypass | `routes/login.ts` | Parameterized query with named placeholders |
| 2 | SQL Injection — UNION-based data exfiltration | `routes/search.ts` | Parameterized query with named placeholders |
| 3 | Sensitive data exposure — public directory listing | `server.ts` (`/ftp` route) | Removed the exposed route |
| 4 | Missing brute-force protection | `routes/login.ts` | Added a failed-attempt counter that blocks further attempts after 5 tries |

Each fix was verified by re-running the exact same exploit and confirming it no longer succeeds, and by a Semgrep SAST scan before and after the change (see `semgrep-report.json`).

## Running the Application

```bash
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
```

Then visit `http://localhost:3000`.

## Tech Stack

- **Backend**: Node.js / Express, SQLite
- **Frontend**: Angular
- **Containerization**: Docker
- **SAST**: Semgrep
- **CI/CD**: GitHub Actions

## Secrets Management

No credentials or API keys are hardcoded in this repository. CI/CD secrets are managed through GitHub Actions encrypted secrets.

## AI Usage Disclosure

Claude (Anthropic) was used to help identify vulnerabilities, write and debug fixes, and troubleshoot the Docker/Kali Linux environment. All code was reviewed, tested, and verified by the group before submission.

## License

This project is based on OWASP Juice Shop, licensed under the MIT License.
