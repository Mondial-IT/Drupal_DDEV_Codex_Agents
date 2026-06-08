# Drupal Styling Rules (Codex / Agents)

## Purpose

This document defines **non-negotiable theming rules** for Drupal implementations.
AI agents, generators, and humans MUST follow these rules to ensure:

- Consistent surface-based theming
- Dark/light mode correctness
- AI-safe overrides
- Long-term maintainability across themes and modules

This document applies to:
- Drupal themes
- Custom modules rendering markup
- Layout plugins
- AI-assisted layout and styling systems

---

## Core Principle

**Themes do not use colors.
Themes use surfaces.**

No component may define visual color directly unless explicitly allowed.

---

## Surface System

All visual containers MUST use the surface system.

### Allowed Surface Levels

| Level | Name       | Intended Use                                |
|------:|------------|----------------------------------------------|
| 0     | page       | Page and application background              |
| 1     | group      | Fieldsets, grouped sections                  |
| 2     | container  | Cards, blocks, panels                        |
| 3     | control    | Inputs, buttons, interactive elements        |
| 4     | overlay    | Modals, dialogs, floating UI                 |

Agents MUST assign exactly one surface level per component.

---

## Markup Contract

### Required Pattern

```html
<div class="surface" data-surface="2">
````

Optional elevation:

```html
<div class="surface" data-surface="2" data-elevate="hover">
```

### Forbidden Patterns

```html
<div style="background:#fff">
<div class="card card--white">
```

---

## CSS Responsibilities

### Allowed

* Use CSS custom properties
* Use cascade layers
* Resolve all visuals via surface tokens

### Forbidden

* Hard-coded colors
* Inline styles
* Theme-specific color classes
* Conditional logic on `prefers-color-scheme`

Dark mode MUST be handled exclusively via CSS variables.

---

## AI Override Rules

AI may influence theming ONLY when explicitly allowed.

### Opt-in Attribute

```html
data-ai-surface="auto"
```

### AI Behavior

* AI MAY override surface level via CSS variables
* AI MUST NOT inject colors
* AI MUST respect surface hierarchy
* Default AI fallback surface: `container (2)`

Example:

```html
<div class="surface"
     data-surface="1"
     data-ai-surface="auto">
```

---

## Cascade Layer Order (Mandatory)

```css
@layer tokens, engine, theme, ai;
```

| Layer  | Responsibility             |
| ------ | -------------------------- |
| tokens | Design truth               |
| engine | Surface resolution logic   |
| theme  | Drupal mappings            |
| ai     | AI overrides (opt-in only) |

AI CSS MUST live exclusively in the `ai` layer.

---

## Drupal Rendering Rules

### PHP / Twig

* Modules MUST output semantic attributes
* No module may assume color values
* Theme decides appearance, not module

Correct:

```php
$attributes['class'][] = 'surface';
$attributes['data-surface'] = '2';
```

Incorrect:

```php
$attributes['style'] = 'background:#fff';
```

---

## Layout & Panels

* Layout plugins MUST expose surface level as configuration
* Stored value MUST be numeric (`0–4`)
* Display logic MUST not alter surface semantics

---

## Accessibility

* Contrast MUST meet WCAG AA
* Elevated surfaces MUST remain visually distinct
* Focus indicators MUST use surface elevation or outline tokens

Agents MUST NOT disable focus styles.

---

## Versioning & Stability

* Surface level meanings are stable
* Adding new levels requires explicit Codex update
* Removing levels is forbidden without migration rules

---

## Enforcement Rule

Any output violating these rules is considered **invalid** and MUST be regenerated.

---

## Summary (Agent Memory)

* Surfaces, not colors
* One surface per component
* AI only with opt-in
* Tokens define truth
* Theme maps intent
* AI reasons semantically
