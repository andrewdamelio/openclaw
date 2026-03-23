---
title: "Creating Skills"
summary: "Build and test custom workspace skills with SKILL.md"
read_when:
  - You are creating a new custom skill in your workspace
  - You need a quick starter workflow for SKILL.md-based skills
---

# Creating Custom Skills 🛠

OpenClaw is designed to be easily extensible. "Skills" are the primary way to add new capabilities to your assistant.

## What is a Skill?

A skill is a directory containing a `SKILL.md` file (which provides instructions and tool definitions to the LLM) and optionally some scripts or resources.

## Step-by-Step: Your First Skill

### 1. Create the Directory

Skills live in your workspace, usually `~/.openclaw/workspace/skills/`. Create a new folder for your skill:

```bash
mkdir -p ~/.openclaw/workspace/skills/hello-world
```

### 2. Define the `SKILL.md`

Create a `SKILL.md` file in that directory. This file uses YAML frontmatter for metadata and Markdown for instructions.

```markdown
---
name: hello_world
description: A simple skill that says hello.
---

# Hello World Skill

When the user asks for a greeting, use the `echo` tool to say "Hello from your custom skill!".
```

### 3. Add Tools (Optional)

You can define custom tools in the frontmatter or instruct the agent to use existing system tools (like `bash` or `browser`).

### 4. Refresh OpenClaw

Ask your agent to "refresh skills" or restart the gateway. OpenClaw will discover the new directory and index the `SKILL.md`.

## Best Practices

- **Be Concise**: Instruct the model on _what_ to do, not how to be an AI.
- **Safety First**: If your skill uses `bash`, ensure the prompts don't allow arbitrary command injection from untrusted user input.
- **Test Locally**: Use `openclaw agent --message "use my new skill"` to test.

## Shared Skills

You can also browse and contribute skills to [ClawHub](https://clawhub.com).

## Skill Metadata Reference

The YAML frontmatter supports these fields:

| Field                                   | Required | Description                                                                                              |
| --------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------- |
| `name`                                  | Yes      | Unique identifier (snake_case)                                                                           |
| `description`                           | Yes      | One-line description shown to the agent; drives automatic skill selection                                |
| `read_when`                             | No       | List of conditions under which the agent should read this skill (shown in system prompt as a hint)       |
| `user-invocable`                        | No       | Whether the skill can be triggered by the user via slash-command. Defaults to `true`                     |
| `disable-model-invocation`              | No       | When `true`, the model cannot auto-invoke this skill; only explicit user commands can trigger it         |
| `metadata.openclaw.os`                  | No       | OS filter (`["darwin"]`, `["linux"]`, etc.)                                                              |
| `metadata.openclaw.always`              | No       | When `true`, the skill is always injected into the system prompt regardless of relevance scoring         |
| `metadata.openclaw.emoji`               | No       | Emoji shown next to the skill name in listings (e.g. `🔍`)                                              |
| `metadata.openclaw.skillKey`            | No       | Override the skill's lookup key (useful when skill `name` differs from the folder name)                  |
| `metadata.openclaw.primaryEnv`          | No       | Primary environment variable this skill depends on (used for `apiKey` wiring in `skills.entries`)        |
| `metadata.openclaw.requires.bins`       | No       | Required binaries on PATH — skill is hidden if any are missing                                           |
| `metadata.openclaw.requires.anyBins`    | No       | At least one binary from this list must be on PATH                                                       |
| `metadata.openclaw.requires.env`        | No       | Required environment variables — skill is hidden if any are unset                                        |
| `metadata.openclaw.requires.config`     | No       | Required config keys — skill is hidden if any are absent                                                 |
