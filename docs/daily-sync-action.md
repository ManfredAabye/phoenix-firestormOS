# Daily Sync Action

This GitHub Action automatically syncs changes from the upstream Firestorm repository into this fork.

## Overview

The `daily-sync.yml` workflow:

1. **Runs daily at 2 AM UTC** - Automatically checks for upstream changes
2. **Can be manually triggered** - Using the GitHub Actions "Run workflow" button
3. **Syncs from upstream** - Fetches changes from `FirestormViewer/phoenix-firestorm`
4. **Handles conflicts gracefully** - Creates pull requests for manual review when merge conflicts occur
5. **Pushes clean merges automatically** - When no conflicts exist

## Manual Triggering

You can manually start the sync action by:

1. Going to the "Actions" tab in GitHub
2. Selecting "Daily Sync from Upstream"
3. Clicking "Run workflow"

## Conflict Resolution

When merge conflicts occur:
- The workflow will create a pull request with the title "Sync conflicts from upstream - Manual review required"
- The branch will be named `sync-conflicts-{run_number}`
- Manual review and resolution will be required

## Upstream Repository

This action syncs from: https://github.com/FirestormViewer/phoenix-firestorm