# Devlog

A lightweight, agent-friendly workflow for keeping a durable engineering log across projects. Capture decisions, bug investigations, ideas, technical discussions, and progress from coding-agent conversations, then recall or query that context later.

This repository contains the reusable skill and the **core template** for a separate devlog. Your actual log entries belong in your own devlog folder or repository, not here.

## What's included

| Path | Purpose |
| --- | --- |
| `SKILL.md` | Optional skill installation for agents that support reusable skills. It helps the agent discover and follow your devlog instructions. |
| `template/AGENTS.md` | The core of the devlog: complete schema, directory layout, and Archive/Recall/Query/Lint workflows. Any coding agent that can read Markdown can follow it. |
| `template/index.md` | Empty root catalog with one row per project. |

The template has no example project folders or notes. Create project folders lazily when you have something to record.

## Quick start

### 1. Clone this repository

```bash
git clone <skill-repository-url> devlog-skill
cd devlog-skill
```

Replace `<skill-repository-url>` with the Git URL of this repository.

### 2. Create your separate devlog from the template

`AGENTS.md` is the main part: it defines the storage structure, frontmatter schema, naming conventions, and how an agent should archive, recall, query, and lint your notes.

```bash
mkdir -p ~/devlog
cp template/AGENTS.md template/index.md ~/devlog/
cd ~/devlog
git init
```

This creates an empty catalog and instructions. There is no need to create a `projects/` directory yet; the agent creates `projects/<slug>/` when first archiving a project. If you use Claude Code, optionally add the conventional instructions filename as a symlink:

```bash
ln -s AGENTS.md CLAUDE.md
```

Review `AGENTS.md` and adjust it to fit your preferred workflow. It is the source of truth for your log. Because your log may contain proprietary project context, use a private remote if you publish or sync it, and back it up appropriately.

### 3. Install the skill (optional)

The skill is optional when your agent can read `AGENTS.md` directly. For Claude Code, install it with:

```bash
mkdir -p ~/.claude/skills/devlog
cp /path/to/devlog-skill/SKILL.md ~/.claude/skills/devlog/SKILL.md
```

Replace `/path/to/devlog-skill` with the location where you cloned this repository. For Codex, Cursor, Gemini, and other agents, install the same `SKILL.md` in their documented skills directory, if they have one. Otherwise, give the agent a direct instruction to read your devlog's `AGENTS.md`.

### 4. Point your agent at the devlog

From any project directory, tell the agent the location of your separate log:

> My development log is in `~/devlog`. Read `~/devlog/AGENTS.md` before using it.

The skill intentionally does not prescribe a fixed path. If you have not told the agent where the log is and it cannot infer the location reliably, it should ask rather than guess.

## How to use it

Use these requests from a project directory. The project/repository directory name is the default project slug, and the root `index.md` is checked before a new project entry is created.

### Archive a conversation

After a design discussion, debugging session, or milestone:

> Archive this discussion in my devlog.

Or ask for a particular durable item:

> Save the decision about the retry strategy, including why we chose it.

The agent distills useful context from the conversation, classifies it according to `AGENTS.md`, updates the project summary, and links related notes. It should prefer updating an existing entry over creating a duplicate.

### Recall project context

When returning to a project:

> Where did we leave off on this project? Check the devlog.

The agent starts with the project's `index.md` and summarizes status, next steps, open bugs, and unresolved questions with links to relevant notes.

### Query a decision or fix

> Why did we choose the queue-based approach?

> How did we fix the duplicate webhook bug?

The agent searches the project log and cites the pages used. If the answer is not recorded, it should say so instead of inventing history.

### Lint the log

> Lint the devlog for stale statuses, contradictions, broken links, and orphaned notes.

The agent reviews the log against `AGENTS.md`, reports issues, and makes focused fixes consistent with the schema.

## Default log layout

`template/AGENTS.md` defines the full schema. The default layout creates content only when needed:

```text
devlog/
├── AGENTS.md
├── CLAUDE.md -> AGENTS.md   # optional symlink
├── index.md                # global project catalog
└── projects/
    └── <project-slug>/
        ├── index.md         # status, next steps, navigation
        ├── log.md           # chronological session history
        ├── decisions/       # technical decisions and rationale
        ├── bugs/            # investigations, root causes, fixes
        ├── ideas/           # proposals and lifecycle status
        └── notes/           # focused technical knowledge
```

Project `index.md` and `log.md` files are concise navigation/history pages. Other entries use YAML frontmatter (`type`, `title`, `status`, `description`, optional `tags`, and `timestamp`) as specified in `AGENTS.md`. The allowed statuses vary by entry type; see the template for the exact vocabulary and examples.

## Privacy and scope

- Keep the actual devlog separate from this reusable skill/template repository.
- Use a private remote for notes that include proprietary, sensitive, or incident context.
- Never store credentials or tokens in the log.
- Keep the log focused on engineering knowledge tied to projects; put general reference material elsewhere.
- Review notes before sharing or publishing them.
