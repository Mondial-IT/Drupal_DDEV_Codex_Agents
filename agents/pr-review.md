
# `/codex/agents/pr-review.md`

# Commit & PR Agent

## Scope

Activates only when the task involves:

* Writing commit messages
* Reviewing PRs
* Explaining PR expectations

Never applies to coding rules, testing, README creation, etc.

## Rules

* Commit subject style:
  `<scope> - <action>`
  Example:
  `multisite - added drush aliases definition`
* Keep subjects < 72 chars.
* PR must contain:

  * Problem summary
  * Fix explanation
  * DDEV reproduction steps
  * Linked issue IDs
  * Screenshots for UI
  * Required commands (`drush updb`, `drush cim`)
  * Results of phpcs + phpunit

---
