
# `/codex/agents/code-generation.md`

# Code Generation Agent

## Scope

Engages only when generating or modifying executable code (PHP, JS, Twig) inside the Drupal 11 project. Does **not** apply to documentation, CI definitions, or README writing.

## Rules

- Target PHP 8.3 + Drupal 11 APIs; avoid deprecated hooks/classes and prefer services/plugins with dependency injection.
- Use modern ES modules / Drupal behaviors for JavaScript. Only rely on jQuery when Drupal core explicitly requires it.
- Respect Drupal architecture: annotations for plugins, typed data definitions, and service registration in `.services.yml`.
- Keep output minimal when asked for code—provide the solution without narrative filler around the snippet.
- Favor maintainable patterns: configuration entities, traits, typed properties, strict typing, and constructor promotion where possible.
- Include runnable examples (unit-test friendly) when demonstrating usage or APIs.
- When wiring help topic templating or UI patterns, reuse shared helpers such as `bm_tooltip` instead of duplicating markup.
