# devcontainer template

a devcontainer for running claude code and codex in yolo mode.

based on anthropic's claude code devcontainer, modified to install codex and tmux, enable passwordless sudo, and remove firewall restrictions.

## use

copy the contents to `.devcontainer/` in your repo.

```sh
cp -r devcontainer path/to/repo/.devcontainer
```

### vscode

open in vscode and run "reopen in container".

```sh
claude --dangerously-skip-permissions
codex --yolo
```

### cli

you can also run devcontainers from the terminal using the [devcontainer cli](https://github.com/devcontainers/cli).

```sh
npm install -g @devcontainers/cli
devcontainer up --workspace-folder .
devcontainer exec --workspace-folder . tmux new -s agent
# inside:
claude --dangerously-skip-permissions
codex --yolo
# reattach with:
devcontainer exec --workspace-folder . tmux attach -t agent
```

auth is persisted across rebuilds — `~/.codex/` and `~/.claude/` are mounted as docker volumes.

## set yolo as default

since the config directories persist, you can set yolo mode as the default so you don't need to pass flags every time.

for claude code, add to `~/.claude/settings.json`:

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
```

for codex, add to `~/.codex/config.toml`:

```toml
approval_policy = "never"
sandbox_mode = "danger-full-access"
```
