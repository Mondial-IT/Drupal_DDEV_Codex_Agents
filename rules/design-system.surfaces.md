# Design System — Surfaces

## Purpose
Surfaces represent visual elevation and importance.
AI agents must reason in terms of surfaces, not colors.

---

## Surface Levels

| Level | Name       | Role                                      | CSS Variable  |
|------:|------------|-------------------------------------------|---------------|
| 0     | page       | Global background                          | --surface-0   |
| 1     | group      | Fieldsets, grouped sections               | --surface-1   |
| 2     | container  | Cards, panels, blocks                     | --surface-2   |
| 3     | control    | Inputs, buttons, interactive elements     | --surface-3   |
| 4     | overlay    | Modals, popovers, floating UI             | --surface-4   |

---

## Agent Rules

- Agents MUST NOT introduce raw colors for surfaces
- Agents MUST assign exactly one surface level per component
- Agents MAY elevate on hover/focus if importance increases
- Agents MUST preserve surface hierarchy (no surface-3 inside surface-0 without intent)

---

## Example Output (Correct)

```html
<div class="surface" data-surface="2" data-elevate="hover">
  <input class="surface" data-surface="3">
</div>
