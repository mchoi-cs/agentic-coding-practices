# Agentic coding practices

A starter repo for working with coding agents, rooted in [Ponytail](https://github.com/DietrichGebert/ponytail) — a "write less code" ruleset by [@DietrichGebert](https://github.com/DietrichGebert).

No app code, no framework, no dependencies. Just the rules, a playbook, and the templates that keep agent work reviewable.

## How Ponytail works

Ponytail tells the agent to act like a lazy senior developer: lazy as in efficient, not careless. Before writing code, it climbs a ladder and stops at the first rung that holds.

1. Does this need to be built at all? (YAGNI)
2. Does it already exist in this codebase? Reuse it.
3. Does the standard library do this? Use it.
4. Does a native platform feature cover it? Use it.
5. Does an already-installed dependency solve it? Use it.
6. Can this be one line? Make it one line.
7. Only then: write the minimum code that works.

Two things keep it honest:

- **The ladder runs after understanding, not instead of it.** A small diff in the wrong place is a second bug.
- **Some things are never cut.** Input validation at trust boundaries, error handling that prevents data loss, security, accessibility, and anything explicitly requested. Non-trivial logic still leaves one runnable check behind.

Full text: [`.cursor/rules/ponytail.mdc`](.cursor/rules/ponytail.mdc) or [`AGENTS.md`](AGENTS.md).

## Use it in Cursor

Copy [`.cursor/rules/ponytail.mdc`](.cursor/rules/ponytail.mdc) into your project at the same path. It is `alwaysApply: true`, so it loads on every request with no further setup.

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/ponytail.mdc \
  https://raw.githubusercontent.com/mchoi-cs/agentic-coding-practices/main/.cursor/rules/ponytail.mdc
```

Or use this repo as a GitHub template and start your project from it.

## Use it with other agents

Copy [`AGENTS.md`](AGENTS.md) to your repo root. Claude Code, Codex, Copilot, Jules, and most other agents read it automatically. If your agent wants a different filename (`CLAUDE.md`, `.github/copilot-instructions.md`), symlink or copy it there too.

## What's in this repo

| Path | What it is |
| --- | --- |
| `.cursor/rules/ponytail.mdc` | The Ponytail rule for Cursor, verbatim from upstream |
| `AGENTS.md` | The same ruleset for agents that read `AGENTS.md` |
| `docs/agentic-practices.md` | Playbook for the parts Ponytail doesn't cover: scoping, tickets, review, pushback |
| `.github/ISSUE_TEMPLATE/agent-task.md` | Issue template for agent-ready tasks |
| `.github/pull_request_template.md` | PR template with a Ponytail check |

## Credit

Ponytail is by [@DietrichGebert](https://github.com/DietrichGebert): <https://github.com/DietrichGebert/ponytail>. The rule files here are copied from upstream; go there for the plugin, updates, and the full project.

## License

MIT. See [LICENSE](LICENSE).
