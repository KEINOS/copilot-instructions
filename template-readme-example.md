# KEINOS Go Project Template

This is the organization template for Go projects under KEINOS.

## Setup Instructions

1. Use this template to create new Go projects
2. The `.github/copilot-instructions.md` is pre-configured with organization standards
3. Update project-specific sections as needed

## Updating Common Instructions

To update the common Go development instructions across all projects:

1. Update `common-instructions.md` in this template repository
2. Projects can pull updates using the sync workflow or manual updates

## File Structure

```text
.github/
├── copilot-instructions.md    # Pre-configured with common instructions
├── workflows/
│   └── sync-instructions.yml  # Auto-sync workflow (optional)
└── PULL_REQUEST_TEMPLATE.md   # PR template with Go checklist
```