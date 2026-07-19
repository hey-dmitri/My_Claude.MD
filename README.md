# My CLAUDE.md

An opinionated baseline for how I want Claude Code to plan, investigate, edit, test, and report software work.

I built it after seeing the same failure modes across projects: unnecessary rewrites, fixes aimed at symptoms, hidden assumptions, and completion claims without evidence. The file gives Claude a default operating style when a repository does not already provide something more specific.

This is a starting point, not universal best practice. Review it and keep the parts that fit your work.

## What it optimizes for

- Small, reversible changes
- Root-cause investigation before implementation
- Explicit handling of meaningful uncertainty
- Secret-safe defaults
- Verification proportional to risk
- Clear reporting about what was and was not tested

## What it is not

`CLAUDE.md` provides instructions that Claude Code loads as context. It is not an enforcement layer, and it is separate from Claude Code's auto memory.

The `v12` label is simply the twelfth iteration of my personal file. It is not a Claude model or product version.

## Use it

Read [`CLAUDE.md`](CLAUDE.md) first. Then copy or merge the rules you want into the appropriate location:

- `~/.claude/CLAUDE.md` for personal defaults across projects
- `./CLAUDE.md` or `./.claude/CLAUDE.md` for instructions shared with one project
- `./CLAUDE.local.md` for private project-specific preferences that should not be committed

Do not overwrite an existing file without reviewing it. Repository-specific build commands, architecture, and team conventions should remain in the repository's own instructions.

Claude Code treats these files as context rather than hard policy. Use settings, permissions, or hooks when a rule must be technically enforced. The current loading behavior is documented in [How Claude remembers your project](https://code.claude.com/docs/en/memory).

## Tradeoffs

This setup asks for more investigation and verification than a lightweight prompt. That is useful for consequential changes, but it can slow down obvious, reversible work. The file therefore tells Claude to scale its process to the risk and use the project's existing task system when one already exists.

## Files

| File | Purpose |
|------|---------|
| `CLAUDE.md` | The reusable instruction baseline |
| `LICENSE` | CC0 public-domain dedication |

## License

CC0-1.0. No rights reserved.
