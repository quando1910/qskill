# qskill

Personal development workflow skills for [Claude Code](https://claude.com/claude-code) and [OpenAI Codex](https://developers.openai.com/codex).

The same repository is a plugin marketplace for both tools. Install it in one or both.

## Skills

| Skill | What it does |
| --- | --- |
| `push-code` | Reviews the current changes, creates a branch if you are on `main`/`trunk`, commits, pushes, and opens a pull request with `gh`. |

## Install for Claude Code

From a terminal:

```bash
claude plugin marketplace add quando1910/qskill
```

```bash
claude plugin install qskill@quando
```

Or inside a Claude Code session:

```text
/plugin marketplace add quando1910/qskill
/plugin install qskill@quando
```

Restart Claude Code, then use:

- `/qskill:push-code` to commit, push, and open a PR
- `/qskill:qskill` to list the available workflows

Claude also loads the `push-code` skill automatically when you ask it to commit and open a PR.

## Install for Codex

```bash
codex plugin marketplace add quando1910/qskill
```

```bash
codex plugin add qskill@quando
```

Restart Codex, then invoke the skill with `$qskill:push-code`, or ask Codex to commit and open a PR.

## Local development

To install from a local clone instead of GitHub, pass the clone's path to either tool:

```bash
claude plugin marketplace add /path/to/qskill
```

```bash
codex plugin marketplace add /path/to/qskill
```

Then install `qskill@quando` as shown above.

## Update and uninstall

| | Claude Code | Codex |
| --- | --- | --- |
| Update | `claude plugin marketplace update quando`, then `claude plugin update qskill@quando` | `codex plugin marketplace upgrade quando`, then `codex plugin add qskill@quando` |
| Uninstall | `claude plugin uninstall qskill@quando` | `codex plugin remove qskill@quando` |

## Requirements

- `git`, with your own `user.name` and `user.email` configured
- [GitHub CLI](https://cli.github.com/) (`gh`), authenticated, for creating pull requests

## Repository layout

```text
.claude-plugin/          Claude Code plugin + marketplace manifests
.codex-plugin/           Codex plugin manifest
.agents/plugins/         Codex marketplace manifest
commands/                Claude Code slash commands
skills/<name>/SKILL.md   Skills shared by both tools
```
