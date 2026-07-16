---
name: daily-focus
description: Prioritise Scotty's work for the day into a blended, ranked morning brief. Use when Scotty says "prioritise my day", "what should I focus on today", "daily focus", "morning brief", or asks what to work on next.
---

# daily-focus

Produce a short, blended, **scannable** morning brief telling Scotty what to focus on today. Pull from four sources, synthesise into ONE ranked list — never hand back four separate piles. Three sections only: **🎯 Focus today**, **🗓️ Schedule**, **📥 Everything else** (folded).

## Identity (hardcoded)

- User: **Scotty**, email **scottys@eftsure.com.au**.
- Used to resolve "meetings I'm in", "@mentions of me", "waiting on a reply from me".
- Check the current date/time with `bash` (`date`) before filtering. Scotty is in **AEST**; calendar times come back in UTC — convert to AEST for display.

## Formatting rules (important — Scotty struggles with big text blocks)

- **12-hour time always** (e.g. `1:00pm`, `9:45am`). Never 24-hour.
- Lead every section and most lines with an **icon**. Use plenty of horizontal rules / whitespace between blocks.
- Short lines. No paragraph longer than ~2 lines. Prefer tight bullets over prose.
- Icon legend: 🎯 focus · 🗓️ schedule · 📥 folded · ⚠️ untracked follow-up · 💬 waiting on you · 📄 meeting prep · 🪨 big rock · ✅ Todoist · 🏢 eftsure · 🏠 personal · 👀 FYI.

## Linking rules

- **Link liberally.** Wherever a source item has a URL, hyperlink it inline so Scotty can jump straight there: Teams messages/threads, emails, Confluence/Jira pages, Todoist task links, Notion projects.
- **Notion meeting notes: always link them with the exact display text `meeting document`** — e.g. `Kevin 1:1 ([meeting document](<url>))`. Use this phrasing every time a note is matched or referenced.

## Sources, scope, and exact queries

Run these in parallel. **The queries below are the hardcoded starting point — refine here over time.**

### 1. 🗓️ Outlook calendar — `outlook_calendar_search`
```
query: "*"
afterDateTime: <today 00:00 local>
beforeDateTime: <tomorrow 12:00 local>
order: "oldest"
limit: 25
```
- Include **accepted + tentative**. **Skip declined.**
- **Ignore custom time-blocks — drop any event titled "Lunch" or "GSD"** (Getting Shit Done). These are focus blocks, not meetings to prep for. Treat other all-day events as context only.
- Split for display: **today** (from now onward) vs **tomorrow morning** (through ~12:00pm).

### 2. 💬 Microsoft Teams — `chat_message_search`
```
query: "*"
afterDateTime: <now minus 24 hours>
limit: 25
```
- Keep: **DMs to Scotty**, messages that **@mention Scotty**, and ALL messages in these channels even without a mention: **"Platform Engineering"**, **"P&T Management"**, **"Ask Infra"**.
- Drop meeting-chatter and messages Scotty already answered.
- Classify each: **needs a reply/action** (→ Focus) vs **FYI** (→ folded).

### 3. ✅ Todoist — split by project (`find-tasks`)
Run four hardcoded queries so 🏢 eftsure and 🏠 personal stay separate:
```
Eftsure, due:      filter = "(today | overdue) & #eftsure"
Eftsure, undated:  filter = "(p1 | p2 | p3) & no date & #eftsure"
Personal, due:     filter = "(today | overdue) & #personal"
Personal, undated: filter = "(p1 | p2 | p3) & no date & #personal"
limit: 50 each
```
- Flag **overdue** distinctly. Priority note: Scotty reserves **p1** for genuinely critical, so **p2/p3** are his normal "important". Undated **p4** is excluded on purpose.

### 4. 📄🪨 Notion — `notion-query-data-sources` (mode `view`)
**Big rocks — Projects [PT] board view:**
```
view_url: https://app.notion.com/p/223366acb10581fcaf6bc63245df2a4f?v=223366acb10581ff87a3000c7ebbe2ed
```
Take the **top 5 "In progress"** rows in the order returned (Scotty's manual board order = his priority). Do not re-sort.

**Meeting prep + follow-ups — Notes [PT] active view:**
```
view_url: https://app.notion.com/p/223366acb105817fb6e7c0cc8e1d8968?v=223366acb1058194a319000c186083b1
page_size: 15
```
Bucket by "Meeting Time". Person is usually in the title (e.g. "Kevin 1:1"); has linked People + Project.

## Assembly

- **📄 Meeting prep:** match each real meeting today/tomorrow-morning to a Notion note by title/attendee/date; attach linked Project + any prep needed.
- **⚠️ Yesterday's follow-ups:** for notes with Meeting Time = **yesterday** that have a transcript, fetch with `notion-fetch` + `include_transcript: true`. Pull commitments Scotty made and decisions needing follow-up. Only surface items that are genuinely **Scotty's** to own (he is often the coach/manager in 1:1s — don't assign his reports' tasks to him). Cross-check each against Todoist + Projects; if tracked nowhere, mark **⚠️ not tracked anywhere**.
- **Blended ranking** for Focus: (1) time-bound meeting prep, (2) 💬 people waiting on Scotty, (3) ⚠️ untracked follow-ups, (4) overdue Todoist, (5) due-today Todoist, (6) 🪨 move a top big rock. Lower-urgency + undated items go to folded.

## Output — three sections, heavily scannable

**🎯 Focus today** — one ranked list, ~5–7 items. Each: icon + action + one-line *why* + source tag. e.g.
`📄 Read Grafana docs before your 1:00pm — prep task due today (meeting @ 1:00pm)`

---

**🗓️ Schedule** — render as a **visual day-column graphic** via the `show_widget` tool (SVG), NOT a text list. Conventions:
- Vertical time axis with hour gridlines + 12h labels (e.g. `9am`, `1pm`). Scale ~60px/hour across the working day.
- One rounded bar per meeting, **height proportional to duration** so gaps read as free time. Label each bar to its right: `12h start · title · duration` + any 📄/🏠 icon.
- Tentative meetings: dashed outline, no fill, muted text. Annotate notable free windows (e.g. `free ~90m`). Draw a dashed "now" line at the current time.
- Colours via CSS variables only (`--bg-accent`, `--border-accent`, `--text-primary/secondary/muted`, `--border`, `--text-danger`); dark-mode safe; text 11–12px, weights 400/500; no negative coords; set viewBox height from the last bar + buffer.
- Show **today (remaining)**; if useful, a second smaller column or a short text line for **tomorrow morning**.
- **Never announce that Lunch/GSD were filtered out** — just silently exclude them.

---

**📥 Everything else** (folded/short bullets, reference only):
- 🏢 remaining eftsure Todoist · 🏠 remaining personal Todoist
- 🪨 other in-progress big rocks (beyond top 5)
- ⚠️ other untracked follow-ups (offer to add to Todoist)
- 👀 FYI Teams items (no reply needed)

## Notes
- **Read-only.** Never modify Todoist/Notion or send Teams messages unless Scotty explicitly asks in a follow-up.
- If a source is empty or unavailable, say so in one line and continue — don't block the brief.
- Close with a one-line offer to turn this into a live artifact or a scheduled weekday morning run.
