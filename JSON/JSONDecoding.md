### Decoding Simple Objects
``` Swift
 import UIKit

// Two ways of decoding JSON in playgrounds...

// 1. At the end of the multi-line string, call the data method on the value
let person1JSON = """
{
    "name": "James",
    "age": 45,
    "gender": "Male",
    "sign": "Sagitarius",
    "partner": "Emily",
    "isEmployed": true
}
""".data(using: .utf8)! // this allows the String unicode to be read by the decoder

let person2JSON = """
{
    "name": "Mary",
    "age": 45,
    "gender": "Female",
    "sign": "Taurus",
    "isEmployed": false
}
"""

struct Person: Codable {
    let name: String
    let age: Int
    let gender: String
//    let sign: String
    let partner: String?
    let isEmployed: Bool
}

// Create the swift object you want to decode to and call the decode method, which parses the JSON data from person1JSON into the swift object person1. A do catch block must be used when implementing try as the function may throw an error
do {
    let person1 = try JSONDecoder().decode(Person.self, from: person1JSON)
//    print("\(person1.name) is currently dating \(person1.partner ?? "nobody").")
} catch {
    fatalError("There was an error")
}


// another way is the following...


// 2. Create a JSONDecoder
let decoder = JSONDecoder()


// 3. Get the data
let person2JSONData = person2JSON.data(using: .utf8)!
// Because it is explicitly known that there is data in person2JSON, force unwrapping is ok

// 4. parse the data into a person object
let person2 = try! decoder.decode(Person.self, from: person2JSONData)

//print(person2.name)

// because there is no partner for person2, the value type for partner in the struct must be an optional so as to not crash the app

// ==============================
//           Arrays
// ==============================

let personsJSON = """
[
    {
        "name": "James",
        "age": 45,
        "gender": "Male",
        "sign": "Sagitarius",
        "partner": "Emily",
        "isEmployed": true
    },
    {
        "name": "Mary",
        "age": 45,
        "gender": "Female",
        "sign": "Taurus",
        "isEmployed": false
    }
]
"""
let personsJSONData = personsJSON.data(using: .utf8)!
let personsArray = try! decoder.decode([Person].self, from: personsJSONData) // personsJSONData is the JSON object we are decoding from

for person in personsArray {
//    print("\(person.name)'s partner is \(person.partner ?? "nobody")")
}


// ===========================================
//           Family Model - First Model
// ===========================================


let familyJSON = """
{
    "familyName": "Smith",
    "members": [
        {
            "name": "James",
            "age": 45,
            "gender": "Male",
            "sign": "Sagitarius",
            "partner": "Emily",
            "isEmployed": true
        },
        {
            "name": "Mary",
            "age": 45,
            "gender": "Female",
            "sign": "Taurus",
            "isEmployed": false
        }
    ]
}
"""

struct Family: Codable {
    let familyName: String
    let members: [Person]
}

let familyJSONData = familyJSON.data(using: .utf8)!
let family = try! decoder.decode(Family.self, from: familyJSONData)
//print("The \(family.familyName) family")

for member in family.members {
//    print(member.name)
}


// ===========================================
//           Family Model - Second Model
// ===========================================

// the below model allows for all elements of the JSON to be decoded 

struct Family2: Codable {
    enum Gender: String, Codable {
        case Male, Female, Other
    }
    
    struct Person: Codable {
        let name: String
        let age: Int
        let gender: Gender
        let partner: String?
        let isEmployed: Bool
    }
    let familyName: String
    let members: [Person]
    
}

let family2 = try! decoder.decode(Family.self, from: familyJSONData)
print("The \(family2.familyName) family")

for member in family2.members {
    print(member.name)
}

```
