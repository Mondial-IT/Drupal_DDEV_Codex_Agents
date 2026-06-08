# `/codex/agents/21-code-style.md`

# Coding Style Agent

## Scope

Activates only when tasks involve conventions (formatting, naming, file placement) for PHP, JS, Twig, or YAML inside this repo. Does **not** cover build steps, testing, or README prose.

## Rules

### Baseline
- Follow PSR-12 and Drupal coding standards for PHP 8.3.
- Run `ddev exec phpcs web/modules/custom --standard=Drupal,DrupalPractice` before delivery.
- Organize classes under each module’s `src/` namespace; file paths must match the namespace exactly.

### Twig & theme structure
- Twig templates use snake_case filenames and live under `web/themes/custom/<theme>/templates` (or the module’s `templates/`).
- Drupal help-topic Twig files **must** start with YAML front matter containing at least `label`, optionally `top_level` / `related`.
- Use `{{ attach_library('module/library') }}` for CSS/JS dependencies; avoid inline assets where possible.

### YAML conventions
- Two space indentation everywhere (services, routing, libraries, config exports).
- `.services.yml` entries reference classes without surrounding quotes and use single backslashes (e.g. `class: Drupal\bm_panels\PanelService`).
- Prefer `extension.path.resolver` for locating module/theme paths instead of calling `drupal_get_path()`.

### Naming & examples
- Private constants use typed declarations, e.g. `private const COUNT_STATE_KEY = 'bm_panels.count';`.
- Factory `create()` methods must include strict typing and docblocks describing injected services.
- Service IDs, form element keys, and CSS classes stay lowercase snake_case unless Drupal core mandates camelCase.

### Front-end specifics
- JavaScript relies on modern ES modules / Drupal behaviors; avoid jQuery unless Drupal core requires it.
- CSS variables default to ASCII names and, when they are provided from markup (e.g. `style="--bm-panel-columns"`), declare them via `@property` to keep IDEs quiet.
- Tooltips use the shared `bm_tooltip` patterns described in `39-ui-tooltips.md`; do not embed tooltip text directly in CSS.
