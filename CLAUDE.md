# ConfettiSwiftUI

Pure SwiftUI confetti library. `Package.swift` declares Swift tools 5.3, iOS 14+, macOS 11+, tvOS 14+ and watchOS 7+. Preserve those deployment targets when changing APIs.

## Build and verification

Run `swift build` and `swift test` from the repository. The current XCTest is an API-construction smoke test without assertions; it does not verify animation behavior. For animation or platform changes, exercise the affected behavior in a SwiftUI host on the relevant platform. Select an available simulator and architecture rather than hardcoding an Intel simulator target.

## Architecture

- `Sources/View+ConfettiCannon.swift` exposes the `confettiCannon(trigger:...)` modifier; `ConfettiCannon<T: Equatable>` in `Sources/ConfettiSwiftUI.swift` observes changes to its trigger binding.
- `ConfettiConfig` owns animation parameters and computed durations. Each repetition schedules a `ConfettiContainer`, whose particles animate through explosion and rain phases.
- `ConfettiType` supports built-in shapes, text, SF Symbols and asset images. Custom `Triangle`, `SlimRectangle` and `RoundedCross` shapes live in `Sources/Shapes/`.
- Keep haptic feedback behind the existing `canImport(UIKit) && !os(tvOS) && !os(visionOS)` guard. Preserve the cross-platform library and shared configuration instead of introducing separate platform implementations.
