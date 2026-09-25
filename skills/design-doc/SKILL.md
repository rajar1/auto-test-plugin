---
name: design-doc
description: Scaffold a docs/design/<TICKET>/ folder (research.md + plan.md) whenever analysis of a Jira ticket starts, so exploration findings and the execution plan have a consistent, discoverable home across any repo. Trigger at the start of ticket analysis, not just once work is deemed "big enough" to warrant it. Not for a quick single-file note — use a flat docs/design/<TICKET>-<topic>.md instead.
argument-hint: <TICKET> <short-topic-slug>
allowed-tools: Read, Write, Glob, Bash
---

Given a Jira ticket key and topic, create:

- `docs/design/<TICKET>/research.md` — findings/exploration. What's true today, what was checked, what wasn't found. Quote the ticket's own text/comments rather than paraphrasing from memory.
- `docs/design/<TICKET>/plan.md` — the execution plan: phases, exit criteria per phase, a risk table, and a short "open questions" list for anything only a human can decide.

## If the ticket is already resolved (status Done/Closed)

Don't just restate the ticket's stated intent — trace what was **actually shipped**:

- Search git history for the real implementation (`git log --all --grep`), and check whether it's actually merged to the repo's main branch (`git merge-base --is-ancestor <commit> origin/<main-branch>`) — a commit that exists somewhere in history is not the same as one that's live.
- A ticket can describe one approach and ship a different one; a planning doc can exist in the repo and never have been implemented. Cite the real diff, not the stated intent, when they diverge.
- Check whether the current branch/checkout actually has the shipped fix — if it's behind the main branch on the relevant commits, say so explicitly in research.md so nobody assumes local behavior matches what shipped.
- Write plan.md retrospectively: document the approach actually taken (including trade-offs visible in commit messages), not a speculative rebuild of what might have happened.

## Convention notes

- No ADRs — decisions live inline in `plan.md`/`research.md` prose instead.
- If the work is small enough that a single file covers it, skip the folder and write `docs/design/<TICKET>-<topic>.md` directly instead — don't create an empty `research.md` just to satisfy the convention.
- This skill is repo-agnostic by design — it assumes nothing about the host repo's language, build system, or folder layout beyond the presence of a `docs/` directory (created if missing).
