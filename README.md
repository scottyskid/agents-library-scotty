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

## Install

### Claude Code

```
/plugin marketplace add ScottySkidmore/agents-library-scotty
/plugin install my-skills@agents-library-scotty
```

(Use the full URL `https://github.com/ScottySkidmore/agents-library-scotty` if the
shorthand doesn't resolve. For a private repo, make sure `git` can authenticate,
e.g. via `gh auth login`.)

### Cowork (Claude Desktop)

1. Open **Settings → Capabilities**.
2. Under plugin marketplaces, choose **Add marketplace** and paste the repo URL:
   `https://github.com/ScottySkidmore/agents-library-scotty`
3. Find **my-skills** in the marketplace listing and click **Install**.

### Verify the install

In either tool, start a fresh session and say:

> run the hello demo

You should get a "👋 hello-demo skill loaded successfully!" message echoing today's
date. If you do, the whole path (repo → marketplace → plugin → skill) works.

## How to sync changes

All edits happen in this repo — never edit installed copies.

1. Edit or add a skill under `my-skills/skills/<skill-name>/SKILL.md`.
2. Bump `version` in `my-skills/.claude-plugin/plugin.json` (e.g. `0.1.0` → `0.1.1`).
3. Commit and push:

   ```
   git add -A
   git commit -m "Update <skill>: <what changed>"
   git push
   ```

4. Pull the update into each tool:
   - **Claude Code:** `/plugin marketplace update agents-library-scotty`
     (then reinstall/update the plugin if prompted)
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

2. Bump the plugin version, commit, push, and sync as above.
