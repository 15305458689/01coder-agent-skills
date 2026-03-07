# 01coder Agent Skills

A Claude Code plugin marketplace containing skills grouped by category.

## Repo Structure

```
01coder-agent-skills/
├── .claude-plugin/
│   └── marketplace.json       # Marketplace catalog (groups skills into plugins)
├── skills/                    # All skills live here
│   ├── {skill-name}/
│   │   ├── SKILL.md           # Skill definition (frontmatter + instructions)
│   │   ├── references/        # Domain knowledge, patterns (optional)
│   │   ├── scripts/           # Automation scripts (optional)
│   │   ├── assets/            # Templates, resources (optional)
│   │   └── templates/         # Report templates (optional)
│   └── ...
├── CLAUDE.md                  # This file
└── README.md
```

## Plugin & Marketplace Architecture

This repo is a **marketplace** — a catalog of plugins that users add to Claude Code.

### Key Concepts

- **Marketplace** = this repo. Users add it with `/plugin marketplace add sugarforever/01coder-agent-skills`
- **Plugin** = a group of related skills. Defined in `marketplace.json` under `plugins[]`
- **Skill** = a `SKILL.md` file that Claude auto-invokes based on context

### marketplace.json Structure

Skills are grouped into plugins by category. Each plugin uses:
- `"source": "./"` — plugin root is the repo root
- `"skills": [...]` — explicit list of skill directory paths

```json
{
    "plugins": [
        {
            "name": "content-skills",
            "source": "./",
            "skills": [
                "./skills/video-script",
                "./skills/share-reading"
            ]
        }
    ]
}
```

This structure ensures that when a plugin is installed and cached, the `skills/` directory hierarchy is preserved and Claude Code can discover the SKILL.md files.

### Current Plugin Groups

- **security-skills** — nextjs-security-scan, python-security-scan
- **content-skills** — video-script, share-reading, publish-x-article, publish-zsxq-article, publish-substack-article, personal-writing-style, cover-image, diagram-to-image
- **productivity-skills** — subtitle-correction, china-stock-analysis, interactive-input, add-feishu

## Adding a New Skill

1. Create `skills/{skill-name}/SKILL.md` with frontmatter:
   ```yaml
   ---
   name: skill-name
   description: When to trigger this skill. Be specific about trigger phrases.
   ---
   ```

2. Add the skill path to the appropriate plugin group in `.claude-plugin/marketplace.json`:
   ```json
   "skills": [
       "./skills/existing-skill",
       "./skills/new-skill"
   ]
   ```

3. Bump the marketplace `version` in marketplace.json

4. Commit and push. Users get the new skill on their next marketplace update.

## SKILL.md Conventions

### Frontmatter
- `name`: kebab-case identifier
- `description`: Must clearly describe **when** to trigger. Include trigger phrases in both Chinese and English if applicable.

### Content Guidelines
- Follow `personal-writing-style` skill conventions when writing Chinese content
- Use fictional examples in skill files, not real user data
- Include interactive workflows (ask user for input before acting)
- Reference external tools/scripts with relative paths from the skill directory

### Personal Data
- User-specific data (promo links, API keys) should be stored in **auto memory** (`~/.claude/projects/.../memory/`), not in the skill repo
- On first use, skills should ask for personal data and save to memory for reuse
