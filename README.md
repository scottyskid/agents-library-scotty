# agents-library-scotty

Personal agent skill library — one git repo as the single source of truth for
skills used in **Claude Code** and **Cowork** (Claude Desktop) via the plugin
marketplace, and in **OpenCode** and **GitHub Copilot** via the
[`skills` CLI](https://github.com/vercel-labs/skills).

## What's inside

```
agents-library-scotty/
├── .claude-plugin/
│   └── marketplace.json        # marketplace manifest (lists the plugins below)
└── my-skills/                  # the plugin
    ├── .claude-plugin/
    │   └── plugin.json         # plugin manifest (name, version)
    └── skills/
        ├── caveman/
        ├── daily-focus/
        ├── diagnose/
        ├── ...                 # 29 personal skills total
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
| daily-focus | Blended, ranked morning brief across calendar, Teams, Todoist, and Notion | Authored by Scotty via Claude Cowork |
| debug-with-grafana | Structured diagnostic workflow using Grafana observability data | [grafana/gcx](https://github.com/grafana/gcx) (`claude-plugin/skills/debug-with-grafana`) |
| diagnose | Disciplined diagnosis loop for hard bugs and perf regressions | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/diagnose`) |
| explore-datasources | Discover available datasources, metrics, labels, and log streams in Grafana | [grafana/gcx](https://github.com/grafana/gcx) (`claude-plugin/skills/explore-datasources`) |
| find-skills | Discover and install agent skills | [vercel-labs/skills](https://github.com/vercel-labs/skills) (`skills/find-skills`) |
| gcx | Manage Grafana Cloud resources via the gcx CLI | [grafana/gcx](https://github.com/grafana/gcx) (`claude-plugin/skills/gcx`); local copy has drifted from upstream |
| grill-me | Relentless interview to stress-test a plan or design | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/grill-me`) |
| grill-with-docs | Plan grilling that updates CONTEXT.md/ADRs as decisions crystallise | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/grill-with-docs`) |
| handoff | Compact the conversation into a handoff document for another agent | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/handoff`) |
| humanizer | Remove signs of AI-generated writing to make text sound natural | [blader/humanizer](https://github.com/blader/humanizer) (`SKILL.md`, v2.8.2, MIT); local addition: default "Match Scotty's Voice" section + `references/scotty-voice-profile.md` |
| improve-codebase-architecture | Find architecture deepening and refactoring opportunities | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/improve-codebase-architecture`) |
| investigate-alert | Determine why a Grafana alert is firing, its scope and impact | [grafana/gcx](https://github.com/grafana/gcx) (`claude-plugin/skills/investigate-alert`) |
| meeting-follow-up | Draft a Teams-ready meeting follow-up (per-topic subheadings, named owners on every action item) and post it to Notion for approval | Authored by Scotty via Claude Cowork; revised 2026-07-17 based on real-use friction |
| prompt-master | Generates token-efficient, tool-specific prompts for 30+ AI tools | [nidhinjs/prompt-master](https://github.com/nidhinjs/prompt-master) (`SKILL.md` + `references/`, v1.7.0, MIT) |
| prototype | Throwaway prototype to flesh out a design before committing | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/prototype`) |
| qa | Conversational QA session that files GitHub issues | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/deprecated/qa`) |
| request-refactor-plan | Refactor plan with tiny commits, filed as a GitHub issue | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/deprecated/request-refactor-plan`) |
| review | Review changes since a fixed point along Standards + Spec axes | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/in-progress/review`) |
| scaffold-exercises | Create exercise directory structures that pass linting | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/misc/scaffold-exercises`) |
| slo-manage | Create, update, pull, push, or delete SLO definitions | [grafana/gcx](https://github.com/grafana/gcx) (`claude-plugin/skills/slo-manage`) |
| teach | Teach the user a new skill or concept within the workspace | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/teach`) |
| to-issues | Break a plan/spec/PRD into independently-grabbable issues | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/to-issues`) |
| to-prd | Turn the current conversation context into a PRD | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/to-prd`) |
| triage | Triage issues through a role-driven state machine | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/triage`) |
| ubiquitous-language | Extract a DDD-style glossary from the conversation | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/deprecated/ubiquitous-language`) |
| write-a-skill | Create new agent skills with proper structure | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/write-a-skill`) |
| writing-beats | Shape an article as a journey of beats, choose-your-own-adventure style | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/in-progress/writing-beats`) |
| zoom-out | Broader context / higher-level perspective on unfamiliar code | [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/engineering/zoom-out`) |

Original sources were captured at import time (June 2026, from the `skills` CLI
lockfile as it was then). The copies in this repo are the source of truth from then
on and may drift from upstream — the lockfile itself now points at this repo.

## Prerequisites

Install whichever of these you plan to use, then verify from a terminal:

| Tool | Needed for | Install | Verify |
|---|---|---|---|
| [Claude Code](https://code.claude.com/docs/en/overview) | plugin install in Claude Code | `npm install -g @anthropic-ai/claude-code` (or the native installer) | `claude --version` |
| [Claude Desktop](https://claude.ai/download) | Cowork | download from claude.ai | open the app → Cowork tab |
| [Node.js](https://nodejs.org/) (includes `npx`) | the `skills` CLI for other agents | download LTS from nodejs.org | `npx --version` |
| git | cloning this repo / marketplace installs | [git-scm.com](https://git-scm.com/downloads) | `git --version` |

## Install

### Claude Code

```
/plugin marketplace add scottyskid/agents-library-scotty
/plugin install my-skills@agents-library-scotty
```

(Use the full URL `https://github.com/scottyskid/agents-library-scotty` if the
shorthand doesn't resolve. In a non-interactive shell, the equivalent CLI commands
are `claude plugin marketplace add …` and `claude plugin install …`.)

### Cowork (Claude Desktop)

1. Open **Settings → Capabilities**.
2. Under plugin marketplaces, choose **Add marketplace** and paste the repo URL:
   `https://github.com/scottyskid/agents-library-scotty`
3. Find **my-skills** in the marketplace listing and click **Install**.

### Other agents (OpenCode, GitHub Copilot)

```
npx skills add https://github.com/scottyskid/agents-library-scotty/tree/main/my-skills/skills -g -y --skill '*' --agent opencode --agent github-copilot
```

This installs every skill into the central store at `~/.agents/skills` and records
this repo as the source in `~/.agents/.skill-lock.json`. The `skills` CLI discovers
skills by scanning for `SKILL.md` files; the `/tree/main/my-skills/skills` URL scopes
that scan to the same folder the Claude plugin uses, so both mechanisms always agree
on what's canonical. Notes: repeat `--agent` (and `--skill`) per value — a
comma-separated list is rejected; add `-l` to preview without installing.

### Verify the install

- **Claude Code:** run `claude plugin list` — `my-skills@agents-library-scotty`
  should show as enabled at the current version. Then in a fresh session, invoke
  any skill (e.g. say "zoom out") to confirm skills load.
- **Cowork:** start a fresh session and check that the my-skills skills appear in
  its capabilities/skills list, or invoke one directly.
- **Other agents:** run `npx skills ls -g` — every skill should list this repo's
  install with `Agents: GitHub Copilot, OpenCode`.

## How to sync changes

All edits happen in this repo — never edit installed copies.

1. Edit or add a skill under `my-skills/skills/<skill-name>/SKILL.md`.
2. Update the **Skills directory** table in this README (new row for a new skill,
   revised description/source note for a modified one).
3. Bump the top-level `version` in `my-skills/.claude-plugin/plugin.json`, and keep
   the duplicated `version` fields in `.claude-plugin/marketplace.json` (marketplace
   metadata + plugin entry) in agreement. Which digit moves depends on the change:
   - **Minor** (second number, e.g. `0.6.0` → `0.7.0`): a skill is **added or removed**.
   - **Patch** (third number, e.g. `0.6.0` → `0.6.1`): an existing skill is **modified**.

   > **Only these top-level versions move.** A skill's own version (e.g. the
   > `version:` in `humanizer/SKILL.md`, or a skill's own README version history)
   > tracks the **upstream source** it was imported from — leave it pinned to that
   > source version even when you edit the skill locally. The top-level
   > `plugin.json` + `marketplace.json` versions (kept in sync) are the only ones
   > that reflect changes made in this repo.
4. Commit and push:

   ```
   git add -A
   git commit -m "Update <skill>: <what changed>"
   git push
   ```

5. Pull the update into each tool:
   - **Claude Code:** `claude plugin marketplace update agents-library-scotty` then
     `claude plugin update my-skills@agents-library-scotty`
     (or `/plugin marketplace update` in the interactive REPL). Restart to apply.
   - **Cowork (Claude Desktop) — Scotty's primary surface:** Cowork keeps its **own**
     plugin materialization (under
     `%APPDATA%\Claude\local-agent-mode-sessions\…\rpm\plugin_*\`), separate from the
     Claude Code CLI's cache. In practice it auto-pulls the pushed marketplace update
     on its own — you do **not** need to run the CLI commands above or the Settings →
     Capabilities re-sync for Cowork to get the new version. The catch is *when* it
     takes effect: see the callout below. (If a full restart still shows the old
     version, then do the manual re-sync: Settings → Capabilities → open the
     marketplace entry → re-sync / update.)
   - **Other agents (OpenCode, GitHub Copilot):** run `npx skills update -g -y` —
     this pulls from the pushed GitHub repo (not your local checkout), so always
     push first. These agents read the central store at `~/.agents/skills`, which
     the `skills` CLI keeps pointed at this repo (see `~/.agents/.skill-lock.json`).
     To wire a *new* skill into them (update only refreshes existing ones), re-run
     the install command from the "Other agents" section above.

> **A session's skill list is frozen when the chat starts.** Adding or updating a
> skill never appears in a conversation that was already open — not in Claude Code and
> not in Cowork. The files update on disk underneath the running session, but its skill
> registry is a snapshot taken at creation, and there is **no way to refresh an existing
> chat** (no reindex, no file edit, nothing). So after syncing:
>
> - **Start a brand-new chat** to use the new/updated skill. Old chats will never see it —
>   if you need it for work started in an old chat, open a fresh one and paste the context across.
> - If even a new chat shows the old version, **fully quit and reopen the app** (Cowork:
>   Ctrl+Q / quit from the tray, not just closing the window; Claude Code: exit the REPL
>   entirely) so it rebuilds the skill registry, *then* start the new chat.
>
> Rule of thumb: **new skills only reach chats created after they were installed.**

## Adding a new skill

1. Use the **write-a-skill** skill (included in this plugin) to author it — ask
   Claude to "write a skill for <what it should do>" and direct the output to
   `my-skills/skills/<new-skill>/`. It handles proper structure, progressive
   disclosure, and bundled resources. At minimum a skill is a
   `my-skills/skills/<new-skill>/SKILL.md` with YAML frontmatter:

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
