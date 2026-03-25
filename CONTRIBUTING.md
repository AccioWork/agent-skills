# Contributing to Accio Agent Skills

Thanks for your interest in contributing! Here's how you can help.

## Ways to Contribute

### Report Issues

- **Skill not working?** Open an issue with the skill name, what you tried, and the error you got
- **Wrong tariff data?** Include the product name, origin/destination, and the incorrect result
- **API errors?** Include the full request and response if possible

### Suggest Improvements

- Better prompts or instructions in SKILL.md
- Additional use cases or example queries
- Better error handling for edge cases
- Translations or localization

### Add New Skills

We're building a complete ecommerce toolkit. If you have an idea for a new skill:

1. Open an issue describing the skill and its use cases
2. If approved, fork the repo and create a new directory
3. Follow the skill format below
4. Submit a PR

## Skill Format

Each skill is a directory containing at minimum a `SKILL.md` file:

```
skill-name/
└── SKILL.md
```

### SKILL.md Structure

```markdown
---
name: skill-slug-name
description: |
  Brief description for search engines and skill marketplaces.
  Include key use cases.
metadata:
  openclaw:
    author: acciowork
    homepage: https://accio.com
    emoji: "🔍"
---

# Skill Title

One-paragraph summary.

## Quick Demo
(Show a user query → agent response example)

## Capabilities
(Table of what the skill can do)

## How It Works
(API details, request/response format)

## Agent Instructions
(Step-by-step guide for the AI agent)

## Error Handling
(Common errors and how to handle them)

## Related Accio Skills
(Cross-links to other skills in the suite)
```

## Development Setup

No build step needed — skills are pure Markdown files.

```bash
# Clone
git clone https://github.com/acciowork/agent-skills.git
cd agent-skills

# Test a skill by installing it locally
clawhub install ./skill-name
```

## Code of Conduct

Be respectful, constructive, and helpful. We're all here to build useful tools.

## License

By contributing, you agree that your contributions will be licensed under [MIT-0](./LICENSE).
