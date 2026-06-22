# Security Checklist

## Injection Attacks

### SQL Injection
```javascript
// ❌ String concatenation
const query = `SELECT * FROM users WHERE id = ${userId}`;

// ✅ Parameterized query
const query = 'SELECT * FROM users WHERE id = ?';
db.execute(query, [userId]);
```
**Always use parameterized queries or ORM abstractions.**

### Cross-Site Scripting (XSS)
```javascript
// ❌ Direct HTML insertion
element.innerHTML = userInput;

// ✅ Safe text insertion
element.textContent = userInput;
```
**Use safe rendering methods. Escape output. Use Content-Security-Policy headers.**

### Command Injection
```javascript
// ❌ User input in shell command
exec(`rm -rf ${userInput}`);

// ✅ Avoid shell commands with user input
// Use safe APIs instead of shell commands
```
**Avoid passing user input to shell commands. Use language-native APIs.**

### Path Traversal
```javascript
// ❌ User-controlled file path
const content = fs.readFileSync(userInput);

// ✅ Validate and restrict paths
const safePath = path.join('/app/data/', path.basename(userInput));
if (!safePath.startsWith('/app/data/')) throw new Error('Invalid path');
```
**Validate and restrict file paths. Don't trust user-provided paths.**

## Authentication & Authorization

- Protected routes must check authentication before processing
- Don't expose internal IDs or tokens in URLs or responses
- Use proper session management (HTTP-only cookies, short-lived tokens)
- Check authorization for every protected operation, not just at route level

## Secrets Management

- Never hardcode API keys, tokens, passwords, or certificates
- Use environment variables or a secrets manager
- Never log secrets — even accidentally in debug output
- Never commit secrets to version control
- Rotate exposed secrets immediately

## Data Validation

- Validate all user input: type, length, format, range
- Sanitize input before using it (trim, escape, normalize)
- Use strict parsing: reject unexpected fields, don't silently ignore them
- Set reasonable limits on input size and complexity

## Dependency Security

- Keep dependencies updated
- Check for known vulnerabilities (`npm audit`, `pip audit`, `cargo audit`)
- Pin dependency versions in production builds
- Minimize dependency count — fewer dependencies = smaller attack surface

## Common Mistakes

| Mistake | Safe Alternative |
|---------|-----------------|
| `innerHTML = userInput` | `textContent = userInput` |
| SQL string concatenation | Parameterized queries / ORM |
| Hardcoded secrets | Environment variables / secrets manager |
| `eval(userInput)` | Never use eval with user input |
| Disabled CSRF protection | Keep CSRF protection enabled |
| Missing rate limiting | Add rate limiting to all public endpoints |
| Overly permissive CORS | Restrict CORS to specific origins |
| Storing passwords in plaintext | Use bcrypt / argon2 for hashing |
