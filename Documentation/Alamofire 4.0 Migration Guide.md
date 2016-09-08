# Alamofire 4.0 Migration Guide

Alamofire 4.0 is the latest major release of Alamofire, an HTTP networking library for iOS, tvOS, macOS and watchOS written in Swift. As a major release, following Semantic Versioning conventions, 4.0 introduces several API-breaking changes.

This guide is provided in order to ease the transition of existing applications using Alamofire 3.x to the latest APIs, as well as explain the design and structure of new and changed functionality.

## Requirements

Alamofire 4 officially supports iOS 9+, tvOS 9+, macOS 10.11+, and watchOS 2+ and uses Swift 3. If you’d like to use Alamofire in a project targeting iOS 8, macOS 10.9, Swift 2.2 or 2.3, use the latest tagged 3.x release.

## Benefits of Upgrading

* Full Swift 3 compatibility. This includes full adoption of the Swift 3 naming guidelines, so most function signatures have changed since 3.x.
* A completely new error model adopting the [latest changes in Swift 3](https://github.com/apple/swift-evolution/blob/master/proposals/0112-nserror-bridging.md). The new `AFError` type centralizes all errors returned by the Alamofire framework while undelrying system errors are passed through as is.
* `RequestAdapter` protocol: inspect and adapt every request made on a `SessionManager`. This allows easy modification of all requests for things like authorization or other common headers.
* `RequestRetrier`protocol: automatically retry requests when an error is encountered.
* Clearer `Request` types. Given how differen the underlying `URLSession` types are in their behavior and API, the `Request` type from Alamofire 3.x has been subclassed for each type of request, creating `DataRequest`, `DownloadRequest`, `UploadRequest`, and `StreamRequest`.
* With the new `*Request` types come type specific serialization, validation, and progress functionality.
* Validation has been enhanced to allow full access to the response data and in the case of a `DownloadRequest`, access to the `destinationURL` so that validators can check the state of any files at that location before allowing serialization to occur.
* A full refactor of the destination APIs for `DownloadRequests`. This allows much easier and more customizable use of Alamofire's `download` functionality.
* `ParameterEncoding` protocol: parameter encodings are no longer limited to the set built into Alamofire. Instead, this new protocol allows more flexible and powerful encoder which can be provided from outside the Alamofire framework. This protocol also allows for safer parameter encoding by throwing errors rather than returning them as part of a tuple.

## Breaking API Changes

Alamofire 4.0 has fully adopted Swift 3's changes, including the new [naming guidelines](https://swift.org/documentation/api-design-guidelines/) and Foundation overlay. This means that most function signatures have changed in some way. 

### request()

The various request related APIs have changed. 