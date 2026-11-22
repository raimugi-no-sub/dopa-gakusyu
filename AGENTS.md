# AGENTS.md

## Architecture
- Static site, no build step. Everything (markup, styles, game logic) lives in one file: `public/index.html`.
- The word list lives in `public/words.txt` (fetched at startup via `WORDS_URL`, parsed by `parseList`). The owner swaps this file to change the list; clients auto-sync by content hash (`leapslot_wsrc` in localStorage), keeping per-word examples/scores, unless they imported their own list. `BUILTIN` (100 words) is only the offline fallback.
- `netlify.toml` publishes `public/` with an empty build command and adds a couple of security headers.
- Cloud save uses the user's own Firebase project (Auth email/password + Firestore `users/{uid}` docs). The `window.FIREBASE_CONFIG` block in `index.html` is the public Firebase web config (not a secret). Firestore security rules are documented in the HTML comment above it.
- External CDNs: Google Fonts (Zen Maru Gothic), gstatic Firebase SDK v10.12.2 (ES modules), cdnjs SheetJS 0.18.5.

## Conventions
- Keep the app as a single self-contained HTML file unless the owner asks for a refactor.
- UI text is Japanese.
- Progress is stored in localStorage first; Firebase sync is optional on top of it.
