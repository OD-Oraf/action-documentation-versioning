# Documentation Versioning and Rollback Automation

A comprehensive GitHub Actions solution for automated semantic versioning of documentation with intelligent change detection and safe rollback capabilities.

## 🚀 Features

### **Smart Documentation Versioning**
- **Automatic semantic versioning** with `documentation-v*` tags
- **Intelligent change detection** - only versions when markdown files change
- **Flexible increment strategies** - major, minor, or patch versions
- **Manual and automatic triggers** with different behaviors
- **Descriptive outputs** explaining what versioning action was taken

### **Safe Documentation Rollback**
- **Manual rollback workflow** with confirmation requirements
- **Tag validation** to prevent rollbacks to non-existent versions
- **Selective markdown merging** from any tagged version
- **Rollback tracking** with dedicated rollback tags
- **Comprehensive notifications** for success/failure states

### **Composite Action for Documentation Merging**
- **Reusable action** for merging markdown files from tagged versions
- **Input validation** for semantic version formats
- **Flexible targeting** to any branch
- **Clean temporary branch management**
- **Detailed outputs** with merged files and commit information

## 📋 Components

This repository contains three main components:

1. **`.github/workflows/workflow.yml`** - Main versioning workflow
2. **`.github/workflows/rollback-docs.yml`** - Rollback workflow  
3. **`.github/action.yaml`** - Composite action for documentation merging

## 🎯 Main Versioning Workflow

### **Triggers**
- **Push to main branch** - Creates versions only when markdown files change
- **Manual dispatch** - Always creates version with selectable increment type
- **Pull requests** - Skipped (no versioning)

### **Version Increment Strategies**
- **`major`**: `1.2.3` → `2.0.0` (breaking changes)
- **`minor`**: `1.2.3` → `1.3.0` (new features)
- **`patch`**: `1.2.3` → `1.2.4` (bug fixes, default)

### **Usage**

**Automatic Versioning (Push Events):**
```bash
# Only creates version if .md files changed
git add docs/guide.md README.md
git commit -m "Update documentation"
git push origin main
# Result: documentation-v1.0.1 (patch increment)
```

**Manual Versioning:**
1. Go to Actions → Documentation Versioning
2. Click "Run workflow"
3. Select version increment type
4. Click "Run workflow"

### **Workflow Outputs**
- `version-tag`: Created tag (e.g., `documentation-v1.2.3`)
- `docs-changed`: Boolean indicating if markdown files changed
- `version-description`: Human-readable explanation of action taken

## 🔄 Rollback Workflow

### **Safety Features**
- **Manual trigger only** - No accidental rollbacks
- **Confirmation requirement** - Must type "CONFIRM"
- **Tag validation** - Verifies rollback target exists
- **Multi-job structure** - Validation gate before execution

### **Usage**

1. Go to Actions → Rollback Documentation
2. Click "Run workflow"
3. Enter target version (e.g., `1.2.0`)
4. Type `CONFIRM` in confirmation field
5. Optionally specify target branch and docs path
6. Click "Run workflow"

### **Example**
```yaml
# Rollback to documentation-v1.2.0
rollback_to_version: "1.2.0"
target_branch: "main"
docs_path: "."
confirm_rollback: "CONFIRM"
```

## 🔧 Composite Action

### **Inputs**
- `version` (required): Semantic version number (e.g., `1.2.0`)
- `target-branch`: Target branch for merge (default: `main`)
- `docs-path`: Path to documentation files (default: `.`)

### **Outputs**
- `merged-files`: List of files that were merged
- `commit-sha`: SHA of the created commit
- `full-tag`: Complete tag name used internally

### **Usage in Other Workflows**
```yaml
- name: Merge documentation from tag
  uses: ./
  with:
    version: "1.2.0"
    target-branch: "main"
    docs-path: "docs/"
```

## 📊 Example Scenarios

### **Scenario 1: Documentation Updates**
```bash
# Edit markdown files
git add README.md docs/api.md
git commit -m "Update API documentation"
git push origin main
# Result: documentation-v1.0.1 created automatically
```

### **Scenario 2: Major Documentation Restructure**
1. Manual trigger with `major` increment
2. Result: `documentation-v2.0.0` created
3. Description: "Manual trigger (major increment): version incremented to documentation-v2.0.0"

### **Scenario 3: Code-Only Changes**
```bash
# Edit only code files
git add src/main.js package.json
git commit -m "Fix bug in main function"
git push origin main
# Result: No version created, "No changes detected in markdown files: version increment skipped"
```

### **Scenario 4: Emergency Rollback**
1. Trigger rollback workflow
2. Target version: `1.5.0`
3. Confirmation: `CONFIRM`
4. Result: Documentation rolled back, tracking tag created

## ⚙️ Setup Requirements

### **Repository Permissions**
```yaml
permissions:
  contents: write
```

### **Git Configuration**
The workflows automatically configure git user settings:
```yaml
git config user.name "GitHub Actions"
git config user.email "action@github.com"
```

## 🏷️ Tag Format

All documentation tags follow the format: `documentation-vX.Y.Z`

**Examples:**
- `documentation-v1.0.0` (initial version)
- `documentation-v1.2.3` (patch update)
- `documentation-v2.0.0` (major version)
- `rollback-to-v1.2.0-20240630` (rollback tracking tag)

## 🔍 Monitoring and Debugging

### **Workflow Logs**
Each workflow provides detailed logging:
- Change detection results
- Version increment decisions
- Tag creation confirmation
- Rollback validation steps

### **Output Descriptions**
The workflows provide human-readable descriptions:
- `"Changes detected in markdown files: version incremented to documentation-v1.2.4 (patch increment)"`
- `"Manual trigger (major increment): version incremented to documentation-v2.0.0"`
- `"No changes detected in markdown files: version increment skipped"`

## 🛡️ Safety Measures

1. **Change Detection** - Prevents unnecessary version bumps
2. **Input Validation** - Validates version increment types and semantic versions
3. **Confirmation Requirements** - Prevents accidental rollbacks
4. **Tag Validation** - Ensures rollback targets exist
5. **Error Handling** - Graceful failure with helpful messages
6. **Temporary Branch Cleanup** - Automatic cleanup of working branches

## 📈 Version History

- **v1.0.0** - Initial release with smart versioning and rollback capabilities

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with the provided workflows
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.