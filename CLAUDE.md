# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ConfettiSwiftUI is a pure SwiftUI library for creating customizable confetti cannon animations. It supports iOS 14+, macOS 11+, tvOS 14+, and watchOS 7+.

## Build Commands

```bash
# Build the package
swift build

# Run tests
swift test

# Build for a specific platform
swift build -Xswiftc "-sdk" -Xswiftc "$(xcrun --sdk iphonesimulator --show-sdk-path)" -Xswiftc "-target" -Xswiftc "x86_64-apple-ios14.0-simulator"
```

## Architecture

### Core Components

- **`ConfettiCannon<T: Equatable>`** (`Sources/ConfettiSwiftUI.swift`): The main view component that orchestrates the animation. Uses a generic trigger binding that fires on any value change.

- **`ConfettiConfig`** (`Sources/ConfettiSwiftUI.swift`): ObservableObject that holds all configuration parameters (num, colors, radius, angles, repetitions, etc.) and computed animation durations.

- **`View.confettiCannon()`** (`Sources/View+ConfettiCannon.swift`): View modifier extension providing the primary API for consumers.

### Animation Flow

1. `ConfettiCannon` observes trigger changes via `.onChange(of:)`
2. For each repetition, it schedules a new `ConfettiContainer` with haptic feedback
3. `ConfettiContainer` spawns `num` individual `ConfettiView` instances
4. Each `ConfettiView` animates in two phases:
   - Explosion phase: moves outward within the opening/closing angle bounds
   - Rain phase: falls downward with optional fade out

### Confetti Types

`ConfettiType` enum supports:
- `.shape(.circle | .triangle | .square | .slimRectangle | .roundedCross)`
- `.text(String)` for emoji/text
- `.sfSymbol(symbolName:)` for SF Symbols
- `.image(String)` for asset images

### Custom Shapes

Located in `Sources/Shapes/`:
- `Triangle`, `SlimRectangle`, `RoundedCross` - custom SwiftUI `Shape` implementations

### Platform-Specific Code

Haptic feedback uses `#if canImport(UIKit) && !os(tvOS) && !os(visionOS)` guards for iOS-only `UIImpactFeedbackGenerator`.
