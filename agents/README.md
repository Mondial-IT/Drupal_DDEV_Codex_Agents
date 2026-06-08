# `/codex/agents/README.md`

# Agents Overview

This directory contains the scoped rule sets that govern Codex inside this Drupal 11 DDEV workspace.
Each document activates only when a task matches its scope (code generation, testing, documentation, etc.) and must **never** override another agent’s domain.
The numeric prefix reflects layer order: lower numbers are broader and read earlier, higher numbers are more specific and override narrower concerns later.

Most commands referenced here assume you are operating inside the project DDEV container (`ddev start`), except for dedicated host-side automation agents such as the init/install shell-scripting agent.

## Drupal 11 Code Priority

When generating or modifying executable Drupal code, use this order:

1. `20-code-generation.md` defines the implementation contract for PHP, JS, Twig, services, plugins, forms, render arrays, cacheability, access, and tests.
2. `22-drupal-rules.md` defines Drupal-specific safety rules shared by all custom modules and themes.
3. More specific Drupal rule files override general guidance for forms, help topics, demo pages, styling surfaces, and tooltips.
4. `21-code-style.md` governs formatting and naming after the implementation direction is clear.
5. `36-testing-guidelines.md` defines the expected verification path.

Do not merge these into one large prompt. The separation keeps code generation focused while still allowing narrow task-specific rules to override broad rules.

## Quick reference

Use `00-quick-context.md` first when you need a fast map of the active agent set.

| Agent | Purpose | Primary Triggers |
| --- | --- | --- |
| `20-code-generation.md` | Authoring PHP/JS/Twig that targets Drupal 11 on PHP 8.3 with services, render arrays, cacheability, access checks, and tests. | Creating or editing executable code. |
| `21-code-style.md` | Naming, formatting, help-topic headers, and structural conventions. | Enforcing PSR-12, Twig/YAML guidance, or template rules. |
| `22-drupal-rules.md` | Shared Drupal 11 implementation safeguards for custom modules and themes. | Applying Drupal API, output, config, JS, and library rules. |
| `30-dev-workflow.md` | DDEV, Composer, and Drush routines. | Running local builds, refreshing installs, permissions repair. |
| `31-repository-structure.md` | Layout of `drupal_root/`, multisite assets, embedded repos. | Moving files or explaining directory intent. |
| `33-documentation.md` / `34-readme-production.md` | Narrative docs vs. README expectations. | Writing docs (non-README) or README refreshes. |
| `35-pr-review.md` | Commit message + PR checklist. | Preparing commits, reviewing PRs. |
| `36-testing-guidelines.md` / `37-testing-ci.md` | Manual PHPUnit authoring vs. CI automation. | Adding regression tests or pipelines. |
| `32-security.md` | Application + environment security. | Handling secrets, reviewing permissions, or file-storage safety. |
| `38-init-and-install-scripts.md` | Shell contract for init and install automation. | Authoring `.scripts/` bootstrap, setup, and verification scripts. |
| `39-ui-tooltips.md` | Usage of the shared `bm_tooltip` patterns. | Building tooltip markup or styling panels. |
| `10-meta-governance.md` & `11-template.md` | How to evolve this system or add new agents. | Adjusting Codex itself or drafting another agent. |
| `00-quick-context.md` | Fast index of the active Codex rules and maintenance reminders. | Starting a task or checking which agent to read first. |

## Maintaining the Codex

1. Copy `11-template.md` when creating a new agent and keep its scope laser-focused.
2. Link every new agent from the table above so contributors can discover it quickly.
3. Update existing agents only when repository-wide processes change (DDEV commands, directory layout, etc.).
4. If any file under `codex/agents/` changes, update `00-quick-context.md` in the same change.
5. If you need automation (linting, tree generation, enforcement scripts), define it through `10-meta-governance.md` so other agents remain independent and auditable.
6. When Codex starts substantive work, it should explicitly say which agent(s) it is using, and it should announce scope changes when it switches to another agent.

## Agents or Skills?

Keep these files as repository agents. They are project-specific, depend on this Drupal codebase, and should be versioned with the modules they govern.

Convert a topic into a Codex skill only when it is reusable outside this repository and has a clear workflow, for example:

- installing or updating Codex agent bundles across multiple Drupal projects;
- generating a standard Drupal module scaffold independent of this repository;
- running a reusable PR/CI triage workflow across repositories.

Do not convert narrow repository rules, module conventions, or client-specific architecture decisions into global skills.
