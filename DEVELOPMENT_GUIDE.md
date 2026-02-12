# OpenClaw Personal Development Setup - Quick Reference

## Setup Complete ✅

Your OpenClaw fork has been configured with a three-branch development workflow.

## Branch Structure

```
upstream (openclaw/openclaw)
    ↓
main (tracks upstream) ← sync only
    ↓
custom (your modifications) ← develop here
    ↓
deploy (main + custom) ← deploy from here
```

## Remote Configuration

- **origin**: `git@github.com:Jun9955/openclaw.git` (your fork)
- **upstream**: `https://github.com/openclaw/openclaw.git` (official repo)

## Branch Details

| Branch   | Purpose            | Track           | Status      |
| -------- | ------------------ | --------------- | ----------- |
| `main`   | Mirror upstream    | `upstream/main` | Read-only   |
| `custom` | Your modifications | `origin/custom` | Development |
| `deploy` | Production         | `origin/deploy` | Deployment  |

## Custom Code Locations

- `extensions/jun-custom/` - Your custom extensions
- `skills/jun-skills/` - Your custom skills

## Common Workflows

### 1. Daily Development

```bash
# Work on new feature
git checkout custom
git checkout -b feature/my-new-feature

# Make changes, test
pnpm install
pnpm build
pnpm test

# Commit and merge to custom
git commit -m "Add new feature"
git checkout custom
git merge feature/my-new-feature
git push origin custom

# Update deploy branch
git checkout deploy
git merge custom
git push origin deploy
```

### 2. Sync with Upstream

```bash
# Update main from upstream
git checkout main
git pull upstream main
git push origin main

# Merge updates into custom (may have conflicts)
git checkout custom
git merge main
# Resolve conflicts if any
git push origin custom

# Update deploy
git checkout deploy
git merge main
git merge custom
git push origin deploy
```

### 3. Deploy to Production

```bash
# Use the deploy branch
git checkout deploy
pnpm install
pnpm build

# Option 1: Install globally
pnpm link --global
# or
npm install -g .

# Option 2: Run directly
node dist/entry.js

# Start gateway
openclaw gateway --port 18789
```

### 4. Create Feature Branch

```bash
# From custom branch
git checkout custom
git checkout -b feature/awesome-feature

# Develop, test, commit
git add .
git commit -m "Implement awesome feature"

# Merge back to custom
git checkout custom
git merge feature/awesome-feature
git push origin custom
```

### 5. Emergency Hotfix

```bash
# Create hotfix from deploy
git checkout deploy
git checkout -b hotfix/critical-fix

# Fix, test, commit
git commit -m "Fix critical bug"

# Apply to all branches
git checkout deploy
git merge hotfix/critical-fix
git push origin deploy

git checkout custom
git cherry-pick <commit-hash>
git push origin custom
```

## Git Status Check

```bash
# Check current status
git status

# Check branch relationships
git branch -vv

# Check remote configuration
git remote -v

# View commit history
git log --graph --oneline --all --decorate
```

## Build & Test

```bash
# Install dependencies
pnpm install

# Build project
pnpm build

# Run tests
pnpm test

# Lint code
pnpm lint

# Format code
pnpm format
```

## Troubleshooting

### Merge Conflicts

```bash
# When syncing upstream
git checkout custom
git merge main

# If conflicts occur
git status  # See conflicted files
# Edit files to resolve conflicts
git add <resolved-files>
git commit
```

### Reset Branch to Upstream

```bash
# If main gets messed up
git checkout main
git reset --hard upstream/main
git push origin main --force
```

### Stash Changes

```bash
# Save work in progress
git stash

# Switch branches, do work

# Restore work
git stash pop
```

## Best Practices

1. **Keep main clean**: Never commit directly to main
2. **Develop in custom**: All your changes go to custom branch
3. **Test before merge**: Always test before merging to deploy
4. **Sync regularly**: Pull from upstream frequently to avoid conflicts
5. **Use feature branches**: Create branches for each feature
6. **Modular code**: Keep customizations in jun-custom directories

## Environment Variables

```bash
# .env.production
OPENCLAW_PROFILE=production
OPENCLAW_GATEWAY_PORT=18789
OPENCLAW_CUSTOM_FEATURES=enabled

# .env.development
OPENCLAW_PROFILE=dev
OPENCLAW_GATEWAY_PORT=18790
OPENCLAW_SKIP_CHANNELS=1
```

## Ignore Personal Files

Add to `.git/info/exclude`:

```
.env.local
.env.production
config/personal/
data/personal/
```

## Current Status

- ✅ Git remotes configured (origin + upstream)
- ✅ Branches created (main, custom, deploy)
- ✅ Custom directories created
- ✅ Build verified successful
- ✅ All branches pushed to GitHub

## Next Steps

1. Configure git user info (if needed):

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

2. Start developing:

   ```bash
   git checkout custom
   # Make your changes in extensions/jun-custom or skills/jun-skills
   ```

3. Deploy when ready:
   ```bash
   git checkout deploy
   pnpm install && pnpm build
   ```

## Repository URLs

- Your fork: https://github.com/Jun9955/openclaw
- Upstream: https://github.com/openclaw/openclaw

---

**Last Updated**: 2026-02-12
**Setup Location**: `/Users/jun/Desktop/openclaw`
