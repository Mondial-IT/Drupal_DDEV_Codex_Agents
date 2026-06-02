
## bm_tooltip rules

Use these rules when adding tooltips in PHP, Twig, or JS.

### Required steps
- Read `bm_tooltip/README.md` before implementing new tooltip UI.
- Attach the shared library once per render array: `bm_tooltip/tooltip`.
- Prefer `bm_tooltip` naming (`bm-tooltip`, `data-bm-tooltip-*`) to avoid clashes with other Drupal tooltip patterns.

### Twig usage
- Use the Twig helper: `{{ tooltip('Tip', 'Label', { theme: 'brand', position: 'right' }) }}`.
- Keep tooltip content short; use line breaks or HTML only when needed.

### PHP usage
- Use the service `bm_tooltip.tooltip_service` for programmatic markup.
- For inline help icons, call `TooltipService::buildIcon($tip, $options)` and pass `theme` + `position` as needed.
- Always escape dynamic tooltip text before injecting into non-service markup.

### Data attributes
- Use `data-bm-tooltip-content` for tooltip text.
- Use `data-bm-tooltip-theme` and `data-bm-tooltip-placement` to control appearance and placement.
