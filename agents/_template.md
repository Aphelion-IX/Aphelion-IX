---
name: template-agent
description: Replace with a concrete description of what this subagent does and — critically — when Claude should delegate to it. This is what Claude sees when deciding whether to invoke the agent.
tools: Read, Grep, Glob
model: sonnet
---

You are a [role]. Replace this with the system prompt the subagent runs under:
what it should do when invoked, how it should investigate, and what its final
report should contain.
