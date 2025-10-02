# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

### Changed

### Fixed

## [] - 2025-10-01

### Added
- Core release and post-release workflows for automated GitHub releases
- Reusable workflow system for cross-repository usage
- Python script for automatic changelog updates
- Comprehensive documentation and examples
- Build extension points for customizable build processes
- Version control system for script downloads
- Workflow reorganization into separate directories
- Support for multiple programming languages and frameworks
- Automatic version detection from workflow tags
- Generic, language-agnostic design
- Single version management system

### Changed
- Simplified version management by removing redundant parameters
- Reorganized workflows into `.github/workflows/` (local) and `workflows/` (reusable)
- Updated all documentation to reflect new structure
- Improved user experience with cleaner examples
- Auto-derive script version from workflow tag
- Eliminated need for separate version parameters

### Fixed
- Removed hardcoded version references in favor of dynamic detection
- Eliminated version duplication across files
- Streamlined configuration for easier adoption
- Version sync issues between workflow and script
- Redundant QUICK_START.md file

### Technical Details
- **Workflows**: 4 total (2 local, 2 reusable)
- **Scripts**: 1 Python script for changelog automation
- **Documentation**: README, examples
- **Version Control**: Single version per workflow (auto-derived)
- **Architecture**: Modular design with clear separation of concerns

### Features
- **Automated Releases**: Create GitHub releases with customizable build processes
- **Changelog Management**: Automatic updates after releases with PR creation
- **Cross-Repository**: Reusable workflows that can be referenced from any repo
- **Single Version Control**: One version per workflow, automatically synced
- **Build Extensibility**: Clear extension points for any language or framework
- **Documentation**: Complete setup guides and examples for immediate use

