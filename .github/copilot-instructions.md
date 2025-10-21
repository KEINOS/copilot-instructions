# Copilot Instructions for copilot-instructions Repository

## Repository Purpose

This repository manages and distributes GitHub Copilot custom instructions for Go development across the
organization ([KEINOS' repositories](https://github.com/KEINOS/)).

**IMPORTANT**: The files `common-instructions.md` and `copilot-instructions-template.md` in the repository root are
**templates for other projects**, NOT instructions for developing this repository itself.

## Project Context

- **Project Name**: copilot-instructions
- **Type**: Template/Documentation Repository
- **Purpose**: Centralized management of organization-wide AI coding standards
- **Main Output**: Markdown documentation and GitHub Actions workflows

## Project-Specific Requirements

### Content Development

When working on this repository, AI agents should:

1. **Documentation Quality**
   - Write clear, concise instructions in English
   - Use professional tone (no emoji in code/documentation guidelines)
   - Ensure examples are practical and reproducible
   - Maintain consistency across all template files

2. **Markdown Standards**
   - Follow strict Markdown linting rules (see `.markdownlint.json` if present)
   - All Markdown files must be UTF-8 without BOM (Byte Order Mark)
   - Must pass `markdownlint` default configuration without errors
   - Add blank lines after headers for readability
   - Surround lists with blank lines (MD032)
   - Use proper code block fencing with language specification
   - Target 80 characters line length

3. **Template Integrity**
   - Never modify `common-instructions.md` unless explicitly requested
   - Preserve SYNC markers in template files (`<!-- SYNC_START -->` / `<!-- SYNC_END -->`)
   - Maintain backward compatibility when updating templates
   - Version control placeholders properly (e.g., `[PROJECT_NAME]`, `[DATE]`)

4. **Security Review**
   - All changes to instruction files trigger automated security review
   - Avoid patterns that could be interpreted as malicious (see `.github/workflows/security-review.yml`)
   - No executable code examples that modify system state destructively
   - Document security best practices clearly

### File Organization

- **Templates** (root directory):
  - `common-instructions.md` - Core Go development guidelines
  - `copilot-instructions-template.md` - Project-specific template
  - `sync-from-repo-example.yml` - Workflow for syncing to projects
  - `sync-workflow-example.yml` - Alternative sync workflow
  - `template-readme-example.md` - README template for projects

- **Documentation** (root directory):
  - `README.md` - Repository overview and usage
  - `USAGE_EXAMPLE.md` - Detailed setup instructions

- **Automation** (`.github/workflows/`):
  - `security-review.yml` - Automated security analysis for PRs

### Development Workflow

1. **Updating Templates**
   - Review impact on existing projects using these templates
   - Test workflow examples before committing
   - Update version/date information in relevant files
   - Ensure documentation reflects template changes

2. **Adding New Features**
   - Document clearly in README.md
   - Provide working examples in USAGE_EXAMPLE.md
   - Consider backward compatibility
   - Update security review patterns if needed

3. **Testing Changes**
   - Validate YAML syntax for workflow files
   - Test Markdown rendering (especially code blocks and lists)
   - Verify external links are not broken
   - Run security review workflow locally if possible

### Quality Standards

- **Clarity Over Complexity**: Instructions should be immediately actionable
- **Examples Required**: Every feature needs a concrete usage example
- **No Assumptions**: Document prerequisites and dependencies explicitly
- **Maintainability**: Consider long-term maintenance when adding features

### Prohibited Actions

- Modifying `common-instructions.md` without explicit approval
- Adding executable code that performs destructive operations
- Introducing dependencies or build tools (this is a docs-only repo)
- Breaking changes to template structure without migration guide

### Language and Style

- **Primary Language**: English (for international OSS collaboration)
- **Tone**: Professional, direct, helpful
- **Code Comments**: N/A (no code in this repository)
- **Documentation**: Use active voice, imperative mood for instructions

---

**Note**: This repository follows a documentation-first approach. Changes should prioritize clarity, accuracy, and
usability for the KEINOS organization's Go developers.

**Last Updated**: October 21, 2025
