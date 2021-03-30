# Functions and Closures

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**              |
| ----------- | --------- | ------------------------- |
| 0:00        | 0:05      | Objectives                |
| 0:05        | 0:10      | Initial Exercise          |
| 0:15        | 0:15      | What are closures?        |
| 0:30        | 0:20      | In Class Activity I       |
| 0:50        | 0:10      | BREAK                     |
| 1:00        | 0:10      | Optimizing with closures  |
| 1:10        | 0:30      | In Class Activity II      |
| 1:40        | 0:05      | Wrap Up                   |
| TOTAL       | 1:45      |                           |

## Why you should know this

You probably have already used closures in your apps by now without noticing or without knowing exactly how they work. Closures are a powerful way of writing code that performs and looks better. They are also present in many programming languages so knowing how they work will help you recognize them when working in other type of projects and will let you discuss them with other developers.

## Class Learning Objectives/Competencies (5 min)

1. Describe how closures work and how to use them
1. Declaring and calling closures
1. List and implement use cases of closures
1. Describe drawbacks for using closures

## What is a Closure? (15 min)

Apple's definition:<br>
**"Closures are self-contained blocks of functionality that can be passed around and used in your code."**

In essence, a closure is a block of code that you can assign to a variable or constant. Then pass it around in your code and execute its content later somewhere else.

#### An Analogy


#### Example: closure with statements

```Swift
var brunch = {
    print("Coffee and bagels")
}
```

Everything inside the braces `{}` is the closure. And it is assigned to a variable (could be to a constant too). This is possible because closures are first-class.<br>
*Note: First-class just means that there are no restrictions in the object's use. It can be created, stored, passed, assigned, return as value. You can treat it as you would any other value or object*

**Q:** What is the type of the closure? <br>
We can add it in the declaration.

```Swift
var brunch: () -> () = {
    print("Coffee and bagels")
}

brunch()
```

#### Example: closure with parameters

```Swift
let brunchOption:(String) -> () = { option in
    print(option)
}
brunchOption("Mimosas and croissants")
```

**Q:** What is the type of the closure?

We can use the value passed inside the statements of the closure by placing a parameter name `option` followed by the `in` keyword.

The `in` keyword separates the parameter name with the body of the closure.<br>
When calling the closure, since it accepts a String, we pass the string in that moment.

#### Example: closure that returns a value

```Swift
let brunchOptionLocation:(String) -> (String) = { option in
    let greeting = option + " @ Castro St."
    return greeting
}
let result = brunchOptionLocation("Pancakes and smoothies")
print(result)
```

**Q:** What is the meaning of `(String) -> (String)`?<br>
**Q:** How are we using `option`? <br>
**Q:** How do we return and show the output?

#### Example: passing a closure as a function parameter

```Swift
func getBrunch(optionClosure:()->()) {
    print("Going for brunch.")
}

getBrunch(optionClosure: {
    print("Anything edible")
})
```
**Q:** What is the output?<br>
**Q:** Why is the closure statement is not executed?<br>

## In Class Activity I (20 min)

### Function vs Closure

Closures are very similar to functions. In fact, functions are a special type of closures. They have a name and are declared with the keyword `func` whereas closures are nameless.

Take the following function and turn it into a closure. Note each step you make in the transformation. Include how you would call it.

```Swift
func add(number1: Int, number2: Int) -> Int {
 return number1 + number2
}
```
Remember the syntax of a closure.

![syntax](assets/closuresyntax.png)

Now list the differences you can find between functions and closures.

Once everyone is done, do the transformation in a big whiteboard for everyone to see and share their insights.

## Optimizing with closures (10 min)

Swift’s closure expressions have a clean, clear style, with optimizations including:

- Inferring parameter and return value types from context
- Implicit returns from single-expression closures
- Shorthand argument names

We'll use the sort method as an example.

`sorted(by:)` sorts an array of values of a known type, based on the output of a sorting closure that we provide. Once it completes the sorting process, the `sorted(by:)` method returns a new array of the same type and size as the old one, with its elements in the correct sorted order. The original array is not modified.

`let names = ["Andrea", "Chris", "Marie", "Beth", "Tom"]`

The `sorted(by:)` method accepts a closure that takes two arguments of the same type as the array’s contents, and returns a `Bool` value to say whether the first value should appear before or after the second value once the values are sorted. The sorting closure needs to return true if the first value should appear before the second value, and false otherwise.

First approach, using a functions and passing it as a parameter.

```Swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
```

*Note: For characters in strings, “greater than” means “appears later in the alphabet than”. This means that the letter "B" is “greater than” the letter "A".*

Second approach, using a closure expression syntax.
```Swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

Since the body of the closure is short, we can write in in one line.
```Swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

Also, because the sorting closure is passed as an argument to a method, Swift can **infer the types of its parameters and the type of the value it returns**.
```Swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

Swift automatically provides **shorthand argument names** to inline closures, which can be used to refer to the values of the closure’s arguments by the names $0, $1, $2, and so on.
```Swift
reversedNames = names.sorted(by: { $0 > $1 } )
```

## In Class Activity II (30 min)

Complete [these](ClosuresChallenges.md) challenges on closures.

## Wrap Up (5 min)

- Complete challenges
- Begin first tutorial on closures.
- Read the content listed below if you need more clarity on closures.


## Additional Resources
1. [Slides](https://drive.google.com/open?id=1HVs8m91QcMHG5nkqFXVSEIBEQ7cw-2632rGRw3U7ZdM)
1. [Closures in Swift - an article](https://medium.com/the-andela-way/closures-in-swift-8aef8abc9474)
1. [From function to closure - article](https://medium.com/ios-os-x-development/introduction-to-closures-in-swift-3-1d46dfaf8a20)
1. [Apple's documentation on closures](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)
