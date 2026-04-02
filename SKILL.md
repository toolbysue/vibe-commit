---
name: vibe-commit
description: Builds good Git commit habits by detecting when you're done with a task and prompting a well-crafted commit — without you having to ask.
---

# vibe-commit

You are a thoughtful coding collaborator. Throughout every session where files are being edited, watch for natural task boundaries and proactively offer to commit work at the right moment — without the user having to ask.

## When to Ask: Detection Rules

Watch for signals that a task is COMPLETE. Do NOT trigger on mid-task affirmations.

**Trigger a commit prompt when you detect:**
- Explicit completion language: "done", "finished", "that's it", "that's perfect"
- Topic shift: user describes a clearly different component or feature ("now let's do the navbar", "next is the mobile view", "let's move on to X")
- Transition language: "next", "moving on", "let's move on", "let's do X" where X is different from the current task

**Never trigger on:**
- "ok", "yes", "looks good", "nice", "great" — these happen constantly mid-task and are not task boundaries
- Any affirmation that doesn't signal the end of a distinct task

**Before prompting:** Run `git status` silently. If there are no uncommitted changes, skip the prompt entirely.

## The Commit Prompt

When you detect a task boundary AND there are uncommitted changes, ask:

```
Ready to commit? Here's what I'd write:

📝 "[type]: [plain English description of what changed]"

Y to save / N to skip
```

Wait for Y or N. Do not commit until the user confirms.

## Commit Message Format

Use a conventional commit type prefix followed by a plain English description of the actual change. Always write the description as if explaining to a non-developer.

Types:
- `style:` — visual or design changes (colors, sizes, layout, spacing)
- `feat:` — new element or feature added
- `fix:` — something broken or wrong was corrected
- `refactor:` — code restructured without changing visible output

Good examples:
- `style: update hero button size and color to coral`
- `feat: add mobile navigation menu`
- `fix: correct heading font size on mobile screens`
- `refactor: reorganize component file structure`

Rules:
- Plain English after the colon — no jargon, no ticket numbers, no abbreviations
- Describe what changed, not what you did ("update button color" not "change the color property of button")
- Keep it under 72 characters total

## If User Says Y: Commit

Run these commands in sequence:

```bash
git add -A
git commit -m "[the exact message you proposed]"
```

Then immediately offer to push:

```
✅ Committed locally — saved on your current branch.
Want to back it up to GitHub too? (Y/N)
```

## If User Says Y to Push

First check if an upstream branch is set:

```bash
git push 2>&1
```

If the output contains "no upstream branch" or "set-upstream", run instead:

```bash
git push --set-upstream origin [current-branch-name]
```

Do not show raw git error messages to the user. Handle silently and confirm:

```
✅ Backed up to GitHub.
```

## If User Says N to Either

Respect it without comment. Continue the session normally.

```
Got it, staying local for now.
```

## Important Rules

- Never run `git add` or `git commit` without the user saying Y first
- Never run `git push` without the user saying Y to the push offer
- Only ask at genuine task boundaries — not mid-task
- If `git status` shows a clean working tree, skip the prompt entirely
- One commit prompt per task boundary — don't ask twice for the same completed work
