# Hue Neovim Theme

Three Huế-inspired moods for [Neovim](https://neovim.io) (and
[LazyVim](https://www.lazyvim.org)), generated from the Hue design token system:

- **Huế Mưa** (`hue-mua`) — a deep dark mood shaped by Huế rain and wet stone.
- **Huế Hương** (`hue-huong`) — a softer dark mood drawn from the Perfume River and incense.
- **Huế Cung** (`hue-cung`) — an ivory light mood informed by imperial lacquer and royal purple.

Each mood is a standard Neovim colorscheme: `:colorscheme hue-mua` /
`hue-huong` / `hue-cung`. The theme sets `background` automatically and never
sets your font — fonts are a terminal/GUI concern.

## How it is built

Everything under `lua/` and `colors/` is **generated** by the token build — do
not edit it by hand. The Hue → Neovim mapping (highlight groups, Treesitter
`@`-captures, LSP semantic tokens, diagnostics, terminal ANSI, and the lualine
theme) lives in
[`packages/tokens/src/adapters/neovim.ts`](../tokens/src/adapters/neovim.ts).

```bash
# Regenerate this package from the source tokens
cd ../tokens && bun run build
```

## Install

This plugin currently lives inside the `hue-theme` monorepo. `lazy.nvim` adds a
repository *root* to the runtimepath (not a subdirectory), so install it by
pointing `dir` at this package:

```lua
-- lua/plugins/colorscheme.lua
{
  dir = vim.fn.expand("~/path/to/hue-theme/packages/nvim-plugin"),
  name = "hue-nvim",
  lazy = false,
  priority = 1000,
}
```

Then tell LazyVim which mood to load:

```lua
{ "LazyVim/LazyVim", opts = { colorscheme = "hue-mua" } }
```

### Transparent background

Call `setup` with `transparent = true` to clear backgrounds (editor, floats,
popups, Telescope/neo-tree) so a translucent terminal shows through. Selection
and active states keep their background for legibility.

```lua
{
  "crafts69guy/hue-nvim",
  priority = 1000,
  lazy = false,
  opts = { transparent = true },
  config = function(_, opts)
    require("hue").setup(opts)
  end,
}
```

For a non-LazyVim config:

```lua
vim.cmd.colorscheme("hue-huong")
```

## Color API

For building your own highlights from Hue colors, the plugin exposes a public
API (so you never reach into the generated `lua/hue/palette.lua` directly):

```lua
local hue = require("hue")

-- Semantic palette grouped by family, for the active mood by default.
local c = hue.colors()          -- or hue.colors("huong")
c.surface.canvas                -- "#0F1313"
c.accent.primary                -- "#79B49A"
c.status.error                  -- "#D2645A"

-- Flat form keyed by the full contract name.
hue.raw()["accent.primary"]     -- "#79B49A"

-- Color math for derived shades (faint diff backgrounds, etc.).
local u = hue.util
u.darken(c.status.success, 0.18, c.surface.canvas)
u.lighten(c.status.error, 0.9)
u.blend("#79B49A", "#0F1313", 0.5)
```

`colors()`/`raw()` resolve the mood from the active `colorscheme` automatically,
so derived highlights follow when you switch between moods.

### lualine

The bundled lualine themes are auto-discovered when lualine uses
`theme = 'auto'` (the LazyVim default), because each mood ships
`lua/lualine/themes/hue-<mood>.lua`.

## Distribution note

For a public release the **contents** of this directory (with `lua/` and
`colors/` at the repository root) should be mirrored to a standalone
`hue-nvim` repository so plugin managers can install it with the usual
`"crafts69guy/hue-nvim"` shorthand.
