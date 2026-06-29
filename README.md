# 🌧 Hue for Neovim

> Huế-inspired colorschemes for Neovim & LazyVim — generated from the
> [Hue design token system](https://github.com/crafts69guy/hue-theme).

Three moods drawn from the atmosphere and visual culture of Huế, Việt Nam:

| Mood | `:colorscheme` | Appearance | Feel |
| --- | --- | --- | --- |
| **Huế Mưa** | `hue-mua` | dark | deep charcoal, rain silver, muted jade |
| **Huế Hương** | `hue-huong` | dark | river green, dusk blue, incense gold |
| **Huế Cung** | `hue-cung` | light | ivory paper, imperial lacquer, royal purple |

## Features

- Treesitter `@`-captures and LSP semantic tokens
- Diagnostics, git signs, and 16-color terminal ANSI
- Plugin integrations: Telescope, neo-tree, which-key, Snacks, noice,
  blink/nvim-cmp, flash, gitsigns, and a lualine theme
- Optional transparent background
- A public color API for your own highlights
- WCAG-audited contrast — and it never changes your font

## Requirements

- Neovim ≥ 0.9 and a true-color terminal (the theme enables `termguicolors`)

## Installation

[lazy.nvim](https://github.com/folke/lazy.nvim):

```lua
{ "crafts69guy/hue-nvim", lazy = false, priority = 1000 }
```

Select a mood. With **LazyVim**:

```lua
{ "LazyVim/LazyVim", opts = { colorscheme = "hue-mua" } }
```

Plain Neovim:

```lua
vim.cmd.colorscheme("hue-huong")
```

## Configuration

`setup` is only needed for options:

```lua
{
  "crafts69guy/hue-nvim",
  lazy = false,
  priority = 1000,
  opts = { transparent = true },
  config = function(_, opts)
    require("hue").setup(opts)
  end,
}
```

| Option | Default | Description |
| --- | --- | --- |
| `transparent` | `false` | Clear backgrounds (editor, floats, popups, and popular plugins) so a translucent terminal shows through. Selection/active states keep their background. |
| `default` | `nil` | Mood used by `require("hue").load()` when called without an argument. |

The bundled **lualine** themes are auto-discovered with `theme = 'auto'` (the
LazyVim default).

## Color API

Build your own highlights from the active mood's palette — no need to touch the
generated files:

```lua
local hue = require("hue")

local c = hue.colors()        -- grouped by family (active mood); or hue.colors("huong")
c.surface.canvas              -- "#0F1313"
c.accent.primary              -- "#79B49A"

hue.raw()["status.error"]     -- flat form, full contract names

-- color math for derived shades
hue.util.darken(c.status.success, 0.18, c.surface.canvas)
hue.util.lighten(c.status.error, 0.9)
```

`colors()` / `raw()` resolve the mood from the active colorscheme, so derived
highlights follow when you switch moods.

## Credits

Generated from the
[Hue design token system](https://github.com/crafts69guy/hue-theme) — `lua/` and
`colors/` are build output, not hand-edited. Rooted in the visual culture of
Huế, Việt Nam.

## License

[MIT](./LICENSE)
