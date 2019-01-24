# Closures and callbacks

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**              |
| ----------- | --------- | ------------------------- |
| 0:00        | 0:05      | Objectives                |
| 0:05        | 0:10      | Initial Exercise          |
| 0:15        | 0:10      | Trailing closures         |
| 0:15        | 0:15      | Capturing values          |
| 0:40        | 0:15      | In Class Activity I       |
| 0:55        | 0:10      | BREAK                     |
| 1:05        | 0:15      | Escaping and non escaping |
| 1:20        | 0:25      | In Class Activity II      |
| TOTAL       | 1:45      |                           |

## Why you should know this
Some uses cases of closures:
- Animations
```Swift
UIView.animate(withDuration: 0.2, animations: {
    // Animate things
}) { finished in
    // Animation ends
}
```
- Fetching data from external APIs
- Completion handlers
- Creating views
```Swift
let customView: UIView = {
 let view = UIView()
 return customView
}()
```
- Higher order Functions: Closures can be passed as an input parameters for higher order functions. A higher order function is just a type of function that accepts function as an input and returns value of type function as output.

Which HOF are you familiar with or know about?

## Class Learning Objectives/Competencies (5 min)

1. Identify trailing closures
1. Identify use cases of closures
1. Understand what capturing values mean
1. Know the difference between escaping and non escaping closures

## Initial Exercise (10 min)

Review answers for closure challenge from last class.

## Trailing closures  (10 min)

We've talked about how a function can receive a closure as parameter.

Example:

```Swift
func getBrunch(optionClosure:()->()) {
    print("Going for brunch.")
    getBrunch() //called from the closure
}

getBrunch(optionClosure: {
    print("Getting anything edible")
})

```

If it turns out that it is the last parameter, the closure can be passed similar to a function body between `{}`.

```Swift
func getBrunch(msg:String, optionClosure:()->()) {
    print(msg)
    optionClosure()
}

getBrunch(msg:"Going for brunch")  {
    print("Getting anything edible")
}
```

The closure is written outside of the function call parentheses, this is known as **trailing closure**. Its purpose is readability (specially for long closures), so it is recommended to use when a function accepts a closure as a final argument.

**Q:** What will be the output when running the program?

`getBrunch()` accepts a closure as a final parameter. When calling the function, instead of passing the closure as an argument, we have used trailing closure, in between `{}`.

**Note that even then the closure looks like the body of a function, it is still an argument.**

### Capturing values

Closures have a distinct feature that makes them stand out: they can capture values from their surrounding context.

From Apple's documentation
*A closure can capture constants and variables from the surrounding context in which it is defined. The closure can then refer to and modify the values of those constants and variables from within its body, even if the original scope that defined the constants and variables no longer exists.*

Variables, functions and closures have a scope. This scope determines if we can access any of them. Simply if a variable, function or closure isn't in a scope, we can access it. *Note: scope can be called context*

Example

```Swift
let name = "Mike"
let goodbye = {
    print("See you later, \(name)!")
}

goodbye()
```

The closure assigned to `goodbye` closes over the local variable `name` since it is available in the scope that the closure is defined in. We can access the constant without declaring it locally inside the closure.


## In Class Activity I (15 min)

Here's another example of closures closing over variables.

```Swift
func addScore(_ points: Int) -> Int
{
    let score = 42

    let calculate = {
        return score + points
    }

    return calculate()
}

let value = addScore(11)
print(value)
```

Using different colors or any other way to mark a difference, show where the global scope, local scope and closure scope are.

Once everyone is done, discuss the solution.

- Closures only capture elements that are used in the closure. When a variable isn’t accessed in the closure, it isn’t captured.
- Capturing only works one way. The closure captures the scope it is defined in, but code “outside” a closure doesn’t have access to values “inside” the closure. Like a one way mirror.


## Escaping and non escaping closures

Closures come in two different variants - escaping and non-escaping. When a closure is escaping it means that it will be stored somehow (either as a property, or by being captured by another closure). Non-escaping closures on the other hand, cannot be stored and must instead be executed directly when used.

In an **escaping closure**, the lifecycle looks like this:

1. Passing the closure to a function as argument
1. Function performs a task
1. Return compiler back
1. The closure runs asynchronously

This means that the closure outlives the function (it is called after the function has returned). An example: completion handlers.

By default, all closures are non escaping. This helps with memory management.

In a **non escaping closure**, the lifecycle looks like this:

1. Passing the closure to a function as argument
1. Function performs a task
1. Running the closure
1. Return compiler back

A non-escaping closure does not outlive the function from where it was called.

## In Class Activity II (25 min)

Closures are used in completion handlers.

Take a look at the code snippet and analyze how closures are being used and the order in which lines get executed.

```Swift
func flyAway(finalStage: String){
    print("\(finalStage) emerged, flying away... ")
}

func metamorphosis(initialStage:String, completion: (String) -> Void){
    print("Caterpillar creates cocoon.")
    // They stay inside for up to 21 days.
    for _ in 1...21 {
        print("\(initialStage) inside cocoon")
    }
    completion("🦋")
}

metamorphosis(initialStage:"🐛", completion: flyAway)

```
**Q:** What is the for loop simulating?<br>
**Q:** Can you make the last call using a trailing closure?<br>
**Q:** Does is make sense to use @escaping for this example? Where would it be?

Now implement your own example. Make sure to put in a context, get creative 🌮 👽 💣.

## Wrap Up (5 min)

Now that we know more about closures. Think of a metaphor or analogy that will help you remember how they work. Share with the rest of the class.

- Finish tutorial on closures
- Read about capture lists. The following tutorial has 2 parts and covers the topic really good with a coding example that you can download. [Part 1](https://medium.com/swift-programming/swift-closures-everyday-gems-part-1-of-2-c1a50c08a458) and [Part 2](https://medium.com/swift-programming/swift-closures-everyday-gems-part-2-of-2-8607157b11c5)

## Additional Resources
1. [Slides]()
1. [Closing over - diagram and article](https://learnappmaking.com/closures-swift-how-to/#capturing)
1. [Completion Handler](https://www.agnosticdev.com/content/how-define-swift-completion-handlers)
1. [Completion Handlers - article](https://www.bobthedeveloper.io/blog/completion-handlers-in-swift-with-bob)
1. [Capture lists - article](https://www.bobthedeveloper.io/blog/swift-capture-list-in-closures)
1. [Capturing with closures - article](https://www.swiftbysundell.com/posts/capturing-objects-in-swift-closures)
1. [Initialization with closures - article](https://www.bobthedeveloper.io/blog/swift-lazy-initialization-with-closures)
1. [All on closures - article with examples](https://www.programiz.com/swift-programming/closures#simple)
