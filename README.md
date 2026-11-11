# Claude-model-router

A Claude Code plugin that keeps your sessions on Sonnet and brings in Opus or Haiku only where testing showed it pays off.

## What I found when testing it

I ran the same prompts in three setups on small practice projects, scored with hidden tests (the model never saw them) or known answers. Costs are list-price dollars from Claude Code's own per-model usage report.

| Job | Opus | Sonnet + this plugin | Sonnet alone |
| --- | --- | --- | --- |
| 3 small jobs (lookup, bug fix, README section) | $0.43 | $0.10 | $0.08 |
| 4 jobs (6,000-line log, two bugs, API docs) | $0.28 | $0.13 | $0.12 |
| 40 support tickets + money-splitting bug + changelog, 3 runs | $0.31–0.33, one miscount | $0.16–0.17 | $0.13–0.14 |
| Scheduler bug around clock changes, 14 hidden tests, 3 runs | $0.30–0.33 | $0.12–0.13 | $0.11–0.13 |

Every setup passed every hidden test. Opus got one ticket count wrong. The rest of the answers were correct everywhere.

What that taught me:

- Running on Sonnet instead of Opus cut usage by 55–75% with no drop in quality on these jobs. That is most of the saving.
- Splitting every prompt into parts and handing them to helpers cost more, not less. Each hand-off means extra steps for the main session, and steps cost more than the cheaper model saves. An earlier version did this and I removed it.
- A skill with `model: opus` only switches the model when you type it yourself (`/hard-task`). When Claude calls the skill on its own, the reply stays on the same model. So Claude can only reach Opus through a helper, and a helper starts without your conversation.
- Sending a risky job (deleting accounts from a database) straight to an Opus helper cost 3–4 times as much and gave the same result as Sonnet.

The practice jobs were small. Sonnet never failed one, so the step up to Opus after a failed attempt has not been tested yet.

## What it does

- Your session runs on Sonnet and does the work itself, including prompts with several parts.
- If Sonnet's own attempt still fails its tests after a second look, the job goes to the `expert` helper (Opus) with a full brief.
- Very large reading jobs (well over a hundred files that have to be read, not searched) go to the `scout` helper (Haiku). Web research goes to the `researcher` helper (Sonnet).
- Before deleting or rewriting data, it makes a backup or does a dry run first.
- Type `/hard-task` when you want the rest of a reply on Opus with the whole conversation in view.
- When a helper did part of the job, the reply ends with a line like `Models: scheduler fix Opus (expert helper, after two failed tries) · rest Sonnet`.

## Install

In Claude Code:

```
/plugin marketplace add M7neeb/Claude-model-router
/plugin install model-router@model-router
/model sonnet
```

To make Sonnet the default, add `"model": "sonnet"` to `~/.claude/settings.json`. In the Claude app, pick Sonnet in the model menu; that choice wins over the settings file.

## What's inside

- `hooks/routing.md`: the rules, added to each session at startup.
- `agents/expert.md`: Opus, high effort.
- `agents/scout.md`: Haiku, low effort, read-only tools.
- `agents/researcher.md`: Sonnet, web search and fetch.
- `skills/hard-task/SKILL.md`: the `/hard-task` command.

## Limits

- Claude follows the rules by judgment and will sometimes get it wrong. Say "give this to the expert helper" or type `/hard-task` to force Opus.
- If you name a model or effort level yourself, that wins.
- It only works in Claude Code. The regular Claude chat app has no plugins.
- The numbers above come from a handful of small runs, not a benchmark.

## License

MIT
