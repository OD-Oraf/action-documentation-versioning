# Documentation Rollback Action

A comprehensive composite action that safely rolls back documentation to any previously tagged version with extensive validation and safety checks.

## Features

- **Safe Rollback**: Comprehensive validation before any changes are made
- **Version Validation**: Ensures target version exists and follows semantic versioning
- **Branch Validation**: Verifies target branch exists before proceeding
- **Clean State Check**: Ensures working directory is clean before rollback
- **Confirmation Required**: Requires explicit "CONFIRM" input to prevent accidents
- **Detailed Outputs**: Provides comprehensive information about the rollback operation
- **Git History Preservation**: Maintains full git history during rollback process

## Usage

### Basic Usage

```yaml
- name: Rollback Documentation
  uses: ./.github/actions/rollback-docs
  with:
    rollback_to_version: "1.2.0"
    confirm_rollback: "CONFIRM"
```

### Advanced Usage

```yaml
- name: Rollback Documentation
  uses: ./.github/actions/rollback-docs
  with:
    rollback_to_version: "1.2.0"
    target_branch: "main"
    docs_path: "./documentation"
    confirm_rollback: "CONFIRM"
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `rollback_to_version` | Documentation version to rollback to (e.g., 1.0.0) | ✅ | - |
| `target_branch` | Branch to rollback documentation in | ❌ | `main` |
| `docs_path` | Path to documentation files | ❌ | `.` |
| `confirm_rollback` | Type "CONFIRM" to proceed with rollback | ✅ | - |

## Outputs

| Output | Description |
|--------|-------------|
| `rollback-completed` | Whether the rollback was completed successfully |
| `merged-files` | List of files that were rolled back |
| `commit-sha` | SHA of the rollback commit |
| `tag-used` | Full tag name used for rollback |
| `latest-version` | Latest documentation version before rollback |

## Validation Steps

The action performs comprehensive validation before executing the rollback:

1. **Confirmation Check**: Verifies that "CONFIRM" was provided
2. **Version Format**: Ensures version follows semantic versioning (X.Y.Z)
3. **Branch Existence**: Confirms target branch exists
4. **Tag Existence**: Verifies the rollback target tag exists
5. **Path Validation**: Checks documentation path exists (if not current directory)
6. **Clean State**: Ensures working directory has no uncommitted changes

## Safety Features

- **Explicit Confirmation**: Requires typing "CONFIRM" to prevent accidental rollbacks
- **Working Directory Check**: Ensures no uncommitted changes before proceeding
- **Branch Validation**: Verifies target branch exists before attempting rollback
- **Tag Validation**: Confirms rollback target version exists
- **Detailed Logging**: Provides comprehensive information throughout the process

## Example Workflow Integration

```yaml
name: Rollback Documentation

on:
  workflow_dispatch:
    inputs:
      rollback_to_version:
        description: 'Documentation version to rollback to (e.g., 1.0.0)'
        required: true
        type: string
      confirm_rollback:
        description: 'Type "CONFIRM" to proceed with rollback'
        required: true
        type: string

jobs:
  rollback:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    
    steps:
      - name: Rollback Documentation
        uses: ./.github/actions/rollback-docs
        with:
          rollback_to_version: ${{ github.event.inputs.rollback_to_version }}
          confirm_rollback: ${{ github.event.inputs.confirm_rollback }}
```

## Error Handling

The action includes comprehensive error handling for common scenarios:

- **Invalid version format**: Clear error message with expected format
- **Non-existent version**: Lists available versions
- **Non-existent branch**: Shows available branches
- **Uncommitted changes**: Displays uncommitted files
- **Missing confirmation**: Prevents accidental execution

## How It Works

1. **Checkout**: Fetches full repository history
2. **Validation**: Performs all safety and existence checks
3. **Tag Access**: Creates temporary branch to access tag content
4. **File Extraction**: Copies markdown files from the target tag
5. **Commit**: Creates rollback commit with detailed message
6. **Cleanup**: Removes temporary branch and provides summary

## Version Tag Format

The action expects documentation tags in the format: `documentation-vX.Y.Z`

Examples:
- `documentation-v1.0.0`
- `documentation-v2.1.3`
- `documentation-v1.2.0`
