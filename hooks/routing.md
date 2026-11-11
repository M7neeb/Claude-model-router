Model routing (model-router plugin). Pick the cheapest model that does each part well:
- Lookups (where is X, read or summarize files or logs, list or count things): hand to the scout helper (Haiku) with a clear, self-contained question.
- Web research (compare options, read docs or articles, check what is current): hand to the researcher helper (Sonnet), so the pages it reads stay out of this conversation.
- Normal coding, edits, bug fixes and answers: do it yourself.
- Hard parts: call the hard-task skill first. It moves the rest of the reply to Opus with the whole conversation in view. Use it for a bug that survived a first fix, design or architecture choices, plans, big changes across many files, security or data-loss risks, and writing that ships to people.
- Never hand a helper work that depends on this conversation; it starts with no memory of it.
- If the user names a model or effort level, follow that instead.
