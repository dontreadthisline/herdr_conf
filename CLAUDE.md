# Herdr Configuration Notes

## keys.command type

- `type = "pane"` - Opens a temporary pane, runs command, closes pane when done. **Focus returns to original pane after command exits.** Use for interactive selectors (fzf) that run in the foreground.
- `type = "shell"` - Runs command detached in background. **Does not steal focus.** Use when the script creates its own tab/pane and manages focus.

**Rule of thumb:** If your script creates a new tab/pane and you want focus to stay there, use `type = "shell"`. If you want focus to return after the command (like fzf selectors), use `type = "pane"`.

## Creating new tab with command

When creating a new tab and running a command in it:

```sh
# Get tab_id and pane_id from result
result=$(herdr tab create --cwd "$cwd" --focus 2>/dev/null)
tab_id=$(echo "$result" | jq -r '.result.tab.tab_id // empty')
pane_id=$(echo "$result" | jq -r '.result.root_pane.pane_id // empty')

# IMPORTANT: Wait for shell to initialize before sending commands
sleep 0.3
herdr pane run "$pane_id" "exec lazygit"
```

## JSON output structure

herdr commands return nested JSON. Common paths:
- `herdr tab create` -> `.result.tab.tab_id`, `.result.root_pane.pane_id`
- `herdr pane list` -> `.result.panes[]`
- `herdr workspace list` -> `.result.workspaces[]`

## Environment variables in keys.command scripts

When `type = "pane"` or `type = "shell"` runs a script, these env vars are available:
- `HERDR_ACTIVE_PANE_ID` - The pane where user pressed the keybinding
- `HERDR_ACTIVE_PANE_CWD` - The cwd of that pane

## Script location

Scripts in `~/.config/herdr/scripts/` are NOT automatically in PATH. Either:
1. Symlink to `~/.local/bin/`: `ln -sf ~/.config/herdr/scripts/herdr-foo ~/.local/bin/herdr-foo`
2. Use full path in config: `command = "/Users/xxx/.config/herdr/scripts/herdr-foo"`

## Use exec for TUI apps

When running TUI apps (lazygit, yazi, nvim) in a new pane/tab, use `exec` so the pane closes when the app exits:
```sh
herdr pane run "$pane_id" "exec lazygit"
```
