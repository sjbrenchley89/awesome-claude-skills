# CLAUDE.md — Awesome Claude Skills

This file provides context for AI assistants working in this repository.

## What This Repository Is

`awesome-claude-skills` is a curated, community-maintained collection of **Claude Skills** — reusable instruction packages that extend Claude's capabilities across Claude.ai, Claude Code, the Claude API, and other AI agent platforms (Codex, Cursor, Gemini CLI, Windsurf, etc.).

The repo has two layers:
1. **Local skills** — skill folders checked into this repo (e.g. `changelog-generator/`, `brand-guidelines/`).
2. **External skill links** — entries in `README.md` that point to third-party GitHub repos.

The primary artifact is `README.md`, which is the authoritative, categorized index of all skills.

---

## Repository Structure

```
awesome-claude-skills/
├── README.md                  # Master index of all skills by category
├── CONTRIBUTING.md            # Contribution guidelines
├── CLAUDE.md                  # This file
├── template-skill/            # Minimal starter template
│   └── SKILL.md
├── skill-creator/             # Meta-skill for building new skills
│   ├── SKILL.md
│   └── scripts/
│       └── init_skill.py      # Scaffolds a new skill directory
│       └── package_skill.py   # Validates and zips a skill for distribution
├── connect-apps/              # Composio integration skill
│   └── SKILL.md
├── connect-apps-plugin/       # Claude plugin companion
├── <skill-name>/              # One folder per local skill
│   ├── SKILL.md               # Required
│   ├── scripts/               # Optional: executable helpers
│   ├── references/            # Optional: docs loaded into context on demand
│   └── assets/                # Optional: templates, images, fonts used in output
└── .github/
    └── workflows/
        └── label-ready-skill.yml  # PR validation + auto-labeling
```

### Key Directories

| Path | Purpose |
|------|---------|
| `template-skill/` | Copy-paste starting point for new local skills |
| `skill-creator/` | Meta-skill; use it to guide creation of other skills |
| `connect-apps/` | Connects Claude to 1000+ apps via Composio |
| `connect-apps-plugin/` | Claude plugin that installs the Composio tool router |
| `.github/workflows/` | CI: validates PR structure and adds `ready-to-merge` label |

---

## Skill Format

Every skill is a folder containing a `SKILL.md` file with YAML frontmatter.

### Minimal valid skill

```
skill-name/
└── SKILL.md
```

### Full skill layout

```
skill-name/
├── SKILL.md              # Required — frontmatter + instructions
├── scripts/              # Executable code (Python, Bash, etc.)
├── references/           # Docs loaded into context as needed
└── assets/               # Files used in output (templates, images, fonts)
```

### SKILL.md frontmatter

```yaml
---
name: skill-name           # Required; kebab-case, matches folder name
description: >             # Required; one sentence, written in third person.
  This skill should be used when ...
license: Apache-2.0        # Optional; defaults to repo-level Apache 2.0
---
```

`name` and `description` are the **only fields Claude sees at load time** (~100 tokens per skill). The full SKILL.md body loads only when Claude determines the skill is relevant. Bundled resources load further on demand.

### Writing style for SKILL.md body

- Use **imperative/infinitive form**: "To accomplish X, do Y" — not "You should do X".
- Write for another Claude instance, not for end users.
- Keep SKILL.md under ~5,000 tokens. Move large reference material to `references/`.
- Do not duplicate information between SKILL.md and `references/` files.

---

## README.md Conventions

`README.md` is the single source of truth for the skills index. Its structure must stay intact — the CI workflow depends on the `## Skills` and `## Getting Started` heading markers.

### Skill entry format

```markdown
- [Skill Name](./skill-name/) - One-sentence description. *By [@author](https://github.com/author)*
```

or for external skills:

```markdown
- [Skill Name](https://github.com/owner/repo) - One-sentence description. *By [@author](https://github.com/author)*
```

### Rules enforced by CI

1. **Only `README.md` may change** in a listing PR (skill folder PRs are separate).
2. **All edits must stay within the `## Skills` … `## Getting Started` window.** Nothing outside that range may be modified.
3. **New bullet links must use external `https://` URLs.** Links to `composio.dev` or `anthropic.com` are blocked.
4. **No crypto/web3/blockchain/NFT keywords** anywhere in added lines.
5. **Entries must be in alphabetical order** (case-insensitive) within their category. Existing disorder in a category is grandfathered; only the new entry must be placed correctly.

### Categories in README.md

- Document Processing
- Development & Code Tools
- Data & Analysis
- Business & Marketing
- Communication & Writing
- Creative & Media
- Productivity & Organization
- Collaboration & Project Management
- Security & Systems
- App Automation via Composio (78 SaaS apps, grouped by type)

---

## CI / GitHub Actions

**`.github/workflows/label-ready-skill.yml`** runs on every PR targeting `master`.

What it does:
1. Downloads the base and head `README.md`.
2. Lists changed files — fails if any file other than `README.md` was changed.
3. Runs a Node.js validation script that checks all the rules listed above.
4. On success, adds the `ready-to-merge` label to the PR.

This workflow uses `pull_request_target` so it can write labels on forks. It only needs `contents: read` and `pull-requests: write`.

---

## Development Workflows

### Adding a new local skill

1. Scaffold the folder:
   ```bash
   python skill-creator/scripts/init_skill.py my-skill-name --path .
   ```
2. Edit `my-skill-name/SKILL.md` — fill in `name`, `description`, and instructions.
3. Add/remove `scripts/`, `references/`, `assets/` subdirectories as needed.
4. Add an entry to `README.md` in the correct category and alphabetical position.
5. Package for distribution (optional):
   ```bash
   python skill-creator/scripts/package_skill.py ./my-skill-name
   ```
6. Open a PR. The CI validates the README entry automatically.

### Listing an external skill (README-only PR)

1. Fork the repo and create a branch.
2. Edit `README.md` only — add one bullet in the correct category, in alphabetical order.
3. The entry must link to an external `https://` URL.
4. Open a PR. CI validates and auto-labels if all checks pass.

### Branch naming convention

For skill additions: `add-<skill-name>`  
For fixes: `fix-<description>`  
For docs: `docs-<description>`

### Commit message style

```
Add [Skill Name] skill
Fix [Skill Name] description
Update README: add [Skill Name] to [Category]
```

---

## Key Conventions

| Convention | Detail |
|------------|--------|
| Folder names | Lowercase kebab-case (`my-skill-name`) |
| SKILL.md description | Third person, one sentence, ≤200 chars |
| README bullet | No trailing period, no emojis |
| Alphabetical order | Case-insensitive, within each category |
| License | Apache 2.0 (repo-wide); individual skills may override in `LICENSE.txt` |
| External attribution | Add `*By [@author](URL)*` at end of bullet |

---

## What Skills Are NOT

- **Not MCP servers** — MCP defines how Claude connects to external systems. Skills define *what to do* once connected.
- **Not tools** — Tools are individual functions. Skills are multi-step workflows.
- **Not prompts** — Skills are persistent, versioned packages loaded progressively, not one-shot prompt fragments.

---

## Progressive Loading Model

Claude loads skills in three stages:

| Stage | What loads | Size |
|-------|-----------|------|
| Always | `name` + `description` from frontmatter | ~100 tokens/skill |
| On relevance | Full `SKILL.md` body | <5,000 tokens |
| On demand | `scripts/`, `references/`, `assets/` | Unlimited (scripts may run without reading) |

This design lets a single agent host hundreds of skills without bloating its context window.

---

## Platform Support

Skills work across:
- **Claude.ai** — via the skill icon (🧩) in chat
- **Claude Code** — place in `~/.config/claude-code/skills/`; loads automatically
- **Claude API** — pass `skills=["skill-id"]` in `client.messages.create()`
- **Third-party agents** — Codex, Cursor, Gemini CLI, Windsurf, Antigravity

---

## Resources

- [Official skills repo](https://github.com/anthropics/skills) — Anthropic's reference implementations
- [Skills API docs](https://docs.claude.com/en/api/skills-guide)
- [Creating custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Skills marketplace](https://claude.ai/marketplace)
