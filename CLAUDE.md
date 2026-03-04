# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

kickstart.nvim is a single-file Neovim configuration meant as a starting point for users to build their own configuration. It is NOT a Neovim distribution - the goal is for users to read and understand every line.

## Commands

- **Format Lua code**: `stylua .` (used in CI)
- **Check health in Neovim**: `:checkhealth kickstart` - verifies system requirements

## Architecture

### Main Configuration (`init.lua`)

The single-file design is intentional for teaching purposes. Configuration flows top-to-bottom:

1. **Leader keys**: Set `vim.g.mapleader` and `vim.g.maplocalleader` before loading plugins
2. **Options**: Basic editor settings (line numbers, mouse, clipboard, etc.)
3. **Keymaps**: Global keymaps (diagnostic, window navigation, terminal)
4. **Autocommands**: Highlight yanked text, etc.
5. **Plugin setup**: lazy.nvim installation and configuration

### Plugin Manager

Uses lazy.nvim. Plugins are configured inline in `require('lazy').setup({...})` with these patterns:
- `opts = {}` - calls `require('plugin').setup(opts)` automatically
- `config = function() ... end` - full control over configuration
- `dependencies = {}` - plugins that must load before this one
- `event = 'VimEnter'` - lazy loading on events

### Plugin Structure

Core plugins are defined inline in init.lua. Optional plugins are in `lua/kickstart/plugins/`:
- `debug.lua` - DAP configuration (nvim-dap, nvim-dap-ui, mason-nvim-dap)
- `lint.lua` - Linting with nvim-lint
- `autopairs.lua`, `gitsigns.lua`, `indent_line.lua`, `neo-tree.lua`

To enable optional plugins, uncomment the `require` lines in init.lua's lazy.setup.

### Custom Plugins

User additions go in `lua/custom/plugins/*.lua`. Enable by uncommenting `{ import = 'custom.plugins' }` in lazy.setup.

### LSP Configuration

- Mason installs LSP servers and tools
- LSP keymaps are set on `LspAttach` autocmd (per-buffer)
- Telescope provides LSP pickers (references, definitions, etc.)

### Keymap Conventions

- `<leader>` is space
- `<leader>s*` - search commands (Telescope)
- `<leader>t*` - toggle commands
- `<leader>h*` - git hunk commands
- `gr*` - LSP-related commands (rename, references, etc.)

## Notes

- Nerd Font support: Set `vim.g.have_nerd_font = true` in init.lua if using one
- Format on save is disabled for C/C++ (no standardized style) - see conform.nvim config
- TypeScript LSP (`ts_ls`) is commented out - consider typescript-tools.nvim for TS projects