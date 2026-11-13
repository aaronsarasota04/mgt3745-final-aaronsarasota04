# Sourcing Audit

## Audit 1, November 2026

### Prediction Stake (commit before any evidence)
This audit will identify at least two dependencies or tools that can be removed without affecting the project's required functionality.

### Weights (commit in their own commit, before scoring)
| Criterion | Weight | Why this weight for this project |
|-----------|--------|----------------------------------|
| Stability | 30 | Students need consistent, dependable results, so service availability and model behavior are important to the project's usefulness. |
| Pricing predictability | 30 | Gemini API usage and pricing may change. If costs rise, switching to another model could help, but would require checking its price and suitability. |
| Institutional fit | 10 | The project is intended especially for seniors seeking jobs, regardless of their school, so fit is not much of a concern in this project. |
| Exit cost | 30 | The project relies on external services. Replacing one because of higher prices or discontinued support could require code changes, data migration, and testing. |
Total: 100

### Inventory
| Dependency | Kind (package, dev package, platform, model, builder, CDN script) | Where it is recorded | Ships to users? |
|------------|------|------|------|
| Node.js | Runtime / test tool | `package.json` (`node --test` scripts), `README.md` | No; used for development and evals |
| npm | Package manager | `README.md` (`npm install`), `package.json` | No |
| Wrangler 4.160.1 | Dev package / Cloudflare CLI; installed but undeclared | `README.md`, `worker/wrangler.toml`, `npm ls --all` | No; deployment tool only |
| Cloudflare Workers + D1 | Platform / database | `worker/wrangler.toml`, `worker/src/index.js`, `README.md`, `context/FEATURES.md`, `context/ARCHITECTURE.md`; no D1 binding is currently configured in `worker/wrangler.toml` | No; hosts the API and is intended to store submitted entries |
| Google Gemini API | Model / API | `worker/src/index.js` calls `gemini-flash-lite-latest`; `worker/wrangler.toml` separately declares `GEMINI_MODEL = "gemini-3.5-flash"` | No; called server-side. The configured model variable does not match the hard-coded endpoint model |
| GitHub Pages | Front-end hosting platform (deployment status not confirmed) | `worker/src/index.js` CORS allow-origin; placeholder deployment details in `README.md` and `docs/DEPLOY.md` | Yes; serves the static front end if deployed there |
| GitHub Codespaces | Cloud development environment | `.devcontainer/devcontainer.json`, `context/TOOLS.md` | No; development environment only |
| Bolt.new | Builder | `context/TOOLS.md`; delegation record will be added to `docs/DDR-LOG.md` by December 14 | No; development-time use only |
| GitHub Copilot | Builder / coding assistant | `context/TOOLS.md`; delegation record will be added to `docs/DDR-LOG.md` by December 14 | No; development-time use only |

`package.json` declares zero direct dependencies (production or development). `npm ls --all --parseable` reports 40 installed package entries, all marked extraneous (41 lines including the project root); therefore zero installed packages are pulled in by declared direct dependencies. Wrangler is present at 4.160.1 but is not declared in the manifest or lockfile.

### Evidence (each fact with its source and the date you read it)
| Dependency | Latest version and date | Maintainers or vendor | License or terms | Limits or price | Other notes | Checked on |
|------------|------|------|------|------|------|------|
| Node.js | [v22.23.5](https://nodejs.org/en/about/previous-releases) was the latest v22 patch (Nov 11); this workspace reports v22.23.4. v26.13.1 was the latest LTS (Nov 10). | [OpenJS Foundation / Node.js project](https://nodejs.org/en/about) | [MIT](https://github.com/nodejs/node/blob/main/LICENSE) | Free; commercial support is optional. | The [devcontainer](../.devcontainer/devcontainer.json) selects the Node 22 image, not a patch version; local `node --version` reports 22.23.4. | 2026-11-12 |
| npm | [12.2.2](https://registry.npmjs.org/npm), published Nov 4; this workspace reports 10.9.9. | Four registry maintainer accounts ([registry metadata](https://registry.npmjs.org/npm)) | [Artistic-2.0](https://registry.npmjs.org/npm) | Free CLI; registry and other npm services have separate terms. | npm is the package manager, not a dependency in [`package.json`](../package.json); local `npm --version` reports 10.9.9. | 2026-11-12 |
| Wrangler | [4.160.1](https://registry.npmjs.org/wrangler), published Nov 11; also the latest registry version. | One npm publisher account, `wrangler-publisher` ([registry metadata](https://registry.npmjs.org/wrangler)) | [MIT OR Apache-2.0](https://api.deps.dev/v3/systems/npm/packages/wrangler/versions/4.160.1) | Free CLI; Cloudflare service usage is billed separately. | Installed but undeclared and extraneous. `npm audit --json` could not run because no lockfile exists (`ENOLOCK`); [deps.dev](https://api.deps.dev/v3/systems/npm/packages/wrangler/versions/4.160.1) reports no advisories for 4.160.1. | 2026-11-12 |
| Cloudflare Workers + D1 | [Workers pricing docs](https://developers.cloudflare.com/workers/platform/pricing/) last updated Oct 2; [D1 docs](https://developers.cloudflare.com/d1/platform/pricing/) last updated Oct 30. Managed services have no project-pinned version. | [Cloudflare](https://www.cloudflare.com/) | [Self-serve agreement](https://www.cloudflare.com/terms/) and [product-specific terms](https://www.cloudflare.com/service-specific-terms-other-terms/); not an open-source license. | Workers Free: 100,000 requests/day and 10 ms CPU/invocation. D1 Free: 5 million rows read/day, 100,000 rows written/day, 5 GB storage. Paid Workers starts at $5/month; paid D1 has usage-based charges. | No D1 binding/database ID is configured in [`wrangler.toml`](../worker/wrangler.toml), so D1 use is intended, not currently deploy-ready. | 2026-11-12 |
| Google Gemini API | [`gemini-3.5-flash-lite`](https://ai.google.dev/gemini-api/docs/deprecations) is listed as released July 21; project code calls the floating `gemini-flash-lite-latest` alias in [`worker/src/index.js`](../worker/src/index.js). | [Google](https://ai.google.dev/) | [Gemini API Additional Terms](https://ai.google.dev/gemini-api/terms), effective Mar 23, 2026; no open-source license. | For versioned 3.5 Flash-Lite, [pricing](https://ai.google.dev/gemini-api/docs/pricing) lists free-tier use and paid rates of $0.30 input / $2.50 output per million tokens. Alias target and account tier are not established here. | [`wrangler.toml`](../worker/wrangler.toml) declares `gemini-3.5-flash`, but the Worker hard-codes a different model alias. Google says free-tier content may be used to improve products; paid-tier content is not. | 2026-11-12 |
| GitHub Pages | Managed service; no versioned release. [Documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages) checked on the date shown. | [GitHub](https://github.com/) | [GitHub Terms of Service](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service); not an open-source license. | [Published site limit](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits) 1 GB; soft bandwidth limit 100 GB/month and soft build limit 10/hour. Plan eligibility and account-specific cost are unconfirmed ([pricing](https://github.com/pricing)). | Deployment status and configured Pages source are not confirmed in the repository. | 2026-11-12 |
| GitHub Codespaces | Managed service; no versioned release. [Billing documentation](https://docs.github.com/en/billing/concepts/product-billing/github-codespaces) checked on the date shown. | [GitHub](https://github.com/) | [GitHub Terms of Service](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service); not an open-source license. | Personal Free accounts include 120 compute hours and 15 GB-month storage. Beyond included quota, 2-core compute is $0.18/hour and storage is $0.07/GB-month; organization accounts have no free quota. | Account type and actual usage are not established by the repository. | 2026-11-12 |
| Bolt.new | Managed builder; no versioned release. [Pricing](https://bolt.new/pricing) checked on the date shown. | [StackBlitz](https://bolt.new/) | [Bolt service terms](https://bolt.new/terms); not an open-source license. | Free: 300,000 tokens/day and 1 million/month. Pro: $25/month billed monthly. | This is a development-time tool ([`context/TOOLS.md`](../context/TOOLS.md)); the account plan and project-specific usage are not recorded. | 2026-11-12 |
| GitHub Copilot | Managed coding assistant; no versioned release. [Plans](https://docs.github.com/en/copilot/get-started/plans) checked on the date shown. | [GitHub](https://github.com/) | [GitHub Terms of Service](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service); applicable Copilot plan terms not verified. | Copilot Free is $0; verified students may qualify for a free student plan. Paid plans and allowances vary. | The account plan and project-specific usage are not recorded; see [`context/TOOLS.md`](../context/TOOLS.md). | 2026-11-12 |

### Scores and Rings
| Dependency | Stability | Pricing | Institutional fit | Exit cost | Weighted | Ring | Reason in one line |
|------------|---|---|---|---|---|---|---|
| Node.js | 3 | 5 | 4 | 1  | 3.1 | Trial | Other languages such as Python or Go may be more easier to manage as complexity grows, so we are not committing to Node.js yet. |
| npm | 3 | 5 | 4| 1  | 3.7 | Trial  | If project complexity eventually prompts a language change, we would also need to replace npm with that language's package manager. |
| Wrangler 4.160.1 | 4 | 5 | 4 | 3 | 4.0 | Adopt | Wrangler is Cloudflare's official CLI and supports the platform we plan to adopt. |
| Cloudflare Workers + D1 | 4 | 4 | 3 | 3  | 3.6 | Adopt | The free tiers should cover a project of this size, but exit plan is required because D1 holds the project's data. |
| Google Gemini API | 2 | 3 | 4 | 4 | 3.1 | Trial  | The Google-issued API key is required to access Gemini; the free tier may not cover testing or production, so alternatives will depend on budget and availability. |
| GitHub Pages | 5  | 5 | 4 | 4 | 4.6 | Adopt | Because this is a public repository, GitHub Pages is a free option for hosting the static frontend. |
| GitHub Codespaces | 4 | 3 | 3 | 5 | 3.9 | Adopt | The GitHub Student Developer Pack includes Codespaces access that is adequate for now. |
| Bolt.new | 3 | 3 | 3| 4 | 3.3 | Trial | Its free tier is adequate for a small project. |
| GitHub Copilot | 3  | 3 | 4 | 5 | 3.7 | Adopt | The GitHub Student Developer Pack provides 200 monthly tokens, which is more than enough for development. |

### Exit Plans (every critical dependency, every Trial)
| Dependency | Trigger | Substitute | Hours to swap | Tested? (date and how) |
|------------|---------|------------|---|---|
| Node.js | The product requires a desktop GUI instead of a web app. | Python with Tkinter | 12 | Not yet |
| npm | Node.js is replaced or npm no longer meets package-management needs. | PyPI packages installed with pip | 12 | Not yet |
| Wrangler 4.160.1 | Wrangler becomes unsupported or incompatible with Cloudflare deployment. | Azure Functions Core Tools and Azure CLI | 5 | Not yet |
| Cloudflare Workers + D1 | Free-tier limits are frequently hit, or service pricing or terms change. | Azure Functions + Azure SQL Database | 5 | Not yet |
| Google Gemini API | Free-tier limits or rate limits are frequently hit, or API pricing or terms change. | Grok API | 2 | Not yet |
| GitHub Pages | The repository must be private, or Pages terms change for public repositories. | Streamlit Community Cloud | 2 | Not yet |

### Changes Caused by This Audit
- No changes were made to the application, dependencies, or configuration: no removal, pin, or replacement was made, and no ADR was changed.
- All listed components are crucial to their documented project roles: Node.js/npm run the test and setup workflow (`package.json:5-7`, `README.md:22-24`); Wrangler configures the Worker and D1 binding (`worker/wrangler.toml:1-15`); the Worker uses D1 for entries (`worker/src/index.js:46-48,185-215`) and Gemini for role suggestions (`worker/src/index.js:76-90`); GitHub Pages is the configured browser origin (`worker/src/index.js:14-16`); Codespaces supplies the development container and forwarded ports (`.devcontainer/devcontainer.json:3-8`); and Bolt.new and Copilot are recorded as tools used to build the feature (`context/TOOLS.md:5,8`).

### Prediction Stake Resolution
The prediction did not hold: the audit found no dependency or tool that can be removed while preserving all required project functionality. No changes or removals were made.
