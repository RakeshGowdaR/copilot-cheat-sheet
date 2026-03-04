<img width="494" height="334" alt="image" src="https://github.com/user-attachments/assets/5e059574-4062-4ed3-bf3c-87112976c2fe" /># GitHub Copilot Complete Cheat Sheet

A practical **developer-focused cheat sheet for GitHub Copilot** covering everyday workflows, agent usage, prompt patterns, and IDE integrations.

Supports:

* VS Code
* Android Studio
* JetBrains IDEs
* Copilot CLI
* Coding Agents
* Prompt Engineering

---

## Preview
<img width="494" height="334" alt="preview_cheat_sheet" src="https://github.com/user-attachments/assets/67095247-f5a2-4d44-b0ed-33334ed2a9b5" />
<img width="494" height="334" alt="preview_cheat_sheet" src="https://github.com/user-attachments/assets/67095247-f5a2-4d44-b0ed-33334ed2a9b5" />



---

## Download

Download the full cheat sheet:

**PDF version**

```
pdf/github-copilot-cheatsheet.pdf
```

---

## What's Inside

This cheat sheet covers the most useful Copilot capabilities developers actually use.

### Quick Reference

* Keyboard shortcuts
* Slash commands
* Chat participants
* Context variables

### Real Developer Workflow

Learn the real process developers use with Copilot:

```
/init → Plan → Agent → Review #changes → Create PR
```

### Repository Setup

Configure Copilot for better results:

```
.github/copilot-instructions.md
.github/prompts/
.github/skills/
.github/agents/
AGENTS.md
```

### Copilot Features

* Chat modes (Ask, Edit, Agent, Plan)
* Agent sessions (Local, Background, Cloud)
* Code review actions
* Test generation
* MCP servers
* CLI agent workflows

### Prompt Patterns

Ready-to-use prompts for:

* Refactoring code
* Generating tests
* Reviewing changes
* Debugging issues
* Migrating code
* Building APIs
* Generating UI

Example:

```
@workspace refactor #file:user_service.ts
to follow repository patterns and improve error handling
```

---

## Example Prompts

### Generate Tests

```
/tests for #selection
using JUnit5 and Mockito
cover edge cases and error paths
```

### Review Changes

```
@workspace review #changes for:
- security vulnerabilities
- performance issues
- naming conventions
- missing error handling
```

### Debug an Issue

```
@workspace the API returns 500 when creating a user
with special characters in the name field.

#file:user_controller.ts
#file:user_model.ts
```

---

## Repository Structure

```
copilot-cheat-sheet
│
├─ README.md
├─ pdf/
│   └─ github-copilot-cheatsheet.pdf
│
├─ markdown/
│   └─ copilot-cheatsheet.md
│
├─ images/
│   └─ preview.png
│
└─ examples/
    ├─ prompt-patterns.md
    └─ repo-setup.md
```

---

## Why This Exists

Most Copilot guides explain **features**.

This cheat sheet focuses on:

* real developer workflows
* prompt engineering
* repository configuration
* agent-driven development

Everything in one place.

---

## Contributing

Pull requests are welcome.

You can contribute:

* new prompt patterns
* IDE tips
* Copilot workflows
* CLI examples

---

## License

MIT License

---

## Star the Project

If this cheat sheet helped you, consider giving it a star.

It helps other developers discover the resource.
