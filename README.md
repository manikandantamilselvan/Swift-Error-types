# Swift-Error-types
 Swift provides robust error handling using types that conform to the Error protocol. Errors in Swift are typically enumerations or custom types that represent different categories of errors. Here’s a breakdown of error types in Swift:

### 1. Error Protocol
- Any type that conforms to the `Error` protocol can be used to represent an error.
- Commonly, an enumeration (`enum`) is used to define specific error cases.
```
enum FileError: Error {
    case fileNotFound
    case insufficientPermissions
    case unknownError
}
```
### 2. Predefined Error Types
Swift includes built-in error types that are frequently used:

#### NSError
- A class from the Foundation framework that provides detailed error information.
- Used extensively in Objective-C and interoperable with Swift.
```
let nsError = NSError(domain: "com.example.error", code: 404, userInfo: nil)
```
#### DecodingError and EncodingError
- Used for JSON or other data decoding/encoding operations.
```
do {
    let decoded = try JSONDecoder().decode([String: String].self, from: Data())

} catch let error as DecodingError {
    print("Decoding failed: \(error)")
}
```
#### URLError
- Represents errors encountered when using URL-based APIs.
```
let urlError = URLError(.notConnectedToInternet)
```
#### CocoaError
- Represents a variety of errors from Cocoa frameworks.
```
let cocoaError = CocoaError(.fileReadNoSuchFile)
```
### 3. Custom Error Types
You can define your own error types to model domain-specific errors. Use `enum` to group related error cases and provide associated values if needed.
```
enum NetworkError: Error {
    case badURL
    case requestFailed(error: Error)
    case invalidResponse(statusCode: Int)
}
```
### 4. Throwing and Handling Errors
- **Throwing Errors:** Functions can throw errors using the `throw` keyword.
```
func fetchData(from url: String) throws -> Data {
    guard let url = URL(string: url) else {
        throw NetworkError.badURL
    }
    // Fetch data logic...
}
```
- **Handling Errors:** Use do-catch blocks to handle errors.
```
do {
    let data = try fetchData(from: "https://example.com")
    print("Data fetched: \(data)")
} catch NetworkError.badURL {
    print("Invalid URL.")
} catch {
    print("An unexpected error occurred: \(error).")
}
```
### 5. Result Type
- Errors can also be modeled using the `Result` type, which encapsulates success and failure states.
```
func fetchDataResult(from url: String) -> Result<Data, NetworkError> {
    guard let url = URL(string: url) else {
        return .failure(.badURL)
    }
    // Fetch data logic...
    return .success(Data())
}

switch fetchDataResult(from: "https://example.com") {
case .success(let data):
    print("Data fetched: \(data)")
case .failure(let error):
    print("Error: \(error)")
}
```
### 6. LocalizedError and Custom Messages
Use LocalizedError for providing user-friendly error descriptions.
```
enum UserError: LocalizedError {
    case invalidCredentials
    case accountLocked

    var errorDescription: String? {
        switch self {
        case .invalidCredentials:
            return "The username or password you entered is incorrect."
        case .accountLocked:
            return "Your account has been locked due to too many login attempts."
        }
    }
}
```
