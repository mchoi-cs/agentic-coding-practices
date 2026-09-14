# Agentic coding practices

Ponytail governs what the agent writes. This covers everything around it: how you hand off work, how you review it, and when you push back.

## Scope tasks tightly

- One outcome per task. If you need "and" to describe it, it's two tasks.
- Hand over the outcome, not the implementation. "Logins fail silently after a token refresh — fix it" beats "add a try/catch in `refreshToken`." You may be wrong about the cause; the agent can read the code.
- Prescribe only where you have a real constraint: an API you must not break, a library you must not add, a pattern the codebase already uses.
- Name the files or area if you know it. That's context, not prescription.
- Long tasks drift. Break anything over ~an hour of agent work into reviewable steps.

## Prefer small diffs and root-cause fixes

- Review effort scales with diff size, not with difficulty. A 40-line diff you understand beats a 400-line diff you skim.
- Fix the shared function once instead of guarding every caller. Ask the agent to grep the callers before it edits.
- Refactors are separate PRs. Mixing a refactor into a bugfix hides the fix.
- If the agent's diff is much bigger than expected, that's a signal to re-read the task, not to merge faster.

## Keep secrets out of prompts and repos

- Never paste API keys, tokens, or customer data into a prompt. Prompts get logged, cached, and replayed.
- Use `.env` files plus a committed `.env.example`. Point the agent at the example.
- Redact logs and stack traces before sharing them.
- Scan before merge: check the diff for keys, `.env` files, and credentials in fixtures. Agents copy what they see.
- Give agents scoped credentials when they need real access. Read-only unless writes are the point.

## Track work in GitHub Issues

Issue → branch → PR → review → merge. It gives you a durable record of intent that chat history doesn't.

- Write the issue first, even for small work. The act of writing it usually sharpens the scope.
- One branch per issue; reference the issue in the PR so it closes on merge.
- Keep the discussion on the issue, not in a chat window. The next agent (or person) can read it.
- Comment on the issue when the plan changes, so the PR doesn't arrive as a surprise.

## Write agent-friendly tickets

Use [`.github/ISSUE_TEMPLATE/agent-task.md`](../.github/ISSUE_TEMPLATE/agent-task.md). Five parts:

- **Goal** — the outcome, in one or two sentences.
- **Context** — files, prior attempts, why this matters. Links beat retyping.
- **Constraints** — what must not change: APIs, schemas, deps, perf budgets.
- **Success criteria** — observable and checkable. "Refresh returns a 401 with a JSON body instead of hanging," not "works correctly."
- **Out of scope** — the adjacent work you do *not* want touched. This is the highest-value field; it's what stops scope creep.

Add **how to verify**: the command to run, the page to load, the input that currently fails. If you can't describe verification, the task isn't ready.

## Review checklist for agent PRs

- **Over-engineering** — Is there an abstraction nobody asked for? An interface with one implementation? A config option with one value? Ask which rung of the ladder was skipped.
- **New dependencies** — Was a package added for something the stdlib does? Check the diff for lockfile changes.
- **Scope** — Does the diff do only what the issue asked? Unrequested renames, reformatting, and "while I was in there" edits go back.
- **Root cause** — Does the fix handle sibling callers, or only the path the ticket named?
- **Tests** — Non-trivial logic needs one runnable check that fails if the logic breaks. Trivial one-liners don't.
- **Security** — Input validation at trust boundaries, authz on new endpoints, no secrets, no string-built SQL, no unescaped user content.
- **Accessibility** — New UI needs labels, keyboard paths, and focus handling. Agents skip this unless told.
- **Comprehension** — Can you explain every line? If not, ask before merging. Unreviewed agent code is the main risk, not wrong agent code.

## When to say no

Push back — and expect the agent to push back on you:

- The request implies a rewrite when a fix would do.
- A new dependency arrives for something small and already solved.
- The task is underspecified and you'd be guessing at the requirements. Ask instead of building the wrong thing.
- Scope grew mid-task. Land what was asked, file an issue for the rest.
- You don't understand the change and can't get to understanding it. Shrink it until you can.

A good agent asks "do you actually need X, or does Y cover it?" That question is the ruleset working, not the agent stalling.
