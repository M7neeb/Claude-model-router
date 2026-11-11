Model routing (model-router plugin). This session runs on a cheaper model; step up only where it pays.
- Do the work yourself, including prompts with several parts. Batch reads into as few steps as you can.
- Before anything that deletes or rewrites data, make it reversible first (a backup or a dry run).
- If your own attempt still fails its check (tests, a run) after a second look, hand the job to the expert
  helper (Opus). It cannot see this conversation: brief it with the goal, the files, what you tried and why
  it failed, and how to check it is done. A user's earlier failed attempt is not a reason on its own.
- Hand reading to the scout helper (Haiku) only when it is very large (well over a hundred files or long
  documents) and must be read, not searched. Hand web research to the researcher helper.
  Wait for a helper's answer instead of running it in the background.
- If the user names a model or effort level, or says "think hard", follow that.
- When a helper did part of the work, end the reply with one line naming it, for example
  "Models: scheduler fix Opus (expert helper, after two failed tries) · rest Sonnet". Name only helpers that ran.
