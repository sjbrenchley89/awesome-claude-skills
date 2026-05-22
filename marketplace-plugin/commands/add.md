---
description: Add a skill marketplace or registry source (GitHub repo or @domain)
allowed-tools: [Bash, Read, Write, AskUserQuestion]
---

# Marketplace Add

Register a new skill source so its packages become available via `/marketplace:install`.

## Usage

```
/marketplace:add <source>
```

- `<source>` is either a GitHub path (`owner/repo`) or a registry domain (`@domain`).

## Instructions

### Step 1: Parse the source argument

The source provided by the user is the first argument after the command. Examples:
- `alirezarezvani/claude-skills` → GitHub repo
- `@claude-code-skills` → registry domain

### Step 2: Resolve the source to a URL

- GitHub path: `https://github.com/<owner>/<repo>`
- Registry domain (e.g. `@claude-code-skills`): derive to `https://github.com/mhattingpete/claude-skills-marketplace` unless a mapping exists in `~/.config/claude-code/marketplace-sources.json`

### Step 3: Persist the source mapping

Read `~/.config/claude-code/marketplace-sources.json` (create if missing). Add or update the entry:

```json
{
  "alirezarezvani/claude-skills": "https://github.com/alirezarezvani/claude-skills",
  "@claude-code-skills": "https://github.com/mhattingpete/claude-skills-marketplace"
}
```

Write the updated file back.

### Step 4: Confirm to the user

Tell the user:
```
Marketplace source added: <source>
Resolved to: <url>
You can now install skill bundles from this source.
Run: /marketplace:install <bundle>@<source>
```

## Important
- Do NOT clone the repo at add-time; cloning happens on install.
- Do NOT overwrite existing sources without asking.
- Keep the sources file valid JSON at all times.
