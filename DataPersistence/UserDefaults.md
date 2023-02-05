# User Defaults
The simplest mathod of saving, persisteing data in an iOS app, is using User Defaults.
Apple's definition for User Defaults is... </br> <b>"An interface to the user’s defaults database, where you store key-value pairs persistently across launches of your app."</b>



## Code example 
The below code will be using examples used to save a user's name

### Set up keys for User Defaults
Because the keys for the User Defaults dictionary are strings, it's best and helpful to write a struct or an enum storing the key values. This way, when fetching the data, no error of typing the wrong key value is present 

``` Swift
struct UserInfoKeys {
    static let userName = "Name"
    static let userSign = "Sign"
}
```

### To better access User Defaults, it's help to create a singleton class that can be used throughout the app
``` Swift
class UserInfo {
    static let shared = UserInfo()
    private init() {}
}
```
### Code will be needed in order to write the user's name as well as read the user's name from User Defaults
