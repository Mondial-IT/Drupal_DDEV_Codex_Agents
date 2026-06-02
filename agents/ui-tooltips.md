

# `/codex/agents/ui-tooltips.md`

# Tooltip System Agent

## Scope

Activates only when the task involves:

* Generating tooltip markup
* Theming tooltip CSS
* Using BlueMarloc tooltip system
* Creating Twig patterns for tooltips

Never activates for general UI rules, code generation, testing, or README production.

## Rules

### When to use

* Brief inline explanations
* CSS-only solutions
* Designer-specified UI patterns
* Environments where JS is disallowed

### Twig pattern

```
{{ tooltip('Help text', 'Label', {
  theme: 'brand',
  position: 'top',
  edge: true,
  tabindex: 0
}) }}
```

### Core rules

* Always store text in `data-tip=""`.
* Never place content in CSS.
* Always attach the `bm_tooltip/tooltip` library.
* Use `tooltip--is-parent` for nested tooltips.
* Only LTR supported.
* Always include `tabindex="0"` unless non-interactive.

### When NOT to use

* Long text
* Interactive content
* Complex dynamic positioning

### Theming rules

* Tooltip CSS lives in `theme/css/components/tooltip.css`.
* Library example:

  ```yaml
  yourtheme.tooltip:
    css:
      theme:
        css/components/tooltip.css: {}
  ```
* Attach via preprocess or Twig.

### Accessibility

* Add `aria-label=""` if content is essential.
* Use `role="tooltip"` when appropriate.
