# The Complete GitHub Copilot Cheat Sheet: Everything You Can Do in Your IDE (2026)

**VS Code · Android Studio · JetBrains · CLI · Agents · Prompt Patterns**

---

I spent the last few weeks going deep into every GitHub Copilot feature across VS Code, Android Studio, and the CLI. Not the marketing stuff — the actual commands, shortcuts, agent workflows, and configuration files that make a real difference in daily development.

The result is a 14-page cheat sheet that covers everything from basic keyboard shortcuts to multi-agent parallel workflows. I had it reviewed by multiple AI models for accuracy and it scored 9.9/10.

This article walks you through the highlights. The full downloadable PDF is linked at the end.

---

## Why Another Copilot Guide?

Most Copilot guides online cover the basics: Tab to accept, Esc to dismiss, maybe a few slash commands. But Copilot in 2026 is a completely different tool than what shipped in 2022. It now has:

- **Four chat modes** (Ask, Edit, Agent, Plan)
- **Three agent session types** (Local, Background, Cloud)
- **Third-party agents** (Claude, Codex) running alongside Copilot
- **Custom agents, skills, and prompt files** for team-wide consistency
- **MCP servers** for external tool integration
- **A full CLI** with autonomous execution and cross-session memory

If you're still just using Tab to accept ghost text, you're using maybe 10% of what's available.

---

## The Super Quick Reference

Before diving deep, here's what you'll use every day.

### Keyboard Shortcuts (VS Code)

| Shortcut | Action |
|----------|--------|
| `Ctrl+I` | Inline Chat (edit code in place) |
| `Ctrl+Alt+I` | Open Chat View |
| `Ctrl+Shift+Alt+I` | Switch to Agent mode |
| `Tab` | Accept suggestion |
| `Esc` | Dismiss suggestion |
| `Alt+]` / `Alt+[` | Next / Previous suggestion |
| `Alt+\` | Trigger suggestion manually |

### The Slash Commands You'll Actually Use

| Command | What It Does |
|---------|-------------|
| `/explain` | Explain selected code in plain language |
| `/tests` | Generate unit tests |
| `/fix` | Fix bugs in selected code |
| `/doc` | Generate documentation |
| `/init` | Create project-level Copilot instructions |
| `/setupTests` | Configure your test framework |
| `/fixTestFailure` | Analyze and fix failing tests |
| `/delegate` | Hand off a task to a cloud agent |

### Context Variables That Change Everything

Type `#` in chat to attach specific context:

| Variable | What It Adds |
|----------|-------------|
| `#file:name` | A specific file |
| `#selection` | Currently selected code |
| `#changes` | Your uncommitted git diff |
| `#testFailure` | Failing test information |
| `#codebase` | Your entire project |
| `#sym:functionName` | A specific symbol |

**Pro tip:** Stack these together for precision. `@workspace refactor #file:auth_service.ts using #selection` gives Copilot the exact context it needs.

---

## The Four Chat Modes (Know When to Use Each)

This is where most developers get confused. Copilot now has four distinct modes, and picking the right one makes a huge difference.

**Ask mode** is read-only Q&A. Use it when you want to understand code without changing anything. Great for onboarding onto a new codebase or exploring options before committing to an approach.

**Edit mode** gives you controlled multi-file edits. You choose which files Copilot can touch, and you review each change. Use it for targeted refactors where you want full control.

**Agent mode** is the autonomous powerhouse. It plans, edits files, runs terminal commands, invokes tools, and self-corrects when things break. Use it for building complete features, scaffolding projects, and complex refactoring.

**Plan mode** creates a structured implementation plan before any code is written. Review and approve the steps first, then let Agent mode execute. Use it when the task is complex enough that you want to validate the approach before Copilot starts writing.

---

## Agent Sessions: Local, Background, and Cloud

This is the feature that most developers haven't explored yet, and it's a game-changer for productivity.

### Local Agents

Run in your VS Code instance. Interactive, immediate feedback. Your open files, terminal, and extensions are all available. This is what most people think of when they hear "agent mode."

### Background Agents

Run via Copilot CLI in an isolated Git worktree on your local machine. They work autonomously without blocking your editor. You can run multiple background agents simultaneously — one writing tests, another refactoring, a third updating docs — all in parallel.

### Cloud Agents

Run on GitHub's infrastructure. They create branches and PRs automatically. Perfect for well-defined tasks where you want team collaboration through pull requests and CI/CD integration.

### The Hand-off Flow

The real power is moving between them:

1. Start with **Plan** (local) to clarify requirements
2. Hand off to **Background** for parallel proof-of-concept work
3. Use `/delegate` to send to **Cloud** for PR-based delivery

You can also assign GitHub issues directly to `@copilot` as the assignee, and the coding agent picks it up, creates a branch, implements the solution, and opens a PR — all without you touching the IDE.

---

## The Golden Workflow: Build a Feature End-to-End

Here's how an experienced developer uses Copilot to build a complete feature:

### Step 1: Initialize (`/init`)

Run `/init` in chat. Copilot analyzes your project structure, coding patterns, and generates `.github/copilot-instructions.md`. This file is auto-applied to every chat request from now on.

### Step 2: Plan

Switch to Plan mode. Describe the feature:

```
[Plan mode]
Implement user authentication with OAuth2.
Requirements:
- Google and GitHub providers
- JWT token refresh
- Role-based access control
```

Review the plan. Ask follow-up questions. Don't approve until you're satisfied with the approach.

### Step 3: Agent Implements

Approve the plan. Agent mode creates files, installs dependencies, writes the code across multiple files automatically.

### Step 4: Tests Run Automatically

Agent runs your test suite. If tests fail, it reads the output, fixes the code, and re-runs — an autonomous test-fix loop. You only intervene if it gets stuck.

### Step 5: Review Changes

Ask Copilot to review its own work:

```
Review #changes for security vulnerabilities,
performance issues, and naming conventions
```

### Step 6: Ship It

Generate a commit message (sparkle icon in Source Control), or delegate to a cloud agent:

```
/delegate Create a PR with the authentication feature.
Include tests and documentation.
```

Cloud agent creates the branch, writes the PR description, and assigns you for review.

---

## Configure Your Repository (This Improves Everything)

Most developers skip this step and wonder why Copilot gives generic suggestions. These files dramatically improve output quality:

```
your-project/
  .github/
    copilot-instructions.md    # Project-wide conventions (auto-applied)
    prompts/
      generate-api-endpoint.md  # Reusable prompt (invoked with /)
      generate-tests.md         # Team-shared test prompt
    skills/
      deploy/
        SKILL.md                # Teaches Copilot your deploy process
    agents/
      security-reviewer.md      # Custom chat agent
  AGENTS.md                      # Universal instructions for all AI agents
```

### Example: copilot-instructions.md

```markdown
# Project Coding Standards
## Language: Use TypeScript strict mode. Prefer async/await over callbacks.
## Naming: camelCase for variables, PascalCase for components, UPPER_SNAKE for constants.
## Testing: Write unit tests (Jest) for all new functions.
## Errors: try/catch for async ops. Log with context. Return typed error responses.
```

### Example: Prompt File (.github/prompts/generate-api-endpoint.md)

```markdown
---
description: "Generate a REST API endpoint following project patterns"
agent: "agent"
tools: ["search/codebase", "editFiles"]
---
# API Endpoint Generator
Generate a new REST endpoint that follows existing route patterns,
includes Zod validation, has proper error handling, includes tests,
and updates the route index.
```

Your team invokes this with `/generate-api-endpoint` in chat. Everyone gets consistent, convention-aware output.

---

## Android Studio / JetBrains Specifics

If you're an Android developer, here's what's different from VS Code:

- **Inline Chat** is `Ctrl+Shift+I` (not `Ctrl+I`)
- **Agent mode with MCP** is available in public preview — enable via Settings > GitHub Copilot > Chat > Agent
- **Agent Skills** are supported (preview) — same `.github/skills/` folders
- **Chat modes** include Ask, Edit, and Agent — selectable via dropdown at bottom of chat
- **Custom Chat Agents** work — define them in `.github/agents/` for specialized roles like accessibility reviewer or security auditor
- **Prompt file frontmatter** only supports `description` in JetBrains (the `tools`, `model`, and `agent` fields are VS Code-only)
- **Coding Agent Jobs** sidebar shows cloud agent sessions, lets you track PR status

One powerful pattern for Android: create a custom prompt file that generates Jetpack Compose `@Preview` functions with sample data and edge-case states. Run it in agent mode with a composable file as context.

---

## Ready-to-Use Prompt Patterns

These are copy-paste ready. The `[Mode]` tag tells you which chat mode to use.

### Refactor Code [Agent]
```
@workspace refactor #file:user_service.ts
to follow repository patterns and improve error handling
```

### Generate Tests [Ask/Agent]
```
/tests for #selection
using JUnit5 and Mockito
cover edge cases and error paths
```

### Code Review [Ask]
```
@workspace review #changes for:
- security vulnerabilities
- performance issues
- naming conventions
- missing error handling
```

### Debug Issue [Ask/Agent]
```
@workspace the API returns 500 when creating a user
with special characters in the name field.
#file:user_controller.ts #file:user_model.ts
Identify the root cause and suggest a fix.
```

### Plan a Feature [Plan]
```
Implement user authentication with OAuth2.
Requirements:
- Google and GitHub providers
- JWT token refresh
- Role-based access control
List all files to create/modify before writing code.
```

### Flutter Widget [Agent]
```
Generate a Flutter widget for #file:profile_screen.dart
with loading, error, and empty states
following Material 3 guidelines
use Riverpod for state management
```

The full cheat sheet has 13 patterns covering refactoring, testing, API generation, debugging, migration, documentation, and more.

---

## Copilot CLI: The Terminal Agent

If you haven't tried Copilot CLI yet, it's worth the setup. It's now GA for all paid subscribers and works on macOS, Linux, and Windows.

Key features:

- **Plan mode** (Shift+Tab): Copilot analyzes your request and builds a plan before coding
- **Autopilot mode**: Fully autonomous execution without stopping for approval
- **Background delegation**: Prefix any prompt with `&` to delegate to the cloud
- **Cross-session memory**: Remembers your project conventions across sessions
- **Built-in agents**: Explore (codebase analysis), Task (builds/tests), Code Review, and Plan

Type `copilot` in any VS Code terminal to start, or install standalone via npm, Homebrew, or WinGet.

---

## Feature Availability Notes

Not everything is available on every plan:

- **Cloud Coding Agents**: Copilot Pro, Pro+, Business, Enterprise
- **Background Agents**: Available via CLI; JetBrains support is in public preview
- **Third-Party Agents (Claude, Codex)**: Cloud agents require Pro+ or Enterprise
- **Agent Skills**: VS Code (Dec 2025+), JetBrains (preview)
- **Free Plan**: 2,000 completions + 50 chat/agent requests per month

Pricing changes frequently — verify current plans at [github.com/features/copilot](https://github.com/features/copilot).

---

## Download the Full Cheat Sheet

The complete 14-page PDF includes:

1. Super Quick Reference (one-page glance card)
2. Real Developer Workflow (end-to-end feature build)
3. Repository Setup Guide (with copy-paste examples)
4. All keyboard shortcuts (VS Code + Android Studio)
5. Every slash command, chat participant, and context variable
6. Four chat modes explained with use cases
7. Agent sessions (Local / Background / Cloud)
8. Third-party and custom agents
9. Code review and smart actions
10. Testing workflows
11. Complete customization guide
12. MCP servers and extensibility
13. Copilot CLI reference
14. 13 ready-to-use prompt patterns
15. Advanced workflows
16. Feature availability matrix
17. Plans and pricing

**[Download the PDF here]** *(add your link)*

**[GitHub Repository]** *(add your repo link)*

---

## Final Thoughts

GitHub Copilot in 2026 is not the same tool it was when it launched. It's evolved from a code completion engine into a full development platform with autonomous agents, team collaboration through PRs, and deep customization.

The developers getting the most value aren't just using Tab to accept suggestions. They're setting up custom instructions, creating reusable prompt files, running parallel background agents, and delegating routine work to cloud agents while they focus on architecture and design.

Start with `/init` to configure your project. Try Agent mode on your next feature. Set up one prompt file your team can share. These small steps compound quickly.

---

*If this was helpful, follow me for more developer productivity content. And if you spot anything that needs updating, drop a comment — Copilot evolves fast.*

*Tags: #GitHubCopilot #VSCode #AndroidStudio #AI #DeveloperTools #Productivity #CopilotAgents*
