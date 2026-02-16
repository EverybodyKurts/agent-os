# GitHub Copilot Integration Guide

Agent OS provides full support for GitHub Copilot as an AI agent, with commands implemented as GitHub Copilot agent skills.

## What is GitHub Copilot Agent Skills?

GitHub Copilot agent skills are reusable, composable units of functionality that extend GitHub Copilot's capabilities. They allow you to:

- Create custom workflows and procedures
- Leverage sub-agents for parallel task execution
- Build domain-specific AI assistance
- Integrate with your project's standards and conventions

Learn more: [GitHub Copilot Agent Skills Documentation](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)

## Installation

### Option 1: Configure in config.yml

Edit your `config.yml` to include GitHub Copilot:

```yaml
version: 3.0
default_profile: default

ai_agents:
  - claude
  - github-copilot  # Add this line
```

Then run the installation:

```bash
./scripts/project-install.sh
```

### Option 2: Command Line Flag

Install directly with the `--agents` flag:

```bash
# GitHub Copilot only
./scripts/project-install.sh --agents github-copilot

# Both Claude and GitHub Copilot
./scripts/project-install.sh --agents claude,github-copilot
```

## Available Skills

Agent OS provides the following GitHub Copilot agent skills:

### 1. Discover Standards
**Location:** `.github/skills/discover-standards/SKILL.md`

Extract tribal knowledge from your codebase into concise, documented standards. This skill helps identify patterns, conventions, and best practices that should be captured as reusable standards.

### 2. Inject Standards
**Location:** `.github/skills/inject-standards/SKILL.md`

Intelligently inject relevant standards into your current context. Supports:
- Auto-suggestion based on current work
- Explicit standard selection
- Integration with conversations, skills, and plans

### 3. Index Standards
**Location:** `.github/skills/index-standards/SKILL.md`

Rebuild and maintain the standards index file (`index.yml`) for quick discovery and matching.

### 4. Shape Spec
**Location:** `.github/skills/shape-spec/SKILL.md`

Gather context and structure planning for significant work. Works in plan mode to create well-scoped and strategized plans.

### 5. Plan Product
**Location:** `.github/skills/plan-product/SKILL.md`

Establish foundational product documentation through an interactive conversation. Creates mission, roadmap, and tech stack files.

## Using GitHub Copilot Skills

### In GitHub Copilot CLI

Skills are automatically available when you use GitHub Copilot CLI:

```bash
# List available skills
gh copilot skills list

# Use a specific skill
Use the /discover-standards skill to identify patterns in src/api/
```

### In GitHub Copilot Chat

Reference skills directly in your chat conversation:

```
@github Use the /discover-standards skill to identify API patterns in src/api/
```

### In GitHub Copilot CLI

Skills are automatically loaded and can be invoked with the skill name:

```bash
# Skills will be available based on context
# GitHub Copilot will suggest relevant skills automatically
Use /discover-standards to identify patterns in src/api/
```

### In Workflow Files

Skills can be referenced in GitHub Actions or other CI/CD workflows that support GitHub Copilot integration.

### With Sub-Agents

GitHub Copilot skills support sub-agents for parallel execution:

```
Use the discover-standards skill with sub-agents to analyze:
1. API patterns in src/api/
2. Database patterns in src/models/
3. UI patterns in src/components/
```

## Directory Structure

When installed, GitHub Copilot agent skills are placed in:

```
.github/
└── skills/
    ├── discover-standards/
    │   └── SKILL.md
    ├── inject-standards/
    │   └── SKILL.md
    ├── index-standards/
    │   └── SKILL.md
    ├── plan-product/
    │   └── SKILL.md
    └── shape-spec/
        └── SKILL.md
```

Each skill is in its own directory with a `SKILL.md` file that contains:
- YAML frontmatter with `name` and `description`
- Detailed instructions for GitHub Copilot to follow

## Standards Directory

Regardless of which AI agent you use, standards are stored in:

```
agent-os/
└── standards/
    ├── index.yml
    ├── api/
    ├── database/
    ├── global/
    └── [other domains]/
```

## Differences from Claude Code

While the core functionality is the same, there are some differences in how skills are invoked:

| Feature | Claude Code | GitHub Copilot |
|---------|-------------|----------------|
| Command prefix | `/` | `/` (in CLI) |
| Location | `.claude/commands/` | `.github/skills/` |
| File name | `command-name.md` | `SKILL.md` |
| Structure | Single file | Directory with SKILL.md |
| Frontmatter | Not required | YAML with name & description |
| Sub-agents | Claude sub-agents | GitHub Copilot sub-agents |

## Benefits of GitHub Copilot Integration

1. **Native GitHub Integration** — Skills work seamlessly with GitHub features
2. **Sub-Agent Support** — Leverage parallel task execution for complex workflows
3. **Enterprise Features** — Access GitHub Copilot Enterprise capabilities
4. **Version Control** — Skills are versioned with your codebase
5. **Team Collaboration** — Share standards and skills across your team

## Updating Skills

To update your GitHub Copilot skills to the latest version:

```bash
./scripts/project-install.sh --commands-only --agents github-copilot
```

This will update the skills without touching your existing standards.

## Troubleshooting

### Skills not appearing in GitHub Copilot

1. Ensure skills are in `.github/copilot/` directory
2. Check file permissions (should be readable)
3. Verify GitHub Copilot is enabled for your repository
4. Try restarting your IDE

### Standards not being found

1. Run `/index-standards` to rebuild the index
2. Check that `agent-os/standards/index.yml` exists
3. Verify standards files are in the correct subdirectories

## Additional Resources

- [Agent OS Documentation](https://buildermethods.com/agent-os)
- [GitHub Copilot Skills Documentation](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Agent OS Changelog](CHANGELOG.md)

## Support

For questions and support:
- [GitHub Issues](https://github.com/buildermethods/agent-os/issues)
- [Builder Methods Pro Community](https://buildermethods.com/pro)
