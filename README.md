# Simple Release

Generic GitHub Actions workflows for automated releases and changelog management.

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
    uses: rocajuanma/simple-release/workflows/reusable-release.yml@v1.0.0
    with:
      build-artifacts: 'dist/* install.sh'  # Your build outputs
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
    uses: rocajuanma/simple-release/workflows/reusable-post-release-changelog.yml@v1.0.0
    with:
      simple-release-version: 'v1.0.0'  # Optional: Match your workflow tag
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

### 4. Add Build Steps

In your release workflow, add your build process in the **BUILD EXTENSION POINT** section:

```yaml
# Example for Go projects (like palantir)
- name: Set up Go
  uses: actions/setup-go@v4
  with:
    go-version: '1.21'

- name: Build binaries
  run: |
    mkdir -p dist
    PROJECT_NAME=$(grep '^module ' go.mod | awk '{print $2}' | sed 's/.*\///')
    GOOS=darwin GOARCH=amd64 go build -o dist/${PROJECT_NAME}-darwin-amd64 ./cmd/terminal
    GOOS=darwin GOARCH=arm64 go build -o dist/${PROJECT_NAME}-darwin-arm64 ./cmd/terminal
    GOOS=linux GOARCH=amd64 go build -o dist/${PROJECT_NAME}-linux-amd64 ./cmd/terminal
    GOOS=linux GOARCH=arm64 go build -o dist/${PROJECT_NAME}-linux-arm64 ./cmd/terminal
    cd dist && shasum -a 256 * > checksums.txt
```

## Usage

1. **Push a tag**: `git tag v1.0.0 && git push origin v1.0.0`
2. **Release created** with your build artifacts
3. **Changelog updated** automatically
4. **PR created** with changelog changes

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


## Files

- `.github/workflows/` - Local workflows (copy to your repo)
- `workflows/` - Reusable workflows (reference from other repos)
- `scripts/update-changelog.py` - Changelog update script
- `examples/` - Usage examples