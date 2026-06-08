# Codex Quick Context

This file is a short entrypoint for the active Codex rules in this repository.
It is not a replacement for the scoped agent files. It exists so an AI can
orient quickly before reading the narrower rules.

## Current focus

- Drupal 11 development in this repository
- Host-side init/install automation under `.scripts/.vpscontainer/`
- Web control panel work under `.scripts/web_install_control_panel/`
- Documentation and repo layout changes under `codex/agents/`

## Read first

1. `codex/agents/README.md`
2. `codex/agents/codex.md`
3. `codex/agents/10-meta-governance.md`
4. The agent file that matches the task scope

## Common agent entry points

- `20-code-generation.md` for Drupal/PHP/JS/Twig implementation
- `22-drupal-rules.md` for shared Drupal safeguards
- `30-dev-workflow.md` for DDEV, Composer, and Drush routines
- `31-repository-structure.md` for directory layout and embedded repos
- `38-init-and-install-scripts.md` for host-side init/install shell automation
- `32-security.md` for secrets and permissions
- `36-testing-guidelines.md` and `37-testing-ci.md` for verification
- `33-documentation.md` and `34-readme-production.md` for docs
- `10-meta-governance.md` for Codex structure and agent maintenance

## Visibility rule

When Codex starts substantive work, it should state which agent(s) it is using.
When it switches scope, it should say so explicitly.

## Maintenance rule

If any file under `codex/agents/` changes, update this quick-context file in the
same change so the summary stays aligned with the active agent set.
