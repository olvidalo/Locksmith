## Locksmith

A sane way to work with the iOS Keychain in Swift.

## Important
*Note: Due to a bug in Swift, the `Swift Compiler - Code Generation` Optimization Level for release builds has to be set to -Onone. [Go here for more infromation on how to change it.](http://matthewpalmer.net/blog/2014/12/11/change-optimization-level-xcode-swift/)*

This means that Locksmith is likely __not suitable for production apps__ until this bug is fixed. For more information, see [issue 13](https://github.com/matthewpalmer/Locksmith/issues/13).

## Installation
Install the framework ([reference c/o Alamofire](https://github.com/Alamofire/Alamofire))


1. `git submodule add https://github.com/matthewpalmer/Locksmith.git`
2. Open the newly created folder, 'Locksmith', in Finder
3. Drag Locksmith.xcodeproj to the file navigator (left sidebar) of your project
4. Click on your app's target, then click on Build Phases
5. Follow the gif ![Locksmith iOS Keychain Library in Swift](http://i.imgur.com/cwB8tAI.gif)
6. `import Locksmith` wherever you need it

## Quick Start

**Save Data**

```swift
Locksmith.saveData(["some key": "some value"], inService: "myService", forUserAccount: "myUserAccount")
```

**Load Data**

```swift
let (dictionary, error) = Locksmith.loadData(inService: "myService", forUserAccount: "myUserAccount")
```

**Update Data**

```swift
Locksmith.updateData(["some key": "another value"], inService: "myService", forUserAccount: "myUserAccount")
```

**Delete Data**
```swift
Locksmith.deleteData(inService: "myService", forUserAccount: "myUserAccount")
```

## Custom Requests
To create custom keychain requests, you first have to instantiate a `LocksmithRequest`. This request can be customised as much as required. Then call`Locksmith.performRequest` on that request.

**Saving**
```swift
let saveRequest = LocksmithRequest(service: service, userAccount: userAccount, data: ["some key": "some value"])
// Customize the request
saveRequest.synchronizable = true
Locksmith.performRequest(saveRequest)
```

**Reading**
```swift
let readRequest = LocksmithRequest(service: service, userAccount: userAccount)
let (dictionary, error) = Locksmith.performRequest(readRequest)
```

**Deleting**
```swift
let deleteRequest = LocksmithRequest(service: service, userAccount: userAccount, requestType: .Delete)
Locksmith.performRequest(deleteRequest)
```

## LocksmithRequest
Use these attributes to customize your `LocksmithRequest` instance.

If you need any more custom attributes, either create a pull request or open an issue.

**Required**
```swift
var service: String
var userAccount: String
var type: RequestType             // Defaults to .Read
```

**Optional**
```swift
var group: String?                // Used for keychain sharing
var data: NSDictionary?           // Used only for write requests
var matchLimit: MatchLimit        // Defaults to .One
var securityClass: SecurityClass  // Defaults to .GenericPassword
var synchronizable: Bool          // Defaults to false
```
