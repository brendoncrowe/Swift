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
Now that there is a path to a specific file in the documents directory, CRUD mehtods can be performed.
Below is a generic class that contains CRUD functions that can be used
``` Swift
import Foundation

public enum DataPersistenceError: Error {
    case propertyListEncodingError(Error)
    case propertyListDecodingError(Error)
    case writingError(Error)
    case deletingError
    case noContentsAtPath(String)
}

// step 1: custom delegation - defining the protocol
protocol DataPersistenceDelegate: AnyObject { // AnyObject works with class types
    func didDeleteItem<T>(_ persistenceHelper: DataPersistence<T>, item: T)
}

typealias Writeable = Codable & Equatable

// DataPersistence is now type constrained to only work with Codable types
// T is a placeholder for the passed in type
class DataPersistence<T: Writeable> {
    
    private let filename: String
    
    private var items: [T]
    
    // step 2: custom delegation - defining a reference property that will be registered at the object listening for notifications
    weak var delegate: DataPersistenceDelegate? // weak is used in order to break a strong reference cycle between the delegate object and DataPersistence class

    
    public init(filename: String) {
        self.filename = filename
        self.items = []
    }
    
    private func saveItemsToDocumentsDirectory() throws {
        do {
            let url = FileManager.getPath(with: filename, for: .documentsDirectory)
            let data = try PropertyListEncoder().encode(items)
            try data.write(to: url, options: .atomic)
        } catch {
            throw DataPersistenceError.writingError(error)
        }
    }
    
    // Create
    public func createItem(_ item: T) throws {
        _ = try? loadItems()
        items.append(item)
        do {
            try saveItemsToDocumentsDirectory()
        } catch {
            throw DataPersistenceError.writingError(error)
        }
    }
    
    // Read
    public func loadItems() throws -> [T] {
        let path = FileManager.getPath(with: filename, for: .documentsDirectory).path
        if FileManager.default.fileExists(atPath: path) {
            if let data = FileManager.default.contents(atPath: path) {
                do {
                    items = try PropertyListDecoder().decode([T].self, from: data)
                } catch {
                    throw DataPersistenceError.propertyListDecodingError(error)
                }
            }
        }
        return items
    }
    
    // for re-ordering, and keeping date in sync
    public func synchronize(_ items: [T]) {
        self.items = items
        try? saveItemsToDocumentsDirectory()
    }
    
    // Update - 2 options
    // 1. this function needs the index in order to update
    @discardableResult // @discardableResult silences the warning if the return value is not used by the caller
    public func update(_ oldItem: T, with newItem: T) -> Bool {
        
        if let index = items.firstIndex(of: oldItem) { // is oldItem == currentItem in array
            let result = update(newItem, at: index)
            return result
        }
        return false
    }
    
    // 2. this function has the index and can be updated via the index
    @discardableResult // @discardableResult silences the warning if the return value is not used by the caller
    public func update(_ item: T, at index: Int) -> Bool {
        items[index] = item
        // save items to documents directory
        do {
            try saveItemsToDocumentsDirectory()
            return true
        } catch {
            return false
        }
    }
    
    // Delete
    public func deleteItem(at index: Int) throws {
        let deletedItem = items.remove(at: index) // remove() returns the element which can be stored in deletedItem
        do {
            try saveItemsToDocumentsDirectory()
            // step 3: custom delegation - use delegate reference to notify observer of deletion
            delegate?.didDeleteItem(self, item: deletedItem)
        } catch {
            throw DataPersistenceError.deletingError
        }
    }
    
    public func hasItemBeenSaved(_ item: T) -> Bool {
        guard let items = try? loadItems() else {
            return false
        }
        self.items = items
        if let _ = self.items.firstIndex(of: item) {
            return true
        }
        return false
    }
    
    public func removeAll() {
        guard let loadedItems = try? loadItems() else {
            return
        }
        items = loadedItems
        items.removeAll()
        try? saveItemsToDocumentsDirectory()
    }
}
```

NOTE: This class uses an extension on FileManager
``` Swift
import Foundation


public enum Directory {
  case documentsDirectory
  case cachesDirectory
}

extension FileManager {
  // returns a URL to the documents directory for the app
  public static func getDocumentsDirectory() -> URL  {
    return FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
  }
  
  public static func getCachesDirectory() -> URL  {
    return FileManager.default.urls(for: .cachesDirectory, in: .userDomainMask)[0]
  }
  
  // function takes a filename as a parameter, appends to the document directory's URL and returns that path
  // this path will be used to write (save) date or read (retrieve) data
  public static func getPath(with filename: String, for directory: Directory) -> URL {
    switch directory {
    case .cachesDirectory:
      return getCachesDirectory().appendingPathComponent(filename)
    case .documentsDirectory:
      return getDocumentsDirectory().appendingPathComponent(filename)
    }
  }
}
```
