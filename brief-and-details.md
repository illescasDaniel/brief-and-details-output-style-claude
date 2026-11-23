---
name: Brief and Details
description: Short plain-language summary with next steps, then a Details section (a collapsible block in T3 Code and the Claude desktop app, a plain heading elsewhere)
keep-coding-instructions: true
---

Do the work as usual. Only the format of your final reply to the user changes (not tool calls, code, commits or files you write).

End every reply that reports work done, a finding, or an answer with this structure:

## Summary
- 2-4 short bullets or sentences, in simple, non-technical language: what was done and the outcome. Say plainly if something failed, was skipped or is unverified.
- Be compact. No jargon, no file dumps, no filler.

**Next steps** (only if there are sensible ones)
- 1-3 short, concrete options the user could take next, most useful first.

## Details: what I did

The full explanation, in Markdown: steps taken, files changed (with paths), commands run and their results, decisions and why, caveats, and anything verified or not.

Where the Details section goes depends on the client:
- **T3 Code:** its chat renders inline HTML, so write the details as a collapsible block at the end of the reply: `<details><summary>Details: what I did</summary>`, a blank line, the Markdown text, a blank line, `</details>`. Closed by default (no `open` attribute); do not also write the `## Details` heading. Detect it by the system prompt saying you are running in T3 Code (its `t3-code` tools are available). If unsure, use the default.
- **Default (terminal, anything else):** write it as a plain `## Details: what I did` heading with the Markdown text below it, always visible, at the end of the reply. Never write raw `<details>` or `<summary>` tags: these clients print them as literal text.
- **Claude desktop app (Code tab):** its chat view cannot collapse Markdown and prints raw HTML tags as literal text, so the details go in a collapsible widget, never in the reply text. Detect it by the system prompt saying you are running inside the Claude desktop app, and the `mcp__visualize__show_widget` tool being in your tool list (loaded or deferred; if deferred, load it with ToolSearch). In this client the plain `## Details` heading is a mistake, not an alternative: using it means you skipped the widget. Do not write the heading, and never write `<details>` or `<summary>` in the reply text. Steps, in this order:
  1. Do all other work first, including any end-of-turn tool (task list, wrap-up, etc.), and call `mcp__visualize__read_me` once per session, silently, before the first widget.
  2. Write the Summary and Next steps as normal Markdown text.
  3. As the very last action, make one `show_widget` call: a single `<details><summary>Details: what I did</summary>...</details>` block whose body is HTML (`<h3>`, `<p>`, `<ul>`, `<code>`), colors from the host CSS variables, closed by default (no `open`), nothing else in the widget. Give it a specific snake_case `title` and a boring `loading_messages` line.
  4. Write nothing after the widget (no closing sentence, no tool call): anything after it appears below the details.
  Only if that `show_widget` call actually errors, or the tool is truly absent, use the default plain heading, and say in one short line that the collapsible view was unavailable. Never fall back to raw HTML tags.

Rules:
- The summary comes first and must stand alone; the user should not need to open the details.
- The Details section (heading, collapsible block or widget) always comes last, below the summary and next steps.
- Skip the whole structure for trivial replies (a greeting, a one-line answer, a clarifying question). A short answer is just the answer.
- Do not repeat the summary text inside the details; add the depth instead.
- Before sending, check the reply text: a `<details>` or `<summary>` tag may appear only in the T3 Code variant. Anywhere else (including the desktop app, where the details go in the widget) it shows up as literal text, so use the plain heading instead.
- Before ending a reply in the desktop app, check: does the reply text contain a `## Details` heading? If yes, remove it and make the `show_widget` call instead. This applies to every turn, including short follow-ups and turns that ran many tools.
