# Swift Style Guide

Welcome to the official [Frame.io](https://www.frame.io) Swift Style Guide.

## Motivation

We created this style guide to document how we write Swift code at [Frame.io](https://www.frame.io) and to streamline the code review process. All rules are numbered so they can be linked directly in code review comments. The guide will continue to evolve with the Swift language and our team.

Apple has published a document on [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) and there are several Swift style guides already, so why write our own? While Apple's guidelines offer a great starting point and plenty of useful examples, some choices are appropriately omitted or left open to interpretation by engineering teams. Our primary goal is to ensure quality, organization, and consistency in our Swift codebases, facilitate discussions about Swift at the company, and to improve our code review process. This goal requires detailed rules on formatting and decisions on conventions that may have multiple acceptable approaches. Other style guides from the community were great resources for us, but we ultimately decided to create our own structure and ideas with the specific goal of code reviews and communication in mind.

## Table Of Contents

- [1. Organization](#1-organization)
    - [1.1 Comments](#11-comments)
    - [1.2 Unnecessary Code](#12-unnecessary-code)
    - [1.3 Files and Sections](#13-files-and-sections)
    - [1.4 Constants and Localization](#14-constants-and-localization)
- [2. Formatting](#2-formatting)
    - [2.1 Indentation and Line Breaks](#21-indentation-and-line-breaks)
    - [2.2 Whitespace, Brackets, Parentheses, and Punctuation](#22-whitespace-brackets-parentheses-and-punctuation)
- [3. Naming](#3-naming)
    - [3.1 Casing and Namespaces](#31-casing-and-namespaces)
    - [3.2 Clarity and Semantics](#32-clarity-and-semantics)
    - [3.3 Code Reviews, Articles, and Documentation](#33-code-reviews-articles-and-documentation)
- [4. Style](#4-style)
    - [4.1 Model and Patterns](#41-model-and-patterns)
    - [4.2 Safety](#42-safety)
    - [4.3 Control Flow](#43-control-flow)
    - [4.4 Access Control](#44-access-control)

## 1. Organization

### 1.1 Comments

* **1.1.1** Remove extraneous comments at the top of a source file. Include only applicable legal rights, licenses, and attribution. Git blame can be used to determine when code was written and who wrote it.
* **1.1.2** Avoid explanatory comments where redundant. Code should be self-documenting through carefully considered structure, descriptive names, and good organization. If code is especially unintuitive (e.g. using specific domain knowledge, or a complex algorithm) comments should be used per-function or per-class.
* **1.1.3** Ensure documentation comments follow the guidelines found in Apple's [Markup Formatting Reference](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/).

### 1.2 Unnecessary Code

* **1.2.1** Keep only the code required for an application. Remove any automatically generated placeholders and comments or code that is no longer used. Save any useful code snippets elsewhere (e.g. gists, playgrounds, separate repos, etc.).

* **1.2.2** Remove unnecessary module imports (e.g. if only Foundation is required, remove UIKit or Cocoa)

* **1.2.3** Use `self` only where it is needed (e.g. to disambiguate properties from arguments, @escaping closures). As a general rule, if the code compiles without using `self`, omit it.

* **1.2.4** Use type inference and shorthand where possible. At a certain scale, slow compilation times are an acceptable reason to add type information.

```swift
let message = "Hello"
let cats = ["tiger", "leopard", "cheetah", "lion"]
view.backgroundColor = .blue
cancelButton.setTitleColor(.white, for: .normal)
```

* **1.2.5** Omit the `get` keyword for read-only computed properties.

```swift
var welcomeMessage: String {
    if let name = name, !name.isEmpty {
        return String.localizedStringWithFormat("Hello, %@", name)
    }
    return NSLocalizedString("Hello, guest!", comment: "")
}
```

* **1.2.6** Omit the `return` keyword for one-line computed properties.
```swift
var size: CGSize { CGSize(width: width, height: height) }
```

* **1.2.7** Avoid full generics syntax when shorthand is available (`[String]`, NOT `Array<String>`).

* **1.2.8** Use trailing closure syntax when there is a single closure parameter at the end of the argument list. Do not use trailing closure syntax otherwise.

```swift
UIView.animate(withDuration: 1.0) {
    self.imageView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
  self.imageView.alpha = 0
}, completion: { finished in
  self.imageView.removeFromSuperview()
})
```

### 1.3 Files and Sections

* **1.3.1** Use extensions for protocol conformance.

```swift
//MARK: - SomeProtocol
extension SomeClass: SomeProtocol {

    /* ... */

}
```

* **1.3.2** Use "// MARK: -" comments to organize code into logical blocks of functionality, including for extensions.

* **1.3.3** Extract types and extensions into separate files where appropriate to ensure files are small and easy to navigate. Multiple small, related types and extensions may be grouped together where it makes sense.

* **1.3.4** Add fileprivate extensions when extended logic is only useful within the context of the current file.

* **1.3.5** Organize files into logical groups, and ensure the file system mirrors the Xcode project navigator.

* **1.3.6** Embed types in containing types when they make sense or are used only within the context of another type.

```swift
class MyView: UIView {

    struct ViewModel { }

}
```

### 1.4 Constants and Localization

* **1.4.1** Use case-less enumerations to define constants. Case-less enumerations are preferred over structs since they cannot be instantiated accidentally and can therefore serve as pure namespaces.

* **1.4.2** Ensure all user-facing text is wrapped in a localized string.

* **1.4.3** Use base language user-facing strings as the keys for localized strings ("Save", NOT "com.identifier.save-action"). Disambiguate localized strings with the same key using comments.

* **1.4.4** Use US English spelling for non-user-facing text in order to match Apple's APIs (e.g. "color", NOT "colour").

## 2. Formatting

### 2.1 Indentation and Line Breaks

* **2.1.1** Use 4 spaces for indentation. (Xcode > Preferences > Text Editing > Indentation)

* **2.1.2** Keep lines under 150 characters (Xcode > Preferences > Text Editing > Editing > Page guide at column)

* **2.1.3** Ensure files end with a newline.

* **2.1.4** Add an extra line break after the opening brace and before the closing brace for type and extension declarations.

```swift
extension SomeClass: SomeProtocol {

    /* ... */

}
```

* **2.1.5** Place opening braces on the same line.

```swift
class SomeClass {

    func someMethod() {
        if someBool {
            /* ... */
        }
    }

}
```

* **2.1.6** Use line breaks for closures, method parameters, collections, etc. when doing so improves readability.

```swift
let airports: [String: String] = [
    "YYZ": "Toronto Pearson",
    "DUB": "Dublin",
    "LHR": "London Heathrow"
]
```

* **2.1.7** Write multiple code statements on separate lines. Separating multiple statements on a single line using semicolons is allowed in Swift but should be avoided.


* **2.1.8** In function or type declarations, if there are more than 2 arguments or the column width rule is
  violated, use line breaks for each argument to improve readability.
```swift
func someMethod(
    authorName: String,
    project: Project,
    comment: Comment
) {
  /* ... */
}

func invokeSomeMethod() {
    /* ... */
    someMethod(
      authorName: authorName,
      project: project,
      comment: comment
    )
}
```

## 2.2 Whitespace, Brackets, Parentheses, and Punctuation

* **2.2.1** Add one space after a colon. Do not add a space before the colon. Exceptions to this rule include the ternary operator `? :`, an empty dictionary `[:]`, and `#selector` syntax `(_:)`.

```swift
class Bicycle: Vehicle {
    /* ... */
}
```

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
airports = [:]
```

* **2.2.2** Add one space after a comma. Do not add a space before a comma.

```swift
let cats = ["tiger", "leopard", "cheetah", "lion"]
```

* **2.2.3** Do not add a space after or before opening and closing square brackets [], angle brackets <>, or parentheses (). For square brackets and parentheses, using a line break when the body is large is encouraged to improve readability (see 2.1.6).

* **2.2.4** Add one space after and one space before opening and closing curly braces, except when using line breaks to improve readability (see 2.1.6).

```swift
names.map { $0.initials }
```

* **2.2.5** Write conditional statements without parentheses.

* **2.2.6** Use only required parentheses when specifying closure types (e.g. optional closure types, closures contained within other closures).

* **2.2.7** Avoid using a semicolon at the end of a line.

* **2.2.8** When using function chaining, use multiple lines to separate complex groups of functions

```swift
let validFileCount = folderItems
    .compactMap { $0 as? File }
    .filter { $0.isValid }
    .count
```

* **2.2.9** When using binary and ternary operators, add a space before and after the operator.

```swift
let aspectRatio = width / height
```

* **2.2.10** Do not leave additional whitespace after any line, including separating newlines. (Xcode > Preferences > Text Editing > Editing > While Editing: > Automatically trim trailing whitespace. Including whitespace-only lines)

## 3. Naming

### 3.1 Casing and Namespaces

* **3.1.1** Avoid prefixes in type names (`Project`, NOT `FIOProject`).

* **3.1.2** Use `UpperCamelCase` for type names (e.g. `struct`, `enum`, `class`, `typedef`, `associatedtype`, generic type parameters, etc.). Use `camelCase` for everything else (functions, methods, properties, constants, variables, arguments, enum cases).

* **3.1.3** Use all caps for acronyms and initialisms, except when the name begins with an acronym or initialism and should use camelCase, in which case the acronym or initialism should be all lowercase.

```swift
let thumbnailURL = ...
let urlString = ...
```

* **3.1.4** Use `strongSelf` when unwrapping `[weak self]` in closures. This clarifies that you've used the safely unwrapped version, as opposed to assigning to `self`, which obscures the weak to strong relationship.

### 3.2 Clarity and Semantics

* **3.2.1** Avoid abbreviations and shorthand (e.g. `index` NOT `idx`)

* **3.2.2** Include type names (or other clarifying nouns) in constant or variable names when it is necessary to avoid ambiguity at the point of use.

```swift
let userCollectionViewCell: UICollectionViewCell
let emailTextField: NSTextField
let name: String
let errorCode: Int
```

* **3.2.3** Use nouns for protocols that describe what something is (e.g. UITableViewDataSource). Use the suffixes `able`, `ible`, or `ing` for protocols that describe a capability (e.g. `Equatable`, `ProgressReporting`).

* **3.2.4** Use descriptive names for generic type parameters where there is a meaningful relationship to the containing type. Otherwise, use a single uppercase letter.

```swift
struct Stack<Element> { ... }
func swap<T>(_ a: inout T, _ b: inout T)
```

* **3.2.5** Use assertive names for Boolean methods and properties when the use is non-mutating (e.g. `isEmpty`, `canSendMessage`, `intersects`, `shouldFade`).

* **3.2.6** Use only the language in method, function, and parameter names required to convey meaning at the point of use. This keeps our funtions descriptive when using auto-complete, and keeps our alignment further left for readability.
        ✅ `func doAction(withItem item: Item)`
        ❌ `func doAction(with item: Item)`
        ❌ `func doActionWithItem(_ item: Item)`

```swift
class ProjectTableViewCell: UITableViewCell {

    func configure(withProject project: Project) {
        /* ... */
    }

}

extension List {
    public mutating func remove(atPosition position: Index) -> Element
}

func presentAlert(withTitle title: String, message: String)
```

* **3.2.7** Use the same name for the optional and unwrapped value when simply unwrapping optionals. When doing more advanced optional binding, different names may be used.

```swift
guard let message = message else { return }

if let movie = item as? Movie {
    /* ... */
}
```

* **3.2.9** Be consistent with naming for `init` and `configure` function parameters. Pass in static context first, then variable properties (with `initial<propertyName>` naming), and then dependencies.
```swift
func configure(withContext context: Context,
               initialState: State,
               endpointProvider: EndpointProviding) { ... }
```

### 3.3 Code Reviews, Articles, and Documentation

* **3.3.1** Use the following syntax for method names when referring to them in prose: `addTarget`, `addTarget(_:action:)`, `addTarget(_: Any?, action: Selector?)`. Use the simplest form possible that avoids ambiguity.

## 4. Style

### 4.1 Model and Patterns

* **4.1.1** Use variables (var) only when a value can change. Use constants (let) in all other cases.

* **4.1.2** Use classes only when things have identity or should use reference semantics. Use structs in all other cases.

* **4.1.3** Use computed properties instead of methods for simple transforming fetches (i.e. no parameters, returns an object or value, complexity is ≤ O(1)).

* **4.1.4** Include the delegate source as the first, unnamed parameter in a delegate method, using the same syntax as Apple's delegate functions:

```swift
func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell
```

* **4.1.5** Use optionals whenever a nil value is possible. Avoid default "empty" values (e.g. "", 0, etc.) when an optional more accurately models the situation.

* **4.1.6** Use failable or throwing initializers whenever required properties cannot be guaranteed to have meaningful values. Similarly, methods that return a value should return an optional or throw when there is no meaningful default.

```swift
class User {

    let id: String
    let name: String

    init?(responseObject: [String: Any] {
        guard let id = responseObject[ServerKey.id] as? String else { return nil }
        self.id = id
        self.name = responseObject[ServerKey.name] as? String ?? NSLocalizedString("Guest", comment: "")
    }

}
```

* **4.1.7** Use the `final` keyword when a class, method, property, or subscript is not being overridden (application) or should never be overridden (framework). Using the `final` keyword has value both in modeling inheritance constraints and in improving compile times.

* **4.1.8** When setting delegate properties, use the property setting directly. Avoid passing the delegate in an initializer or configuration method.

```swift
final class SomeManager {

    weak var delegate: SomeManagerDelegate?

}
```

```swift
let manager = SomeManager()
manager.delegate = self
```

### 4.2 Safety

* **4.2.1** Use implicitly unwrapped optionals and forced unwrapping only when a property, constant, or variable is guaranteed to be non-nil (e.g. IBOutlets, casting a table or collection view cell to a custom subclass). Conditional unwrapping (`if let`, `guard let`) is always preferred.

* **4.2.2** Unwrap optionals only when it is necessary to do so. If all that is required is to check for the existence of a value but the value itself is not needed, avoid unwrapping the optional (e.g. avoid `if let _ = sessionToken { logInSilently() }`)

### 4.3 Control Flow

* **4.3.1** Use `guard let` statements to return early from a method when possible.

* **4.3.2** Use a single `guard` statement to unwrap multiple optionals or check multiple conditions with the same exit path.

```swift
guard let message = message,
    let recipients = message.recipients,
    !recipients.isEmpty
    else { return }
```

* **4.3.3** Use a single line break after multi-line `guard` statements
```swift
guard let asset = asset,
    let file = asset.file
    else { return }

print(file.displayName)
```

* **4.3.4** Use a switch statement (instead of `guard let` or `if let`) where possible when there are multiple conditions and order does not matter.

```switch
switch segue.destination {
case let messagesViewController as MessagesViewController:
    /* ... */
case let companyDirectoryViewController as CompanyDirectoryViewController:
    /* ... */
}
```

* **4.3.5** Avoid exiting a method that the caller expects will do something without handling all possible cases. Consider making a method throw or return a value to accomplish this. If a method only needs to do something if a condition is met, make that clear in the method name (e.g. "layoutIfNeeded").

### 4.4 Access Control

* **4.4.1** Place access modifiers first when declaring types, extensions, methods, properties, etc.

* **4.4.2** Avoid specifying `internal` since it is the default. The one exception is when the getter and setter have different access levels.

```swift
public internal(set) var userName: String
```

* **4.4.3** Prefer lower access control levels. In frameworks, only the public-facing interface should be marked `public` or `open`. Where possible, `private` should be used instead of `fileprivate`. However, proper use of extensions for good code organization is more important than the preference for `private` over `fileprivate`.

## References

* [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)
* [LinkedIn Swift Style Guide](https://github.com/linkedin/swift-style-guide)
* [GitHub Swift Style Guide](https://github.com/github/swift-style-guide)

