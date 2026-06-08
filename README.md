<img src="https://bluemarloc.com/bpiofiles/default_images/logos/mondial-it.nl.png" alt="logo mondial-it.nl">

# Mondial-IT Drupal 11 Codex

This directory contains the repository-specific Codex instruction system for the Mondial-IT Drupal 11 SaaS CMS platform.

The Codex is used to keep AI-assisted development aligned with the local Drupal architecture, DDEV workflow, embedded Git repositories, documentation rules, and module conventions.

## What This Codex Optimizes

- Drupal 11 and PHP 8.3 compatible code generation.
- Services, dependency injection, plugins, forms, render arrays, cacheability, config schema, and access checks.
- Modern Drupal JavaScript behaviors with library-based asset attachment.
- Consistent help topics, module README files, and wiki pages.
- DDEV-based Composer, Drush, PHPUnit, phpcs, and permission workflows.
- Safe Git handling for nested module/theme repositories.

## Agent Map

The scoped agent files live in `codex/agents`.
The numbered filenames are ordered from broader layers to more specific overrides, so lower numbers should be read first.

Start with:

- `codex/agents/codex.md` for the overall model and scope isolation.
- `codex/agents/README.md` for the active agent map.
- `codex/agents/20-code-generation.md` for executable Drupal code.
- `codex/agents/22-drupal-rules.md` for shared Drupal 11 implementation safeguards.
- `codex/agents/30-dev-workflow.md` for DDEV, Composer, Drush, and permissions.
- `codex/agents/36-testing-guidelines.md` for test placement and execution.

## Working Model

Each agent has a narrow scope. Do not merge all rules into one prompt:

- Code generation rules do not control README writing.
- README rules do not control PHP architecture.
- CI rules do not control tooltip markup.
- Meta-governance rules only apply when changing the Codex itself.

When scopes overlap, use the more specific rule file.

## Agents or Skills?

Keep these instructions as repository agents. They are tied to this codebase and should be versioned with it.

Use Codex skills only for reusable workflows that should apply across repositories, for example:

- installing a standard Codex agent bundle;
- scaffolding a generic Drupal module;
- running a cross-project GitHub sync/status workflow.

Do not turn client-specific architecture, module naming, Blue Marloc helper usage, or DDEV paths into global skills.

## Maintenance

- Update `codex/agents/README.md` when adding or removing agent files.
- Update `codex/agents/wiki/Home.md` when the overall Codex model changes.
- Keep detailed implementation rules in the most specific agent file.
- Check official Drupal coding standards and Drupal core change records when Drupal 11 APIs evolve.
