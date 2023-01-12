### Extension on Bundle to easily get a local json file and convert it to data

``` Swift 
extension Bundle {
    static func readRawJSONData(filename: String, ext: String) -> Data {
        guard let fileUrl = Bundle.main.url(forResource: filename, withExtension: ext) else {
            fatalError("resource \(filename) not found")
        }
        var data: Data!
        do {
            data = try Data.init(contentsOf: fileUrl)
        } catch {
            fatalError("contents not found \(error)")
        }
        return data
    }
}
```
