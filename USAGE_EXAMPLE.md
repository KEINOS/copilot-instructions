# Usage Example: Setting Up KEINOS Copilot Instructions in Your Go Project

## 1. New Project Setup

```bash
# 1. Navigate to your project directory
cd your-go-project

# 2. Create .github directory structure
mkdir -p .github/workflows

# 3. Download the sync workflow
curl -o .github/workflows/sync-copilot-instructions.yml \
  https://raw.githubusercontent.com/KEINOS/copilot-instructions/main/sync-from-repo-example.yml

# 4. Run initial sync manually
# Go to GitHub Actions tab and run "Sync Copilot Instructions" workflow
```

## 2. Existing Project Setup

```bash
# Backup existing copilot-instructions.md if present
[ -f .github/copilot-instructions.md ] && \
  cp .github/copilot-instructions.md .github/copilot-instructions.md.backup

# Add the sync workflow
curl -o .github/workflows/sync-copilot-instructions.yml \
  https://raw.githubusercontent.com/KEINOS/copilot-instructions/main/sync-from-repo-example.yml
```

## 3. Generated .github/copilot-instructions.md Example

```markdown
# Copilot Instructions for my-awesome-go-api

## Organization Standards

This project follows the KEINOS organization's common Go development guidelines.

<!-- SYNC_START: DO NOT EDIT THIS SECTION MANUALLY -->
<!-- Last synced: 2024-01-15T00:00:00Z -->
<!-- Source commit: abc1234 (2024-01-14T15:30:00Z) -->

[Complete content of common-instructions.md automatically inserted here]

<!-- SYNC_END -->

## Project-Specific Instructions

### Project Context

- **Project Name**: my-awesome-go-api
- **Purpose**: RESTful API for user management
- **Main Dependencies**: gin, gorm, jwt-go

### Special Considerations

- API follows OpenAPI 3.0 specification
- All endpoints require authentication except health check
- Rate limiting: 100 requests per minute per user

### Development Environment

- Go 1.21+ required
- PostgreSQL for development database
- Redis for session storage

### Testing Specifics

- Use testcontainers for integration tests
- Mock external API calls with httptest
- Database tests use separate test database
```

## 4. Daily Operations

### Automatic Sync (Recommended)

- **Runs daily at 00:00 UTC (09:00 JST)**
- Automatically creates PR when changes are detected
- Simply review and merge the PR

### Manual Sync (When Needed)

1. Go to GitHub Actions tab
2. Select "Sync Copilot Instructions" workflow
3. Click "Run workflow" button
4. Check "Force update" if needed

### Review Checklist for Updates

✅ **Review Auto-Generated Changes**

- Verify new instructions are appropriate
- Ensure no conflicts with project-specific settings

✅ **Update Project-Specific Sections**

- Add new dependencies
- Update special requirements

✅ **Team Communication**

- Notify team of important changes
- Explain new best practices

## 5. Troubleshooting

### Sync Failures

```bash
# Check workflow execution logs
gh run list --workflow="Sync Copilot Instructions"
gh run view [RUN_ID]

# Test manual sync
curl -f https://raw.githubusercontent.com/KEINOS/copilot-instructions/main/common-instructions.md
```

### Project-Specific Settings Disappearing

```bash
# Restore from backup
cp .github/copilot-instructions.md.backup .github/copilot-instructions.md

# Move project-specific settings outside SYNC markers
# Place them after <!-- SYNC_END --> comment
```

### Security Review Errors

1. **False Positive Cases**

   - Explain legitimate usage in PR comments
   - Consult with security team

2. **Actual Security Issues**

   - Fix the problematic code
   - Consider alternative approaches

## 6. Advanced Configuration

### Custom Sync Schedule

Edit the cron expression in `.github/workflows/sync-copilot-instructions.yml`:

```yaml
on:
  schedule:
    # Run twice daily at 00:00 and 12:00 UTC
    - cron: '0 0,12 * * *'
```

### Skip Auto-Sync for Specific Projects

Add this to your workflow file to disable auto-sync:

```yaml
# Add this condition to skip auto-sync
if: github.event_name != 'schedule'
```

### Custom PR Settings

Modify the PR creation step to customize reviewers or labels:

```yaml
- name: Create Pull Request
  uses: peter-evans/create-pull-request@v5
  with:
    reviewers: team-lead,security-reviewer
    labels: dependencies,automation
    # ... other settings
```
