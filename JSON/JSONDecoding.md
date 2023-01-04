# JSON Decoding 

``` Swift
struct Person: Codable {
    let name: String
    let age: Int
    let occupation: String
}


let jsonData = """
{
    "name": "Brendon Crowe",
    "occupation": "Student",
    "age": 27
}
""".data(using: .utf8)!



// Person is the object that is being decoded/parsed to
// Person.self implies that the JSON root level object is a dictionary
// [Person.self] implies that the JSON root level object is an array
// try must always be used when decoding data
do {
    let person = try JSONDecoder().decode(Person.self, from: jsonData)
    print("Person's name is \(person.name) and occupation is \(person.occupation).")
} catch {
    print("decoding error: \(error)")
}
```
