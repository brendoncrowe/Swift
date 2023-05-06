# Key Value Observing
KVO is part of the observer pattern for 1-many </br>
NotificationCenter is also an observer pattern for 1-many

KVO is a one-to-many pattern relationship as apposed to custom delegation which is a one-to-one pattern (i.e tableView.dataSource) </br>
KVO is an objective-C runtime API

There are other essentials required for KVO
1. The object being observed needs to be a class
2. The class needs to inherit from NSObject, which is the top abstract class in objective-C. The class also needs to be marked @objc
3. Any property being marked for observation needs to be prefixed with @objc dynamic. dynamic means that the property is being dynamically dispatched (at runtime the compiler verifies the underlying property)
  - In swift, types are statically dispatched which means they are checked at compile time vs objective-C which is dynamically dispatched and checked at runtime

Key-Value Observing in Swift

In Objective-C, you can think of objects as having keys that are strings (the property names) and value (the value for that property name).  Whenever the value for a key changes, it broadcasts the new value to any observers who might be listening.  We can create observers to receive updates about these changes, and execute code accordingly.

To do so, we'll need to create a class that's able to have its property observed:

```swift
@objc class Dog: NSObject {
    var name: String
    @objc dynamic var age: Int
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
```

It must:

- Be an `@objc` class that inherits from `NSObject`
- Have any properties that you want to observe be `@objc` and `dynamic`
- Have any properties that you want to observe be able to be read by the Objective-C runtime.

Now that we have our `Dog` class, we can build a `DogWalker` class that takes in a `Dog`, and prints a message when it has a birthday:

```swift
class DogWalker {
    let dog: Dog
    var birthdayObservation: NSKeyValueObservation?

    init(dog: Dog) {
        self.dog = dog
        configureBirthdayObservation()
    }

    func walk() {
        print("Now walking \(dog.name)")
    }

    private func configureBirthdayObservation() {
        birthdayObservation = dog.observe(\.age, options: [.old, .new], changeHandler: { (dog, change) in
            print("Hey \(dog.name), happy birthday from the dog walker!")
        })
    }
}
```

Note the odd syntax after the `observe` call.  The "\." syntax is called a [Key Path](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html).  It lets us access properties of the dog that we are observing.

We can create another class with a separate observation:

```swift
class DogGroomer {
    let dog: Dog
    var birthdayObservation: NSKeyValueObservation?

    init(dog: Dog) {
        self.dog = dog
        configureBirthdayObservation()
    }

    func groom() {
        print("Now grooming \(dog.name)")
    }

    private func configureBirthdayObservation() {
        birthdayObservation = dog.observe(\.age, options: [.old, .new], changeHandler: { (dog, change) in
            print("Hey \(dog.name), happy birthday from the dog groomer!")
        })
    }
}
```

Then, we can create a dog, its walker and groomer, then have the dog have a birthday:

```swift
let snoopy = Dog(name: "Snoopy", age: 5)
let walker = DogWalker(dog: snoopy)
let groomer = DogGroomer(dog: snoopy)

snoopy.age += 1


// Prints the following
// Hey Snoopy, happy birthday from the dog groomer!
// Hey Snoopy, happy birthday from the dog walker!
```

We have successfully configured multiple observers to watch changes to the same property in a given subject.
