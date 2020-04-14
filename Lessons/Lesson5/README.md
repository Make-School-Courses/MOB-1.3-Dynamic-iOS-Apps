<!-- Run this slideshow via the following command: -->
<!-- reveal-md README.md -w -->


<!-- .slide: class="header" -->

# Memory Management

## [Slides](https://make-school-courses.github.io/MOB-1.3-Dynamic-iOS-Apps/Slides/Lesson5/README.html ':ignore')

<!-- > -->

## Why you should know this

The key to developing **high-performance** iOS apps is to know how your components consume memory and how to optimize memory use.

Poor optimization can result in code issues, including **memory leaks** and potentially **fatal errors**.

***Important:*** *iOS keeps track of how much memory each app uses on a given device, and it is set up to kill apps that consume too much.*

<!-- > -->

## Learning objectives

At the end of this class, you should be able to...

1. Explain how memory management works in Swift, including when and why to use **strong**, **weak**, or **unowned**
2. Identify and resolve **strong reference** cycles (aka, retain cycles)
3. Demonstrate proficiency in using **built-in tools** and techniques to find memory leaks caused by retain cycles

<!-- > -->

## Memory Leaks

<iframe src="https://giphy.com/embed/l3q2MDnkLri1t7i5a" width="480" height="360" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

When an instance of a **reference type** remains in memory even after its lifecycle has ended. Leaked memory still counts as a portion of an app’s total memory, even though the objects causing the leaks are no longer needed or useful.

<!-- > -->

## Value Types

When you create a new instance of a **value type**, the right amount of memory is set aside for it. Whenever you do anything with it, Swift creates a **copy** of it.

When that instance no longer exists, Swift automatically reclaims its allocated memory. ♻️

In Swift, you do not need to do anything to manage memory used by value types. 🙌🏼

<!-- v -->

*Below is a general list of Value and Reference Types in Swift.*

![syntax](assets/value_and_ref_types.png)

<!-- > -->

## Reference Types & ARC

With **reference types** its a different story. 😰

Passing around an instance of a **reference type** (class, closure) or storing it as a property does not copy it — it creates an additional reference to the same instance.

In other words, you are creating an additional reference to the same memory location on the heap.

<!-- > -->

## Reference Counting

The action to store a **reference count** of every initialized objects (classes, pointers, blocks).

Reference count = The number of references to the actual memory allocated for that instance.

**As long as an instance’s reference count is greater than 0, the instance remains alive, and its memory will not be reclaimed.**

<!-- > -->

## Using the deinit() function

A `deinit()` function is called immediately before a class instance is deallocated.

You can use this function to perform **clean up** or other actions just before the instance is deallocated.

As soon as the instance's reference count becomes 0, its `deinit()` function will run, and its memory will be deallocated.

<!-- v -->

<!--VIDEO NEEDED-->

The *deinit()* function in the following example from [Apple](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html#//apple_ref/doc/uid/TP40014097-CH19-XID_182) uses a gaming metaphor to illustrate how `deinit()` can be used to manage the behavior of reference type instances just prior to their deallocation.  

When the reference count of an instance of the Player class becomes 0, its coins are returned to the Bank immediately before the instance is removed from memory.

<!-- > -->

## Leaky Starship

Part 1 - Individual
1. Download the starter app, [LeakyStarship](https://github.com/VanderDev1/LeakyStarship)
2. Find one of the three `deinit()` functions in the app.
3. Set a breakpoint that will stop at one of the `deinit()` functions
3. Run the app, click the button on main scene, and examine what happens at each breakpoint (When done, disable all breakpoints)

Part 2 - In Pairs
1. Discuss with your partner what occurred at each `deinit()` breakpoint and why?

<!-- > -->

## Automatic Reference Counting (ARC)

In 2011, Apple introduced Automated Reference Counting (ARC) for Objective-C. Watch this video on ARC.

<iframe src="https://youtube.com/embed/D2FWPh0IbFA" data-autoplay  width="700" height="500"></iframe>

<aside class="notes">
With ARC, the compiler is now responsible for analyzing your code and for managing reference counts of class instances — so you do not have to.

However, because Swift handles memory automatically, it is still critical to understand how iOS manages memory, as there are some common mistakes that can cause memory issues…
</aside>

<!-- > -->

## Strong References & Ownership

<iframe src="https://youtube.com/embed/usvj1E8mGyI" data-autoplay  width="700" height="500"></iframe>

<aside class="notes">
A **strong** reference increments the reference count of the instance to which it points. When one instance of a reference type (A) has a reference to another (B), we say that A is an “owner” of B.

By retaining a reference to B, A protects B from being deallocated by ARC.

By default, all references you create are strong references.
</aside>

<!-- > -->

### Strong Reference Cycles

<iframe src="https://youtube.com/embed/9g23hKXZWEg" data-autoplay  width="700" height="500"></iframe>

<aside class="notes">
If two reference types each hold strong references to each other — if A retains a reference count for B, and B also retains A — they have a strong reference cycle (aka, a retain cycle).

Strong reference cycles are one type of memory leak.

</aside>

<!-- > -->

## How to Break Strong Reference Cycles

<iframe src="https://youtube.com/embed/3uS6IbDHeTA" data-autoplay  width="700" height="500"></iframe>


<aside class="notes">
A variable marked with the *weak* keyword does not take ownership of the object it refers to — it does not increment the reference count of its referenced object.

In the example above, if we add the *weak* attribute to the tenant variable in the Apartment class, and the Person instance is successfully deallocated, the Apartment class's reference to it will now be `nil`.

When you access a weak reference, it will either be a valid object, or `nil`.
</aside>

<!-- > -->

Because weak references can be changed to `nil` if the instance they point to is deallocated, they come with two inherent requirements:

- Weak references must always be declared as Optional, since Optionals are the only types that can become `nil`.

- Weak references can never be declared as `let`. Instances declared as `let` cannot change, thus weak references must always be declared as var.

**Q:** Where or when have you seen the `weak` keyword used so far in your iOS work?

<!-- > -->

A Simple Example - An instance of the Person class will have a strong reference to an instance of the Apartment class, and the Apartment class instance will also have a strong reference to the instance of the Person class. Neither instance's reference count can ever be 0.

```Swift
class Person {
    var apartment: Apartment?
}

class Apartment {
    var tenant: Person?
}
```

How would you fix this? Post you answer in the slack channel.

<!--
```Swift
class Person {
     var apartment: Apartment?
}

class Apartment {
    weak var tenant: Person?
}
```
-->

<!-- > -->

## Unowned References

Like a weak reference, an unowned references **does not increase the retain count** of the object it references.

Unlike a weak reference, however, an unowned reference **is assumed to always have a value** — it behaves somewhat like an implicitly unwrapped optional.

Because of this, an unowned reference is always defined as a non-Optional type. An unowned reference **cannot** be `nil`.


<!-- > -->

## In Class Activity

**Requirements:** The [LeakyStarship](https://github.com/VanderDev1/LeakyStarship) starter app

Individual
1. Follow the steps in the [Using the Debug Memory Graph Tool](https://github.com/Make-School-Courses/MOB-1.3-Dynamic-iOS-Apps/blob/master/Lessons/Lesson5/Mem_Graph_Tutorial/MemGraphTutorial.md) tutorial to find and fix memory leaks...

**Q** In addition to adding the *weak* keyword, what other change was required?

<!-- > -->

## Closures and Retain Cycles

Read [this article](https://www.avanderlee.com/swift/weak-self/) that explains when to use `weak self` and `unowned self`. While you read, discover the answers to:

- Why does a retain cycle happens in the Blog/Blogger example?
- How can we solve the retain cycle?
- What is `weak self` inside a closure?
- When should we NOT use `weak self`?
- How does a retain cycle occurs in a closure?
- Where would you use `unowned` over `weak`?
- Why don’t we need this with value types like structs?

**Turn in on Gradescope.**

<!-- > -->

## In Class Activity

**Requirements:** The [LeakyStarship](https://github.com/VanderDev1/LeakyStarship) starter app

- Using the tools and knowledge you've experienced in this class, **find and fix the memory leak** in the Starship class (hints: there is a **closure** involved -

<!-- > -->

## Outro Exercise

1. **Role Play Exercise** - A Mini Practice Interview

- Pair up. For 3 to 5 minutes in each role, take turns playing a Hiring Manager, then a Candidate for an iOS developer position. As the Hiring Manager, ask your Candidate to answer the following questions:

1. When and why would you use the keyword *weak*?
2. What is a *retain cycle*? Can you give examples of when a retain cycle might occur?
3. In Swift, is memory management for *value types* the same as memory management for *reference types*?

<!-- > -->

## Lab suggestions

1. Complete Classwork

1. Read content listed below for clarity on the topics relevant to iOS Memory Management covered in this class. And take notes 📝 these topics require you to study a little bit of theory before being able to identify these cases in your own code.

<!-- > -->

## Study Challenges

1. Look up *weak* and *unowned*:
- How are they similar? How do they differ?
- When would you use one over the other?
2. In a fresh (leaky) version of the LeakyStarship app, use the Memory Graph Tool to examine the two `ContiguousArrayStorage` objects:
- Research why these objects are causing memory leaks
- Resolve those leaks
3. Use the **Instruments -> Leaks** tool to identify memory leaks:
- Execute this [Instruments tutorial by Ray Wenderlich](https://www.raywenderlich.com/397-instruments-tutorial-with-swift-getting-started)
- Apply what you have learned in the above tutorial to the original (leaky) version of LeakyStarship to find and fix its leaky CrewMember objects
4. What are the defaults (*weak*, *strong*, *unowned*) for the following constructs? What is your guess as to why Apple chose those defaults for each construct?
- Arrays?
- @IBOutlets?
- Closures?
5. Research how the `isKnownUniquelyReferenced(_:)` function can be used to guard against creating retain cycles
- Using a fresh version of the LeakyStarship app, implement the `isKnownUniquelyReferenced(_:)` function to find memory leaks

<!-- > -->

## Additional Resources

- [Heaps & Stacks by Sarin Swift](https://heartbeat.fritz.ai/memory-management-in-swift-heaps-stacks-baa755abe16a)
- [Weak vs Unowned in a closure](https://www.uraimo.com/2016/10/27/unowned-or-weak-lifetime-and-performance/) (everything that comes before the Performance section)
- [Strong, Weak & Unowned - an article](https://krakendev.io/blog/weak-and-unowned-references-in-swift)
- [Avoiding Retain Cycles - an article](https://medium.com/mackmobile/avoiding-retain-cycles-in-swift-7b08d50fe3ef)
- [Deinitialization - from Apple](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html#//apple_ref/doc/uid/TP40014097-CH19-XID_182)
- [Reference Types & Value Types in Swift - an article](https://www.raywenderlich.com/9481-reference-vs-value-types-in-swift)
- [Deinitialization to Deallocate Memory Space - a tutorial from tutorialspoint.com](https://www.tutorialspoint.com/swift/swift_deinitialization.htm)
- [Manual Memory Management in iOS (Pre-ARC) - article](https://www.tomdalling.com/blog/cocoa/an-in-depth-look-at-manual-memory-management-in-objective-c/)
- [Apple Docs](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)
