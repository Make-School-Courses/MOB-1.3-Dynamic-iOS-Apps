# Memory Management

## Minute-by-Minute [Optional]

| **Elapsed** | **Time**  | **Activity**              |
| ----------- | --------- | ------------------------- |
| 0:00        | 0:05      | Objectives                |
| 0:05        | 0:10      | Initial Exercise          |
| 0:15        | 0:15      | Allocation/Deallocation   |
| 0:30        | 0:20      | In Class Activity I       |
| 0:50        | 0:10      | BREAK                     |
| 1:10        | 0:30      | In Class Activity II      |
| 1:40        | 0:05      | Wrap Up                   |
| TOTAL       | 1:45      |                           |

## Why you should know this

The key to developing high-performance iOS apps is to know how your components consume memory and how to optimize memory use.

Poor optimization can result in code issues, including memory leaks and potentially fatal errors.

***Important Note:*** *iOS keeps track of how much memory each app uses on a given device, and it is set up to kill apps that consume too much.*

## Class Learning Objectives/Competencies (5 min)

And the end of this class, you should be able to...

1. Explain how memory management works in Swift, including when and why to use Strong, Weak, or Unowned
2. Identify and resolve strong reference cycles (retain cycles)
3. Demonstrate knowledge of:
- how weak references can be used to break reference cycles
- how to use built-in tools and techniques to find memory leaks caused by retain cycles

## Initial Exercise (10 min)

In Pairs, discuss how you would respond to these iOS interview questions:

1. When and why would you use the keyword *weak*?
2. What is a *retain cycle*? Can you give examples of when a retain cycle might occur?
3. In Swift, is memory management for *value types* the same as memory management for *reference types*?
4. What is the default attribute declared for an @IBOutlet: *weak* or *strong*? Why?


## Memory Allocation/Deallocation  (15 min)
### Memory Leaks, Reference Counting, and Retain cycles

#### Memory Leaks

A memory leak occurs when an instance of a reference type remains in memory even after its lifecycle has ended.

Leaked memory still counts as a portion of an app’s total memory, even though the objects causing the leaks are no longer needed or useful.

#### Value Types

When you create a new instance of a *value type*, the right amount of memory is set aside for it. Whenever you do anything with it, Swift creates a copy of it.

When that instance no longer exists, Swift automatically reclaims its allocated memory.

In Swift, you do not need to anything to manage memory used by value types.

#### Reference Types

But passing around an instance of a *reference type* (class, closure) or storing it as a property does not copy it — it creates an additional reference to the same instance.

In other words, you are creating an additional reference to the same memory location on the heap.

**Q:** If there are multiple references to the *same* instance of a class (object), what is the impact to all its references if any single reference makes a change to it?

#### Reference Counting

Every class instance has a reference count — the number of references to the actual memory on the heap allocated for that instance.

As long as an instance’s reference count is greater than 0, the instance remains alive, and its memory will not be reclaimed.

As soon as its reference count becomes 0, its memory is deallocated, and its `deinit()` function will run.

```Swift
class Player {
    var coinsInPurse: Int
    init(coins: Int) {
        coinsInPurse = Bank.distribute(coins: coins)
    }
    func win(coins: Int) {
        coinsInPurse += Bank.distribute(coins: coins)
    }
    deinit {
        Bank.receive(coins: coinsInPurse)
    }
}
```

## In Class Activity I (20 min)

<!-- TODO: need to add starter app link  -->


Part 1 - Individual
1. Download LeakyStarship starter app
2. Examine the 3 `deinit()` functions in the app
3. Run the app, click the button on main scene, and examine what happens at each breakpoint

Part 2 - In Pairs
1. Discuss with your partner what occurred at each `deinit()` breakpoint and why?


## Automatic Reference Counting (ARC) (xx min)

In 2011, Apple introduced Automated Reference Counting (ARC) for Objective-C.

Swift is built on top of ARC.

With ARC, the compiler is now responsible for analyzing your code and for managing reference counts of class instances — so you do not have to.

However, because Swift handles memory automatically, it is still critical to understand how iOS manages memory, as there are some common mistakes that can cause memory issues…

## Strong Reference Cycles (xx min)

### Strong References & Ownership

A strong reference increments the reference count of the instance to which it points. When one instance of a reference type (RefA) has a reference to another (RefB), we say that RefA is an “owner” of RefB.

By retaining a reference to RefB, RefA protects RefA from being deallocated by ARC.

![syntax](assets/Strong_refA_to_refB.png)

By default, all references you create are strong references.


### Strong Reference Cycles

If two reference types each hold strong references to each other — if RefA retains a reference count for RefB, and Ref B also retains A — they have a strong reference cycle (aka, a retain cycle).

Strong reference cycles are one type of memory leak.

![syntax](assets/strong_ref_cycle.png)

***< TODO: needs code sample >***


## How to Break Strong Reference Cycles

### Weak & Unowned References

#### Weak References

**Q:** Where or when have you seen the weak keyword used?

A variable marked with the *weak* keyword does not take ownership of the object it refers to — it does not increment the reference count of its referenced object.

When the instance (RefB) to which a weak reference (RefA) refers is successfully deallocated — when RefB’s reference count is zeroed out — RefA will now be `nil`.

As a *weak* variable, RefA does not protect RefB from being deallocated by ARC.

This ensures that when you access a weak reference, it will either be a valid object, or `nil`.

![syntax](assets/weak_reference.png)

***< TODO: needs code sample >***

Because weak references can be changed to `nil` if the instance they point to is deallocated, they come with two inherent requirements:

- Weak references must always be declared as Optional, since Optionals are the only types that can become nil.

- Weak references can never be declared as let. Instances declared as let cannot change, thus weak references must always be declared as var.

#### Unowned References

Like a weak reference, an unowned references does not increase the retain count of the object it references.

Unlike a weak reference, however, an unowned reference is assumed to always have a value — it behaves somewhat like an implicitly unwrapped optional.

Because of this, an unowned reference is always defined as a non-optional type. An unowned reference cannot be nil.

This makes them easier to manage rather than resorting to using optional binding.
However, if you try and access an unowned reference, and it’s not there, it will crash the app.

***< TODO: needs diagram and a code sample >***

## In Class Activity II (20 min)
(Requirements: The LeakyStarship starter app)

***< TODO: add Tutorial here >***

Individual
1. Follow the steps in the Using the Debug Memory Graph Tool tutorial to Find and Fix Memory Leaks


## Closures and Reference Cycles

By default, captured values in closures are strong references.

Because closures — like classes — are reference types, a strong reference cycle can also occur with a closure if you assign it to a property of a class instance, and the body closure captures a reference to that class instance.

***< TODO: small code sample here >***



## In Class Activity III (20 min)
(Requirements: The LeakyStarship starter app)

Individual
1. Part of the iOS developer "toolbox" is the ability to quickly find the most useful information from Internet research.
- Using the tools and knowledge you've experienced in this class, find and fix the memory leak in the Starship class (hints: there is a closure involved; you will need to research conditions under which closures can have strong reference cycles and how to resolve them)

## Challenges HOF
1. xxx

## Wrap Up (5 min)

- Complete challenges
- Begin first tutorial on < topic >.
- Read the content listed below if you need more clarity on closures.

***< TODO: review interview questions again >***

## Additional Resources
- [Strong, Weak & Unowned - an article](https://agostini.tech/2017/07/23/memory-management-in-swift-the-strong-the-weak-and-the-unowned/)
(https://krakendev.io/blog/weak-and-unowned-references-in-swift)
- [Avoiding Retain Cycles - an article](https://medium.com/mackmobile/avoiding-retain-cycles-in-swift-7b08d50fe3ef)
- [Deinitialization - from Apple](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html#//apple_ref/doc/uid/TP40014097-CH19-XID_182)
- Pre-ARC (Manual) Memory Management in iOS
< add URLs >
- Reference Types & Value Types in Swift
< add URLs >
