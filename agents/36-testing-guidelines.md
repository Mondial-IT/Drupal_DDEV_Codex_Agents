# `/codex/agents/36-testing-guidelines.md`

# Testing Agent

## Scope

Activates when writing or updating PHPUnit coverage (Kernel, Functional, Browser) for Drupal 11 modules. Does **not** define CI workflows—that belongs to `37-testing-ci.md`.

## Rules

1. **How to run tests**
   ```bash
   ddev exec phpunit -c web/core/phpunit.xml.dist modules/custom/<module>/tests
   ```
   - Provide module-specific paths so suites stay fast and isolated.
   - Prefer the curated runner when you need to mix suites or inspect logs:
     ```bash
     ddev test unit|kernel|functional|module <name>|module all
     ddev test -vvv module bm_aggrid   # prints each class::method + file before execution
     ```
     * `-v` = runner messages only, `-vv` = stream PHPUnit output, `-vvv` = list every test method (module, class, file:line) before it runs.
     * Runner outputs live under `.ddev/phpunit_logs/` (`phpunit_master.log`, per-test logs, `summary.tsv`, optional coverage HTML). Reference these files when sharing failures with teammates.

2. **Directory layout & naming**
   - Kernel tests → `tests/src/Kernel`
   - Functional/browser tests → `tests/src/Functional`
   - Test class naming: `<ModuleName><Behavior>Test`

3. **State management**
   - Reset mutated config/content in `tearDown()`.
   - Mock remote or rate-limited services; never leave outbound HTTP calls in tests.

4. **Regression policy**
   - Every bug fix must ship with at least one regression test proving the failure and the fix.
   - Exercise new service methods, routes, and forms to keep baseline coverage intact.

5. **Best practices**
   - Use Drupal’s traits (e.g., `KernelTestBase`, `BrowserTestBase`) instead of reinventing bootstraps.
   - Prefer dependency injection/mocks over global state.
   - Document any data fixtures inside the test class so contributors can understand the scenario quickly.
