# Quick Start Guide

Get up and running with automated releases in 5 minutes! 🚀

## 🎯 Two Ways to Use This

### Option 1: Reusable Workflows (Recommended)
Use the workflows directly from this repository - no copying needed!

### Option 2: Copy & Customize
Copy the workflows to your repository and customize them.

## 🚀 Quick Setup (Reusable Workflows)

### 1. Create Release Workflow

Create `.github/workflows/release.yml` in your repository:

```yaml
name: Release

on:
  push:
    tags:
      - 'v*.*.*'
  workflow_dispatch:

jobs:
  release:
    uses: rocajuanma/simple-release/.github/workflows/reusable-release.yml@v1.0.0
    secrets:
      RELEASE_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 2. Create Post-Release Workflow

Create `.github/workflows/post-release-changelog.yml` in your repository:

```yaml
name: Post-Release Changelog Update

on:
  release:
    types: [published]

jobs:
  update-changelog:
    uses: rocajuanma/simple-release/.github/workflows/reusable-post-release-changelog.yml@v1.0.0
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 3. Create Changelog

Create `CHANGELOG.md` in your repository:

```markdown
# Changelog

## [Unreleased]

### Added
- Your new features here

### Changed
- Your changes here

### Fixed
- Your bug fixes here

## [1.0.0] - 2024-01-01

### Added
- Initial release
```

### 4. Test It!

1. **Commit and push** your changes
2. **Create a tag**: `git tag v1.0.0 && git push origin v1.0.0`
3. **Watch the magic happen** ✨

## 🔧 Customization Options

### Adding Build Steps
The workflows include a **BUILD EXTENSION POINT** where you can add your build process:

```yaml
# Add this in the BUILD EXTENSION POINT section of the workflow
- name: Set up Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '18'

- name: Build and package
  run: |
    npm ci
    npm run build
    npm pack
```

### Custom Paths
```yaml
jobs:
  release:
    uses: rocajuanma/simple-release/.github/workflows/reusable-release.yml@v1.0.0
    with:
      changelog-path: 'docs/CHANGELOG.md'
      main-package: './cmd/myapp'
    secrets:
      RELEASE_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## 📋 What Happens Next

1. **Tag pushed** → Release workflow triggers
2. **Build process runs** (your custom build steps)
3. **Release created** with artifacts and formatted release notes
4. **Post-release workflow** updates changelog
5. **PR created** with changelog updates for review

## 🎉 You're Done!

Your repository now has:
- ✅ Automated releases on tag push
- ✅ Customizable build process
- ✅ Automatic changelog updates
- ✅ Professional release notes

## 🆘 Need Help?

- Check the [full documentation](README.md)
- Look at the [examples](examples/) directory
- Open an issue if you need help!

## 🔗 Links

- [Full Documentation](README.md)
- [Examples](examples/)
- [Changelog Template](examples/CHANGELOG.md)
