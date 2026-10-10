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
- [What is `Decodable`?](#what-is-decodable)
- [What is `CaseIterable`?](#what-is-caseiterable)

### What does `nonisolated` mean on a struct?

It opts the type out of actor isolation. With Swift 6.2 default actor isolation (`.defaultIsolation(MainActor.self)` or the *Default Actor Isolation* build setting), every type is implicitly `@MainActor`, and with `InferIsolatedConformances` so are its conformances (`Hashable`, `Decodable`, …). A `nonisolated struct` is a plain value type again, usable from any thread. Use it for models, DTOs and pure helpers that cross actors; keep views and view models on the main actor. It doesn't make a type `Sendable` by itself. See [Default Actor Isolation in Swift 6.2](https://www.avanderlee.com/concurrency/default-actor-isolation-in-swift-6-2/) and [SE-0466](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0466-control-default-actor-isolation.md).

### What is `Decodable`?

A protocol for types that can be created from external data such as JSON, e.g. `try JSONDecoder().decode(Item.self, from: data)`. If every stored property is `Decodable`, the compiler synthesizes the conformance: property names map to keys, optionals decode missing or `null` values as `nil`, and a missing or mistyped required field throws a `DecodingError`. Write a custom `init(from:)` for special cases, such as `try?` to turn one malformed array element into `nil` instead of failing the whole payload. Use `CodingKeys` to rename keys, and decoder strategies like `.convertFromSnakeCase` for key or date formats. `Codable` is `Decodable & Encodable`. Keep `Decodable` on DTOs in the data layer that mirror the JSON, and map them to domain models so the wire format stays an implementation detail. See [Decodable](https://developer.apple.com/documentation/swift/decodable) and [Encoding and Decoding Custom Types](https://developer.apple.com/documentation/foundation/encoding-and-decoding-custom-types).

### What is `CaseIterable`?

A protocol that gives a type a static `allCases` collection with every value. For an enum without associated values, the compiler synthesizes it in declaration order: `enum Appearance: CaseIterable { case system, light, dark }` gets `Appearance.allCases == [.system, .light, .dark]`. Use it to build UI from the cases, e.g. `ForEach(Appearance.allCases)` in a `Picker`, so a new case shows up without view changes, and to loop over every case in tests. Synthesis only works when the conformance is declared in the enum's own file and no case has associated values or `@available` attributes; otherwise implement `allCases` yourself. See [CaseIterable](https://developer.apple.com/documentation/swift/caseiterable).
