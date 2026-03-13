---
name: security-audit
description: Audits code changes for security vulnerabilities and data exposure. Use when the user asks for a security audit, SEC review, or before merge of auth, API, or sensitive data changes. Required for authentication, API endpoints, and sensitive data handling. Applies to frontend and backend.
---

# Security Audit

## When to use this skill

- User asks to "security audit", "SEC", or "audit for vulnerabilities"
- Before merge when changes touch authentication, API endpoints, or sensitive data
- Implementation involves user input, file uploads, or third-party integrations
- After quality-assurance when tests revealed security-sensitive code paths

## Workflow

- [ ] Load the spec from `specs/shaped-specs/<name>/spec.md`
- [ ] Load changed files and identify security-relevant code
- [ ] Run through the security checklist (frontend and backend as applicable)
- [ ] Assign severity to each finding (Critical, High, Medium, Low)
- [ ] Save audit report to `specs/shaped-specs/<name>/security-audit.md`
- [ ] Block merge for Critical and High; document Medium and Low for resolution

## Instructions

### When this skill is required

Run security-audit before merge when the change involves:

- Authentication or authorization flows
- API endpoints (new or modified)
- Sensitive data (passwords, tokens, PII, API keys)
- User input processing or file uploads
- Third-party integrations or external service calls

### Security checklist

**Data protection (frontend and backend)**

- No sensitive data in client-side code, logs, or error messages
- No passwords, tokens, or API keys in localStorage/sessionStorage without encryption
- Secrets use environment variables only; .env never committed
- Validate all user inputs before processing or sending to backend
- Sanitize outputs to prevent XSS

**Frontend-specific**

- No eval(), Function(), or innerHTML with user-controlled data
- Token handling uses httpOnly cookies when possible
- API calls use centralized client; no arbitrary external requests
- Third-party scripts and dynamic imports reference trusted sources

**Backend-specific**

- Auth checks occur server-side; never trust client for authorization
- SQL/NoSQL injection prevented via parameterized queries or ORM
- File uploads validated (type, size) before processing
- Rate limiting and CORS configured appropriately

**Network**

- HTTPS only for production
- No credentials in URLs or query params
- CORS explicitly defined on backend

**Dependencies**

- New dependencies checked for known vulnerabilities (npm audit, etc.)
- Avoid packages with excessive permissions or unclear provenance

**Configuration**

- .cursorignore blocks agent access to sensitive config
- Workspace trust enabled for unknown repositories

### Severity levels

| Level | Examples | Action |
|-------|----------|--------|
| Critical | Data exposure, auth bypass, arbitrary code execution | Block merge |
| High | XSS, insecure storage, missing input validation | Block merge |
| Medium | Verbose errors, dependency vulns, missing error handling | Document for resolution |
| Low | Style issues that could cause security confusion | Document |

### Audit report format

Save to `specs/shaped-specs/<name>/security-audit.md`:

```markdown
## Security audit — [date]

### Summary
[Pass / Block — list Critical/High if blocking]

### Findings
| Severity | Location | Issue | Recommendation |
|----------|----------|-------|----------------|
| High | path/to/file.ts:42 | [description] | [fix] |

### Checklist coverage
- [x] Data protection
- [x] Input validation
- [ ] [any unchecked items]
```

## Handoff

- Recommend running `code-review` after audit
- If Critical or High: list required fixes; do not recommend merge until resolved

## Resources

- Spec: `specs/shaped-specs/<name>/spec.md`
- Output: `specs/shaped-specs/<name>/security-audit.md`
- Standards: `standards/backend/authentication.md`, `standards/backend/api-design.md` if present
- Next step: `.agent/skills/code-review/SKILL.md`
