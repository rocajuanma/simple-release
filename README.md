# Simple Release

Speed up your release process: create a tag to trigger a new GitHub release, automatically update your changelog with release details, and receive a pull request with the latest changelog changes.

## Quick Setup

### 1. Add Release Workflow

Create `.github/workflows/release.yml`:

```yaml
name: Release
on:
  push:
    tags: ['v*.*.*']
jobs:
  release:
    # Use @latest for newest version, or pin to specific version like @v1.0.0
    uses: rocajuanma/simple-release/workflows/reusable-release.yml@latest
    with:
      release-files: ''  # Optional: Additional files to include
    secrets:
      RELEASE_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 2. Add Post-Release Workflow

Create `.github/workflows/post-release-changelog.yml`:

```yaml
name: Post-Release Changelog Update
on:
  release:
    types: [published]
jobs:
  update-changelog:
    # Use @latest for newest version, or pin to specific version like @v1.0.0
    uses: rocajuanma/simple-release/workflows/reusable-post-release-changelog.yml@latest
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # Required: Creates PR and updates changelog
```

> **Note**: The `GITHUB_TOKEN` is required for the post-release workflow to create pull requests and update the changelog. For some repository settings, you may need to create a Personal Access Token (PAT) with `repo` permissions and add it as a repository secret.

### 3. Create Changelog

Create `CHANGELOG.md`:

```markdown
# Changelog

## [Unreleased]

### Added
- Your new features here

## [1.0.0] - 2024-01-01

### Added
- Initial release
```

## Usage

1. **Push a tag**: `git tag v1.0.0 && git push origin v1.0.0`
2. **Release created** with changelog content
3. **Changelog updated** automatically
4. **PR created** with changelog changes

## Version Management

### Using @latest (Recommended)
- **Always gets the newest version** with latest fixes and features
- **Automatic updates** when new versions are released
- **Best for most users** who want the latest functionality

```yaml
uses: rocajuanma/simple-release/workflows/reusable-release.yml@latest
```

### Pinning to Specific Versions
- **Use for production** where you need stability
- **Prevents unexpected changes** from automatic updates
- **Update manually** when you want new features

```yaml
uses: rocajuanma/simple-release/workflows/reusable-release.yml@v1.0.0
```

### Available Versions
Check [releases page](https://github.com/rocajuanma/simple-release/releases) for all available versions.

## Required Permissions

### GitHub Token Requirements
- **`GITHUB_TOKEN`**: Automatically provided by GitHub Actions
- **Permissions needed**: 
  - `contents: write` (to update changelog)
  - `pull-requests: write` (to create PR)
  - `metadata: read` (to read repository info)
- **Fallback option**: If default token fails, create a Personal Access Token (PAT) with `repo` scope and add as repository secret

### Troubleshooting Token Issues
If the post-release workflow fails to create PRs:
1. **Create a PAT**: Go to GitHub Settings → Developer settings → Personal access tokens
2. **Required scope**: Select `repo` (full repository access)
3. **Add as secret**: Repository Settings → Secrets → Add `GITHUB_TOKEN` with your PAT
4. **Test**: Push a new tag to verify the workflow works

> **Note**: Always use versioned references (e.g., `@v1.0.0`) instead of `@main` to ensure you're using the latest stable version. Set `simple-release-version` to match your workflow tag for consistency.

