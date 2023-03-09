## Closures
Closures are very important to understand as well as necessary to be a concise and top tier swift programmer

Below is a long segment of code with various examples of closures used as a parameter for various methods.

### Code
``` Swift 
import UIKit


// ***Closures*** are self-contained blocks of functionality that can be passed around as values and executed later on in your code.

// In order to better understand the syntax of a closure, review of the syntax for a function is helpful.

func sum(_ numbers: [Int]) -> Int { // similar to the reduce method that will be seen later
    var total = 0
    for number in numbers {
        total += number // total = total + number. With [total += number] (shorthand), the addition and the assignment are combined into one operator that performs both tasks at the same time.
    }
    return total
}

sum([3,2,3,4])

// the function above has a name (sum), a parameter (numbers), no argument (_), and returns an Int value.

// Example of the same function but as a closure

let sumClosure = { (numbers: [Int]) -> Int in
    var total = 0
    
    for number in numbers {
        total += number // total = total + number. With [total += number] (shorthand), the addition and the assignment are combined into one operator that performs both tasks at the same time.
    }
    return total
}

let sum = sumClosure([4,5,6])

// All values in Swift have a type, including closures. For closures, there are four types

// Type 1: A closure with no parameters and no return value
let printClosure1 = { () -> Void in
    print("This closure does not take in any parameters and does not return a value")
}

// Type 2: A closure with parameters and no return value
let printClosure2 = { (string: String) -> Void in
    print(string)
}

// Type 3: A closure with no parameters and a return value
let randomNumberClosure1 = { () -> Int in
    // code that returns a number
    return 1
}

// Type 4: A closure with parameters and a return value
let randomNumberClosure = { (minValue: Int, maxValue: Int) -> Int in
    // Code that returns a random number between `minValue` and `maxValue`
    return 1
}

// Example of using a closure in the sorted(by:) array function

struct Track {
    var trackNumber: Int
    var trackRating: Int?
}

let tracks = [Track(trackNumber: 3), Track(trackNumber: 1), Track(trackNumber: 2), Track(trackNumber: 4)]


// With a closure, you can sort by any property i.e. trackNumber or trackRating

//let sortedTracks = tracks.sorted { (firstTrack, secondTrack) -> Bool in
////    return firstTrack.trackRating < secondTrack.trackRating
//    return firstTrack.trackNumber < secondTrack.trackNumber
//}


// ==========================
//      Syntactic Sugar
// ==========================

// Original syntax
let sortedTracks = tracks.sorted { (firstTrack, secondTrack) ->
    Bool in
    return firstTrack.trackNumber < secondTrack.trackNumber
}

// Swift can infer that the closure must return a Bool, so the return type can be dropped

let sortedTracks1 = tracks.sorted { firstTrack, secondTrack in
    return firstTrack.trackNumber < secondTrack.trackNumber
}

// Because the compiler expects two instances of Track to compare, Swift provides placeholder names that can be used rather than actual names. Placeholder names begin with $ followed by the index of the parameter.
// First parameter = $0
// Second parameter = $1

let sortedTracks2 = tracks.sorted { return $0.trackNumber < $1.trackNumber }

// When using only one line of code in Swift, the return keyword is not needed as the compiler knows what to return

let sortedTracks3 = tracks.sorted { $0.trackNumber < $1.trackNumber }


// When a closure is the last argument of a function, you can move the closing parenthesis after the second-to-last argument, drop the name of the final argument, and leave the curly braces at the end. When the function's only argument is a closure, the parentheses can be omitted entirely, along with the argument name. This is called “trailing closure syntax.

func performRequest(url: String, response: (_ code: Int) -> Void) {
    
}

// Here's what it would look like to call the function using trailing closure syntax. The closing parenthesis is moved to the previous argument, the response argument label is dropped, and the curly braces hang nicely off the end.

performRequest(url: "https://www.apple.com") { (data) in
    print(data)
}

// Swift includes some useful functions for iterating over collections that take a closure argument to define common actions.

// There are three functions that can take in a closure that can be helpful when dealing with a collection

// =============================
//            map()
// =============================

// the map function is an instance method that can be used on an array to create a **NEW ARRAY**. It take a closure parameter that tells it what to do to each object before adding it to the new array.

// Example with a function
let names1 = ["Brad", "Tom", "Susie", "Mike", "Pearl"] // original array

var fullNames1 = [String]() // new array using the elements from the names1 array

for name in names1 {
    let fullName = name + " Smith"
    fullNames1.append(fullName)
}

print(fullNames1)

// Example using the map function
let names2 = ["Brad", "Sam", "Timmy", "Laura"]

let fullNames2 = names2.map { (name) -> String in
    return name + " Wilkes"
}

print(fullNames2)

// Example with further syntactic sugar

let names3 = ["Lilly", "Steven", "Ben", "Hope"]

let fullNames3 = names3.map { $0 + " Billings" }

print(fullNames3)

struct Person {
    var name: String
    var age: Int
}

let people = [Person(name: "Bob", age: 24), Person(name: "Susie", age: 17), Person(name: "Tom", age: 25), Person(name: "David", age: 32)]

let peopleAge = people.map { $0.age }.sorted(by: < ) // create a new array of just ages and sort them by default ascending
print(peopleAge)


// =============================
//            filter()
// =============================

// The filter function creates a new array with only the objects from the starting array that match a specific use case.

// Example with function
let numbers1 = [1, 2, 3, 7, 8, 9, 11, 12]
var evenNumbers1 = [Int]() // can also write value with type annotation; var eveNumbers: [Int] = []

for number in numbers1 {
    if number % 2 == 0 { // the filtering wanted on the numbers array
        evenNumbers1.append(number) // if the number is even, add it to new array
    }
}

print(evenNumbers1)

// The above code took the numbers1 array and filtered through it based on the code in the for loop block

// Example using filter method with a closure

let oddNumbers1 = numbers1.filter { (number) in
    return number % 2 != 0
}

print(oddNumbers1)

// Example with shorthand syntax
let oddNumbers2 = numbers1.filter { $0 % 2 != 0 }

print(oddNumbers2)


// =============================
//            reduce()
// =============================
// The reduce function combines all the values in an array into one value.

let numbers3 = [9, 4, 5, 6, 10, 100]

var total = 0

for number in numbers3 {
    total += number
}

print(total)

let numbers4 = [9, 4, 5, 6, 3000, 100]

let total2 = numbers4.reduce(0) { currentTotal, newValue -> Int in
    return currentTotal + newValue
}

print(total2)

// the number in the parenthesis acts as a baseline, starting value from which to add, 0 or 1000

// Example with shorthand syntax

let total3 = numbers4.reduce(0) { $0 + $1 }
print(total3)

let total4 = numbers4.reduce(0, +)

```

