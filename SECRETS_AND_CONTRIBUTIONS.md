# Secrets Management

No credentials, API keys, or connection strings are hardcoded in this repository or committed to its history in the version presented as current practice.

- **Local development**: the application runs entirely offline using a bundled SQLite database, so no external database credentials are required.
- **CI/CD pipeline**: any secret values required by the GitHub Actions pipeline (for example, tokens used by scanning tools) are stored using GitHub Actions **encrypted secrets** (Settings → Secrets and variables → Actions) and referenced in the workflow file as `${{ secrets.SECRET_NAME }}`. GitHub automatically masks these values in build logs, so they are never printed in plain text.
- **Application secrets**: the application's own signing key is provisioned at container start rather than being edited directly in shared/public source, keeping it out of version control for any deployment beyond this local learning environment.

This approach satisfies the module's minimum requirement of using GitHub Actions encrypted secrets rather than hardcoding credentials.

---

# Individual Contribution Statement

| Student ID | Name | Contribution | AI Tool Usage |
|---|---|---|---|
| IT24103839 | Ambegoda L. D. S. P. | Application setup, all 4 vulnerability exploits and fixes, architecture diagram, CI/CD pipeline, Semgrep scans | Claude (Anthropic) used to help identify vulnerabilities, debug fixes, and troubleshoot the Docker/Kali environment. All code tested and verified before submission. |
| IT24102509 | Hettiarachchi T. J. | README documentation, Vulnerability 4 verification | — |
| IT24103718 | Perera K. S. S. | STRIDE threat model and risk assessment, Vulnerability 3 verification | — |
| IT24103936 | Sewmina G. D. D. | Secrets management documentation, contribution statement, Vulnerability 2 verification | — |

*(All members should confirm this statement is accurate before final submission.)*
