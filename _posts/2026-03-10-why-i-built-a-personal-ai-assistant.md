---
layout: post
title: Why I Built a Personal AI Assistant
date: 2026-03-10
tag: Personal AI Assistant
description: I wanted a personal AI assistant that felt simpler, lighter, and more private by default.
hero: /blog/images/atombot.png
permalink: /2026/03/10/why-i-built-a-personal-ai-assistant/
---

Most personal AI assistants feel too heavy to actually use.

You open one and there is too much going on. System prompts are huge, token overhead adds up, and there are too many layers between what you ask and what happens. I also did not want my personal data leaving my machine.

I wanted something I could actually run locally, understand, and trust. That is why I built [**Atombot**](https://github.com/daegwang/atombot).

The core is about 500 lines of Python. Small enough that you can open it, read it, and know what it is doing.

Running local models well was important to me. Most frameworks throw a lot at the model before you even say anything. System prompts, tool definitions, extra logic. Local models slow down fast under that. Atombot auto-detects Ollama and LM Studio during setup and keeps the context small so local inference stays fast.

Memory uses plain searchable daily history logs. No vector database, no embeddings, no extra dependencies. Just files the agent looks through to remember what you told it. You can open the logs, read them, and delete them if you want.

The scheduler handles one-time and recurring reminders. A Telegram gateway with an allowlist lets you reach it from outside the terminal.

Features:

- local LLM support with simple onboarding (Ollama, LM Studio)
- ~500 lines of core code, small enough to read and modify
- persistent memory via searchable daily history logs
- one-time and recurring reminders with cron-style scheduling
- Telegram support so you can use it outside the terminal


Use cases:
<div class="blog-gallery">
  <div class="blog-gallery-grid">
    <img src="/blog/images/atombot-chat.png" alt="Atombot chat use case" />
    <img src="/blog/images/atombot-web.png" alt="Atombot web search use case" />
    <img src="/blog/images/atombot-remind.png" alt="Atombot reminder use case" />
  </div>
</div>


- **Personal assistant with long-term memory:** remembers your preferences and past conversations across sessions using plain daily history logs
- **Web explorer:** visits websites, reads the content, and summarizes what it finds
- **Reminders and scheduling:** set one-time or recurring reminders in plain language with cron-style scheduling in the background
- **File and code assistant:** reads, edits, and helps with code directly in your local workspace, nothing leaves your machine

All the use cases came from what I actually needed. I wanted an agent I could trust, run locally, and understand.

[**Atombot** is on GitHub](https://github.com/daegwang/atombot), feel free to try it and I would love any feedback!
