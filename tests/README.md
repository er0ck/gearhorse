# End-to-End Tests

Playwright tests for the Gearhorse landing page.

## Requirements

- Node.js 18+
- npm

## Setup

```sh
cd tests/
npm install
npx playwright install chromium
```

## Running locally

Run against a local dev server (default: `http://localhost:3000`):

```sh
npm test
```

Run against a specific URL:

```sh
BASE_URL=https://staging.gearhorse.com npm test
```

Run with a visible browser window:

```sh
npm run test:headed
```

Open the HTML report after a run:

```sh
npm run test:report
```

## Configuration

The base URL is controlled by the `BASE_URL` environment variable. It defaults
to `http://localhost:3000` when not set.

All other Playwright options live in [playwright.config.ts](playwright.config.ts).

## GitHub Actions

The workflow at [../.github/workflows/e2e.yml](../.github/workflows/e2e.yml)
runs these tests on every push and pull request. Set the `BASE_URL` secret (or
variable) in your repository settings to point at the environment you want to
test against:

1. Go to **Settings > Secrets and variables > Actions**.
2. Add a repository variable named `E2E_BASE_URL` with your target URL
   (e.g. `https://www.gearhorse.com`).

The workflow reads that variable and passes it to Playwright as `BASE_URL`.
