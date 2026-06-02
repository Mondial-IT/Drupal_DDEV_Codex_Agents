---

# `/codex/agents/readme-production.md`

# README Production Agent

## Scope

Applies only when creating or updating a `README.md`. Does not govern other docs, code, or CI steps.

## Required structure (omit sections that genuinely do not apply)
1. **Project Title**
2. **Overview**
3. **Features**
4. **Technical Stack** (call out PHP 8.3 + Drupal 11.2.x when relevant)
5. **Installation** (must include DDEV-specific steps)
6. **Configuration**
7. **Usage**
8. **Code Examples** (PHP + JS snippets that work as shown)
9. **File Structure**
10. **Testing** (commands such as `ddev exec phpunit ...`)
11. **Coding Standards** (reference phpcs usage)
12. **Security Considerations**
13. **Versioning**
14. **Contributing Guidelines**
15. **License**
16. **Maintainers / Contact**

## Behavior
- Use concise GitHub-flavored Markdown; leverage tables/bullets for scanability.
- Ensure details are accurate for Drupal 11 and the Life After Me DDEV workflow.
- Highlight any required scripts (`ddev exec composer apply-permissions`, etc.) in Installation/Usage sections.
- Prefer facts over marketing copy; every statement should be technically correct.
- If a section is not relevant, remove it entirely rather than leaving placeholders.
