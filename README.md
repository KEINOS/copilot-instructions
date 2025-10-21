# KEINOS Copilot Instructions

This repository manages GitHub Copilot custom instructions for Go development across the KEINOS' repositories.

## Purpose

- Standardize Go development best practices organization-wide
- Enforce Test-Driven Development (TDD) methodology
- Promote security-first development culture
- Maintain consistent code quality across all projects

## Repository Structure

- `common-instructions.md` - Organization-wide Go development instructions (main file)
- `copilot-instructions-template.md` - Template for individual projects
- `.github/workflows/markdown-lint.yml` - Markdown linting and UTF-8 validation
- `sync-from-repo-example.yml` - Example sync workflow (with PR creation)
- `sync-workflow-example.yml` - Alternative sync workflow (direct commit)
- `template-readme-example.md` - README template for Go project repositories
- `USAGE_EXAMPLE.md` - Detailed setup and usage examples

## Quick Start

### 1. New Go Project Setup

Add the sync workflow to your Go project:

```bash
# 1. Copy the sync workflow
curl -o .github/workflows/sync-copilot-instructions.yml \
  https://raw.githubusercontent.com/KEINOS/copilot-instructions/main/sync-from-repo-example.yml

# 2. Run initial sync manually
# Go to GitHub Actions tab and run "Sync Copilot Instructions"
```

### 2. Existing Project Integration

```bash
# Run from project root
curl -o .github/workflows/sync-copilot-instructions.yml \
  https://raw.githubusercontent.com/KEINOS/copilot-instructions/main/sync-from-repo-example.yml

# Execute initial sync via GitHub Actions
```

### 3. Automatic Sync Configuration

- **Automatic execution**: Daily at 00:00 UTC (09:00 JST)
- **Manual execution**: Run from GitHub Actions tab anytime
- **PR creation**: Automatically creates PR when changes are detected

> **Note**: For detailed setup instructions, see [USAGE_EXAMPLE.md](USAGE_EXAMPLE.md)

## Security Features

### GitHub Copilot Code Review

This repository uses GitHub Copilot's automatic code review feature to analyze all pull requests. Copilot provides:

- **Context-aware security analysis**: Intelligent detection of security issues with minimal false positives
- **Code quality review**: Best practices and maintainability suggestions
- **Documentation review**: Clarity, consistency, and completeness checks
- **Automated feedback**: Inline comments on potential issues

To enable Copilot Code Review for your pull requests, configure a branch ruleset in repository settings
(Settings > Rules > Rulesets) with "Automatically request Copilot Code Review" enabled.

### Markdown Quality Assurance

All Markdown files are automatically validated for:

- **Linting**: Adherence to markdownlint rules
- **Encoding**: UTF-8 without BOM (Byte Order Mark)
- **Format consistency**: Proper headers, lists, and code blocks

## Update Process

### 1. Updating Common Instructions

```bash
# 1. Create a branch
git checkout -b update-instructions

# 2. Edit common-instructions.md
vim common-instructions.md

# 3. Create PR
git add common-instructions.md
git commit -m "update: improve TDD workflow instructions"
git push origin update-instructions

# 4. Create PR on GitHub
# → GitHub Copilot will automatically review the PR
```

### 2. Propagation to Projects

Once updates are merged, each project will:

1. **Automatic sync**: PR automatically created on next daily run
2. **Manual sync**: Run workflow manually as needed
3. **Review**: Review PR content and merge

## Usage Monitoring

### Project Sync Status

Sync information is recorded at the top of each project's `.github/copilot-instructions.md`:

```markdown
<!-- Last synced: 2024-01-15T00:00:00Z -->
<!-- Source commit: abc1234 (2024-01-14T15:30:00Z) -->
```

### Check Which Repositories Use These Instructions

```bash
# Search for repositories that reference this instruction set
gh search repos "KEINOS copilot-instructions" --owner KEINOS

# Or check network insights on GitHub
# Navigate to: Insights > Network > Dependents
```

## Customization

### Project-Specific Instructions

Add project-specific requirements in the "Project-Specific Instructions" section of each project's `.github/copilot-instructions.md`:

```markdown
## Project-Specific Instructions

### API Server Specific
- REST API design follows OpenAPI 3.0 specification
- All endpoints must include rate limiting
- Authentication uses JWT tokens

### Database Integration
- Use GORM for ORM operations
- Migration files in db/migrations/
- Connection pooling required for production
```

### Sync Configuration Customization

The following can be adjusted in `sync-copilot-instructions.yml`:

- **Execution frequency**: Modify `cron` settings
- **Branch name**: Change PR creation branch name
- **Reviewers**: Configure automatic reviewer assignment

## Contributing

### Bug Reports & Feature Requests

Please report them via [Issues](https://github.com/KEINOS/copilot-instructions/issues).

### Improvement Proposals

1. Fork this repository
2. Create an improvement branch
3. Implement changes
4. Create PR (automated security review will be executed)

### Contribution Guidelines

- Follow TDD principles in changes
- Focus on security-first content
- Use English for comments and documentation
- Provide clear and practical instructions

## License

MIT License - See [LICENSE](LICENSE) for details

## Support

- **Issues**: Bug reports and feature requests
- **Discussions**: Usage questions and improvement suggestions
- **Security**: Report security-related issues privately

## Related Resources

- **[Common Instructions](common-instructions.md)**: Complete Go development guidelines
- **[Usage Examples](USAGE_EXAMPLE.md)**: Detailed setup and troubleshooting guide
- **[Template](copilot-instructions-template.md)**: Project-specific instruction template

---

- **Last Updated**: ![GitHub last commit](https://img.shields.io/github/last-commit/KEINOS/copilot-instructions)
- **Maintainer**: KEINOS Organization
