# Security Policy

## Supported Versions

Only the latest version on the `master` branch receives security updates. Since this is a client-side only application deployed to GitHub Pages, users always receive the latest version automatically.

| Version | Supported |
|---------|-----------|
| Latest (master) | ✅ |
| Older deployments | ❌ |

## Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, email the maintainer directly at: **[ayushshukla73@gmail.com](mailto:ayushshukla73@gmail.com)**

Include the following information:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Any suggested mitigations

You will receive a response within 72 hours. If the vulnerability is confirmed, we will:
1. Acknowledge the report
2. Develop a fix
3. Deploy the fix to GitHub Pages
4. Credit the reporter (unless anonymity is requested)

## Security Model

### Threat Model

Glitch Lab is a **client-side only** application with the following security properties:

| Aspect | Details |
|--------|---------|
| **Data Flow** | Images never leave the browser — processed entirely in memory via Canvas API |
| **Network** | No API calls, no backend, no authentication, no tracking |
| **Storage** | No localStorage, sessionStorage, IndexedDB, or cookies used |
| **Dependencies** | Zero external JavaScript dependencies |
| **Content Security Policy** | Compatible with strict CSP (only inline styles/scripts from single HTML file) |

### Attack Surface

| Vector | Risk | Mitigation |
|--------|------|------------|
| Malicious image upload | Low | Canvas API handles decoding; no code execution from image data |
| XSS via file name | None | File names never rendered in DOM |
| Memory exhaustion | Low | Large images limited by browser canvas memory limits (~50MB practical) |
| CSP bypass | None | No `eval()`, `new Function()`, or dynamic code execution |

### Known Limitations

- **No server-side validation** — All processing happens client-side; malicious users can modify local code
- **Large images** — Very large images (>20000px) may hit canvas size limits or cause OOM
- **No integrity checking** — GitHub Pages serves static files; users should verify HTTPS

## Security Best Practices for Users

1. **Verify HTTPS** — Always access via `https://ayushshukla73.github.io/digital-geometric-glitch-art/`
2. **Don't process sensitive images** — While images don't leave your browser, treat any web page as potentially observable
3. **Keep browser updated** — Canvas API vulnerabilities are patched in browser updates

## Disclosure Policy

- **Coordinated disclosure** — We prefer to fix before public disclosure
- **Credit** — Reporters credited in CHANGELOG.md (unless anonymous requested)
- **Timeline** — Target fix within 30 days for confirmed vulnerabilities

## Contact

Security contact: **ayushshukla73@gmail.com**

GPG key available on request for encrypted communication.