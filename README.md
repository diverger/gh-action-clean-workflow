# 🗑️ Clean Workflow Logs Action

A GitHub Action to clean up old workflow runs and save storage space in your repository.

## Features

- Clean workflow runs older than a specified number of days
- Keep a specified number of latest runs per workflow
- Support for dry run mode to preview what would be deleted
- Handles orphaned workflow runs
- Provides detailed output statistics

## Usage

```yaml
name: Clean Workflow Logs

on:
  schedule:
    - cron: "0 16 * * 1" # Weekly cleanup
  workflow_dispatch:

jobs:
  cleanup:
    runs-on: ubuntu-latest
    permissions:
      actions: write
      contents: read
    steps:
      - name: Clean old workflow runs
        uses: ./path/to/this/action
        with:
          runs_older_than: "30"
          runs_to_keep: "5"
          dry_run: "false"
```

## Inputs

| Input | Description | Default | Required |
|-------|-------------|---------|----------|
| `runs_older_than` | Number of days old to delete | `21` | No |
| `runs_to_keep` | Number of latest runs to keep per workflow | `0` | No |
| `dry_run` | Only show what would be deleted | `false` | No |
| `github_token` | GitHub token with actions:write permissions | `${{ github.token }}` | No |

> [!NOTE]
> The `dry_run` parameter defaults to `false`. Set it to `true` to preview in the log output to see what would be deleted without actually deleting anything.

> [!TIP]
> For regular maintenance, consider setting `runs_older_than` to 30+ days and `runs_to_keep` to 5-10 runs per workflow.

> [!IMPORTANT]
> This action requires `actions: write` permissions to delete workflow runs.

> [!WARNING]
> Deleted workflow runs cannot be recovered. Always test with `dry_run: true` first.

> [!CAUTION]
> Be careful when setting `runs_to_keep` to 0, as this will delete all runs older than the specified days.

## Outputs

| Output | Description |
|--------|-------------|
| `total_runs` | Total number of workflow runs found |
| `deleted_runs` | Number of workflow runs deleted |
| `kept_runs` | Number of workflow runs kept |

## Permissions

The action requires the following permissions:

```yaml
permissions:
  actions: write
  contents: read
```

## License

MIT
