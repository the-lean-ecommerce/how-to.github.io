---
layout: post
title: "How to Automate the Real Claude Code Terminal UI With dash-p"
description: "Use dash-p to script the real Claude Code terminal UI from shell scripts or TypeScript without switching to a separate headless path."
date: 2026-07-21 23:39:40 +0000
categories: [how-to]
tags: [claude, typescript, cli, automation, terminal]
canonical_url: ""
image: "/assets/img/posts/2026-07-21-how-to-automate-the-real-claude-code-terminal-ui-with-dash-p/cover-1550bb1c20ee.png"
---

If you want to automate Claude Code from a script but keep the real interactive session you already trust, dash-p is the local bridge. It launches the official `claude` command in a pseudo-terminal, injects input, reads the rendered terminal output, and exposes that interaction through a CLI and a `query()` API.

![Dark aurora workflow diagram showing a terminal, a script file, and structured output connected by glowing automation lines](/assets/img/posts/2026-07-21-how-to-automate-the-real-claude-code-terminal-ui-with-dash-p/image-01-922e7147c954.png)

That matters because the tool does not replace Claude Code. It drives the real TUI, which means your normal account, local session, and permissions stay in place. For personal automation, repo summaries, or TypeScript helpers, that is usually the right tradeoff.

If you want the wider context for why this pattern is useful, start with [How I Keep Claude Code Scriptable After the Agent SDK Split](https://the-lean-ecommerce.gitlab.io/2026/07/04/how-i-keep-claude-code-scriptable-after-the-agent-sdk-split/) and [How I Build a Local Bridge to Claude Code's Real TUI](https://the-lean-ecommerce.gitlab.io/2026/07/08/how-i-build-a-local-bridge-to-claude-code-s-real-tui/). For a shell-first variant, [How to Automate Claude Code From a Local Script With dash-p](https://how-to.the-lean-ecommerce.com/2026/07/19/how-to-automate-claude-code-from-a-local-script-with-dash-p/) is the closest companion post.

## 1. Install dash-p and confirm Claude Code works

You need two basics before anything else: the official `claude` CLI installed and logged in, and Node.js 20 or newer. dash-p sits on top of that local setup instead of bypassing it.

Install the package globally:

```bash
npm install -g @ybouane/dash-p
dash-p "summarize this repo"
```

If you prefer not to install it globally, use `npx` instead:

```bash
npx @ybouane/dash-p "what changed in this branch"
```

Expected result: Claude Code runs through your normal local session and returns a useful answer in the terminal. If `claude` is missing or you are not logged in yet, fix that first. dash-p depends on the same interactive setup.

![Clean terminal-to-file pipeline rendered as a dark aurora scene](/assets/img/posts/2026-07-21-how-to-automate-the-real-claude-code-terminal-ui-with-dash-p/image-02-a76de24afc22.png)

## 2. Use `query()` when you want typed automation

The TypeScript API is the most useful part if you are building a real workflow. Instead of shelling out manually every time, you can treat Claude Code like a local helper that returns structured messages.

```ts
import { query } from "@ybouane/dash-p";

for await (const msg of query({
  prompt: "Review this diff and return JSON with summary, risks, and next_steps.",
  options: { model: "sonnet", includePartialMessages: true },
})) {
  if (msg.type === "result") {
    console.log(msg.result);
  }
}
```

That pattern is useful when you want progress updates during a longer request, or when you want to capture the final result and send it into your own scripts. The product also supports JSON and stream JSON output modes, which makes it easier to pipe the result into other tools without scraping terminal text.

![Abstract terminal automation diagram with a real local session flowing into TypeScript output](/assets/img/posts/2026-07-21-how-to-automate-the-real-claude-code-terminal-ui-with-dash-p/image-03-d5a4f454d880.png)

## 3. Put it into a shell pipeline

For lightweight automation, a shell wrapper is often enough. Ask for the shape of response you want, then hand the output to the next command in your pipeline.

```bash
summary=$(dash-p "summarize the staged changes in one short paragraph")
printf "%s\n" "$summary"
```

If you want the model to produce a checklist, a risk list, or a short handoff note, say that in the prompt and keep the command simple. The point is not to build a giant abstraction layer. It is to preserve the same Claude Code session you already use while giving your scripts a reliable entry point.

## 4. Know the tradeoffs before you depend on it

dash-p is powerful because it uses the real interface, and that is also the main limitation. A few constraints matter before you rely on it heavily:

- It depends on the rendered Claude Code TUI, so UI changes can affect scraping.
- It requires the official `claude` CLI and a working local login.
- It uses `node-pty`, so some environments may need local build tools.
- It is a local workflow tool, not a replacement for a production API integration.

For Anthropic's current Claude Code plan guidance, the official support article is the best reference point: [Using Claude Code with your Pro or Max plan](https://support.anthropic.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan). If you already set API credentials in your shell, double-check which execution path you are actually testing before you compare costs or limits.

![Premium desktop scene showing a local script, a terminal window, and a structured result block](/assets/img/posts/2026-07-21-how-to-automate-the-real-claude-code-terminal-ui-with-dash-p/image-01-922e7147c954.png)

If you want more background on the same tooling, the recent companion posts are worth skimming: [How to Automate Claude Code From a Shell Script](https://how-to-blog.gitlab.io/2026/06/26/how-to-automate-claude-code-from-a-shell-script/), [How to Automate the Claude Code TUI With dash-p](https://productivity-tech-business.blogspot.com/2026/06/how-to-automate-claude-code-tui-with.html), [How I Automate the Real Claude Code TUI With dash-p](https://the-lean-ecommerce.gitlab.io/2026/07/06/how-i-automate-the-real-claude-code-tui-with-dash-p/), and [How to Script Claude Code Locally With dash-p](https://how-to-blog.gitlab.io/2026/06/23/how-to-script-claude-code-locally-with-dash-p/).

## Conclusion

If you already trust Claude Code interactively and just need a scriptable entry point, dash-p keeps the workflow local and understandable. Start with the CLI, move to `query()` when you need structure, and keep the integration small until it proves useful.

Try the package here: [dash-p on GitHub](https://github.com/ybouane/dash-p) and [@ybouane/dash-p on npm](https://www.npmjs.com/package/@ybouane/dash-p).
