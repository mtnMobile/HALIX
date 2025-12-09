# Changelog

All notable changes to this project will be documented in this file.

## [2025.12.09.1] — Major HALIX Structural Rewrite

### Changed
- Completely reorganized HALIX into a hierarchical, modern structure separating Swift, SwiftUI, UIKit/Obj-C, navigation, concurrency, data flow, and tooling.
- Fully rewrote the previous AGENTS.md content into the new HALIX format with clearer sectioning, consistent tone, and improved readability.
- Consolidated duplicated rules and modernized all examples to align with Swift 6.2 and iOS 26 patterns.
- Strengthened guidance for SwiftUI layout, state management, navigation patterns, and concurrency best practices.
- Rewrote UIKit/Objective-C interop rules to clarify when `@objc` and selectors are permitted.
- Updated wording across the entire spec to match the new HALIX mental model.
- Strengthened the Networking section to align with HALIX concurrency expectations and prevent common LLM‑generated threading mistakes.

### Added
- Added new HALIX Networking rules clarifying that:
  - Networking work must never originate from `@MainActor`.
  - `URLSession` async APIs do **not** return on the main actor and require explicit hops back to `MainActor` for UI updates.
  - All networking functions must be `async` and must **not** be marked `@MainActor`.

### Removed
- Old AGENTS.md fragments that no longer match the HALIX spec.
- Redundant or outdated rule wording left over from earlier drafts.

## [2025.12.09] — Initial HALIX Specification

### Added
- Introduced a project-wide `CHANGELOG.md` for tracking updates to the HALIX specification.
- Created the initial `HALIX.md` ruleset to guide LLMs in generating modern, idiomatic Swift and SwiftUI code.
- Documented high-level Swift and SwiftUI guidance, including layout, concurrency, state management, and architecture expectations.
- Added initial modern API usage requirements and guidelines for avoiding deprecated or legacy patterns.
- Added guidance for NavigationStack usage, typed navigation models, and Swift Concurrency best practices as part of the initial spec.

### Changed
- Migrated and rewrote previous `AGENTS.md`-style guidance into the new `HALIX.md` format with tighter language and clearer structure.
- Reorganized rules into coherent sections (Swift, SwiftUI, Deprecated APIs, Navigation, State Management, Concurrency, Testing, etc.) for easier scanning by both humans and LLMs.
- Consolidated redundant rules and polished wording for clarity and readability.
- Updated terminology and examples to align with current Swift and iOS platform conventions.
- Strengthened guidance on avoiding UIKit in new SwiftUI-focused code unless explicitly required for interoperability.

### Removed
- Legacy or fragmented entries that were inconsistent with the new HALIX mental model.
- Outdated Swift and SwiftUI examples that no longer reflect modern usage.
- Redundant sections that complicated onboarding or introduced ambiguity into the ruleset.

---

This changelog follows the spirit of Keep a Changelog (https://keepachangelog.com/en/1.1.0/), adapted for an LLM-focused engineering standards document.

## Planned / Upcoming
- Add example code snippets (before/after) for key Swift and SwiftUI rules in `HALIX.md`.
- Introduce optional SwiftLint (or similar) rule definitions aligned with the documented standards.
- Break `HALIX.md` into modular documents under a `/docs` directory (Swift.md, SwiftUI.md, Concurrency.md, Navigation.md, etc.) once the spec stabilizes.
- Expand Navigation, Concurrency, and Testing sections with concrete patterns and sample implementations.
- Continue tightening voicing and terminology for even clearer guidance to both agents and developers.