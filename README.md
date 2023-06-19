# EnumPath

> Swift Macro that generates `enum`'s associated value getters, setters or boolean checks.

![Swift Version](https://img.shields.io/badge/swift-5.9-brightgreen)

## Problem

There is no way to create binding from enum's associated value.

```swift
enum Model {
    case none
    case label(String)
}

struct ContentView: View {
    @State var model: Model = .label("hello world")
 
    var body: some View {
        TextField("View Label", text: ...) // no way to create binding label's associated value
    }
}
```

## Solution

Swift Macro `EnumPath` generates getters, setters and boolean checks for all enum cases.

```swift
@EnumPath
enum Model {
    case none
    case label(String)
}
```

Macro `EnumPath` expands into:

```swift
enum Model {
    case none
    case label(String)

    // generated by `EnumPath` macro
    var isNone: Bool {
        get {
            switch self {
            case .none: 
                true
            default: 
                false
            }
        }
        set {
            if newValue {
                self = .none
            }
        }
    }

    var label: String {
        get {
            switch self {
            case let .label(label):
                label
            default:
                nil
            }
        }
        set {
            if let newValue {
                self = .label(newValue)
            }
        }
    }
}
```

After the macro expansion the usage can be:


```swift
@EnumPath
enum Model {
    case none
    case label(String)
}

struct ContentView: View {
    @State var model: Model = .label("hello world")
 
    var body: some View {
        if let label = Binding(model.label) {
            TextField("View Label", text: label)
        }
    }
}
```

## Installation

### Swift Package Manager

```swift
// In your `Package.swift`

dependencies: [
    .package(url: "https://github.com/artbobrov/EnumPath", branch: "main")
],
targets: [
    .target(
        name: ...,
        dependencies: [
            .product(name: "EnumPath", package: "EnumPath"),
            ...
        ]
    ),
    ...
]
```

## Author

* [artbobrov](https://github.com/artbobrov)