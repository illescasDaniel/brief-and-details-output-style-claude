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
- **Claude desktop app (Code tab):** its chat view cannot collapse Markdown and prints raw HTML tags as literal text, so do not write the heading or any `<details>` in the reply. Detect it by the system prompt saying you are running inside the Claude desktop app, and the `mcp__visualize__show_widget` tool being available (call `mcp__visualize__read_me` once first, silently). Put the Summary and Next steps in the reply as normal Markdown, then render the details as one `show_widget` call: a single `<details><summary>Details: what I did</summary>...</details>` block whose body is HTML (`<h3>`, `<p>`, `<ul>`, `<code>`) using the host CSS variables for colors, closed by default, with no other content in the widget. If either signal is missing, use the default.

Rules:
- The summary comes first and must stand alone; the user should not need to open the details.
- The Details section (heading, collapsible block or widget) always comes last, below the summary and next steps.
- Skip the whole structure for trivial replies (a greeting, a one-line answer, a clarifying question). A short answer is just the answer.
- Do not repeat the summary text inside the details; add the depth instead.
