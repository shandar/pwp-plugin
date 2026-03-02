---
name: pwp-security-audit
description: "Security review protocol — systematic checks for common vulnerabilities and security hygiene. Use this skill whenever the user asks about security, wants a security audit, mentions vulnerabilities, OWASP, XSS, SQL injection, authentication issues, or secrets management. Also use when they say 'is this secure', 'check for vulnerabilities', 'security review', 'audit this for security', or 'are there any security issues'. Covers secrets, input validation, auth, authorization, data protection, dependencies, and error handling."
---

# Security Audit Skill

This skill defines how to perform a security-focused code review. Security changes have high blast radius — always flag findings and get explicit approval before making fixes.

## Security Mindset

- **Assume all input is hostile.** User input, API responses, URL parameters, cookies, headers — all untrusted until validated.
- **Defense in depth.** Never rely on a single layer of protection. Validate on client AND server.
- **Least privilege.** Give code, users, and services only the permissions they need. Nothing more.
- **Flag, don't fix (without approval).** Security changes can break auth flows, lock users out, or introduce regressions. Always get approval first.

## Security Review Checklist

### 1. Secrets & Credentials
- [ ] No API keys, tokens, or passwords in source code
- [ ] No secrets in client-side bundles
- [ ] .env files are in .gitignore
- [ ] .env.example exists with placeholder values (no real secrets)
- [ ] No secrets in commit history
- [ ] Server-side environment variables used for sensitive config

### 2. Input Validation & Sanitization
- [ ] All user input validated on the server (not just client-side)
- [ ] HTML output sanitized to prevent XSS (no dangerouslySetInnerHTML with user data)
- [ ] SQL queries use parameterized statements (no string concatenation)
- [ ] File uploads validated (type, size, content — not just extension)
- [ ] URL parameters and query strings validated before use
- [ ] JSON payloads validated against expected schema

### 3. Authentication
- [ ] Passwords hashed with bcrypt/argon2 (never MD5/SHA1, never plaintext)
- [ ] Session tokens are cryptographically random and sufficiently long
- [ ] JWT tokens have expiration and are validated on every request
- [ ] Password reset tokens are single-use and time-limited
- [ ] Rate limiting on login endpoints (prevent brute force)
- [ ] Failed login attempts logged (for detection)

### 4. Authorization
- [ ] Every API endpoint checks user permissions (server-side)
- [ ] UI hiding is NOT the only access control
- [ ] Resource access checks ownership (user can only access their own data)
- [ ] Admin routes require admin role verification on every request
- [ ] CORS configured to allow only trusted origins

### 5. Data Protection
- [ ] HTTPS enforced (no mixed content)
- [ ] Sensitive data not logged (passwords, tokens, PII)
- [ ] Sensitive data not in URL parameters
- [ ] Database connections use TLS
- [ ] PII handling complies with relevant regulations (GDPR, CCPA)

### 6. Dependencies
- [ ] npm audit shows no critical or high vulnerabilities
- [ ] Dependencies are from trusted sources (no typosquatting)
- [ ] Lock file is committed and up to date
- [ ] No dependencies with known CVEs in production

### 7. Error Handling
- [ ] Error messages don't leak internal details (stack traces, file paths, SQL queries)
- [ ] Custom error pages for 4xx and 5xx (no framework defaults)
- [ ] Errors are logged server-side for debugging
- [ ] API errors return consistent, safe error shapes

## OWASP Top 10 Quick Reference

| # | Vulnerability | What to Check |
|---|--------------|---------------|
| 1 | **Broken Access Control** | Authorization on every endpoint, resource ownership |
| 2 | **Cryptographic Failures** | HTTPS, hashing, no plaintext secrets |
| 3 | **Injection** | SQL parameterization, input sanitization |
| 4 | **Insecure Design** | Threat modeling, principle of least privilege |
| 5 | **Security Misconfiguration** | Default credentials removed, CORS configured |
| 6 | **Vulnerable Components** | Dependency audit, no known CVEs |
| 7 | **Auth Failures** | Session management, password policies, rate limiting |
| 8 | **Data Integrity Failures** | Input validation, signed updates, CI/CD security |
| 9 | **Logging Failures** | Sufficient logging, no sensitive data in logs |
| 10 | **SSRF** | URL validation, no user-controlled server-side requests |

## Reporting Format

```markdown
## Security Finding: {title}

**Severity:** Critical | High | Medium | Low
**Category:** {OWASP category or custom}
**Location:** {file:line}

**Description:** What the vulnerability is and why it matters.
**Recommendation:** Specific fix with code suggestion.
**Blast Radius:** What else might be affected by the fix.
```

## Rules

1. **Never commit secrets** — even for testing. Use environment variables.
2. **Never weaken security for convenience** — no disabling CORS, no skipping auth.
3. **Flag findings, don't auto-fix** — security changes need human review.
4. **Don't guess at security** — if unsure, flag it as a question.
5. **Redact sensitive data** — never include real credentials in plans or communication.
