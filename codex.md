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

* README.md
* code-generation.md
* code-style.md
* dev-workflow.md
* documentation.md
* meta-governance.md
* pr-review.md
* readme-production.md
* repository-structure.md
* script-communication.md
* security.md
template.md
* testing-ci.md
* testing-guidelines.md
* ui-tooltips.md
---

## How To Use the Codex

### 1. When performing a task…
Determine which agent scope applies:

- Writing code? → **Code Generation Agent**
- Writing a README? → **README Production Agent**
- Adjusting project layout? → **Repository Structure Agent**
- Writing tests? → **Testing Agent**
- Reviewing PRs? → **PR/Commit Agent**
- Updating environment rules? → **Environment & Security Agent**
- Creating UI tooltips? → **Tooltip Agent**
- Updating CI workflows? → **Testing & CI Agent**
- Editing the Codex itself? → **Meta-Governance Agent**

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
