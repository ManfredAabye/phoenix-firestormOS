# Daily Sync Action

The Phoenix FirestormOS repository includes an automated synchronization workflow that keeps the fork up-to-date with the upstream Firestorm repository.

## Overview

The daily sync action (`daily-sync.yml`) automatically syncs changes from the official [FirestormViewer/phoenix-firestorm](https://github.com/FirestormViewer/phoenix-firestorm) repository to maintain this fork current with upstream development.

## How It Works

### Automatic Schedule
- Runs daily at **2:00 AM UTC**
- Fetches the latest changes from upstream
- Attempts to merge changes automatically
- Handles conflicts gracefully

### Manual Triggering
You can manually trigger the sync action at any time:

1. Go to the **Actions** tab in the GitHub repository
2. Select **"Daily Sync from Upstream"** workflow
3. Click **"Run workflow"** button
4. Choose the branch (usually `master` or `main`)
5. Click **"Run workflow"** to start

## Sync Process

### Successful Merge
When no conflicts are detected:
1. Fetches upstream changes
2. Automatically merges the changes
3. Pushes the merged changes to the repository
4. Shows a success summary

### Merge Conflicts
When conflicts are detected:
1. Creates a new branch named `sync-conflicts-{run_number}`
2. Commits the conflicted files with conflict markers
3. Automatically creates a pull request for manual review
4. Provides detailed instructions for resolution

## Conflict Resolution

When the sync action detects conflicts, it will:

1. **Create a Pull Request** with a descriptive title like "Sync conflicts from upstream (Run #123)"

2. **Provide Details** including:
   - Upstream commit that caused conflicts
   - Run number for tracking
   - Branch name with conflicts

3. **Give Instructions** on what to do:
   - Review files with conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
   - Manually resolve all conflicts
   - Test changes thoroughly
   - Merge the pull request when ready

### Manual Resolution Steps

1. **Check out the conflict branch**:
   ```bash
   git checkout sync-conflicts-{run_number}
   ```

2. **Find files with conflicts**:
   ```bash
   git status
   # Look for files marked as "both modified"
   ```

3. **Resolve conflicts** by editing files and removing conflict markers

4. **Test the changes** to ensure functionality

5. **Commit resolved changes**:
   ```bash
   git add .
   git commit -m "Resolve sync conflicts"
   ```

6. **Merge the pull request** through GitHub interface

## Authentication

The workflow uses `secrets.GITHUB_TOKEN` which is automatically provided by GitHub Actions. No additional configuration is needed.

## Error Handling

The action includes robust error handling:

- **Missing upstream branches**: Checks for both `main` and `master` branches
- **Network issues**: Git operations include appropriate error handling  
- **Permission issues**: Uses GitHub's built-in authentication
- **History differences**: Uses `--allow-unrelated-histories` flag

## Monitoring

Check the **Actions** tab to monitor sync runs:
- ✅ **Green checkmark**: Successful sync or no changes needed
- ⚠️ **Yellow warning**: Conflicts detected, PR created
- ❌ **Red X**: Failure (check logs for details)

## Troubleshooting

### Common Issues

**Q: The sync failed with "Could not find upstream main or master branch"**  
A: This usually indicates a network issue. Try running the workflow manually.

**Q: I have multiple conflict PRs open**  
A: Resolve and merge the oldest one first, then close the newer duplicate PRs.

**Q: The action isn't running daily**  
A: Check that the repository has Actions enabled and the workflow file is on the default branch.

**Q: How do I disable the automatic sync?**  
A: You can disable the workflow in the Actions tab, or modify the `daily-sync.yml` file to remove the `schedule` trigger.

## Repository Structure

The sync action maintains:
- **Source code** from upstream Firestorm
- **Custom modifications** specific to Phoenix FirestormOS
- **Build configurations** and scripts
- **Documentation** and metadata

Any custom changes should be clearly marked and documented to avoid conflicts during future syncs.