---
name: devlog
description: Use when recording or recalling project decisions, bugs, ideas, technical discussions, or progress; searching an existing project log; or reviewing a log for stale or inconsistent entries.
---

# Devlog — project development log

Use a devlog to preserve durable engineering knowledge for a project. Keep entries concise, searchable, and connected to the project they describe.

## Before working

1. Locate the project's devlog repository or log directory. Do not assume a fixed path; ask if it cannot be identified.
2. Read its `AGENTS.md` or equivalent project instructions first. Treat those instructions as the source of truth for directory structure, metadata, and conventions.
3. Check the root index or project catalog for an existing project entry before creating one. Use the repository or project directory name as the default project slug.
4. Keep the log limited to project-bound engineering knowledge. Put general reference material in an appropriate general knowledge system instead.

## Operations

- **Archive:** Distill the relevant discussion into concise entries classified by the project's conventions (for example: decisions, bugs, ideas, and notes). Preserve context and rationale; avoid transcripts and speculation.
- **Recall:** Start from the project's index or hub, then read the linked entries needed to summarize current context and open questions.
- **Query:** Search the log for evidence and answer with links or file-path citations. Say when the log does not contain enough information.
- **Lint:** Review for stale statuses, contradictions, missing links, and orphaned entries. Report findings before making broad or destructive changes.

## Writing principles

- Prefer durable decisions, root causes, and actionable learnings over transient activity.
- Separate confirmed facts from hypotheses and unresolved questions.
- Follow existing naming, frontmatter, and linking conventions; do not invent a competing schema.
- Never copy unrelated private notes or credentials into the project log.
