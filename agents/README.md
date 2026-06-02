# `/codex/agents/README.md`

# Agents Overview

This directory contains the scoped rule sets that govern Codex inside the Life After Me DDEV workspace.
Each document activates only when a task matches its scope (code generation, testing, documentation, etc.) and must **never** override another agent’s domain.

All commands referenced here assume you are operating inside the project DDEV container (`ddev start`).

## Quick reference

| Agent | Purpose | Primary Triggers |
| --- | --- | --- |
| `code-generation.md` | Authoring PHP/JS/Twig that targets Drupal 11 on PHP 8.3. | Creating or editing executable code. |
| `code-style.md` | Naming, formatting, help-topic headers, and structural conventions. | Enforcing PSR-12, Twig/YAML guidance, or template rules. |
| `dev-workflow.md` | DDEV, Composer, and Drush routines. | Running local builds, refreshing installs, permissions repair. |
| `repository-structure.md` | Layout of `drupal_root/`, multisite assets, embedded repos. | Moving files or explaining directory intent. |
| `documentation.md` / `readme-production.md` | Narrative docs vs. README expectations. | Writing docs (non-README) or README refreshes. |
| `pr-review.md` | Commit message + PR checklist. | Preparing commits, reviewing PRs. |
| `testing-guidelines.md` / `testing-ci.md` | Manual PHPUnit authoring vs. CI automation. | Adding regression tests or pipelines. |
| `security.md` | Application + environment security. | Handling secrets, reviewing permissions, or file-storage safety. |
| `script-communication.md` | Output contract for `.scripts/` automation. | Authoring DevOps scripts in `.scripts/.ds03container`. |
| `ui-tooltips.md` | Usage of the shared `bm_tooltip` patterns. | Building tooltip markup or styling panels. |
| `meta-governance.md` & `template.md` | How to evolve this system or add new agents. | Adjusting Codex itself or drafting another agent. |

## Maintaining the Codex

1. Copy `TEMPLATE.md` when creating a new agent and keep its scope laser-focused.
2. Link every new agent from the table above so contributors can discover it quickly.
3. Update existing agents only when repository-wide processes change (DDEV commands, directory layout, etc.).
4. If you need automation (linting, tree generation, enforcement scripts), define it through `meta-governance.md` so other agents remain independent and auditable.
