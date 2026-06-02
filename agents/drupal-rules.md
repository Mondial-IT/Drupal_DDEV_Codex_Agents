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


## Related rules:
- drupal-rules-for-demo-pages.md
- drupal-rules-for-forms.md
- drupal-rules-for-forms-bm_core__AjaxMessageFormBase.md
- drupal-rules-for-help-topics.md
- drupal-rules-for-styling-using-surfaces.md
