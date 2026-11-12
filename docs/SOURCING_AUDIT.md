# Sourcing Audit

## Audit 1, November 2026

### Prediction Stake (commit before any evidence)
This audit will identify at least two dependencies or tools that can be removed without affecting the project's required functionality.

### Weights (commit in their own commit, before scoring)
| Criterion | Weight | Why this weight for this project |
|-----------|--------|----------------------------------|
| Stability | | |
| Pricing predictability | | |
| Institutional fit | | |
| Exit cost | | |
Total: 100

### Inventory
| Dependency | Kind (package, dev package, platform, model, builder, CDN script) | Where it is recorded | Ships to users? |
|------------|------|------|------|
| Node.js | Runtime / test tool | `package.json` (`node --test` scripts), `README.md` | No; used for development and evals |
| npm | Package manager | `README.md` (`npm install`), `package.json` | No |
| Wrangler 4.160.1 | Dev package / Cloudflare CLI; installed but undeclared | `README.md`, `worker/wrangler.toml`, `npm ls --all` | No; deployment tool only |
| First-party `app.js` | Local browser script (not a CDN script) | `index.html` (`<script src="app.js">`), `app.js` | Yes; downloaded by browser |
| Cloudflare Workers + D1 | Platform / database | `worker/wrangler.toml`, `worker/src/index.js`, `README.md`, `context/FEATURES.md`, `context/ARCHITECTURE.md`; no D1 binding is currently configured in `worker/wrangler.toml` | No; hosts the API and is intended to store submitted entries |
| Google Gemini API | Model / API | `worker/src/index.js` calls `gemini-flash-lite-latest`; `worker/wrangler.toml` separately declares `GEMINI_MODEL = "gemini-3.5-flash"` | No; called server-side. The configured model variable does not match the hard-coded endpoint model |
| GitHub Pages | Front-end hosting platform (deployment status not confirmed) | `worker/src/index.js` CORS allow-origin; placeholder deployment details in `README.md` and `docs/DEPLOY.md` | Yes; serves the static front end if deployed there |
| GitHub Codespaces | Cloud development environment | `.devcontainer/devcontainer.json`, `context/TOOLS.md` | No; development environment only |
| Bolt.new | Builder | `context/TOOLS.md`; delegation record will be added to `docs/DDR-LOG.md` by December 14 | No; development-time use only |
| GitHub Copilot | Builder / coding assistant | `context/TOOLS.md`; delegation record will be added to `docs/DDR-LOG.md` by December 14 | No; development-time use only |

`package.json` declares zero direct dependencies (production or development). `npm ls --all --parseable` reports 40 installed package entries, all marked extraneous (41 lines including the project root); therefore zero installed packages are pulled in by declared direct dependencies. Wrangler is present at 4.160.1 but is not declared in the manifest or lockfile. The only HTML script found is the local `app.js`; there are no CDN scripts.

### Evidence (each fact with its source and the date you read it)
| Dependency | Latest version and date | Maintainers or vendor | License or terms | Limits or price | Other notes | Checked on |
|------------|------|------|------|------|------|------|

### Scores and Rings
| Dependency | Stability | Pricing | Institutional fit | Exit cost | Weighted | Ring | Reason in one line |
|------------|---|---|---|---|---|---|---|

### Exit Plans (every critical dependency, every Trial)
| Dependency | Trigger | Substitute | Hours to swap | Tested? (date and how) |
|------------|---|---|---|---|

### Changes Caused by This Audit
- <removal, pin, or replacement, with commit hash>
- <ADR amended or superseded, if any>
- <DDR lines added, if AI helped>

### Prediction Stake Resolution
<What happened, and whether the prediction held.>