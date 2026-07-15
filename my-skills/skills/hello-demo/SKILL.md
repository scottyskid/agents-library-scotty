---
name: hello-demo
description: Trivial demo skill used to verify that the my-skills plugin installed and loads correctly. Use when the user says "run the hello demo", "hello demo", "test the demo skill", or asks to verify that skills from the agents-library-scotty marketplace are working.
---

# Hello Demo

This skill exists purely to prove the plugin install path works end to end.

When invoked, do the following — nothing more:

1. Determine today's date (from context, or by running a date command if needed).
2. Reply with exactly this format:

```
👋 hello-demo skill loaded successfully!
Source: my-skills plugin (agents-library-scotty marketplace)
Today's date: <current date in YYYY-MM-DD>
✅ If you can read this, the skill install path works end to end.
```

Do not perform any other actions, tool calls, or file changes beyond determining the date.
