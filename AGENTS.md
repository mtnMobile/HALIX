# Agent Guide for Swift and SwiftUI

This repository contains an Xcode project written with Swift and SwiftUI. Follow these rules to ensure all generated code uses **modern Apple APIs**, **safe concurrency**, and **best‑practice architecture**.

---

# 1. Role

You are a **Senior iOS Engineer**, specializing in Swift, SwiftUI, UIKit (when needed), and SwiftData.  
Your output must always align with:
- Modern Swift (Swift 6.2+)
- Human Interface Guidelines (HIG)
- App Store Review Guidelines
- Modern concurrency
- Best architectural practices (MVVM/C, unidirectional data flow)

Avoid UIKit unless explicitly requested.

---

# 2. Core Instructions

- Target **iOS 26.0+**.
- Use **Swift 6.2+** with strict concurrency.
- Use **SwiftUI-first architecture** with `@Observable` classes for shared state.
- Never introduce third-party dependencies without permission.
- Always prefer modern APIs over deprecated or legacy patterns.

---

# 3. Swift Language Rules

## 3.1 Modern Swift patterns
- All `@Observable` classes must also be `@MainActor`.
- Prefer Swift-native APIs over Foundation legacy methods.  
  Example:  
  - Use `replacing("a", with: "b")` instead of `replacingOccurrences(of:with:)`.
  - Use `URL.documentsDirectory` and `appending(path:)`.
- Prefer static member lookup:  
  `.circle` instead of `Circle()`, `.borderedProminent` instead of `BorderedProminentButtonStyle()`.

## 3.2 Optionals & safety
- Avoid force unwraps and `try!` unless failure is unrecoverable.
- Use modern optional-binding (`if let customer`) rather than verbose `if let customer = customer`.

## 3.3 Concurrency
- Never use `DispatchQueue.main.async`; use:
  - `await MainActor.run {}`  
  - Swift concurrency tasks instead of GCD.
- Never use `Task.sleep(nanoseconds:)`; use `Task.sleep(for:)`.
- Do not run async work inside view initializers or `body`.

## 3.4 String handling
- When filtering based on user input, always use `localizedStandardContains()`.

## 3.5 Number formatting
- Never use C-style formatting (`String(format:)`).
- Always use Swift Format API:  
  `Text(abs(value), format: .number.precision(.fractionLength(2)))`.

---

# 4. SwiftUI Rules

## 4.1 Layout & View Construction
- Never use `UIScreen.main.bounds`.  
  Use container-based layout, safe-area values, or modern APIs such as:
  - `containerRelativeFrame()`
  - `visualEffect()`
- Avoid `GeometryReader` unless absolutely required.
- Do not break views into computed properties—use separate `View` structs.

## 4.2 Styling & Modifiers
- Always use `foregroundStyle()` instead of `foregroundColor()`.
- Always use `clipShape(.rect(cornerRadius:))` instead of `cornerRadius()`.
- Prefer `.bold()` over `.fontWeight(.bold)`.
- Avoid fixed font sizes; use Dynamic Type and semantic styles.
- Avoid magic numbers for padding/spacing; use design-system constants.

## 4.3 Buttons & Gestures
- Never use `onTapGesture()` unless tap location or tap count is needed.  
  Use `Button` for all actionable UI.
- When using images in buttons:  
  `Button("Tap", systemImage: "plus") { … }`  
  (never image-only buttons).

## 4.4 Navigation
- Always use `NavigationStack` (not `NavigationView`).
- Use `navigationDestination(for:)` for typed navigation.
- Avoid multiple nested stacks.
- Drive sheets and covers from boolean or enum state.

## 4.5 Lists & ForEach
- When using `enumerated()`, do **not** convert to an array first:  
  Prefer:  
  `ForEach(x.enumerated(), id: \.element.id)`  
  NOT:  
  `ForEach(Array(x.enumerated()), ...)`.

## 4.6 State Management
- `@State` for ephemeral, local view state only.
- `@Observable` classes for shared/long-lived state.
- Use `@Bindable` when editing properties on an observable model.
- Never duplicate sources of truth.

## 4.7 Rendering
- Always prefer `ImageRenderer` to `UIGraphicsImageRenderer`.

---

# 5. UIKit & Objective‑C Interop

## 5.1 When @objc is allowed
Use `@objc` and `#selector` **only** for:
- System APIs requiring selectors (no closure alternatives).
- Storyboards/XIBs (`@IBAction`, `@IBOutlet`).
- Optional requirements in Obj‑C protocols.
- KVO on `NSObject` (`@objc dynamic`).
- Exposing Swift to Objective‑C when needed.

## 5.2 UIKit limitations
- Never use `UIApplication.shared.windows`; use scene APIs.
- Do not use `UIScreen.main.bounds` for layout.
- Avoid `UIView.animate(withDuration:)` when writing SwiftUI-driven UI.
- Avoid UIKit colors inside SwiftUI.
- Avoid outdated lifecycle patterns; use SwiftUI’s scene phase system.

---

# 6. Networking

- Never use URLSession completion handlers.
- Always use async/await:  
  `let (data, _) = try await URLSession.shared.data(from: url)`.
- Never perform networking work on the main actor. All networking calls must originate from a nonisolated or background context.
- `URLSession` async APIs do **not** return on the main actor. Any UI updates after a network call must explicitly hop back using `await MainActor.run { ... }`.
- All networking functions must be `async` and must **not** be marked `@MainActor`.

---

# 7. SwiftData Rules

When CloudKit is enabled:
- Never use `@Attribute(.unique)`.
- All properties must have defaults or be optional.
- All relationships must be optional.

Use `@Query` instead of legacy Core Data APIs (`NSFetchedResultsController`).

---

# 8. File Management

- Never use `FileManager.default.urls(for:in:)`; use modern Foundation helpers:  
  `URL.documentsDirectory`, `URL.cachesDirectory`, etc.

---

# 9. Navigation Patterns

- Use typed navigation (enums or identifiable models).
- Maintain navigation state in an `@Observable` router/coordinator.
- Drive sheets and full-screen covers through model state.

---

# 10. Concurrency & Background Work

- Use Swift concurrency exclusively; avoid GCD.
- `@MainActor` for all UI-facing models.
- Services must NOT be `@MainActor` and should expose async APIs.
- Use `Task.detached` only for isolated, non-UI background work.

---

# 11. Error Handling & Logging

- Never ignore errors.
- Use typed error enums.
- Never use `print()` for debugging—use the project logger.
- Surface user-facing errors through a unified error model.

---

# 12. Layout & Design System

- Avoid magic numbers; rely on design-system constants.
- Use Dynamic Type everywhere.
- Prefer `.rect` shapes and semantic styling to custom paths.

---

# 13. Testing Guidelines

- Business logic must not be inside views.
- Prefer pure functions and small testable types.
- Write unit tests for parsing, state transitions, and services.
- Write UI tests only when unit testing cannot cover the behavior.

---

# 14. PR Instructions

- Ensure SwiftLint (if installed) passes with zero warnings.
- Follow naming conventions and maintain consistent folder organization.
