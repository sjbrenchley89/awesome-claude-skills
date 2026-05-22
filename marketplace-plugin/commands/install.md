---
description: Install a skill bundle from a registered marketplace source
allowed-tools: [Bash, Read, Write, AskUserQuestion]
---

# Marketplace Install

Install a named skill bundle from a registered source into `~/.config/claude-code/skills/`.

## Usage

```
/marketplace:install <bundle>@<source>
/marketplace:install <bundle>@<source>  # with explicit source
```

Examples:
```
/marketplace:install engineering-skills@claude-code-skills
/marketplace:install engineering-advanced-skills@claude-code-skills
/marketplace:install product-skills@claude-code-skills
/marketplace:install marketing-skills@claude-code-skills
/marketplace:install ra-qm-skills@claude-code-skills
/marketplace:install pm-skills@claude-code-skills
/marketplace:install c-level-skills@claude-code-skills
/marketplace:install business-growth-skills@claude-code-skills
/marketplace:install finance-skills@claude-code-skills
/marketplace:install skill-security-auditor@claude-code-skills
/marketplace:install playwright-pro@claude-code-skills
/marketplace:install self-improving-agent@claude-code-skills
/marketplace:install content-creator@claude-code-skills
```

## Known Bundle Sizes

| Bundle | Skills |
|--------|--------|
| engineering-skills | 24 core engineering |
| engineering-advanced-skills | 25 POWERFUL-tier |
| product-skills | 12 product |
| marketing-skills | 43 marketing |
| ra-qm-skills | 12 regulatory/quality management |
| pm-skills | 6 project management |
| c-level-skills | 28 C-level advisory (full C-suite) |
| business-growth-skills | 4 business & growth |
| finance-skills | 2 finance (analyst + SaaS metrics) |

## Instructions

### Step 1: Parse `<bundle>` and `<source>`

Split on `@`. If no `@`, ask the user which source to install from.

### Step 2: Resolve source URL

Read `~/.config/claude-code/marketplace-sources.json`. Look up the source key. If not found, try the built-in defaults:
- `@claude-code-skills` → `https://github.com/mhattingpete/claude-skills-marketplace`
- `alirezarezvani/claude-skills` → `https://github.com/alirezarezvani/claude-skills`

If still not found, tell the user to run `/marketplace:add <source>` first.

### Step 3: Install the bundle

```bash
SKILLS_DIR=~/.config/claude-code/skills
mkdir -p "$SKILLS_DIR"

# Try sparse checkout for the specific bundle path
cd /tmp
rm -rf _marketplace_install_tmp
git clone --depth 1 --filter=blob:none --sparse \
  <resolved_url> _marketplace_install_tmp
cd _marketplace_install_tmp
git sparse-checkout set <bundle_path>
cp -r <bundle_path>/* "$SKILLS_DIR/"
cd /tmp && rm -rf _marketplace_install_tmp
```

The bundle path within the repo depends on the source layout. For `@claude-code-skills`:
- The skills live at `skills/<bundle-name>/` within the repo.

### Step 4: Verify and confirm

Check that at least one `SKILL.md` was copied. Then tell the user:
```
Installed <bundle> from <source>.
Skills are now in ~/.config/claude-code/skills/.
Restart Claude Code to activate them: exit and run `claude` again.
```

## Important
- Always install into `~/.config/claude-code/skills/` — do NOT install to the current project.
- If a skill already exists, ask the user before overwriting.
- After install, print a list of the installed skill names.
