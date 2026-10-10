# iOS Learning Notes

Personal notes, articles, Q&As, and references for learning and revisiting iOS and Swift development.

## Resources

- [Apple Developer](https://developer.apple.com/) — Official Apple documentation, guides, API references, videos, and resources for building apps across Apple platforms.
- [App Store Connect](https://appstoreconnect.apple.com/apps) — Manage apps, builds, TestFlight, App Store listings, submissions, analytics, and related distribution settings.

## Testing

- [Modern Unit Testing with Swift Testing](https://www.avanderlee.com/swift-testing/modern-unit-test/) — An introduction to Apple's Swift Testing framework, including `@Test`, `#expect`, `#require`, parameterized tests, and how it compares with XCTest.

## Libraries

- [Nimble](https://github.com/Quick/Nimble) — A matcher framework for Swift and Objective-C that provides expressive, readable assertions and can be used with Swift Testing or XCTest.

## Q&A

- [What does `nonisolated` mean on a struct?](#what-does-nonisolated-mean-on-a-struct)

### What does `nonisolated` mean on a struct?

It opts the type out of actor isolation. With Swift 6.2 default actor isolation (`.defaultIsolation(MainActor.self)` or the *Default Actor Isolation* build setting), every type is implicitly `@MainActor`, and with `InferIsolatedConformances` so are its conformances (`Hashable`, `Decodable`, …). A `nonisolated struct` is a plain value type again, usable from any thread. Use it for models, DTOs and pure helpers that cross actors; keep views and view models on the main actor. It doesn't make a type `Sendable` by itself. See [Default Actor Isolation in Swift 6.2](https://www.avanderlee.com/concurrency/default-actor-isolation-in-swift-6-2/) and [SE-0466](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0466-control-default-actor-isolation.md).
