---
scope: Drupal 11 code rules shared by custom modules and themes
---

# Drupal 11 Rules

This file contains Drupal-specific implementation rules that support `20-code-generation.md`.
It applies when changing code under `drupal_root/web/modules/custom` or `drupal_root/web/themes/custom`.

## Drupal 11 Baseline

- PHP target is 8.3; Drupal target is 11.x as installed by this repository.
- Prefer current Drupal 11 APIs and check core change records before introducing unfamiliar APIs.
- Match existing module architecture first. Do not introduce a new abstraction if the surrounding module already has a clear service/form/plugin pattern.
- Keep custom code compatible with Composer-managed Drupal. Do not modify core or vendor files directly.
- Treat custom modules as deployable packages: code, config schema, libraries, help topics, README, and tests belong with the module.

## Safe Drupal Output

- Prefer render arrays and Twig templates over manual HTML strings.
- Translate user-visible strings with `t()`/`$this->t()` and placeholders.
- Use escaped placeholders for plain text (`@name`) and only use markup placeholders when the value is intentionally safe markup.
- Add cacheability metadata to render arrays that depend on config, permissions, entities, route params, language, theme, or user context.
- Enforce access in routes, access callbacks/checkers, forms, and services. UI hiding alone is not access control.

## Services and Dependency Injection

- Inject services through constructors and `ContainerInjectionInterface`/`create()` where Drupal requires factories.
- Register reusable services in `.services.yml`.
- Avoid static service calls (`\Drupal::...`) except in procedural hooks, update hooks, or narrow compatibility boundaries.
- For filesystem paths, prefer injected services such as `extension.path.resolver` over deprecated helpers.

## Configuration and State

- Use config entities or editable config for deployable settings.
- Provide config schema for every custom config key.
- Use State API only for environment-local runtime values that should not deploy.
- Do not store secrets in config, state, code, README examples, or test fixtures.

## JavaScript and Assets

- Use Drupal behaviors with `(context, settings)` and `once()` so AJAX reattachment is safe.
- Declare dependencies in `.libraries.yml`; do not rely on incidental global scripts.
- Attach libraries through render arrays or Twig, never by emitting ad hoc `<script>` or `<link>` tags.

## Library attachment rule

Always attach Drupal libraries as `module_machine_name/library_machine_name`, matching the exact key from `.libraries.yml`. Skipping the suffix prevents Drupal from locating the CSS/JS file, so nothing is added to the `<head>`.

### Correct
```php
$form['#attached']['library'][] = 'bm_config_sync/bm_config_sync.admin_styles';
```

### Incorrect
```php
$form['#attached']['library'][] = 'bm_config_sync/admin_styles';
```

### Sample `.libraries.yml`
```yaml
bm_config_sync.admin_styles:
  version: 1.0
  css:
    theme:
      css/bm_config_sync_admin.css: {}
```

## Form weight handling

- Drupal respects element `#weight` only; relative insertion order is not guaranteed once other alters/processors run. Always set an explicit `#weight` (or insert before a known sibling) rather than relying on array order.
- When placing elements near existing items (e.g., before a table built from a fieldset like `links`), read the target element’s weight and set your element’s weight slightly lower (e.g., `links` at `0.04` → set to `0.035`) to ensure placement.

## Off-canvas dialog links

To open links in Drupal’s off-canvas sidebar:
- Add `use-ajax` plus dialog attributes: `data-dialog-type="dialog"`, `data-dialog-renderer="off_canvas"`, and optional `data-dialog-options` (e.g., `{"position":"end","width":"40%"}`).
- Ensure the attached library depends on `core/drupal.ajax`, `core/drupal.dialog.ajax`, and `core/drupal.dialog.off_canvas`.
- Generate links with render arrays (`#type: link`, `#url`, `#title`) and avoid client-side injection of actions when server-side output can include the attributes.


## Related Rules

- 28-drupal-rules-for-demo-pages.md
- 23-drupal-rules-for-forms.md
- 29-drupal-rules-for-forms-bm_core__AjaxMessageFormBase.md
- 24-drupal-rules-for-help-topics.md
- 25-drupal-rules-for-styling-using-surfaces.md
