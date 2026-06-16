# gha-reusable-nodejs

Reusable GitHub Actions workflow for Node.js projects: a **lint + unit-test**
quality gate, with an optional **end-to-end** job and optional **code coverage**
upload. All commands are configurable so it fits any project that exposes npm
scripts.

## Usage

Call it from a workflow with `jobs.<id>.uses`:

```yaml
jobs:
  ci:
    uses: danielpadua/gha-reusable-nodejs/.github/workflows/ci.yml@latest
    with:
      node-version: "20.19.1"
      run-e2e: true
      coverage: true
```

Reference it by the moving `@latest` tag, or pin to `@main`.

## Inputs

| Input                 | Type    | Default                                          | Description                                              |
| --------------------- | ------- | ------------------------------------------------ | -------------------------------------------------------- |
| `node-version`        | string  | `20.x`                                           | Node.js version to use.                                  |
| `install-command`     | string  | `npm ci`                                          | Command to install dependencies.                         |
| `lint-command`        | string  | `npm run lint`                                    | Command to run the linter.                               |
| `test-command`        | string  | `npm test`                                        | Command to run unit tests (when `coverage` is `false`).  |
| `coverage`            | boolean | `false`                                           | Generate and upload a code coverage report.              |
| `coverage-command`    | string  | `npm run test:coverage`                           | Command used to run tests with coverage.                 |
| `coverage-path`       | string  | `coverage`                                         | Path of the coverage report uploaded as an artifact.     |
| `run-e2e`             | boolean | `false`                                           | Run the end-to-end job.                                  |
| `e2e-install-command` | string  | `npx playwright install --with-deps chromium`    | Command to install e2e browsers/dependencies.            |
| `e2e-command`         | string  | `npm run test:e2e`                                | Command to run end-to-end tests.                         |
| `runs-on`             | string  | `ubuntu-latest`                                  | Runner to execute the jobs on.                           |

## Jobs

- **`lint-test`** — install → lint → test. When `coverage` is `true`, runs
  `coverage-command` instead of `test-command` and uploads `coverage-path` as a
  `coverage` artifact (e.g. an `lcov.info` ready for Sonar later).
- **`e2e`** — runs only when `run-e2e` is `true`: install → e2e setup → e2e.

## Conventions

- Uses the vendored actions from
  [`danielpadua/gha-actions-dependencies`](https://github.com/danielpadua/gha-actions-dependencies)
  (`checkout`, `setup-node`, `upload-artifact`).
- Branch model: `main`, with a moving `latest` tag — same as the other
  `gha-reusable-*` repositories.
