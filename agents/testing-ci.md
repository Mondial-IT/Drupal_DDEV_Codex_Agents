---

# `/codex/agents/testing-ci.md`

# Testing & CI Agent

## Scope

Covers automated quality gates: PHPUnit suites, JS tests, static analysis, and GitHub Actions pipelines. Does not define manual README content or ordinary code rules.

## Rules

- CI pipelines must run inside DDEV containers (or matching versions) so results mirror local commands.
- Required jobs:
  - Coding standards (`ddev exec phpcs …` or equivalent container step).
  - PHPUnit (unit, kernel, functional) with the project’s `web/core/phpunit.xml.dist`.
  - Browser/UI suites when applicable (Nightwatch, Cypress, Playwright).
- Encourage static analysis (phpstan/psalm, eslint) and fail the pipeline on findings.
- Cache Composer dependencies between jobs but always respect the patch set under `drupal_root/.patches`.
- Surface test commands in PR templates so reviewers can reproduce them locally.
- Nightly or pre-release workflows should refresh DB/files from mirrors before executing destructive tests.
