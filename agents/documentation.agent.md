---

# `/codex/agents/documentation.md`

# Documentation Agent

## Scope

Active only when producing documentation **other than** project READMEs: architecture notes, ADRs, runbooks, help topics, or wiki updates. Does not govern executable code or CI rules.

## Rules

1. **Audience-first structure**
   - Use hierarchical Markdown headings, short paragraphs, and bullet lists for scanability.
   - Store long-form operational notes under `drupal_root/wiki/` (matching the existing numbering scheme).

2. **Drupal help topics**
   - Each help-topic Twig file begins with YAML front matter (at least `label`; optional `top_level`, `related`).
   - Descriptions can reference Twig includes/snippets, but keep business logic in PHP services or plugins.

3. **Environment accuracy**
   - Reference commands exactly as they run inside DDEV (`ddev exec ...`).
   - Document secrets handling and file locations instead of inlining sensitive values (see `security.md`).

4. **Link to agents when needed**
   - Point to `dev-workflow.md` for DDEV routines, `testing-guidelines.md` for regression policy, etc., instead of duplicating the content.

5. **Diagrams & visuals**
   - Prefer Markdown tables/ASCII art; if richer diagrams are required, note the source tool and keep the source files under version control.

6. **Terminology**
   - Align language with Drupal core (entities, bundles, services), BlueMarloc modules, and Life After Me naming standards.


7. **The documentation agent follows rules from:**
  - drupal-rules-for-demo-pages.md
  - drupal-rules-for-forms.md
  - drupal-rules-for-forms-bm_core__AjaxMessageFormBase.md
  - drupal-rules-for-help-topics.md

8. **Default instance**
`ddev drush @ziston.ddev` is the default instance used in commands.
