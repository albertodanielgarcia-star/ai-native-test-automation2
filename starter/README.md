# Starter — Recovery Playwright Project

This directory is a **ready-to-run Playwright + TypeScript project** —
the recovery path for Lab 0. It is not the completed solution and not
the enterprise framework. You will build that framework during the
course; this project exists only so that one broken environment never
blocks your progress.

## When to use it

``` text
Lab 0 issue (npm init playwright fails, scaffold confusion,
broken config, browser install fails)
  ↓
Quick Fix → ../docs/troubleshooting.md
  ↓
Fixed in <5 min?
 ├── Yes → continue with your own project
 └── No  → use this starter project → continue the course
```

Use it when `npm init playwright` fails, the generated scaffold is
confusing or broken, or browser installation will not cooperate and
the fix is not quick. Resolve the underlying environment issue later
with your instructor or IT — do not let it consume the session.

## Run it

From the repository root:

``` bash
cd starter
npm ci
npx playwright install
npx playwright test
```

Expected result: the example tests in `tests/` run and pass, and a
summary like `6 passed` is printed. Other commands you saw in Lab 0
work the same way:

``` bash
npm test              # same as npx playwright test
npm run test:ui       # interactive UI Mode
npm run show-report   # open the HTML report
```

## Publish it to your own GitHub repository

The starter is committed inside the starter-kit repository. To see a
GitHub Actions run under **your** account, publish it to a repository
you own:

1. Create a new repository on GitHub (web UI). Do **not** initialize
   it with a README.
2. Point `origin` at your repository and push:

   ``` bash
   git remote set-url origin <your-repository-url>
   git branch -M main
   git push -u origin main
   ```

   If `git remote -v` shows no `origin`, use `git remote add origin
   <your-repository-url>` instead of `set-url`.

3. Open your repository → **Actions** → the **Playwright Tests**
   workflow runs automatically on the push. The HTML report is
   uploaded as a `playwright-report` artifact on each run.

If push authentication fails, see
[../docs/troubleshooting.md](../docs/troubleshooting.md).

## Honest limitations

Clone-and-continue removes the scaffolding step, **not** the
environment requirements. This path still needs:

- Node.js LTS and npm (`npm ci` installs dependencies).
- Network access to the npm registry and the Playwright
  browser-download hosts (`npx playwright install` downloads browser
  binaries).
- Access to `playwright.dev`, which the example tests visit.

If your blocker is a restricted or corporate network, the fix is a
different network (for example, home or a hotspot) or an IT
escalation — this project cannot download packages or browsers it
cannot reach. See
[../docs/troubleshooting.md](../docs/troubleshooting.md) for the exact
endpoints to send to your IT team.

## What this project intentionally is not

This is the plain output of a Playwright initializer plus pinned
dependencies. It has **no** Page Objects, fixtures, API clients,
test-data layers, custom reporters, or environment configuration on
purpose — those are course topics you will build properly later. Treat
it as a spare tire, not the car.

## API Interception & Mocking

This project includes examples of browser network interception and API response mocking using Playwright's native capabilities.

See [docs/api-interception-mocking.md](docs/api-interception-mocking.md) for:
- What it is and what problem it solves
- Difference between real API tests and browser mocking
- When to use it and when to avoid it
- How it works with commented examples
- How to run it and debug it
- How to add new mocks
- Maintenance and limits

## CI/CD & Quality Gates

This project includes a GitHub Actions workflow that runs automated checks on every push and pull request.

### Workflow Checks

The `.github/workflows/playwright.yml` workflow executes:
1. **TypeScript Validation**: `npx -p typescript tsc --noEmit` - validates type safety
2. **Playwright Tests**: `npx playwright test` - runs all UI, API, and mocking tests
3. **Report Artifacts**: HTML report uploaded on every run (retention: 30 days)
4. **Trace Artifacts**: Trace files uploaded on failure (retention: 30 days)

### Quality Gates (Manual Configuration)

To enforce quality standards, the following checks can be configured as required in GitHub branch protection rules:

| Check | Command | Purpose |
|-------|---------|---------|
| TypeScript Validation | `npx -p typescript tsc --noEmit` | Ensures type safety before merge |
| Playwright Tests | `npx playwright test` | Ensures all tests pass before merge |
| SauceDemo UI Tests | `npx playwright test tests/saucemo.spec.ts` | Ensures UI automation passes |
| DummyJSON API Tests | `npx playwright test tests/api/products.spec.ts` | Ensures API tests pass |

**Note**: These checks are documented here for reference but are NOT automatically enabled. To enable them as required checks, configure branch protection rules in your GitHub repository settings.

### Local Validation

Before pushing, validate locally:
```bash
cd starter
npm ci
npx -p typescript tsc --noEmit
npx playwright test
```

### Viewing CI Results

After pushing or opening a PR:
1. Go to the **Actions** tab in your GitHub repository
2. Click on the latest **Playwright Tests** workflow run
3. Download artifacts (`playwright-report` for HTML report, `playwright-trace` for failure traces)
