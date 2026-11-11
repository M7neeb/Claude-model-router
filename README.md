# Claude-model-router

A Claude Code plugin that cuts usage by giving each part of a task to the cheapest model that can do it well.

Claude Code runs a whole session on one model. Opus is overkill for "find the file that sets the port", and Haiku is the wrong pick for a tricky bug. This plugin lets the session move work up or down as it goes:

| Kind of work | Goes to | How |
| --- | --- | --- |
| Lookups: find code, read or summarize files and logs, count things | Haiku | `scout` helper |
| Web research: compare options, read docs, check what's current | Sonnet | `researcher` helper |
| Everyday coding, edits, bug fixes, answers | Your session model | done in the main session |
| Hard parts: stubborn bugs, design choices, plans, large changes, writing people will read | Opus | `hard-task` skill |

## Why the hard parts stay in the main session

A helper (subagent) starts with an empty context. It doesn't see your conversation, what you've tried, or why. That's fine for a lookup but bad for a hard problem, so hard work isn't handed off. The `hard-task` skill switches the rest of the current reply to Opus instead, with the full conversation still there. The next message goes back to your normal model.

## Install

In Claude Code:

```
/plugin marketplace add M7neeb/Claude-model-router
/plugin install model-router@model-router
```

Then set your session to Sonnet, which is where the savings come from:

```
/model sonnet
```

To make Sonnet the default, add `"model": "sonnet"` to `~/.claude/settings.json`.

## What's inside

- `agents/scout.md`: Haiku, low effort, read-only tools.
- `agents/researcher.md`: Sonnet, web search and fetch, returns a short summary with links.
- `skills/hard-task/SKILL.md`: switches the reply to Opus.
- `hooks/routing.md`: a few lines of routing rules added to each session at startup, so Claude knows when to use each one.

## Limits

- Claude decides the routing, and it will sometimes guess wrong. Say "use scout for this" or "use hard-task" to correct it.
- If you name a model or effort level yourself, that wins.
- It only works in Claude Code. The regular Claude chat app has no plugins.
- Your plan or organization has to allow Haiku, Sonnet and Opus. If one is blocked, that part runs on your session model.

## License

MIT
