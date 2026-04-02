# vibe-commit

> **vibe-commit is the only git commit skill that comes to you, not the other way around.**

A Claude Code skill that builds good Git commit habits for designers who just started using Git — by automatically detecting when work is done and prompting a well-crafted commit.

---

## Who It's For

Designers using Claude Code (CLI or IDE) who:
- Know what Git is and have a repo set up
- Don't commit consistently, or forget to
- Struggle to write descriptive commit messages
- Want a safety net without having to think about Git mid-flow

**Not for:** Complete Git beginners with no repo set up, or experienced developers who already commit regularly.

---

## What Makes It Unique

Every existing Git commit tool — `/commit` slash commands, `aicommits`, even the popular `git-commit` skill on skills.sh — requires **you** to remember to trigger it. They assume Git knowledge and put the burden on you.

`vibe-commit` is different in three ways:

**1. Claude initiates, not you.**
No command to remember. Claude watches the conversation for natural task boundaries — "done", topic shifts, "let's move on" — and brings up the commit unprompted. You never have to think about Git mid-flow.

**2. Context-aware, not keyword-triggered.**
It doesn't wait for a magic word like `/commit`. It reads intent — a clear topic shift, explicit completion, moving to a new component — and surfaces the commit at exactly the right moment.

**3. Designed for the Git-curious, not Git-fluent.**
The message format teaches by example (conventional commits in plain English). The Y/N flow keeps you in control without requiring Git knowledge. Over time, you absorb good commit habits without being explicitly taught.

---

## How It Works

### 1. You finish something
Just work normally. When Claude detects you've completed a task — you say "done", "next", or shift to something new — it checks for uncommitted changes.

### 2. Claude asks
```
Ready to commit? Here's what I'd write:

📝 "style: update hero button size and color"

Y to save / N to skip
```

### 3. You say Y or N
That's it. Claude handles the rest.

### 4. Push offer (optional)
After committing:
```
✅ Committed locally — saved on your current branch.
Want to back it up to your remote too? (Y/N)
```

---

## Commit Message Format

`vibe-commit` uses [Conventional Commits](https://www.conventionalcommits.org/) with plain English descriptions:

| Prefix | Use for |
|---|---|
| `style:` | Visual/design changes |
| `feat:` | New element or feature |
| `fix:` | Something broken corrected |
| `refactor:` | Restructured without changing output |

Examples:
```
style: update hero button size and color to coral
feat: add mobile navigation menu
fix: correct heading font size on mobile
refactor: reorganize component file structure
```

---

## Commit vs Push — What's the Difference?

**Commit** = saves a snapshot on your local machine, on your current branch. Nothing goes anywhere. Git just remembers that state exists. You can always come back to this exact moment.

**Push** = sends those local commits up to your remote. Now it's backed up online and visible to collaborators.

When you hit Y to commit, it stays on your machine. When you hit Y to push, it goes to `origin/<your current branch>` — whatever branch you're currently on, not necessarily `main`.

---

## Install

```bash
npx skills add soobinhwang/vibe-commit
```

Works with: Claude Code CLI and Claude Code IDE integration (VS Code, Cursor, etc.)

---

## Token Usage

vibe-commit is designed to stay out of the way — and that includes token usage.

When idle (no commit happening), only a short description of the skill is loaded: roughly **20 tokens per message**. The full skill instructions only load when Claude is actually running the commit flow.

For a typical 20-message coding session, the extra cost is roughly:
- **~$0.06** on Claude Sonnet
- **~$0.28** on Claude Opus

That's less than a fraction of a cent per session on Sonnet — lightweight by design.

---

## What It Doesn't Do

- No Git setup or repo initialization
- No automatic committing without asking
- No force-pushing or branch switching
- No conflict resolution
- Doesn't push to `main` or any specific branch — always your current branch
