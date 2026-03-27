---
layout: default
title: Security and PII Skill
---

# Security and PII Skill — Example

This is an example Agent Skill for security and PII handling. It activates automatically when Copilot is working on route handlers, service functions, logging code, or database queries. The rules are drawn from [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab) and OWASP guidance, making it especially useful for Defra services handling citizen data.

Copy the `security-and-pii/` folder into `.github/skills/` in your repository.

## Directory structure

```
.github/skills/
└── security-and-pii/
    └── SKILL.md
```

## Example file contents

The content below goes into your `.github/skills/security-and-pii/SKILL.md` file:

---

````markdown
---
name: security-and-pii
description: UK government data security, PII protection, and OWASP Top 10 guidance for government digital services. Use when writing or reviewing route handlers, service functions, logging code, database queries, or any component that processes personal or sensitive data in a Defra service.
license: OGL-UK-3.0
metadata:
  author: defra-digital
  version: "1.0"
compatibility: Applies to Node.js, .NET, and Python government services
---

# Security and PII

## UK data classifications

| Classification | Applies to | Handling rules |
|----------------|-----------|----------------|
| `OFFICIAL` | Most government data, including citizen personal data | Standard security controls — HTTPS, access control, logging |
| `OFFICIAL-SENSITIVE` | Data that could harm individuals or operations if disclosed | Additional controls — need-to-know access, no AI tool processing without approval |
| `SECRET` or above | National security data | Never processed in developer AI tools |

Defra digital services handle `OFFICIAL` data by default. Some services handling sensitive casework may process `OFFICIAL-SENSITIVE` data — confirm data classification before designing a new service.

## PII categories

The following are always personal data under UK GDPR:

- Full name, email address, postal address, phone number
- National Insurance number, date of birth
- IP address, device identifiers, user account IDs linkable to individuals
- Bank account or payment details, biometric data, health information, location data

**Never log any of the above.** Log a reference (e.g. a case ID) not the value.

## Logging — what to include and exclude

✅ Log safely:
- Request IDs and correlation IDs
- HTTP method, path, status code
- Timestamps
- Error types and categories (not content)
- System performance metrics

❌ Never log:
- Email addresses, names, phone numbers, NI numbers
- Request or response bodies when they may contain personal data
- Bearer tokens, API keys, session tokens, passwords
- `.env` values or any credential loaded from environment variables

```js
// ✅ Safe log
logger.info({ requestId, path: req.path, status: 200 }, 'Request complete')

// ❌ Unsafe log — do not do this
logger.info({ email: req.body.email, name: req.body.name }, 'User submitted')
```

## Secrets management

- Load all secrets from environment variables using `process.env` — never hard-code
- Do not commit `.env` files containing real credentials — add them to `.copilotignore` and `.gitignore`
- Rotate credentials immediately if they are ever logged, printed, or committed to source control
- Use Azure Key Vault for credentials in deployed environments

```js
// ✅ Load from environment
const apiKey = process.env.AZURE_API_KEY

// ❌ Never hard-code
const apiKey = 'sk-abc123...'
```

## Injection prevention

### SQL injection

Always use parameterised queries. Never build queries by string concatenation.

```js
// ✅ Parameterised
const result = await pool.query('SELECT * FROM users WHERE id = $1', [userId])

// ❌ Vulnerable to SQL injection
const result = await pool.query(`SELECT * FROM users WHERE id = ${userId}`)
```

### NoSQL injection (MongoDB)

Sanitise all inputs before using them in queries. Never pass raw user input as a query object.

```js
// ✅ Safe
const user = await db.collection('users').findOne({ _id: new ObjectId(userId) })

// ❌ Vulnerable to NoSQL injection
const user = await db.collection('users').findOne({ _id: req.params.id })
```

### Command injection

Never pass user input to shell commands. If you must run a subprocess, use argument arrays not shell strings.

```js
// ✅ Safe
execFile('ffmpeg', ['-i', sanitisedFilePath, outputPath])

// ❌ Vulnerable
exec(`ffmpeg -i ${req.body.filePath} output.mp4`)
```

## Cross-site scripting (XSS)

- Use Nunjucks `{% raw %}{{ variable }}{% endraw %}` (auto-escaped) — never `{% raw %}{{ variable | safe }}{% endraw %}` unless the value is known-safe HTML you generated yourself
- Apply Content Security Policy headers (Helmet CSP middleware)
- Never inject user input into `innerHTML`, `document.write`, or `eval`

## Broken access control

- Check permissions on every request — do not rely solely on frontend hiding of UI elements
- Log authorisation failures — they may indicate reconnaissance or attack
- Apply the principle of least privilege to service accounts and API clients

```js
// Check the user has access to the specific resource, not just the route
if (req.auth.userId !== resource.ownerId && !req.auth.isAdmin) {
  throw Boom.forbidden('You do not have access to this resource')
}
```

## Session and cookie security

Set all cookies with `Secure`, `HttpOnly`, and `SameSite=Strict` (or `Lax`). See the [Node.js backend instructions](node-backend.instructions.md) for Hapi/Yar configuration.

## CSRF protection

Use `@hapi/crumb` for all state-changing forms. Verify the CSRF token on every `POST`, `PUT`, `PATCH`, and `DELETE` handler.

## Dependency security

- Run `npm audit` before every release — fail CI on high-severity vulnerabilities
- Keep dependencies up to date; address vulnerabilities within 30 days
- Review transitive dependency changes in `package-lock.json` during code review

## File handling

If your service accepts file uploads: validate type by magic bytes (not extension), set a maximum file size, never serve uploads from the same domain without validation, and store files outside the web root.

## Security testing checklist

Before a service goes to production, confirm:

- [ ] All user inputs are validated with `joi` or equivalent
- [ ] No PII in logs
- [ ] No secrets in source code or version control
- [ ] CSRF protection active on all mutating forms
- [ ] Content Security Policy headers configured
- [ ] `npm audit` passes with no high/critical vulnerabilities
- [ ] Dependency versions pinned in `package-lock.json`
- [ ] File upload validation in place (if applicable)
- [ ] Authentication verified on every protected route
- [ ] Authorisation checked at the resource level, not just the route level
````

---

[Back to skills index]({{ "/pages/skills" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
