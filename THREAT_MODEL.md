# STRIDE Threat Model — OWASP Juice Shop

This threat model was built against the application's architecture: a browser (untrusted zone) communicating over REST API calls with a Node.js/Express backend (trusted zone), which queries a SQLite database.

| ID | STRIDE Category | Threat Scenario | Likelihood (1-5) | Impact (1-5) | Score | Justification |
|---|---|---|---|---|---|---|
| T1 | Spoofing / Elevation of Privilege | SQL Injection on the login form allows an attacker to bypass authentication and log in as any user, including admin, without a valid password | 4 | 5 | 20 (Critical) | The payload is trivial to find and automate; success grants full admin access with zero valid credentials |
| T2 | Information Disclosure | SQL Injection (UNION-based) on the product search feature allows an attacker to dump the entire Users table, including emails and password hashes | 4 | 5 | 20 (Critical) | Search is a public, unauthenticated endpoint; a single crafted query exfiltrates every user's credentials |
| T3 | Information Disclosure | An unauthenticated `/ftp` route serves a public directory listing of internal files, including a file explicitly marked "confidential, do not distribute" | 3 | 4 | 12 (High) | No authentication is required; discovery only requires guessing a common folder name |
| T4 | Spoofing | No rate limiting or lockout on the login endpoint allows unlimited automated password-guessing attempts | 4 | 3 | 12 (High) | Trivial to automate with tools like Hydra or Burp Suite Intruder; no resistance from the application |

## Threat-to-Control Mapping

| Threat ID | Control Implemented | Control Type | Location in Codebase |
|---|---|---|---|
| T1 | Parameterized SQL query using named placeholders (`:email`, `:password`) instead of string concatenation | Preventative | `routes/login.ts` |
| T2 | Parameterized SQL query using named placeholders (`:criteria`) instead of string concatenation | Preventative | `routes/search.ts` |
| T3 | Removed the `/ftp` route entirely from the Express application | Preventative | `server.ts` |
| T4 | Added a failed-login-attempt counter per email; blocks further attempts with HTTP 429 after 5 failures | Preventative | `routes/login.ts` |

## Notes

- Ratings use a 5x5 scale where Score = Likelihood × Impact.
- All four threats were independently proven exploitable against the unmodified application (see Technical Report Section 3), then fixed and re-verified by repeating the same exploit against the patched code.
- Each fix was also checked with the Semgrep SAST tool before and after the change (see `semgrep-report.json` in this repository).
