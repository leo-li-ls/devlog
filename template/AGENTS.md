# Devlog — project development log

This repository is a Markdown-based development log for durable, project-bound engineering knowledge: ideas, technical discussions, decisions, bug investigations, and progress. Maintain it so a future coding agent or human can quickly rebuild project context.

This file is the source of truth for the log's layout, frontmatter schema, and workflows. Read it before creating or editing log entries. `CLAUDE.md` may point to this file for agents that look for that filename.

## Layout

```text
index.md         Global catalog: one row per project and a short status. No frontmatter.
projects/        One directory per project, created lazily when first needed.
AGENTS.md        This schema and workflow guide.
CLAUDE.md        Optional symlink or copy of AGENTS.md for Claude Code.
```

Create project files and category directories only when they are needed:

```text
projects/<slug>/
├── index.md      Project hub: summary, current status, next steps, and navigation. No frontmatter.
├── log.md        Chronological history of meaningful sessions and events.
├── decisions/    Technical decisions and their rationale.
├── bugs/         Bug investigations, root causes, and fixes.
├── ideas/        Proposals and their lifecycle.
└── notes/        Focused technical knowledge that does not fit elsewhere.
```

### Project identity

- Use the current repository or project directory name as the default `<slug>`.
- Before creating a project directory, check the root `index.md` for an existing entry. Do not create duplicate slugs for the same project.
- Related but distinct repositories may have separate project entries; cross-link them when useful.
- If the conversation is clearly about a different project, use that project's existing slug. Ask only when the project identity is genuinely ambiguous.

## Document schema

The root `index.md`, each project `index.md`, and `log.md` are reserved files and do not use frontmatter. Every other Markdown entry must start with YAML frontmatter:

```yaml
---
type: Decision | Bug | Idea | Note
title: Human-readable title
status: One allowed status for the selected type
description: One-sentence summary for previews and search
tags: [optional, tags]
timestamp: 2026-01-31T12:00:00Z
---
```

Use these status values:

| Type | Allowed statuses |
| --- | --- |
| Decision | `active`, `superseded` |
| Bug | `open`, `solved`, `wontfix` |
| Idea | `proposed`, `adopted`, `rejected`, `parked` |
| Note | `current`, `outdated` |

When a decision is superseded, link to its successor. When an idea is adopted, link to the resulting decision and link back from that decision. Use ISO 8601 UTC timestamps for meaningful edits.

Suggested entry structures (adapt them to the content):

- **Decision:** Context / Options considered / Decision / Consequences.
- **Bug:** Symptom / Investigation / Root cause / Fix / Prevention.
- **Idea:** Proposal / Motivation / Rough sketch / Open questions.
- **Note:** Focused, atomic technical knowledge.

Use lowercase kebab-case filenames, for example `choose-queue-over-polling.md`. Link related entries with repository-root-relative Markdown links, for example:

```markdown
[Retry policy decision](/projects/example-service/decisions/retry-policy.md)
```

## Operations

### Archive

Archive when the user asks to log or save a discussion, or at the end of a substantial work session when requested.

1. Identify the project and check the root catalog. For a new project, create its directory and project hub, then add it to the catalog.
2. Distill only durable knowledge a future session needs: rationale, confirmed outcomes, root causes, important tradeoffs, and actionable next steps. Omit pleasantries, uninformative dead ends, and details that are trivially derivable from the code.
3. Classify the material as a Decision, Bug, Idea, or Note. Search for and update an existing entry when appropriate instead of creating a near-duplicate.
4. Keep the project hub current, especially its status and next steps. It is the first page Recall reads.
5. Append a concise dated entry to the project's `log.md`, linking the pages created or updated.
6. Update the root catalog's one-line status when it changes.
7. Keep the log project-bound. Do not record credentials, secrets, or unrelated personal information.

### Recall

Read the project's `index.md` first, then follow relevant links. Summarize current status, next steps, open bugs, and unresolved questions with citations to the notes used.

### Query

Search from the root catalog to the project's hub and then relevant entries. Answer with links or file-path citations. If the log does not contain enough evidence, say so rather than inventing history. If an answer reveals durable knowledge worth preserving, ask or archive it as a Note when appropriate.

### Lint

Review for invalid or stale statuses, contradictions, orphaned entries, missing required frontmatter, broken links, and outdated project summaries. Report findings clearly and make focused corrections consistent with the established schema.

## Writing principles

- Prefer durable decisions, root causes, and actionable learnings over a transcript of activity.
- Distinguish confirmed facts from hypotheses and unresolved questions.
- Keep pages focused and concise; split entries that have grown to cover multiple topics.
- Write the entry in the language used in the discussion; keep schema keys and status values as specified above.
- Do not paste large code blocks. Reference the relevant project file and line where possible.
- When new information conflicts with an existing entry, preserve the history and call out the contradiction rather than silently rewriting the past.
- Respect the project's existing conventions if the log already contains entries; migrate schemas only when asked.
