
# `/codex/agents/code-generation.md`

# Code Generation Agent

## Scope

Engages only when generating or modifying executable code (PHP, JS, Twig) inside the Drupal 11 project. Does **not** apply to documentation, CI definitions, or README writing.

## Rules

- Target PHP 8.3 and Drupal 11 APIs. Avoid deprecated hooks/classes/functions and check the surrounding module for the established Drupal 11 pattern before adding code.
- Prefer services, plugins, controllers, forms, event subscribers, and access checkers with dependency injection. Avoid `\Drupal::service()`, `\Drupal::config()`, and other container lookups except in procedural hooks or thin legacy boundaries where dependency injection is not available.
- Use strict typing in new PHP classes: `declare(strict_types=1);`, typed properties, typed method signatures, constructor property promotion where it improves clarity, and `final` for classes that are not designed for extension.
- Respect Drupal architecture: route definitions in `.routing.yml`, permissions in `.permissions.yml`, menu links in `.links.menu.yml`, service definitions in `.services.yml`, config schema for config forms, and plugin metadata using the style already present in that plugin family.
- Return render arrays from controllers/forms where possible. Do not manually concatenate HTML except inside narrow Drupal help hooks where existing code requires it; escape translated placeholders correctly.
- Preserve cacheability. When output depends on config, route params, entities, permissions, or contexts, add appropriate cache tags, cache contexts, and max-age metadata.
- Use Drupal access APIs. Do not hide security-sensitive operations only in UI visibility; enforce permissions in routes, access checkers, forms, or services.
- Use entity/query APIs, database placeholders, and typed data APIs. Do not build SQL, URLs, links, or markup through unsafe string concatenation.
- Use configuration APIs for deployable settings and State API only for runtime/local state. Never put deployable settings only in State.
- Use modern Drupal behaviors for JavaScript: attach behaviors with `(context, settings)`, use `once()` for idempotency, and attach dependencies through `.libraries.yml`. Only rely on jQuery when the existing Drupal API or core dependency requires it.
- Attach assets through libraries only. Use `module_name/library_name` exactly as defined in `.libraries.yml`.
- Keep output minimal when asked for code: provide the change, not broad explanation, unless the user asks for design rationale.
- Include focused tests when behavior changes. Prefer unit/kernel/functional coverage based on the risk and existing module test style.
- When wiring help topic templating, forms, tooltips, or panels, reuse shared helpers such as `bm_tooltip`, `bm_notify`, `bm_core\Form\AjaxMessageFormBase`, and `bm_main_panel_title_and_help()` instead of duplicating local variants.

## Generation Checklist

Before handing off generated Drupal code, verify:

- No deprecated Drupal APIs were introduced.
- All user-visible strings are translatable with placeholders, not concatenated variables.
- User-provided output is escaped by render arrays, Twig autoescape, or explicit Drupal escape APIs.
- Form, route, controller, and AJAX callbacks enforce permissions.
- Libraries are declared in `.libraries.yml` and attached with the exact `module/library` key.
- Config forms include config schema and use editable config for deployable settings.
- AJAX responses preserve state inside the replaced wrapper and do not depend on refreshed `drupalSettings`.
- Any new service is registered, injected, and covered by a narrow test or a documented manual verification step.
