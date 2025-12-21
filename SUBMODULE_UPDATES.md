# Embedded Repo Auto-Update System

This repository uses GitHub Actions to keep embedded repositories synchronized with their upstream sources. It does not use Git submodules.

## Workflow

### Update Embedded Repos (`.github/workflows/update-submodules.yml`)

**Purpose**: Clones upstream repositories and syncs their files into local directories.

**Behavior**:
- Runs daily at 02:00 UTC
- Can be triggered manually via `workflow_dispatch`
- Syncs `Lampac/` and `lampa-source/` via clone + rsync
- Commits and pushes directly to `main`
- Only commits when changes are detected

## How It Works

1. **Checkout**: The repository is checked out normally.
2. **Clone**: Upstream repos are cloned into temporary directories.
3. **Sync**: Contents are rsynced into `Lampac/` and `lampa-source/` (excluding `.git`).
4. **Detect**: A git diff determines whether updates exist.
5. **Commit**: If changes exist, a commit is created.
6. **Push**: Updates are pushed to `main`.

## Manual Repo Updates

```bash
# Sync Lampac
(tmp="$(mktemp -d)" \
  && git clone --depth 1 https://github.com/immisterio/Lampac "$tmp" \
  && rm -rf "$tmp/.git" \
  && rsync -a --delete "$tmp"/ Lampac/ \
  && rm -rf "$tmp")

# Sync lampa-source
(tmp="$(mktemp -d)" \
  && git clone --depth 1 https://github.com/yumata/lampa-source "$tmp" \
  && rm -rf "$tmp/.git" \
  && rsync -a --delete "$tmp"/ lampa-source/ \
  && rm -rf "$tmp")
```

## Configuration

### Schedule
Edit the cron schedule in the workflow file:

```yaml
schedule:
  - cron: '0 2 * * *'  # Daily at 02:00 UTC
```

Common cron examples:
- `0 2 * * *` - Daily at 02:00 UTC
- `0 */6 * * *` - Every 6 hours
- `0 2 * * 0` - Weekly on Sunday at 02:00 UTC
- `0 2 1 * *` - Monthly on the 1st at 02:00 UTC

### Permissions
The workflow requires:
- `contents: write` to commit and push changes.

## Benefits

1. **Automation**: Regular updates without manual work.
2. **Deepwiki-friendly**: Files live in the main repo for indexing.
3. **Transparency**: All updates are traceable via commits.
4. **Safety**: No commits if there are no changes.

## Security Notes

- Uses `GITHUB_TOKEN` with minimal permissions.
- Commits are authored by `github-actions[bot]`.
- Updates are pushed directly to `main`.

## Troubleshooting

If the workflow fails:

1. Check the Actions tab for logs.
2. Ensure upstream repositories are reachable.
3. Verify branch protection allows `github-actions[bot]` to push.
4. Confirm Actions permissions include `contents: write`.
