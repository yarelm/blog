+++
title = "Tips for Fruitful AI-Assisted Coding"
description = "Practical insights for getting the most out of agentic AI coding tools like Claude Code, Cursor, and Roo Code"
date = 2025-03-18T12:00:00Z
author = "Yarel Maman"
+++

Feeling the AI FOMO yet? 🤖😂 I get it — AI buzz is everywhere, and it often feels like magic when used for demos and prototypes. But, as an engineer who's racked up a few scars from production mishaps, and is involved in LLM app development, I've always approached AI-assisted coding tools with cautious optimism. I've had my doubts, thinking, "We're not there yet."

However, after months of exploring different tools, things changed for me recently. I had an "aha" moment where AI coding assistance went from being a "hit or miss" tool for specific cases to something I now rely on more frequently to help me code. The game-changer? Agentic tools like Claude Code, Cursor, Windsurf, and Cline. Personally, I really like Roo Code, which feels like the hipster choice 😍

So, what's the secret sauce? Developing a healthy intuition for using AI correctly, or else you might end up with bad results and feel discouraged. Here are some insights I've gained that significantly improved my workflow with AI-assist tools:

**Context is Everything:** Specificity is your friend. Treat your AI request like you're briefing a new grad engineer. Include all the details you have in mind: implementation nuances, module structures, edge cases—everything! Avoid vague prompts like "implement X for me" and instead provide clear, detailed instructions. Remember, even if you're a senior engineer, coding without knowing the project context will not result in good quality code. Providing context is crucial for achieving the desired results, whether you're human or AI.

**Show and Tell:** Make sure the AI only sees what's relevant to the task by selecting the right files. Non-relevant data leads to subpar results.

**Project Blueprints:** If you are working on a complex project, write an architecture.md to describe your project in human terms. Remember, every new session is a blank slate for the AI. This blueprint will guide it toward a quality solution without fumbling through your files. Tip: You can even use AI tools to help generate this file! 😊

**Design it Yourself, Code it with AI:** If you provide the agent with a spec, a plan, a design doc, you will discover there is less room for fuckups, and it will do a much better job at implementing your desire. Another added bonus is - You are thinking about the important details (reliability, scalability, security) and defering the "coding" details (please excuse me, coding purists ;D ) to your assistant.

**Collaborative Spec Design:** Engage it in discussions about product requirements, your tech stack, and code structure to craft a detailed action plan. Some tools, like "Roo Code," have an "Architect" mode for this very purpose. Given an action plan, the coding becomes more straightforward.

**Stay Engaged:** Skip the full-autonomous "YOLO" mode. Oversight minimizes mistakes. Review each step, steer the conversation as needed, and keep the AI on track.

**Good Software Design Goes a Long Way:** It turns out that many principles of quality software design that benefit us humans also enhance AI tool performance. Concepts like separation of concerns, modularity, and keeping files and functions concise are key. Building on a strong foundation sets both humans and AI up for success.

**Mind the Context Window:** Long conversations generate lots of tokens. LLMs struggle with large context, so if you are getting near 100k tokens, or feel the agent is getting "lost" - start fresh to maintain quality. Give AI a break.

**Bring Docs to the Party:** For niche tech or lesser-known software packages, LLMs might struggle. Sharing relevant documentation with the AI (some tools can paste or search docs for you) can lead to better results.

**Set the Rules:** Treat the tool like a new developer joining the team and doing an onboarding. Use a .rules file to introduce AI to your coding conventions and team preferences.

**Documenting with Style:** I'm loving agentic tools for creating visual documentation and design docs. My recent workflow: plan a solution → create a quick POC → have the tool generate a design doc and mermaid diagram of the flow (try it, it's incredible) → share and discuss with peers and iterate → finalize or rethink the strategy.

These steps have taken my interaction with AI-assisted tools to a new level, and I hope they can do the same for you. Happy coding! 💻✨
