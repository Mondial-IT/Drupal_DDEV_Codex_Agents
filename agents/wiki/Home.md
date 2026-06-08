<img src="https://bluemarloc.com/bpiofiles/default_images/logos/mondial-it.nl.png" alt="logo mondial-it.nl">

# Mondial-IT Drupal 11 Codex

This wiki documents the repository-specific Codex agent rules for the Mondial-IT Drupal 11 SaaS CMS platform.

The Codex is not a generic Drupal tutorial. It is the working instruction system for AI-assisted development in this repository: how code should be generated, where files belong, how scripts communicate, how tests are run, and how documentation stays aligned with the modules.

## Main Goal

The primary goal is predictable Drupal 11 code generation:

- PHP 8.3 compatible custom modules.
- Drupal 11 APIs, services, plugins, render arrays, config schema, and cacheability.
- Safe output, explicit access checks, and no deprecated API additions.
- Modern Drupal behaviors and library attachment for JavaScript/CSS.
- Help topics, README updates, and tests included when user-facing behavior changes.

## Code Generation Priority

When Codex changes executable code, apply the rule files in this order:

1. `20-code-generation.md` - core contract for PHP, JS, Twig, services, plugins, forms, access, cacheability, and tests.
2. `22-drupal-rules.md` - shared Drupal 11 implementation safeguards.
3. Specific Drupal rule files - forms, help topics, demo pages, styling surfaces, and tooltips.
4. `21-code-style.md` - formatting, naming, Twig/YAML conventions.
5. `36-testing-guidelines.md` - test placement and verification.
6. `30-dev-workflow.md` - DDEV, Composer, Drush, and permissions workflow.

The specific rule wins when rules overlap.

## Practical Guardrails

- Read the local module pattern before generating new abstractions.
- Prefer dependency injection over static service lookup.
- Prefer render arrays and Twig over manual HTML.
- Attach CSS/JS only through `.libraries.yml`.
- Add cache metadata whenever output depends on context, permissions, config, or entities.
- Add config schema for custom config.
- Add or update help topics and README content when admin-facing behavior changes.
- Run verification from DDEV unless the task is documentation-only.

## Agents or Skills?

These files should remain repository agents.

They encode local architecture, module naming, DDEV commands, Blue Marloc helper modules, and client-specific conventions. Those rules must travel with the repository.

Create Codex skills only for reusable workflows that make sense outside this project, such as:

- a reusable Drupal module scaffold workflow;
- a cross-repository GitHub sync/status workflow;
- an installer for Codex agent bundles across Drupal projects.

Do not convert repository-specific implementation rules into global skills.

## Maintenance Notes

- Keep agent scopes narrow.
- Update `README.md` and this wiki when the agent map changes.
- Do not duplicate detailed rules across files; link to the more specific agent instead.
- When Drupal 11 APIs change, update `20-code-generation.md` and `22-drupal-rules.md` first.
