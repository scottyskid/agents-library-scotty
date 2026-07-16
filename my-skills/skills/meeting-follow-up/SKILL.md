---
name: meeting-follow-up
description: >-
  Draft a follow-up recap message after a meeting and post it back onto the
  meeting's Notion page, ready for Scotty to send. Reads the meeting notes and
  transcript from Notion, pulls attendees from the matching Outlook calendar
  invite, interviews Scotty to confirm the key points and action items,
  humanizes the wording, and appends the final message to the Notion page for
  approval. Use when Scotty says "write a follow-up", "meeting follow-up",
  "recap that meeting", "send a follow-up on my meeting", or provides a Notion
  meeting link and asks for a summary to send attendees.
---

# Meeting Follow-Up

Turn a meeting's Notion notes + transcript into a humanized follow-up message,
posted back onto the same Notion page for Scotty to review and send himself.

**The skill NEVER sends the message.** It drafts, gets approval, and posts to
Notion. Scotty sends it.

## Meeting notes database

Meetings live in this Notion database:
`https://app.notion.com/p/223366acb105817fb6e7c0cc8e1d8968?v=223366acb1058194a319000c186083b1`

## Workflow

Follow these steps in order. Do not skip the approval gate at the end.

### 1. Find the meeting page

- **If the query contains a Notion link**, use that page directly. Fetch it with
  `notion-fetch`.
- **If there is no link**, query the meeting-notes database for pages dated
  **today or yesterday** and match against the meeting description in the
  prompt (title, attendees, topic). Use `notion-query-meeting-notes` or
  `notion-query-data-sources` against the database above.
  - If exactly one clearly matches, proceed with it but state which page you
    chose.
  - If several plausibly match, list them and ask Scotty to pick before
    continuing. Never guess silently.

### 2. Read the meeting

Read the full page: notes **and** transcript. Capture what was discussed,
decisions made, and anything phrased as a commitment or next step.

### 3. Get attendees from the calendar

Match the meeting to an Outlook calendar invite to get the attendee list:

- Search the calendar (`outlook_calendar_search`) around the meeting's
  date/time for an event matching the meeting title/topic.
- Use the invite's attendee list as the recipients for the follow-up.
- If no invite matches, fall back to attendee names in the Notion page or
  transcript, and tell Scotty you couldn't find a matching calendar invite.

The skill only needs **names** to personalise the message — it does not send
email, so email addresses aren't required.

### 4. Interview Scotty (hybrid)

Do the heavy lifting first, then let Scotty edit:

1. Present a **draft** of:
   - the most important points discussed, and
   - the action items, each with an **owner** (map owners to attendee names).
2. **Number both lists** (key points 1, 2, 3…; action items 1, 2, 3…) so Scotty
   can reference each one by number to strike it out or edit it.
3. Ask Scotty to confirm, edit, or remove each point and action item.
4. Finish with one catch-all question: **"Anything important I missed, or
   anything that shouldn't go in the follow-up?"**

Incorporate his answers before drafting the message.

**Selecting key points** — focus on substantive things worth putting on record:
decisions made, incidents, root causes, process changes, and matters that need
follow-up. Leave out routine progress updates and general "how's it going"
chatter unless Scotty flags them as worth recording. Quality over coverage.

**Keeping action items tight** — consolidate aggressively. The final follow-up
should land on **2–3 main action items** where appropriate. Merge closely
related tasks into a single owner-led action, and drop minor or already-handled
items. Don't produce a long checklist; produce the few things that actually
matter.

### 5. Draft the follow-up message

Write a single, copy-paste-ready message in a **warm, professional colleague
tone**, first person, as if Scotty wrote it. Structure:

- Brief greeting to attendees.
- A 2–4 sentence summary of what was discussed.
- A short **Action items** section, each item with its owner.
- A friendly sign-off.

Write it as one coherent message (not scattered blocks). No meeting-minutes
formality.

### 6. Humanize

Run the drafted message through the **`my-skills:humanizer`** skill to strip AI
tells (inflated phrasing, em-dash overuse, rule-of-three, filler, corporate
boilerplate, etc.). Keep it natural, direct, and human.

> Note: a `my-writing-style` profile is not used yet. If one is set up later,
> prefer it for matching Scotty's exact voice.

### 7. Approval gate

Show Scotty the **final humanized message in chat** and wait for explicit
approval. Do not write to Notion until he says yes. If he requests changes,
revise and show again.

### 8. Post to Notion

Once approved, append the message to the **bottom** of the meeting page under a
dated heading, e.g. `Follow-up — 16 Jul 2026`. Use `notion-update-page` to add
a new block section. Keep it under its own heading so re-running the skill for a
later meeting never overwrites an earlier follow-up.

Confirm to Scotty that it's posted and remind him it's ready for him to send.

## Guardrails

- Never send the message anywhere — Notion is the only destination.
- Never post to Notion without step 7 approval.
- If the meeting page, transcript, or calendar invite can't be found, say so
  plainly rather than inventing content.
- Attribute action items to real named owners from the meeting; don't assign
  owners you can't justify from the notes/transcript/attendees.
