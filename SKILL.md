---
name: vibe-commit
description: Builds good Git commit habits by detecting when you're done with a task and prompting a well-crafted commit — without you having to ask.
agents: [claude-code]
---

# vibe-commit

Watch for natural task boundaries and proactively offer to commit — without the user having to ask.

## Detection Rules

Trigger when the current task is clearly COMPLETE:
- Explicit completion: "done", "finished", "that's it", "that's perfect"
- Topic shift to a different component/feature — only if prior task was finished. Mid-task redirects ("actually, let's do X first") are NOT boundaries.
- Transition language: "next", "moving on", "let's do X" where X is a new task

Never trigger on: "ok", "yes", "looks good", "nice", "great" — these are mid-task, not boundaries.

**Before prompting:** Run `git status` silently. Skip if no uncommitted changes. If it returns a fatal error (not a git repo), suppress it and offer once per session:

```
I noticed this folder isn't set up for version control yet. Want me to set that up? (Y/N)
```

## The Commit Prompt

When a task boundary AND uncommitted changes are detected:

```
Ready to commit? Here's what I'd write:

📝 "[type]: [plain English description of what changed]"

Y to save / N to skip
```

Wait for Y or N. Do not commit until confirmed.

## Commit Message Format

Conventional commit prefix + plain English description. Types: `style:` (visual/design), `feat:` (new feature), `fix:` (correction), `refactor:` (restructured, same output)

Examples: `style: update hero button size and color to coral` / `feat: add mobile navigation menu`

No jargon, ticket numbers, or abbreviations. Describe what changed, not what you did. Under 72 characters.

## If User Says Y: Commit

Run `git status --short` and show files before staging:

```
This will save: [list of changed files]
```

Then commit:

```bash
git add -A
git commit -m "[the exact message you proposed]"
```

Offer to push:

```
✅ Committed locally — saved on your current branch.
Want to back it up to your remote too? (Y/N)
```

## If User Says Y to Push

Run `git push 2>&1`. If output includes "no upstream branch" or "set-upstream", use `git push --set-upstream origin [current-branch-name]`. Never show raw git errors. Confirm:

```
✅ Backed up to your remote.
```

## If User Says N

Respect without comment. Continue normally.
- Commit: `No problem, I won't save a snapshot right now.`
- Push: `Got it, staying local for now.`

## Important Rules

- Never run `git add`, `git commit`, or `git push` without explicit Y
- Only ask at genuine task boundaries — not mid-task
- Skip if `git status` shows a clean working tree
- One prompt per boundary — don't ask twice for the same work
