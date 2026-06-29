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

For a non-LazyVim config:

```lua
vim.cmd.colorscheme("hue-huong")
```

### lualine

The bundled lualine themes are auto-discovered when lualine uses
`theme = 'auto'` (the LazyVim default), because each mood ships
`lua/lualine/themes/hue-<mood>.lua`.

## Distribution note

For a public release the **contents** of this directory (with `lua/` and
`colors/` at the repository root) should be mirrored to a standalone
`hue-nvim` repository so plugin managers can install it with the usual
`"crafts69guy/hue-nvim"` shorthand.
