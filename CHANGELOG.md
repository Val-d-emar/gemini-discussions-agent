# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-04-21

### Added
- **Collapsible Error Logs:** When the agent fails, it now posts a detailed error report in the discussion thread with technical logs hidden under a `<details>` spoiler.
- **Automated Tool Permissions:** Added automatic creation of `.gemini/settings.json` to grant the agent necessary permissions (e.g., `run_shell_command`) in GitHub Actions environments.
- **Approval Mode:** Enabled `--approval-mode=yolo` for seamless non-interactive execution of AI-suggested commands.
- **Internal Model Extraction:** The logic to parse `@model` tags from comments is now handled internally by the action, simplifying the user's workflow configuration.

### Changed
- **Simplified Workflow Example:** README now features a much cleaner setup guide without the need for manual bash scripts to extract model tags.
- **Improved Reliability:** The `Post Comment` step now uses `if: always()` and `set +e` to ensure feedback is provided even if early stages of the action fail.

### Fixed
- Fixed `Tool not found` and `Unauthorized tool call` errors occurring in recent versions of `gemini-cli`.
- Fixed an issue where the action would fail silently without providing feedback in the discussion thread.

---

## [1.0.0] - 2026-03-15
### Added
- Initial release of Gemini Discussion Agent.
- Support for context-aware responses in GitHub Discussions.
- Multi-language support (English/Russian).
- Integration with `gemini-cli`.

[1.1.0]: https://github.com/Val-d-emar/gemini-discussions-agent/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/Val-d-emar/gemini-discussions-agent/releases/tag/v1.0.0
