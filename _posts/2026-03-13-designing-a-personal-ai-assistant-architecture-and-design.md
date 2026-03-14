---
layout: post
title: Designing a Personal AI Assistant - Architecture & Design
tag: Personal AI Assistant
description: "A deep dive into Atombot's components: memory, tool calls, LLM integration, the gateway and security."
---

As I shared [Atombot](https://github.com/daegwang/atombot) in this [post](https://daegwang.github.io/2026/03/10/why-i-built-a-personal-ai-assistant/), I wanted a personal AI assistant that I could actually keep running on my own machine. That meant it had to stay understandable, work well with local models. In this post, I'm going to share more technical details of how each component works and the tradeoffs behind each design decision.

<div class="blog-image blog-image-full"><img src="/blog/images/atombot_architecture.png" alt="Atombot architecture overview" /></div>

## The Agent Loop

When you send a message, agent loop will execute following steps in order:

1. Builds a system prompt from prebuilt prompts and long-term memory (including last N conversation as context).
2. Calls the LLM with system prompt and the tool schema.
3. If the LLM returns tool calls, executes them locally and loops back to step 2.
4. When the LLM returns a plain text response (no tool calls) return to user.

I've added limit of tool iterations to prevent infinite loops. Most conversations finish in 3~5 steps. However, coding requests requires more tool calls to manage file and folders.

This agent loop is simple by design, LLM decides what tools to call and when to stop. There is no extra complexity such as planner, queue and orchestration layer. 

<div class="blog-image"><img src="/blog/images/atombot_agent_loop.png" alt="Atombot agent loop" /></div>

## Memory

Memory has two parts: long-term and history.

**Long-term memory** is stored in `memory/MEMORY.md`, a plain Markdown file that the agent can update using the `update_memory` tool. The LLM reads the current memory, merges in new facts, and writes the whole thing back. 

**History** is stored in `memory/history/` as JSONL files. Each line is a JSON object with a timestamp, the user message, and the assistant response. At the start of each turn, `search_history()` scans all history files and injects the most relevant past conversation into the system prompt.

The memory design is intentionally minimal. No embeddings, no vector DB. 

## Tool Calls

Tools are the primary way the agent acts on the world. Atombot exposes tool functions to support following tasks: file I/O, shell execution, web fetching and scheduling reminders.
The LLM never executes code, it emits a structured tool call, the agent runs it, and the result goes back into the message thread. Tool output size is limited to avoid overloading LLM context window.

## LLM Integration

Atombot supports two ways to connect to a LLM model. **Standard mode** works with any OpenAI compatible endpoints (LM Studio or Ollama) out of the box. **Codex mode** runs the Codex CLI directly as a subprocess. Temperature is fixed at 0.2 for standard mode to keep tool calls predictable and consistent.

## Gateway

Atombot uses a bot token to connect to Telegram. While it's working on a request, it continues listening for new messages in the background. A typing indicator lets you know a response is on the way, and any messages that come in during that time are handled right after.
Only approved Telegram user IDs can interact with the bot. This keeps Atombot personal and private by default.
For scheduled tasks, the gateway checks schedule every 20 seconds, runs any due tasks, and send results back to the chat automatically.

## Security

The agent includes several built-in safeguards to protect system and prevent misuse.

- **Workspace sandboxing.** File operations are resolved and checked against the workspace boundary. Path traversal and outside absolute paths are rejected.
- **Command blocklist.** Dangerous system commands (`sudo`, `dd`, `shutdown`, `chmod`, and others) are blocked. General shell use within the workspace is otherwise permitted.
- **URL validation.** Only `http://` and `https://` URLs with a valid network location are accepted. `file://` and other schemes are rejected.
- **Timeouts.** All operations have a timeout to prevent indefinite hangs.
- **Prompt injection mitigation.** Before passing fetched web content to the LLM, HTML tags and scripts are removed and the content is clearly marked as external data. This lowers the chance of malicious pages affecting the bot.

## Onboarding and Setup

Setting up atombot takes about a minute. Run `atombot onboard` and just follow the steps.
It picks up whatever you already have, LM Studio, Ollama, or Codex CLI, and asks you to choose a model. After that you add your Telegram token and allowlist your user ID and you're done.
Your workspace is set up with folders for projects, memory, skills, and tasks. The built-in skills and templates are already in there, edit them anytime you want.

---

If you're building something similar, I hope this post saves you some time. 
If you want to try it out, You can find it and get started at [Github](https://github.com/daegwang/atombot)
