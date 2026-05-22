# Marketplace Plugin

Install Claude skill bundles from GitHub registries and domain sources with a single command.

## Install

```bash
claude --plugin-dir ./marketplace-plugin
```

## Commands

### Add a marketplace source

```
/marketplace:add alirezarezvani/claude-skills
/marketplace:add @claude-code-skills
```

Registers the source. Subsequent installs will resolve against it.

### Install skill bundles

```bash
# Full domain bundles
/marketplace:install engineering-skills@claude-code-skills          # 24 core engineering
/marketplace:install engineering-advanced-skills@claude-code-skills  # 25 POWERFUL-tier
/marketplace:install product-skills@claude-code-skills               # 12 product skills
/marketplace:install marketing-skills@claude-code-skills             # 43 marketing skills
/marketplace:install ra-qm-skills@claude-code-skills                 # 12 regulatory/quality
/marketplace:install pm-skills@claude-code-skills                    # 6 project management
/marketplace:install c-level-skills@claude-code-skills               # 28 C-level advisory
/marketplace:install business-growth-skills@claude-code-skills       # 4 business & growth
/marketplace:install finance-skills@claude-code-skills               # 2 finance

# Individual skills
/marketplace:install skill-security-auditor@claude-code-skills
/marketplace:install playwright-pro@claude-code-skills
/marketplace:install self-improving-agent@claude-code-skills
/marketplace:install content-creator@claude-code-skills
```

Skills are installed to `~/.config/claude-code/skills/` and activate automatically the next time you start Claude Code.

## Supported Sources

| Source | GitHub |
|--------|--------|
| `@claude-code-skills` | [mhattingpete/claude-skills-marketplace](https://github.com/mhattingpete/claude-skills-marketplace) |
| `alirezarezvani/claude-skills` | [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) |

Add your own registry with `/marketplace:add <owner/repo>`.
