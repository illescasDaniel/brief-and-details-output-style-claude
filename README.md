<p align="center"><img src="assets/icon.svg" width="96" height="96" alt="Brief and Details icon: two summary bars above a collapsed disclosure triangle"></p>

# Brief and Details

A Claude Code output style that ends every substantive reply with a short, plain-language summary and a separate details section for the full explanation.

## What you get

| Part | Content |
|---|---|
| **Summary** | 2-4 bullets in simple language: what was done and the outcome, including anything that failed, was skipped or is unverified. |
| **Next steps** | 1-3 concrete options, most useful first (only when there are sensible ones). |
| **Details: what I did** | Steps, files changed, commands and results, decisions, caveats, what was and was not verified. |

Trivial replies (a greeting, a one-line answer, a clarifying question) skip the structure. Only the final reply changes; tool calls, code, commits and files are written as usual.

## How the details section is shown

| Client | Details section |
|---|---|
| Claude desktop app (Code tab) | A collapsible block drawn with the `show_widget` visual tool, closed by default. Needs the `mcp__visualize__show_widget` tool. |
| T3 Code | A collapsible `<details><summary>Details: what I did</summary>` block written inline in the reply, closed by default. |
| Terminal, anything else | A plain `## Details: what I did` heading with the text below it, always visible. |

The desktop chat view cannot collapse Markdown and prints raw `<details>` tags as literal text, so the style writes them only in T3 Code, which renders inline HTML. If the desktop signals are missing (the app is not detected or the visual tool is unavailable), the plain heading is used.

## Quick start

Copy the style into your user styles folder, then select it in Claude Code:

```bash
mkdir -p ~/.claude/output-styles
cp brief-and-details.md ~/.claude/output-styles/
```

Then run `/output-style` and choose **Brief and Details**.

## Requirements and limits

- Claude Code with output style support.
- The collapsible block needs the Claude desktop app with the visualize tool; everywhere else you get the plain heading.
- Client detection relies on the model following the instructions; there is no technical enforcement.
- Not evaluated or benchmarked; behavior was checked by hand in the desktop app only; the T3 Code variant is new and untested.

## Files

- [`brief-and-details.md`](brief-and-details.md): the output style.
- [`assets/icon.svg`](assets/icon.svg): the icon, hand-written SVG with title and description.
- [`LICENSE`](LICENSE): MIT license.

## License

[MIT](LICENSE)
