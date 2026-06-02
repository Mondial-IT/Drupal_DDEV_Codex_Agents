# `/codex/agents/dev-workflow.md`

# Build & Development Workflow Agent

## Scope

Activates when a task involves the DDEV toolchain, Composer/Drush lifecycles, or any local-environment maintenance for this Drupal 11 project.
Does **not** handle documentation, CI definitions, or commit/PR conventions.

## Rules

### 1. Always operate through DDEV
- Run `ddev start` before any other command so the PHP 8.3/Apache/MariaDB stack and SSH key loader hook are available.
- Use `ddev ssh` if you need an interactive shell; otherwise prefix commands with `ddev exec` / `ddev composer` / `ddev drush`.

### 2. Composer & dependencies
- Install dependencies via `ddev composer install`; this also applies the patches from `drupal_root/.patches`.
- Only run Composer scripts inside DDEV so `@check-env-and-user` passes (no host-side Composer operations).
- After syncing assets from elsewhere, run `ddev exec composer apply-permissions` to restore ACLs under `web/`.

### 3. Permissions helpers
- When touching files inside `drupal_root/web`, rerun `ddev exec composer apply-permissions` (or locally call `bash /home/docker_volume/git_root/.scripts/.ds03container/init_drupal_permissions.sh` when outside DDEV).

### 4. Drush utilities
- Cache rebuild: `ddev exec drush cr`.
- One-time login links: `ddev exec drush uli`.
- Use `ddev drush` for any other site-level maintenance so the correct site alias & PHP version load automatically.

### 5. Database & files refresh
- Import current DB snapshot with `ddev exec composer import-installation-database`.
- Pull the synchronized files mirror via `ddev exec composer get-files-repo` when media assets are stale.

### 6. Testing & verification hooks
- Launch PHPUnit from DDEV (`ddev exec phpunit -c web/core/phpunit.xml.dist modules/custom/<module>/tests`).
- For richer output and logging, use the bundled runner `ddev test …` (unit/kernel/functional/module). `-v/-vv/-vvv` control verbosity, and logs land in `.ddev/phpunit_logs/` (per-test log files, `summary.tsv`, and optional coverage HTML).
- For coding standards, run `ddev exec phpcs web/modules/custom --standard=Drupal,DrupalPractice` (see `code-style.md`).

### 7. Local bulk data
- Keep temporary imports/logs under `/database` and `/apache_logs` locally and delete them before pushing changes.

### 8. Embedded upstream repos
- If you work inside a nested module/theme that ships its own `update_and_push_to_github.sh`, run that script from the nested directory so the child upstream stays independent. Do **not** add those directories to this repo.

Follow this checklist before handing off work:
1. Stack started (`ddev start`).
2. Dependencies + patches installed (`ddev composer install`).
3. Permissions refreshed after touching assets.
4. DB/files refreshed if your change depends on them.
5. Cache rebuilt and tests/coding standards run from inside DDEV.
