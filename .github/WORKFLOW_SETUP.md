# GitHub Workflows Setup

The GitHub Actions workflows have been created but need workflow scope permissions to push.

## Quick Setup

To enable the workflows, you need to grant `workflow` scope to the GitHub CLI:

```bash
# Refresh GitHub authentication with workflow scope
gh auth refresh -h github.com -s workflow
```

This will provide a one-time code and URL. Follow the prompts to authorize.

## Alternative: Push via Web Interface

If you prefer not to use the CLI:

1. Go to https://github.com/tkirkland/string_output.sh
2. Click "Add file" → "Upload files"
3. Upload the `.github/` directory
4. Commit directly to master

## Alternative: Manual Token Setup

1. Create a personal access token with `workflow` scope:
   - Go to https://github.com/settings/tokens
   - Generate new token (classic)
   - Select `workflow` scope
   - Copy the token

2. Use the token:
   ```bash
   git remote set-url origin https://YOUR_TOKEN@github.com/tkirkland/string_output.sh.git
   git push origin master
   ```

## After Setup

Once pushed, the workflows will:

1. **CI Workflow** - Run on every push/PR:
   - ShellCheck linting
   - Bash syntax validation
   - Library sourcing tests
   - Bash compatibility tests (4.4, 5.0, 5.1, 5.2)

2. **Auto-Release Workflow** - Run when version changes:
   - Detect version changes in `string_output.sh`
   - Automatically create version tags (e.g., `v1.1.0`)

3. **Release Workflow** - Run when tags are pushed:
   - Run all CI checks
   - Create GitHub release
   - Attach library files
   - Generate release notes

## Testing the Workflow

After pushing, update the version to trigger an auto-release:

1. Edit `string_output.sh` line 7: `# Text Handler Library v1.1.1`
2. Add changelog entry
3. Commit and push
4. Watch GitHub Actions create the tag and release automatically!

## Current Status

✅ Workflows created locally
⏳ Waiting for push with workflow scope
⏳ Workflows will activate after first push
