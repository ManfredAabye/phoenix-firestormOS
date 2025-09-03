# Daily Sync Workflow

This document explains the `daily-sync.yml` GitHub Actions workflow that automatically synchronizes this Phoenix-FirestormOS fork with the upstream Firestorm repository.

## Overview

The daily sync workflow helps maintain this fork up-to-date with the latest changes from the upstream [FirestormViewer/phoenix-firestorm](https://github.com/FirestormViewer/phoenix-firestorm) repository.

## Features

### Automatic Scheduling
- **Daily execution**: Runs automatically every day at 02:00 UTC
- **Manual trigger**: Can be manually triggered via GitHub Actions UI

### Smart Sync Detection
- Checks if upstream has new commits compared to the current fork
- Only creates pull requests when there are actual updates
- Provides detailed commit summaries and counts

### Conflict Handling
- **Successful merges**: Creates a pull request for review
- **Merge conflicts**: Creates an issue with detailed conflict information
- **Safe operation**: Never pushes directly to the main branch

### Comprehensive Reporting
- Pull request descriptions include commit summaries and review checklists
- Issues created for conflicts include resolution instructions
- GitHub Actions summary provides quick status overview

## Workflow Inputs

When manually triggering the workflow, you can configure:

- **force_sync**: Force sync even if no changes detected (default: false)
- **upstream_branch**: Upstream branch to sync from (default: master)

## How It Works

1. **Repository Setup**
   - Checks out the current repository with full history
   - Configures Git with GitHub Actions credentials
   - Adds upstream repositories as remotes

2. **Change Detection**
   - Fetches latest changes from upstream
   - Compares commit hashes between local and upstream
   - Counts new commits and extracts summaries

3. **Sync Process**
   - Creates a timestamped branch for the sync
   - Attempts to merge upstream changes
   - Handles merge conflicts gracefully

4. **Result Handling**
   - **Success**: Creates pull request with detailed information
   - **Conflicts**: Creates issue with conflict details and resolution steps
   - **No changes**: Logs successful no-op status

## Pull Request Details

When updates are found and successfully merged, the workflow creates a pull request containing:

- Number of commits to sync
- List of recent upstream commits
- Review checklist for maintainers
- Automatic labels for easy identification

## Merge Conflict Resolution

If merge conflicts occur, the workflow creates an issue with:

- List of conflicted files
- Step-by-step resolution instructions
- Commands to manually resolve conflicts
- Urgent priority labels

## Repository Structure

The workflow is designed for Phoenix-FirestormOS which:
- Forks from FirestormViewer/phoenix-firestorm
- May have custom modifications and features
- Requires careful review of upstream changes

## Manual Sync Process

If you need to sync manually:

1. Clone the repository locally
2. Add upstream remote:
   ```bash
   git remote add upstream https://github.com/FirestormViewer/phoenix-firestorm.git
   ```
3. Fetch upstream changes:
   ```bash
   git fetch upstream
   ```
4. Create a sync branch:
   ```bash
   git checkout -b sync/manual-$(date +%Y%m%d)
   ```
5. Merge upstream:
   ```bash
   git merge upstream/master
   ```
6. Resolve any conflicts and commit
7. Push and create a pull request

## Monitoring

- Check the [Actions tab](../../actions) for workflow runs
- Monitor for new pull requests with the `sync` label
- Watch for issues with the `conflicts` label that need attention

## Configuration

The workflow uses these environment variables:
- `UPSTREAM_REPO_FIRESTORM`: FirestormViewer/phoenix-firestorm
- `UPSTREAM_REPO_VARIABLES`: FirestormViewer/fs-build-variables  
- `TARGET_BRANCH`: master

## Security

- Uses GitHub's built-in `GITHUB_TOKEN` for authentication
- Only has permissions for contents and pull requests
- Never force-pushes or modifies the main branch directly
- All changes go through pull request review process

## Troubleshooting

### Common Issues

1. **Permission Errors**
   - Ensure the repository has Actions enabled
   - Check that the `GITHUB_TOKEN` has sufficient permissions

2. **Merge Conflicts**
   - Review the created issue for detailed conflict information
   - Follow the manual resolution steps provided

3. **Failed Workflow**
   - Check the Actions logs for detailed error messages
   - Verify upstream repositories are accessible

### Support

For issues with the sync workflow:
1. Check the Actions tab for error logs
2. Review any created issues for conflict details
3. Manually sync following the steps above if needed