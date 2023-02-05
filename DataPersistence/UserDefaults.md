# User Defaults
The simplest mathod of saving, persisteing data in an iOS app, is using User Defaults.
Apple's definition for User Defaults is... </br> <b>"An interface to the user’s defaults database, where you store key-value pairs persistently across launches of your app."</b>



## Code example 
The below code will be using examples used to save a user's name

### Set up keys for User Defaults
Because the keys for the User Defaults dictionary are strings, it's best and helpful to write a struct or an enum storing the key values. This way, when fetching the data, no error of typing the wrong key value is present 

``` Swift
struct UserInfoKeys { // using a struct
    static let userName = "Name"
    static let userSign = "Sign"
}

enum UnitMeasurement: String { // if toggling a setting, you can use an enum with a raw value type
  case miles = "Miles"
  case kilometers = "Kilometers"
}

enum DefaultImage: String {
  case run = "run"
  case bike = "bike"
}
```

### To better access User Defaults, it's help to create a singleton class that can be used throughout the app
``` Swift
class UserInfo {
    static let shared = UserInfo()
    private init() {}
}
```
### Code will be needed in order to write the user's name
A function to set the user's name
``` Swift 
  func setUserName(name: String) {
        UserDefaults.standard.set(name, forKey: UserInfoKeys.userName)
    }
  ```
If there are multiple fucntions being written to write different points of data, you can use a generic function...
``` Swift
 func updateDefaults<T>(with value: T, for key: String) {
    UserDefaults.standard.set(value, forKey: key)
  }
  ```
 
 ### Code to read the user's name
 ``` Swift
     func getUsername() -> String? {
        guard let userName = UserDefaults.standard.object(forKey: UserInfoKeys.userName) as? String else {
            return nil
        }
        return userName
    }
```
OR
``` Swift 
func getDefaultValue<T>(for key: String) -> T? {
    guard let value = UserDefaults.standard.object(forKey: key) as? T else {
      return nil
    }
    return value
  }
  ```
