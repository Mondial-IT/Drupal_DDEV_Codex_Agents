# General

**Rule: Every module in drupal_root/web/modules/custom should have the following comprehensive help information.**
- A module is defined as the directory where a .info.yml file is present.
- A module can be in a subdirectory of other modules.

**Rule: The help information consists of:

* help_topics - information specifically targeted to the admin user of the module.
* .module help - information specifically targeted to the admin user of the module.
* README.md - information for the installation manager and developers.
* wiki help - information for the public visiting GitHub and want to be informed about what the modules features are.


## about help_topics

* Drupal modules provide help using the help_topics directory, where twig files are placed with the help information.
structure:
  * module_root/`help_topics`
  * module_root/`help_topics/{modulename}.overview.html.twig`
  * module_root/`help_topics/{modulename}.help_topics.yml`


### Help topic YML requirements

Example from bm_tooltip module:

```yaml
bm_tooltip.overview:
  title: 'BM Tooltip overview'
  body: help_topics/bm_tooltip.overview
  weight: 0
bm_tooltip.twig_helper:
  title: 'Using the bm_tooltip Twig helper'
  body: help_topics/bm_tooltip.twig_helper
  parent: bm_tooltip.overview
  weight: 10
```

### Help topic Twig requirements

- Help topic Twig files must include a front-matter–style comment with a `label:` (and optionally `description:`).
- Drupal requires a `label` key; omit it and the help topic will be considered invalid.
- Format without twig {# and #}:
- Example:
```
---
label: 'Adding a new submenu to the Blue Marloc admin menu'
---
```

## about .module  help
Each .module should contain the module help information.
- it describes the modules features.
- it lists the forms
- it lists the available route urls with their menu options (paths)
- it lists exposed api's available to developers
- it provides the link to help_topics

```php
function bm_main_help($route_name, RouteMatchInterface $route_match): string
{
  if ($route_name == 'help.page.bm_main') {
    $output = '<h2>' . t('BM Main Module') . '</h2>';
    $output .= '<p>' . t('Provides the shared admin navigation, base libraries, and cache utilities that every Blue Marloc sub-module relies on.') . '</p>';

    $output .= '<h3>' . t('Menus') . '</h3><ul>';
    $output .= '<li>' . t('<strong>Blue Marloc</strong> — top-level entry under Configuration » System that opens the overview page.') . '</li>';
    $output .= '<li>' . t('<strong>Administration</strong> — section for operational modules.') . '</li>';
    $output .= '<li>' . t('<strong>Entity</strong> — section for entity-level tooling.') . '</li>';
    $output .= '<li>' . t('<strong>Data production</strong> — section for data/import/export tooling.') . '</li>';
    $output .= '<li>' . t('<strong>Management</strong> — section focused on managerial dashboards.') . '</li>';
    $output .= '<li>' . t('<strong>Development</strong> — section focused on developer utilities.') . '</li>';
    $output .= '<li>' . t('<strong>bm_main</strong> — settings link under Administration when enabled.') . '</li>';
    $output .= '</ul>';

    $output .= '<h3>' . t('URIs') . '</h3><ul>';
    $output .= '<li>' . t('<code>@uri</code> (<code>@route</code>) — Overview of all Blue Marloc sections.', [
      '@uri' => '/admin/blue-marloc',
      '@route' => 'bm_main.section_overview',
    ]) . '</li>';
    $output .= '<li>' . t('<code>@uri</code> (<code>@route</code>) — Administration section landing.', [
      '@uri' => '/admin/blue-marloc/admin',
      '@route' => 'bm_main.section_admin',
    ]) . '</li>';
    $output .= '<li>' . t('<code>@uri</code> (<code>@route</code>) — Entity tooling section.', [
      '@uri' => '/admin/blue-marloc/entity',
      '@route' => 'bm_main.section_entity',
    ]) . '</li>';
    $output .= '<li>' . t('<code>@uri</code> (<code>@route</code>) — Data production section.', [
      '@uri' => '/admin/blue-marloc/data',
      '@route' => 'bm_main.section_data',
    ]) . '</li>';
    $output .= '<li>' . t('<code>@uri</code> (<code>@route</code>) — Management section.', [
      '@uri' => '/admin/blue-marloc/manager',
      '@route' => 'bm_main.section_manager',
    ]) . '</li>';
    $output .= '<li>' . t('<code>@uri</code> (<code>@route</code>) — Developer tooling section.', [
      '@uri' => '/admin/blue-marloc/developer',
      '@route' => 'bm_main.section_developer',
    ]) . '</li>';
    $output .= '</ul>';

    $link = Link::fromTextAndUrl(
      t('Help Topics overview'),
      Url::fromRoute('help.main')
    )->toString();

    $output .= '<p>' . t('See the @link for detailed how-to guides.', ['@link' => $link]) . '</p>';
    return $output;
  }
  return '';
}

```


## about README.md

The readme file contains installation instructions and a copy of the .module help text in markdown format.

## about wiki

In the module directory there can be a wiki directory.
- When there is no wiki directory, create it in the module directory and copy Home.md, _Footer.md, _Sidebar.md from git_root/codex/agents/wiki directory into the wiki directory as starting point.

Structure:
* module_name/`wiki`
* module_name/`wiki/Home.md`
* module_name/`wiki/_Footer.md`
* module_name/`wiki/_Sidebar.md`
* module_name/`wiki/{specific topic 1}.md`

- Where the Home.md file contains the README.md text with extra general information for non technical users
- The specific topics describe features of the module wich logically belong together.
- Wiki files typically contain examples of how to use functionalities
- Wiki files typically contain explicit list of api or public functions and how to use them.

* module_name/`wiki/{specific topic 1}.md`
