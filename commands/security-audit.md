Perform a security audit of this codebase. Focus on finding real vulnerabilities, not theoretical risks.

## What to Check

### 1. Secrets & Credentials
- Scan all files for hardcoded API keys, passwords, tokens, connection strings
- Check `.gitignore` includes `.env`, `*.pem`, `*.key`
- Verify `.env.example` exists and contains no real values
- Check git history for accidentally committed secrets: `git log --all -p -S "password\|secret\|api_key\|token" --diff-filter=A`

### 2. Input Validation
- Every API endpoint validates input with Zod (or equivalent)
- No raw `req.body`, `req.query`, `req.params` used without validation
- Check for SQL injection: any raw queries or string concatenation?
- Check for XSS: any `dangerouslySetInnerHTML` with user input?

### 3. Authentication & Authorization
- Auth middleware on protected routes
- Permission checks at the API level, not just UI
- Tokens in httpOnly cookies (not localStorage)
- Rate limiting on login/auth endpoints

### 4. Dependencies
- Run `npm audit` and report high/critical findings
- Check for unused dependencies: `npx depcheck`
- Verify lockfile is committed
- Check if production deps use exact or tilde versions

### 5. Configuration
- Check for CORS wildcard (`*`) in production config
- Verify security headers (Helmet or equivalent)
- Check error responses don't leak internal details

### 6. GitHub Repository
- Is Dependabot enabled?
- Is CodeQL/security scanning configured?
- Is branch protection enabled on `main`?
- Is secret scanning/push protection enabled?

## Output Format

### Security Score: X/10

### Critical Issues (fix immediately)
- ...

### High Issues (fix this sprint)
- ...

### Medium Issues (fix soon)
- ...

### Low Issues (improve when possible)
- ...

### Checklist Status
- [ ] No secrets in code
- [ ] Input validation on all endpoints
- [ ] Auth checks on protected routes
- [ ] Dependencies audited
- [ ] Security headers configured
- [ ] GitHub security features enabled
