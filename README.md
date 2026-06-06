# herdr configuration

Custom keybindings and helper scripts for [herdr](https://herdr.dev) terminal multiplexer.

## Directory Structure

```
~/.config/herdr/
├── config.toml          # Main configuration
├── scripts/             # Helper scripts
│   ├── herdr-files      # Fuzzy file picker
│   ├── herdr-urls       # Fuzzy URL opener
│   ├── herdr-lazygit    # Open lazygit in new tab
│   ├── herdr-nvim       # Open nvim in new tab
│   ├── herdr-yazi       # Open yazi in split pane
│   ├── herdr-new-workspace
│   ├── herdr-switch-agent
│   └── herdr-switch-workspace
└── .gitignore

~/.local/bin/
└── herdr-* -> ~/.config/herdr/scripts/*  (symlinks)
```

## Requirements

### Required

| Tool | Description | Install |
|------|-------------|---------|
| [herdr](https://herdr.dev) | Terminal multiplexer | `brew install herdr` or see [docs](https://herdr.dev/docs/install) |
| [jq](https://stedolan.github.io/jq/) | JSON processor | `brew install jq` or `apt install jq` |
| [fzf](https://github.com/junegunn/fzf) | Fuzzy finder | `brew install fzf` or `apt install fzf` |

### Optional

| Tool | Description | Used by |
|------|-------------|---------|
| [zoxide](https://github.com/ajeetdsouza/zoxide) | Smart directory jumping | `herdr-new-workspace` |
| [nvim](https://neovim.io/) | Text editor | `herdr-files`, `herdr-nvim` |
| [lazygit](https://github.com/jesseduffield/lazygit) | Git TUI | `herdr-lazygit` |
| [yazi](https://github.com/sxyazi/yazi) | File manager | `herdr-yazi` |
| [leaf](https://github.com/antonmedv/leaf) | Markdown viewer | `herdr-files` (markdown files) |

### System Tools

These are typically pre-installed on most systems:

- `grep`, `sed`, `sort`, `awk`, `cut`, `basename`
- `file` - MIME type detection
- `open` (macOS) / `xdg-open` (Linux) - Default file opener

## Installation

```bash
# Clone configuration
git clone <repo-url> ~/.config/herdr

# Create symlinks for scripts
mkdir -p ~/.local/bin
for script in ~/.config/herdr/scripts/herdr-*; do
    ln -sf "$script" ~/.local/bin/$(basename "$script")
done

# Ensure ~/.local/bin is in PATH (add to shell config if needed)
export PATH="$HOME/.local/bin:$PATH"

# Reload herdr configuration
herdr server reload-config
```

## Keybindings

| Key | Action |
|-----|--------|
| `prefix+space` | Next agent |
| `prefix+tab` | Previous agent |
| `prefix+1..9` | Focus agent by index |
| `prefix+a` | Fuzzy switch agent |
| `prefix+w` | Workspace picker (j/k to navigate) |
| `prefix+shift+n` | Fuzzy new workspace |
| `prefix+t` | New tab |
| `prefix+shift+j/k` | Next/previous tab |
| `prefix+m` | Rename tab |
| `prefix+s` | Split horizontal |
| `prefix+v` | Split vertical (default) |
| `prefix+0` | Copy mode |
| `prefix+f` | Fuzzy file opener |
| `prefix+u` | Fuzzy URL opener |
| `prefix+y` | Open yazi file manager |
| `prefix+shift+g` | Open lazygit (git repos only) |
| `prefix+shift+v` | Open nvim in new tab |
| `prefix+shift+x` | Close tab |

> `prefix` = `ctrl+space`

## License

MIT
