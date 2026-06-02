
A demo page is created by implementing a Drupal form with the following rules to create consistent demo pages.

* Start with a fieldset that explains the demo: title “How to use {component}”, typically a details element open by default.
* It should explain the drupal implementation
  * the form elements, form definitions,
  * and example markup/css when the user is expected to work with the markup/css specifically.
* Follow with as many topic fieldsets as needed to fully explain the demo; each has a clear topic title.
* In each topic fieldset, render a set of example items as rows.
* Attach the component’s library on the form (`$form['#attached']['library'][] = 'module/library'`) so CSS/JS is present.
* Use `#type => container` rows with `example` and `code` children; give the container a class like `bm-demo-row` to align example left, code right (CSS handles layout).
* When embedding markup, use `Markup::create()` for trusted snippets and `Html::escape()` for code samples to avoid XSS.
* Prefer theme hooks/components (`#theme` => …) instead of custom HTML where available, to exercise the real output.
* Optional toggles/controls (e.g., theme switch) should be AJAX-enabled and stay inside the same form wrapper.

Each example item consists of a `#type=>container` which contains an `example` element and a `<code>` element.

**When in doubt of how to implement a feature, inform the user of this agent instruction, and ask for input.**


Example form php:

```
      'dark' => [
        '#type' => 'container',
        '#attributes' => ['class' => ['bm-demo-row']],
        'example' => [
          '#markup' => Markup::create('<span class="tooltip" data-tippy-content="Dark theme (default)" data-bm-tooltip-theme="dark">Dark theme</span>'),
        ],
        'code' => [
          '#markup' => '<code>&lt;span class="tooltip" data-tippy-content="Dark theme (default)" data-bm-tooltip-theme="dark"&gt;Dark theme&lt;/span&gt;</code>',
        ],
      ],
      'light' => [
        '#type' => 'container',
        '#attributes' => ['class' => ['bm-demo-row']],
        'example' => [
          '#markup' => Markup::create('<span class="tooltip" data-tippy-content="Light theme" data-bm-tooltip-theme="light">Light theme</span>'),
        ],
        'code' => [
          '#markup' => '<code>&lt;span class="tooltip" data-tippy-content="Light theme" data-bm-tooltip-theme="light"&gt;Light theme&lt;/span&gt;</code>',
        ],
      ],
      'brand' => [
        '#type' => 'container',
        '#attributes' => ['class' => ['bm-demo-row']],
        'example' => [
          '#markup' => Markup::create('<span class="tooltip" data-tippy-content="Brand theme" data-bm-tooltip-theme="brand">Brand theme</span>'),
        ],
        'code' => [
          '#markup' => '<code>&lt;span class="tooltip" data-tippy-content="Brand theme" data-bm-tooltip-theme="brand"&gt;Brand theme&lt;/span&gt;</code>',
        ],
      ],
    ];
```
* The css displays the example code on the left and the code element on the right.
* Add a css file for each demo form with the content as displayed below;
* ensure to use the correct identifier indicated by {module} hereunder.
* Keep this CSS in a demo-specific file (e.g., `css/{Component}DemoForm.css`)
* and attach it via a dedicated library so you don’t clutter the component’s production styles.
* use the below displayed css verbatim but update the start class with the form id class.

```css
.{module}-demo-admin-form {
  .fieldset {
    .fieldset__label {
      border-bottom: 1px solid var(--input-border-color);
      background: var(--color-gray-050);
    }
    .form-wrapper {
      display: grid;
      grid-template-columns:500px 500px;
      gap:1em;
      input{
        height: 2em;
        width: 400px;
        padding: 1em;
        border: 1px solid var(--input-border-color);
      }
      button{
        background-color: var(--button--active-bg-color--primary);
        color: var(--button-fg-color--primary);
      }
      code {
        font-family: 'system-ui', sans-serif;
        font-size: 0.9em;
        padding: 20px;
        margin-block: var(--space-m);
        color: var(--color-text);
        border: var(--details-border-size) solid var(--details-border-color);
        border-radius: var(--details-border-size-radius);
        background-color: var(--color-white);
        box-shadow: var(--details-box-shadow);
      }
    }
  }
}
```
