# Doom Emacs Configuration

This repository is a [Doom Emacs](https://github.com/doomemacs/doomemacs) configuration. Doom is a framework on top of Emacs that provides opinionated defaults, a module system, and lazy-loading via `use-package!` / `after!` macros.

## Repository Structure

- **`.doom.d/`** — Personal configuration (this is `$DOOMDIR`)
  - `config.el` — Main configuration: keybindings, package settings, custom functions
  - `init.el` — Doom module declarations (`:lang`, `:tools`, `:ui`, etc.)
  - `packages.el` — Extra package declarations (`package!`)
  - `modules/` — Custom Doom modules (e.g., `editor/evil/config.el`)
- **`modules/`** (repo root) — Doom's built-in modules (upstream)

## Conventions

### Keybindings
- Use `map!` macro with `:leader`, `:localleader`, or specific keymaps
- Use `:after <package>` on `map!` to defer until package loads
- Check Doom's `+evil-bindings.el` and module configs before choosing keys to avoid collisions
- Existing leader prefixes: `SPC n` = notes, `SPC d` = diff/ediff, `SPC m` = org localleader, `SPC t` = toggle

### Custom Functions
- Prefix with `my/` (e.g., `my/org-text-to-todo`, `my/ediff-current-file`)

### Section Headers
Two styles are used in `config.el`:
- Major sections: `;;; ═══` double-line box
- Subsections: `;; ──` single-line separator
- Inline blocks (inside `after!`): `;; ┌──` / `;; │` / `;; └──` box-drawing

### Package Configuration
- Use `use-package!` for packages declared in `packages.el`
- Use `after!` for configuring Doom built-in modules (e.g., `(after! org ...)`)
- Use `setq` inside these blocks for settings

## Key Existing Customizations

| Area | Description |
|------|-------------|
| Clipboard | macOS pbcopy/pbpaste integration |
| Org-roam | Notes in `~/org-roam/`, daily templates, custom keybindings |
| Ediff | Three-way merge config, git diff helpers under `SPC d` |
| Org beautification | Nord-themed priorities/TODO faces, variable-pitch fonts, org-modern, org-superstar |
| Vterm | Reduced flashing configuration |
| Evil | Custom evil module overrides in `modules/editor/evil/` |

## Testing Changes

```bash
# Hot-reload config without restarting Emacs
emacsclient -e '(doom/reload)'

# Full rebuild (after changing init.el or packages.el)
doom sync
```
