# NeuraCraft: Changelog

All notable changes to the NeuraCraft project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and we adhere to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- Preview of plugin marketplace integration in CLI
- New `neura debug` command for pipeline inspection

### Changed
- Improved error messages during pipeline validation
- Optimized memory usage for large context operations  

## [1.0.0] - 2024-05-15
### Added
- Official plugin SDK with TypeScript support
- Core pipeline execution engine v1
- CLI interface with autocomplete
- Plugin hot-reloading support
- Telemetry for performance monitoring

### Changed
- Complete documentation restructuring
- Plugin manifest validation system overhaul
- Default prompt throttling parameters  
- Improved Linux ARM64 compatibility  

### Fixed
- Memory leak in plugin sandboxing
- Race condition in parallel node execution  
- Incorrect type inference for CSV/JSON auto-parsing  
- Pipeline abort signal propagation  

## [0.9.0] - 2024-04-20
### Added
- Interactive pipeline builder (beta)
- GitHub Actions integration template
- Cloud sync capabilities
- Basic NLP toolkit plugin  

### Changed
- Migrated configuration to YAML format
- Redesigned plugin dependency resolver  
- Enhanced security model for third-party plugins  

## [0.8.1] - 2024-03-12
### Fixed
- Critical vulnerability in sandboxed execution
- Windows path resolution regression  
- Plugin installation path validation  

### Removed
- Deprecated `--legacy-authentication` flag  

---

For release comparison links, see our [GitHub Releases](https://github.com/neura-craft/releases) page.

[Unreleased]: https://github.com/org/neura-craft/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/org/neura-craft/compare/v0.9.0...v1.0.0
[0.9.0]: https://github.com/org/neura-craft/compare/v0.8.1...v0.9.0
[0.8.1]: https://github.com/org/neura-craft/releases/tag/v0.8.1