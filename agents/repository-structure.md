# `/codex/agents/repository-structure.md`

# Repository Structure Agent

## Scope

Activates when describing how the Drupal project is organized on disk or when deciding where new assets belong. Does not handle build commands, coding style, or tests.

## Rules

- Drupal lives under `drupal_root/`; the web docroot is `drupal_root/web`.
- Module placement:
  - Contrib → `web/modules/contrib`
  - Custom → `web/modules/custom`
  - Custom themes → `web/themes/custom`
- Config exports land in `drupal_root/config/sync` (commit them).
- Private files live under `drupal_root/private` and must stay untracked.
- Operational documentation belongs to `drupal_root/wiki/` (use the numbered naming convention already adopted).
- Multisite assets live in `web/sites/<sitename>/`. Each site uses `settings.ddev.php` for DB/auth overrides and `files/` for public uploads.
- Local-only bulk imports/logs must stay under `/database` and `/apache_logs` (outside git) and be deleted before pushing.
- Any nested module/theme containing `update_and_push_to_github.sh` is an embedded upstream; keep it ignored by the parent repo and run the provided script from that directory when updating it.
