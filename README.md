# Action Documentation Versioning 

This GitHub Action automatically creates and pushes version tags for documentation changes.

## Required Permissions

To push commits and tags to the repository, the workflow needs the following permission:

```yaml
permissions:
  contents: write
```

This permission allows the action to:
- Create new tags
- Push tags to the repository
- Configure git user settings

## Version 1.1.4