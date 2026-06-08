# Codex Overview

The Codex is the authoritative knowledge system for this repository.
It defines:
- Standardized development workflows
- Scoped agent rules
- Architecture and documentation conventions
- System-wide governance policies

All AI agents, contributors, and automation tools must follow the Codex.
Each section is isolated by design—rules apply **only within their defined scope** and must never bleed into other domains.

---

## Directory Structure

Each agent is defined in its own file under `/codex/agents/`, and each file declares its own **scope boundary**.
The leading numeric prefix is part of the hierarchy: lower numbers are broader and loaded first; higher numbers are more specific and should be treated as stronger overrides within their domain.

Start with `README.md` for the current agent map.
Use `00-quick-context.md` as the short-form entrypoint when you need a fast overview before reading the full agent files.

High-impact code-generation subjects:

* `20-code-generation.md` - Drupal 11 implementation contract.
* `22-drupal-rules.md` - shared Drupal 11 safeguards.
* `23-drupal-rules-for-forms.md` - form and AJAX form conventions.
* `24-drupal-rules-for-help-topics.md` - admin help-topic generation.
* `25-drupal-rules-for-styling-using-surfaces.md` - surface-based UI styling rules.
* `26-drupal-rules-for-tooltips-bm_tooltip.md` and `39-ui-tooltips.md` - shared tooltip usage.
* `21-code-style.md` - formatting and naming.
* `36-testing-guidelines.md` - test expectations.

Workflow and governance subjects:

* `30-dev-workflow.md`
* `31-repository-structure.md`
* `38-init-and-install-scripts.md`
* `32-security.md`
* `37-testing-ci.md`
* `35-pr-review.md`
* `33-documentation.md`
* `34-readme-production.md`
* `10-meta-governance.md`
* `00-quick-context.md`
* `11-template.md`
---

## How To Use the Codex

### 1. When performing a task…
Determine which agent scope applies:

- Writing code? → **Code Generation Agent**
- Writing Drupal code? → **Code Generation Agent**, then **Drupal Rules**, then any specific Drupal rule file.
- Writing a README? → **README Production Agent**
- Adjusting project layout? → **Repository Structure Agent**
- Writing tests? → **Testing Agent**
- Reviewing PRs? → **PR/Commit Agent**
- Updating environment rules? → **Environment & Security Agent**
- Creating UI tooltips? → **Tooltip Agent**
- Updating CI workflows? → **Testing & CI Agent**
- Editing the Codex itself? → **Meta-Governance Agent**

Before substantive work begins, say which agent(s) you are using. If the task crosses into a new scope, announce the switch.

If the task falls outside all scopes, use general documentation under `/codex/sections`.

---

## Rule Isolation Principles

1. **No agent may enforce rules outside its scope.**
2. When scopes overlap, the *more specific* agent overrides the general one.
3. Codex-wide rules live only in `/codex/sections/` and never inside agent files.
4. All agents must remain forward-compatible with:
  - PHP 8.3
  - Drupal 11.2.5
  - Modern JS (no jQuery unless required)
5. README and documentation rules cannot influence code generation.
6. Code style rules cannot influence PR or commit guidelines.
7. CI rules cannot influence tooltip generation, and vice versa.

This ensures clean, modular behavior whether executed by humans, automation scripts, or AI assistants.

---

## Adding New Agents

New agents must announce to user there is no instruction for them, inform user to create a file in `agents` and not proceed until there is a file made for it.
