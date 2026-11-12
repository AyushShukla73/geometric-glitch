# Contributing to Glitch Lab

Thank you for your interest in contributing! This document outlines the guidelines for contributing to this project.

## Code of Conduct

By participating, you agree to abide by our [Code of Conduct](CODE_OF_CONDUCT.md).

## How to Contribute

### Reporting Issues

- Search existing issues first to avoid duplicates
- Use the issue templates when available
- Provide clear steps to reproduce, expected vs actual behavior
- Include browser version, OS, and screenshots if relevant

### Suggesting Features

- Open a feature request issue with the `enhancement` label
- Describe the use case and why it would benefit users
- Consider implementation complexity (this is a zero-dependency project)

### Pull Requests

1. **Fork** the repository
2. **Create a branch** from `master`: `git checkout -b feat/your-feature-name`
3. **Make changes** — All code lives in `index.html`
4. **Test locally** with any static server
5. **Commit** with clear messages (see Commit Guidelines)
6. **Push** and open a Pull Request

## Development Guidelines

### Architecture Constraints

- **Single file** — The entire application is in `index.html` (HTML + CSS + JS)
- **Zero dependencies** — No npm packages, no build tools, no external JS
- **Vanilla only** — Plain HTML, CSS, JavaScript (ES2020+)
- **Browser-native APIs only** — Canvas 2D, FileReader, Drag-and-Drop, Fetch (if needed)

### Code Style

- **Indentation**: 4 spaces (consistent with existing code)
- **Naming**: camelCase for JS variables/functions, kebab-case for CSS classes
- **Comments**: Only for non-obvious algorithms (strip recombination, mirror transforms)
- **Organization**: Keep CSS/JS sections clearly separated with comment headers

### Accessibility Requirements

Every new UI element must:
- Be keyboard reachable (`tabindex`, native elements preferred)
- Have visible focus styles (`:focus-visible`)
- Include appropriate ARIA attributes (`aria-label`, `aria-live`, roles)
- Respect `prefers-reduced-motion`
- Maintain WCAG AA contrast in both light/dark modes

### Performance Considerations

- Avoid creating unnecessary canvases or large base64 strings
- Clean up `URL.createObjectURL()` if used
- Limit simultaneous canvas contexts (current max: ~7)
- Keep total file size minimal (~36 KB target)

## Commit Message Convention

Use conventional commits format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat` — New feature
- `fix` — Bug fix
- `docs` — Documentation only
- `style` — Formatting, missing semicolons, etc. (no code change)
- `refactor` — Code restructuring (no behavior change)
- `perf` — Performance improvement
- `test` — Adding tests (not applicable currently)
- `chore` — Maintenance, tooling

**Examples:**
```
feat(pipeline): add diagonal strip recombination option
fix(upload): handle HEIC files gracefully
docs(readme): add architecture diagram
style(css): normalize spacing in upload card
refactor(js): extract mirror transform to helper function
```

## Testing Checklist

Before submitting a PR, verify:

- [ ] Loads and runs in Chrome, Firefox, Safari (latest)
- [ ] Keyboard navigation works end-to-end
- [ ] `prefers-reduced-motion` disables animations
- [ ] Light/dark mode both render correctly
- [ ] Mobile touch drag-and-drop works
- [ ] No console errors/warnings
- [ ] File size hasn't grown significantly
- [ ] Progress indicator updates correctly
- [ ] Download produces valid PNG

## Review Process

1. Automated checks (if any) must pass
2. Maintainer reviews for:
   - Adherence to zero-dependency constraint
   - Accessibility compliance
   - Performance impact
   - Code clarity and organization
3. Address feedback, push updates
4. Squash and merge (maintainer handles)

## Questions?

Open a discussion or issue — we're happy to help!

---

**Note:** This is a personal/portfolio project. While contributions are welcome, response times may vary. For substantial changes, open an issue first to discuss scope.