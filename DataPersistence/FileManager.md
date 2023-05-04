# Persistence: Writing to the Documents Directory 
There is nothing more frustrating than losing something! The same goes for data. If a user of an app inputs data, comes back later, and finds that their data is missing, that wil not make for a good ux...

Within iOS development, there are several ways a developer can add persistence to an app.
1. **UserDefaults** - an interface to the user’s defaults database, where you store key-value pairs persistently across launches of your app.
2. **Documents Directory** - where information related to the app can be stored and modified.
3. **CoreData** - framework that allows saving an application’s permanent data for offline use, to cache temporary data, and to add undo functionality to the app on a single device
4. **iCloud** - a magical cloud that does more than give high-fives!

For something simple such as persisting a collection (i.e an array of flash card decks) of a single object, writing to the documents directory is ideal.

## Steps for writing to the documents directory 
1. The first step in writing to the documents directory is getting the url path to the documents directory 
``` Swift
let documentsDirectory = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0] // or .first!

```
The result of the code above is an array of URL objects, which point to the directories that match the search. But there's only one Documents directory, so you request the first result of the search to assign to the constant.

2. Since the url path to the documents directory has been accessed, the next step involves providing a full path that provides a filename and extension as well, which can be appended.
``` Swift
let fullPath = documentsDirectory.append(path: "fileName.plist") // .appendingPathComponent("fileName").appendingPathExtension("plist") can also be used
```
The console output if printed
``` Swift
/* 
/Users/*user name*/Library/Developer/XCPGDevices/8CB01CF9-BEA3-4862-8051-DEE9F9209625/data/Containers/Data/Application/D60B7E68-6261-439C-9931-88EC05801663/Documents/fileName.plist
```
