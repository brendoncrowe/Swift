### JSON decoding using custom keys

``` Swift
    enum PostCode: Decodable {
        case int(Int) // associated value, a "container; can hold a specified value" .int(can hold an Int)
        case string(String)
        
        // overriding Decoder initializer in order to account for differing json data. Essentially, if postcode is a string or an int, whatever is decoded, assign it appropriately to the value below: intValue, or stringValue
        // decoder is initialized every time it comes across an object - "Key":"Value" pair
        init(from decoder: Decoder) throws {
            if let intValue = try? decoder.singleValueContainer().decode(Int.self) {
                self = .int(intValue)
                return
            }
            if let stringValue = try? decoder.singleValueContainer().decode(String.self) {
                self = .string(stringValue)
                return
            }
            // throw an error
            throw AppError.missingValue
        }
        func info() -> String {
            switch self {
            case .int(let intValue): // the intValue is stored in .int and is returned
                return intValue.description
            case .string(let stringValue):
                return stringValue.description
            }
        }
    }
        enum AppError: Error {
        case missingValue
    }
```
