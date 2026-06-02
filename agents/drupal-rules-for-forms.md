
Drupal forms need to be created with the following structure.
This structure is meant to facilitate a later stage refactoring of the form and be able to move related functionalities to separate forms.

## AJAX forms standard

- Unless explicitly stated otherwise, build forms as AJAX forms and extend `\Drupal\bm_core\Form\AjaxMessageFormBase`.
- This base disables Drupal’s auto message injection and provides a single controlled message container plus helpers (`renderMessagesAjax()`, `replaceWrapperAjax()`).
- Do not render `status_messages` yourself or target `[data-drupal-messages]`; use the base helpers instead.
- For toast-style notifications, use `bm_notify` (bm_drupal_enhancements): attach the library `bm_notify/notify`, queue messages via the `bm_notify` service (`addStatus/addWarning/addError/addInfo/addNotification`), and append them with `addToResponse($response)`. `AjaxMessageFormBase` already does this when rendering messages.

## AJAX rebuilds and drupalSettings

- Drupal does not re-send `drupalSettings` on partial AJAX responses. If a behavior depends on fresh settings (e.g., AG Grid rows/page totals), include the data in the AJAX-replaced markup (hidden field/container) and read it client-side instead of relying on `drupalSettings` updates.
- When using AJAX wrappers that replace only part of the form, ensure required hidden inputs (state) are inside the wrapper so they are included in the response.

## other rules
* A first level: `fieldset`, `details` or `container` is in this document identified by the word `panel`.
* With 'first level' is meant the first wrapper within the form wrapper separating topics.
* Every panel needs to be defined in a separate method.
* The method should be named: panel_{identifier}
* When a panel has a `submit` and/or `validation`  then those are in a separate method named submit_{identifier} and validation_{identifier}
* Every form should be fully based on ajax dynamics.
* Every form should have an introduction doc block explaining the form
* Every method should have clear explanation Markdown doc block
* The code should have ample comment to explain what is happening.
* Every form should have an explanation in a specific help_topic in that module.
  * When help_topics directory is not defined, then follow the rules from document 'documentation.md'
* Every form should have a mention in the README.md of that module.
* Every form uses bm_tooltip to add descriptions and help information to the user using an icon.
* Every 'panel' uses the function from `bm_main.module`: `function bm_main_panel_title_and_help($title, $helpText): MarkupInterface|string ` to create a title, with help icon and panel description text.
