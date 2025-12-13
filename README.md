# quadrate.nvim

> **Canonical repository:** https://git.sr.ht/~klahr/quadrate.nvim

Neovim plugin for the [Quadrate](https://git.sr.ht/~klahr/quadrate) programming language with LSP support and tree-sitter syntax highlighting.

---

## Features

- **Syntax Highlighting**: Tree-sitter parser (recommended) or Vim regex fallback
- **LSP Integration**: Full language server support via nvim-lspconfig
- **Auto-completion**: Built-in instructions and user-defined functions
- **Diagnostics**: Real-time error detection
- **Go to Definition**: Navigate to function and variable definitions
- **Find References**: Find all usages of a symbol
- **Rename Symbol**: Scoped renaming of variables and functions
- **Document Formatting**: Format code with `<space>f`
- **Signature Help**: Function signatures while typing
- **Document Highlight**: Highlight other occurrences of symbol under cursor
- **Code Folding**: LSP-based folding for functions and blocks

---

## Prerequisites

### Required

1. **Neovim 0.8+** (for LSP support)
2. **nvim-lspconfig** plugin
3. **quadlsp** binary (from the Quadrate compiler)

### Optional (for Tree-sitter)

4. **nvim-treesitter** plugin
5. **tree-sitter-cli** (`npm install -g tree-sitter-cli`)

### Installing quadlsp

```bash
# From the Quadrate repository
git clone https://git.sr.ht/~klahr/quadrate
cd quadrate
make release && sudo make install
```

Or use a local build:
```bash
make debug
# Binary at: build/debug/cmd/quadlsp/quadlsp
```

---

## Installation

### lazy.nvim

```lua
{
  'quadrate-lang/quadrate.nvim',
  dependencies = { 'neovim/nvim-lspconfig' },
}
```

### packer.nvim

```lua
use {
  'quadrate-lang/quadrate.nvim',
  requires = { 'neovim/nvim-lspconfig' },
}
```

### vim-plug

```vim
Plug 'quadrate-lang/quadrate.nvim'
```

### Manual Installation

Clone and symlink:

```bash
git clone https://git.sr.ht/~klahr/quadrate.nvim ~/.local/share/nvim/site/pack/plugins/start/quadrate.nvim
```

Or symlink individual files:

```bash
ln -s /path/to/quadrate.nvim/ftdetect/quadrate.vim ~/.config/nvim/ftdetect/
ln -s /path/to/quadrate.nvim/plugin/quadrate.lua ~/.config/nvim/plugin/
ln -s /path/to/quadrate.nvim/syntax/quadrate.vim ~/.config/nvim/syntax/
ln -s /path/to/quadrate.nvim/indent/quadrate.vim ~/.config/nvim/indent/
```

---

## Configuration

### Basic Setup

The plugin auto-configures LSP when loaded. No additional setup required if `quadlsp` is in your PATH.

### Custom LSP Path

If using a local build, configure before the plugin loads:

```lua
-- In your init.lua, before loading the plugin
require('quadrate').setup({
  lsp = {
    path = '/path/to/quadrate/build/debug/cmd/quadlsp/quadlsp'
  }
})
```

### Disable Auto-Setup

To configure LSP manually:

```lua
vim.g.quadrate_no_auto_setup = true
```

Then call setup yourself:
```lua
require('lspconfig').quadlsp.setup(require('quadrate').get_lsp_opts())
```

### Linting Configuration

```lua
require('quadrate').setup({
  lint = {
    enabled = true,   -- Enable/disable linting
    path = 'quadlint' -- Path to linter binary
  }
})
```

---

## Tree-sitter Setup

Tree-sitter provides faster and more accurate syntax highlighting.

### Build the Parser

```bash
cd /path/to/quadrate.nvim/tree-sitter-quadrate
tree-sitter generate
```

### Copy to Neovim Config

```bash
cp -r tree-sitter-quadrate ~/.config/nvim/
# Or symlink
ln -s /path/to/quadrate.nvim/tree-sitter-quadrate ~/.config/nvim/tree-sitter-quadrate
```

### Verify

```vim
:TSInstallInfo quadrate
:TSBufEnable highlight
```

---

## Key Mappings

Default mappings (defined in `plugin/quadrate.lua`):

| Key | Action |
|-----|--------|
| `K` | Hover documentation |
| `gd` | Go to definition |
| `gD` | Go to declaration |
| `gi` | Go to implementation |
| `gr` | Find references |
| `<space>f` | Format document |
| `<space>rn` | Rename symbol |
| `<space>ca` | Code actions |
| `<space>e` | Show diagnostics |
| `[d` | Previous diagnostic |
| `]d` | Next diagnostic |
| `<C-k>` | Signature help (normal and insert mode) |
| `<space>D` | Type definition |
| `<space>wa` | Add workspace folder |
| `<space>wr` | Remove workspace folder |
| `<space>wl` | List workspace folders |
| `<space>q` | Set location list with diagnostics |

---

## Using with coc.nvim

If you prefer [coc.nvim](https://github.com/neoclide/coc.nvim), add to your `coc-settings.json` (`:CocConfig`):

```json
{
  "languageserver": {
    "quadrate": {
      "command": "quadlsp",
      "filetypes": ["quadrate"],
      "rootPatterns": [".git/"],
      "settings": {}
    }
  }
}
```

---

## Troubleshooting

### LSP Not Starting

Check if quadlsp is accessible:
```bash
which quadlsp
```

Enable LSP debug logs:
```lua
vim.lsp.set_log_level('debug')
:lua vim.cmd('e ' .. vim.lsp.get_log_path())
```

Check LSP status:
```vim
:LspInfo
```

### No Completions

1. Verify LSP is attached: `:LspInfo`
2. Check omnifunc: `:set omnifunc?`
3. Try manual completion: `<C-x><C-o>`

### Diagnostics Not Showing

Diagnostics update on file open/save:
1. Save the file: `:w`
2. Check diagnostics: `:lua vim.diagnostic.open_float()`

---

## Contributing

Patches welcome!

**Email**: ~klahr/quadrate@lists.sr.ht
**GitHub**: https://github.com/quadrate-lang/quadrate.nvim

---

## License

GNU General Public License v3.0

See [LICENSE](./LICENSE) for full terms.

---

## Resources

- **Quadrate Language**: https://git.sr.ht/~klahr/quadrate
- **Documentation**: https://quad.r8.rs
- **nvim-lspconfig**: https://github.com/neovim/nvim-lspconfig
- **nvim-treesitter**: https://github.com/nvim-treesitter/nvim-treesitter
