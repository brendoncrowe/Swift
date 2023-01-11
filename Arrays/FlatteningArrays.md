## Flattening Arrays with several examples


``` Swift
import UIKit

// ====================================================================
//          Flattening Arrays - How to flatten an array of arrays
// ====================================================================

var array = [[1, 2, 3], [4, 5, 6, 7, 8, 9], [10]]


// =========================================
//          1. Using the flatMap() method
// =========================================

// Returns an array containing the concatenated results of calling the given transformation with each element of this sequence.
// In layman's terms, this method returns a single array that is composed of all the elements of the 3 arrays

let flattenedArray = array.flatMap { $0 }
print(flattenedArray)

// =========================================
//          2. Using the reduce() method
// =========================================

// Returns the result of combining the elements of the sequence using the given closure.
// the parameters are reducing the outer array, and then adding the left over array elements

let reducedArray = array.reduce([], +)
print(reducedArray)


// =========================================
//          3. Using the joined() method
// =========================================

// Returns the elements of this sequence (array) of sequences (array), concatenated.

var array2 = [[4, 5, 6], [7, 8 ,9]]
let joinedArray = Array(array2.joined())
print(joinedArray)

// ================================================
//          4. Using functions for more complexity
// ================================================

// Way 1
var array3:[Any] = [1,2,[[3,4],[5,6,[7]]],8]

func flattenArrayWay1(array: [Any]) -> [Int] {
    var newArray = [Int]()
    
    for item in array {
        if let item = item as? Int {
            newArray.append(item)
        }
        if let item = item as? [Any] {
            let result = flattenArrayWay1(array: item)
            for i in result {
                newArray.append(i)
            }
        }
    }
    return newArray
}


// Way 2
func flattenArrayWay2(array: [Any]) -> [Int] {
    var newArray = [Int]()
    
    array.map { (element) -> () in
        if let element = element as? Int {
            newArray.append(element)
        }
        if let element = element as? [Any] {
            newArray.append(contentsOf: flattenArrayWay1(array: element))
        }
    }
    return newArray
}

print(flattenArrayWay2(array: array3))
```
