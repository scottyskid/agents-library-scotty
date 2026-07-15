# agents-library-scotty

Personal Claude plugin marketplace — one git repo as the single source of truth for
skills used in **Claude Code** and **Cowork** (Claude Desktop).

## What's inside

```
agents-library-scotty/
├── .claude-plugin/
│   └── marketplace.json        # marketplace manifest (lists the plugins below)
└── my-skills/                  # the plugin
    ├── .claude-plugin/
    │   └── plugin.json         # plugin manifest (name, version)
    └── skills/
        ├── hello-demo/         # trivial skill to verify installs work
        ├── caveman/
        ├── diagnose/
        ├── ...                 # 21 personal skills total
        └── zoom-out/
```

> Note: `plugin.json` lives at `my-skills/.claude-plugin/plugin.json` — that is the
> location the Claude plugin spec requires.

## Skills directory

> Keep this table up to date: add a row when a skill is added, and revise the row
> whenever a skill is modified (note local changes in the Source column if the
> skill has drifted from its original source).

| Skill | Description | Original source |
|---|---|---|
| caveman | Ultra-compressed communication mode (~75% fewer tokens) | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/caveman`) |
| diagnose | Disciplined diagnosis loop for hard bugs and perf regressions | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/diagnose`) |
| find-skills | Discover and install agent skills | [vercel-labs/skills](https://github.com/vercel-labs/skills) (`skills/find-skills`) |
| gcx | Manage Grafana Cloud resources via the gcx CLI | [grafana/gcx](https://github.com/grafana/gcx) (`claude-plugin/skills/gcx`); local copy has drifted from upstream |
| grill-me | Relentless interview to stress-test a plan or design | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/grill-me`) |
| grill-with-docs | Plan grilling that updates CONTEXT.md/ADRs as decisions crystallise | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/grill-with-docs`) |
| handoff | Compact the conversation into a handoff document for another agent | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/handoff`) |
| hello-demo | Verifies the plugin install path works end to end | original to this repo |
| improve-codebase-architecture | Find architecture deepening and refactoring opportunities | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/improve-codebase-architecture`) |
| prototype | Throwaway prototype to flesh out a design before committing | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/prototype`) |
| qa | Conversational QA session that files GitHub issues | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/deprecated/qa`) |
| request-refactor-plan | Refactor plan with tiny commits, filed as a GitHub issue | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/deprecated/request-refactor-plan`) |
| review | Review changes since a fixed point along Standards + Spec axes | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/in-progress/review`) |
| scaffold-exercises | Create exercise directory structures that pass linting | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/misc/scaffold-exercises`) |
| teach | Teach the user a new skill or concept within the workspace | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/teach`) |
| to-issues | Break a plan/spec/PRD into independently-grabbable issues | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/to-issues`) |
| to-prd | Turn the current conversation context into a PRD | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/to-prd`) |
| triage | Triage issues through a role-driven state machine | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/triage`) |
| ubiquitous-language | Extract a DDD-style glossary from the conversation | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/deprecated/ubiquitous-language`) |
| write-a-skill | Create new agent skills with proper structure | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/write-a-skill`) |
| writing-beats | Shape an article as a journey of beats, choose-your-own-adventure style | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/in-progress/writing-beats`) |
| zoom-out | Broader context / higher-level perspective on unfamiliar code | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/zoom-out`) |

Sources were recorded from the `skills` CLI lockfile (`~/.agents/.skill-lock.json`)
at the time the skills were imported (June 2026); copies in this repo are the source
of truth from then on and may drift from upstream.

## Install

### Claude Code

```
/plugin marketplace add scottyskid/agents-library-scotty
/plugin install my-skills@agents-library-scotty
```

(Use the full URL `https://github.com/scottyskid/agents-library-scotty` if the
shorthand doesn't resolve. For a private repo, make sure `git` can authenticate,
e.g. via `gh auth login`.)

### Cowork (Claude Desktop)

1. Open **Settings → Capabilities**.
2. Under plugin marketplaces, choose **Add marketplace** and paste the repo URL:
   `https://github.com/scottyskid/agents-library-scotty`
3. Find **my-skills** in the marketplace listing and click **Install**.

### Verify the install

In either tool, start a fresh session and say:

> run the hello demo

You should get a "👋 hello-demo skill loaded successfully!" message echoing today's
date. If you do, the whole path (repo → marketplace → plugin → skill) works.

## How to sync changes

All edits happen in this repo — never edit installed copies.

1. Edit or add a skill under `my-skills/skills/<skill-name>/SKILL.md`.
2. Update the **Skills directory** table in this README (new row for a new skill,
   revised description/source note for a modified one).
3. Bump `version` in `my-skills/.claude-plugin/plugin.json` (e.g. `0.1.0` → `0.1.1`).
4. Commit and push:

   ```
   git add -A
   git commit -m "Update <skill>: <what changed>"
   git push
   ```

5. Pull the update into each tool:
   - **Claude Code:** `claude plugin marketplace update agents-library-scotty` then
     `claude plugin update my-skills@agents-library-scotty`
     (or `/plugin marketplace update` in the interactive REPL)
   - **Cowork:** Settings → Capabilities → open the marketplace entry and re-sync /
     update the plugin.

## Adding a new skill

1. Create `my-skills/skills/<new-skill>/SKILL.md` with YAML frontmatter:

   ```markdown
   ---
   name: new-skill
   description: What it does and the phrases that should trigger it.
   ---

   Instructions for the agent...
   ```

2. Add the skill to the **Skills directory** table above, including where it was
   sourced from (or "original to this repo").
3. Bump the plugin version, commit, push, and sync as above.
