# Documentation Versioning Action

A composite GitHub Action that automatically creates semantic version tags for documentation when markdown files change.

## Features

- 🏷️ **Automatic Semantic Versioning**: Creates `documentation-vX.Y.Z` tags
- 📝 **Smart Change Detection**: Only versions when markdown files are modified
- 🎯 **Flexible Increment Strategy**: Support for patch, minor, and major version increments
- 🔧 **Configurable Path Filtering**: Check for changes in specific documentation paths
- 🚀 **Force Versioning**: Option to create versions regardless of file changes
- 📊 **Detailed Outputs**: Provides comprehensive information about versioning actions

## Usage

### Basic Usage

```yaml
- name: Version Documentation
  uses: ./.github/actions/version-docs
  with:
    version_increment: 'patch'
```

### Advanced Usage

```yaml
- name: Version Documentation
  id: version-docs
  uses: ./.github/actions/version-docs
  with:
    version_increment: 'minor'
    force_versioning: 'false'
    docs_path: 'documentation'

- name: Use outputs
  run: |
    echo "Created tag: ${{ steps.version-docs.outputs.version-tag }}"
    echo "Docs changed: ${{ steps.version-docs.outputs.docs-changed }}"
    echo "Description: ${{ steps.version-docs.outputs.version-description }}"
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `version_increment` | Version increment strategy (`patch`, `minor`, `major`) | No | `patch` |
| `force_versioning` | Force versioning even if no markdown changes detected | No | `false` |
| `docs_path` | Path to documentation files to check for changes | No | `.` |

## Outputs

| Output | Description |
|--------|-------------|
| `version-tag` | The created version tag (e.g., `documentation-v1.2.3`) |
| `docs-changed` | Whether documentation files were changed (`true`/`false`) |
| `version-description` | Description of the versioning action taken |
| `tag-created` | Whether a new tag was created (`true`/`false`) |

## Behavior

### Change Detection

The action checks for changes in markdown files (`.md` extension) within the specified `docs_path`:

- **Push Events**: Compares current commit with previous commit
- **Workflow Dispatch**: Always proceeds with versioning
- **Other Events**: Skips versioning by default

### Version Increment Logic

- **First Run**: Creates `documentation-v1.0.0`
- **Subsequent Runs**: Increments based on `version_increment` input:
  - `patch`: `v1.0.0` → `v1.0.1`
  - `minor`: `v1.0.0` → `v1.1.0`
  - `major`: `v1.0.0` → `v2.0.0`

### Git Configuration

The action automatically configures git with:
- **Name**: "GitHub Actions"
- **Email**: "action@github.com"

## Example Workflow

```yaml
name: Documentation Versioning

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:
    inputs:
      version_increment:
        description: 'Version increment strategy'
        required: false
        default: 'patch'
        type: choice
        options:
          - patch
          - minor
          - major

jobs:
  version-docs:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Required for tag operations
      
      - name: Version Documentation
        id: version
        uses: ./.github/actions/version-docs
        with:
          version_increment: ${{ github.event.inputs.version_increment || 'patch' }}
          docs_path: 'documentation'
      
      - name: Create Release
        if: steps.version.outputs.tag-created == 'true'
        run: |
          echo "Created documentation version: ${{ steps.version.outputs.version-tag }}"
          echo "Description: ${{ steps.version.outputs.version-description }}"
```

## Requirements

- Repository must be checked out with `fetch-depth: 0` to access git history
- Workflow must have `contents: write` permission to create and push tags
- Git repository must be initialized and have at least one commit

## Integration with Existing Workflow

This action can replace the versioning logic in your existing workflow while maintaining the same functionality. Simply replace the versioning steps with a call to this action and use the outputs as needed.
