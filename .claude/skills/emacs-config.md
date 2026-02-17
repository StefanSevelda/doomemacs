---
name: emacs-config
description: Use when modifying Doom Emacs configuration — adding keybindings, functions, package settings, or org-mode customizations
---

# Doom Emacs Configuration Skill

## Before Making Changes

1. **Read the target file** — always read `config.el`, `init.el`, or `packages.el` before editing
2. **Identify the right block** — find the relevant `after!` or `use-package!` block for the package being configured
3. **Check for keybinding collisions**:
   - Search Doom's `+evil-bindings.el` in the repo modules
   - Search the existing `config.el` for the key sequence
   - Check Doom's org module config if binding in org-mode

## Adding a Function + Keybinding

### Pattern

```elisp
;; Function goes INSIDE the relevant (after! <package> ...) block
(after! org
  ;; ... existing config ...

  (defun my/function-name ()
    "Docstring."
    (interactive)
    ;; implementation
    ))

;; Keybinding goes OUTSIDE, using :after for deferred loading
(map! :after org
      :map org-mode-map
      :localleader
      :desc "Description" "key" #'my/function-name)
```

### Rules
- Function name prefix: `my/`
- Place `defun` inside the `after!` block so org functions are available at definition time
- Place `map!` outside with `:after` — this is the idiomatic Doom pattern
- Use `:localleader` for mode-specific bindings (accessed via `SPC m`)
- Use `:leader` for global bindings (accessed via `SPC`)

## Section Header Style

Match existing patterns in `config.el`:

```elisp
;; Inside after! blocks — box-drawing characters:
  ;; ┌─────────────────────────────────────────────────────────────────────────┐
  ;; │ Section Title                                                           │
  ;; └─────────────────────────────────────────────────────────────────────────┘

;; Top-level major sections:
;;; ═══════════════════════════════════════════════════════════════════════════
;;; Section Title
;;; ═══════════════════════════════════════════════════════════════════════════

;; Top-level subsections:
;; ──────────────────────────────────────────────────────────────────────────────
;; Section Title
;; ──────────────────────────────────────────────────────────────────────────────
```

## Existing Leader Key Map

| Prefix | Purpose | Example |
|--------|---------|---------|
| `SPC n` | Notes / org-roam | `SPC n r f` = find node |
| `SPC d` | Diff / ediff | `SPC d d` = ediff current file |
| `SPC m` | Localleader (org) | `SPC m t` = TODO state, `SPC m h` = heading, `SPC m H` = text-to-TODO |
| `SPC t` | Toggles | Various UI toggles |

## Testing

```bash
# Hot-reload after config.el changes (no restart needed)
emacsclient -e '(doom/reload)'

# Full sync after init.el or packages.el changes
doom sync

# Check for errors
emacsclient -e '(doom/reload)' 2>&1
```

Always test via `emacsclient` after changes. If Emacs is not running, start it first or test by launching `emacs` directly.

## Common Pitfalls

- **Paren balancing**: `after!` blocks often end with 3-4 closing parens. Count carefully when inserting before the closing paren
- **Wrong block**: Don't put org settings outside `(after! org ...)` — org functions may not be loaded yet
- **Duplicate keys**: Doom silently shadows bindings. Always grep before adding
- **`use-package!` vs `after!`**: Use `use-package!` for packages in `packages.el`; use `after!` for Doom built-in modules
