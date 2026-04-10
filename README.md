# rules-bash

Shell command safety and hygiene rules for AI coding agents. Blocks destructive bash operations (`rm -rf`, `chmod 777`, `curl | sh`) and enforces agent-specific best practices — keeping stderr clean, preventing global npm installs, and auditing background processes.

**10 rules · 2 files**

![rules-bash — AI agent shell safety governance demo](demo.cast)

> [▶ Watch interactive demo on SigmaShake Hub](https://hub.sigmashake.com/ruleset/rules-bash)


## Install

```bash
ssg hub pull rules-bash
```

Available on the [SigmaShake Hub](https://hub.sigmashake.com) — the open registry for AI agent governance rules. Compatible with Claude Code, GitHub Copilot, Cursor, Windsurf, Aider, and any AI coding agent using the `ssg` hook protocol.

## Rules

### bash_exec_safety.rules — Dangerous command prevention (5 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-rm-rf` | DENY | error | Blocks `rm -rf` and recursive deletes |
| `no-chmod-777` | DENY | error | Blocks `chmod 777` and world-writable permissions |
| `no-curl-pipe-shell` | DENY | error | Blocks `curl | sh` — remote code execution risk |
| `ask-sudo` | ASK | warning | Prompts before any `sudo` command |
| `no-kill-9` | ASK | warning | Suggests SIGTERM before SIGKILL |

### bash_exec_hygiene.rules — Agent-specific hygiene (5 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-git-add-all` | DENY | error | Blocks `git add -A` and `git add .` — stage explicitly |
| `no-stderr-redirect` | DENY | error | Blocks `2>&1` — keep stderr separate for debugging |
| `no-npm-install-global` | ASK | warning | Flags global npm installs — use local or npx |
| `ask-env-override` | ASK | warning | Flags inline `VAR=value cmd` env overrides |
| `log-background-process` | LOG | info | Logs background processes started with `&` or `nohup` |

## Why this matters

AI agents run bash commands autonomously — without the hesitation a human developer would feel before `rm -rf /` or `chmod 777`. These rules act as a last line of defense at the tool-call boundary, blocking irreversible operations before they execute.

- **Destructive command prevention**: Blocks `rm -rf` variants, force-chmod, and pipe-to-shell patterns that can compromise a system in a single command
- **Agent hygiene**: AI agents using `2>&1` silently discard diagnostic information; `git add -A` can accidentally stage secrets — both are caught here
- **Audit trail**: Background process spawns are logged so agents can clean up after themselves

## Compatible with

- Any bash/zsh shell environment
- macOS, Linux, WSL
- Works alongside static analysis tools — these rules operate at the AI agent tool-call level, intercepting commands before execution

## About

Part of the [SigmaShake Hub](https://hub.sigmashake.com) — open-source governance rules for AI coding agents.
Install the `ssg` CLI to enforce these rules: `npm install -g @sigmashake/ssg`
