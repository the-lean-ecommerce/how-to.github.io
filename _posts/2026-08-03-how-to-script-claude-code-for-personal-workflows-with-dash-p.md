---
layout: post
title: "How to Script Claude Code for Personal Workflows With dash-p"
description: "A practical guide to scripting the real Claude Code TUI with dash-p for smoke tests, TypeScript helpers, and personal automation."
date: 2026-08-03 12:00:00 +0000
categories: [how-to]
tags: [dash-p, claude-code, typescript, automation, terminal]
canonical_url: ""
image: "/assets/img/posts/2026-08-03-how-to-script-claude-code-for-personal-workflows-with-dash-p/cover-3b02ea19b3af.png"
---

I wanted a way to script my own Claude Code session without rebuilding the workflow around a different programmatic path. <code>dash-p</code> is the small bridge for that job: it launches the official <code>claude</code> command, drives the rendered TUI, and gives you both a CLI and a <code>query()</code> API. If you have been following the same line from [How to Automate the Real Claude Code Terminal UI With dash-p](https://how-to.the-lean-ecommerce.com/2026/07/21/how-to-automate-the-real-claude-code-terminal-ui-with-dash-p/) through [How to Build a Local Query API for Claude Code With dash-p](https://how-to.the-lean-ecommerce.com/2026/07/28/how-to-build-a-local-query-api-for-claude-code-with-dash-p/), this is the version I would use for personal workflows that should stay local and understandable.

If you want the current policy context, Anthropic's [Use Claude Code with your Pro or Max plan](https://support.anthropic.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan), [Manage usage credits for paid Claude plans](https://support.anthropic.com/en/articles/12429409-manage-usage-credits-for-paid-claude-plans), and [Pricing - Claude Platform Docs](https://docs.anthropic.com/en/docs/about-claude/pricing) pages are the source of truth. The important part for this post is simpler: dash-p keeps you in the real interactive Claude Code session instead of making you rewrite the workflow around a separate API surface.

## 1. Start with the real Claude Code session

Before you install anything, confirm the manual session works.

1. Open a terminal where Claude Code is already installed.
2. Run <code>claude</code> by hand.
3. Send one plain prompt and wait for a normal reply.

Expected result: Claude Code opens normally in the terminal, accepts input, and returns an answer without any wrapper involved.

If that part fails, fix the local Claude setup first. dash-p depends on the same session, so there is no point debugging the wrapper before the base tool works. For the shell-first version of this same idea, [How to Automate the Real Claude Code Terminal UI With dash-p](https://how-to.the-lean-ecommerce.com/2026/07/21/how-to-automate-the-real-claude-code-terminal-ui-with-dash-p/) is the closest companion read.

## 2. Use a smoke test before wiring it into a script

The first command I reach for is still the smallest one.

    npm install -g @ybouane/dash-p
    dash-p "summarize this repo"

If you do not want a global install, the no-install path is fine for a one-off test:

    npx @ybouane/dash-p "what color is the sky"

Expected result: the command opens the real TUI, sends the prompt, and prints the answer back into your shell.

![Dark aurora desk scene for the first dash-p smoke test](/assets/img/posts/2026-08-03-how-to-script-claude-code-for-personal-workflows-with-dash-p/image-01-4fe2e538ee14.png)

The point of the smoke test is not to prove everything works forever. It is to prove that <code>dash-p</code> is seeing the same interactive session you are seeing. That is also why I like keeping the command human-sized until it passes once.

## 3. Move the same prompt into TypeScript

Once the command-line path is healthy, the TypeScript API is the part that makes the tool feel composable.

    import { query } from "@ybouane/dash-p";

    for await (const msg of query({
      prompt: "In one sentence, what is a pseudo-terminal?",
      options: { model: "sonnet", includePartialMessages: true },
    })) {
      if (msg.type === "result") console.log(msg.result);
    }

Expected result: the loop streams messages, then emits the final result when the Claude session finishes.

![TypeScript query flow from dash-p to structured output](/assets/img/posts/2026-08-03-how-to-script-claude-code-for-personal-workflows-with-dash-p/image-02-d867947bf326.png)

If you want the deeper API-shaped version of this, see [How I Built a Local Query Layer for Claude Code With dash-p](https://how-to.the-lean-ecommerce.com/2026/07/31/how-i-built-a-local-query-layer-for-claude-code-with-dash-p/) and [How to Build a Local Query API for Claude Code With dash-p](https://how-to.the-lean-ecommerce.com/2026/07/28/how-to-build-a-local-query-api-for-claude-code-with-dash-p/). They cover the same bridge from two different angles.

## 4. Match the output shape to the next step

dash-p is most useful when the output is immediately useful to another command. A few examples I use are:

- repo summaries before a review
- five-bullet commit summaries
- short handoff notes
- risk lists before merging
- prompts that need a structured answer in a shell script

If the next step is another command, keep the prompt explicit and the result narrow. For example:

    summary=$(dash-p "summarize the staged changes in five bullets")
    echo "$summary" > /tmp/dash-p-summary.txt

Expected result: the next script can consume the text without parsing a long transcript.

This is also why the tool feels closer to a local dependency than to a remote service. The value is not that it is fancy. The value is that the Claude session stays inside the workflow I already use.

## 5. Know when not to use it

<code>dash-p</code> is a good fit for local, personal automation. It is not the right boundary when you need a hosted product, an unattended backend, or a workflow that must stay stable even if the UI changes. Because it drives the real TUI, it inherits the same fragility that makes it useful.

This is the tradeoff I would keep in mind:

- use dash-p when you want your own Claude Code session to be scriptable
- use the official API or Agent SDK when you need a production integration boundary
- use the current Anthropic docs when you need the current billing rules, not a blog post summary

![Decision board comparing a local Claude Code session with a separate headless path](/assets/img/posts/2026-08-03-how-to-script-claude-code-for-personal-workflows-with-dash-p/image-03-6897fc387273.png)

If the billing split is the part you care about most, [How I Script Claude Code From Bash Without Switching to the Agent SDK](https://the-lean-ecommerce.gitlab.io/2026/07/24/how-i-script-claude-code-from-bash-without-switching-to-the-agent-sdk/) and [How to Build Subscription-Aware Claude Code Workflows With dash-p](https://how-to-blog.gitlab.io/2026/08/03/how-to-build-subscription-aware-claude-code-workflows-with-dash-p/) are the two posts I would read next.

## 6. Keep the first integration boring

The safest rollout is the one that proves value before it tries to do too much.

1. Start with a repo summary.
2. Move to a commit summary or handoff note.
3. Only then wire the same command into a repeatable shell script or TypeScript helper.

Expected result: you learn the shape of the tool without depending on it before you trust it.

The practical goal is not to replace Claude Code. It is to keep the interactive session and make it scriptable when you need repeatable local automation. Start with one repo summary, then decide whether the shell path or the TypeScript path fits better.

If you want the tool itself, start with [dash-p on GitHub](https://github.com/ybouane/dash-p) and run the smoke test once before you build anything larger.
