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

The key to developing high-performance iOS apps is to know how your components are consuming memory and how you can optimize memory use.

Poor optimization can result in code issues including memory leaks and potentially fatal errors.

## Class Learning Objectives/Competencies (5 min)

1. Be able to explain and to demonstrate knowledge of how:
- memory management works in Swift, including when and why to use Strong, Weak, or Unowned
- to recognize strong reference cycles (retain cycles) and how to use weak references to break them
- to use built-in tools and techniques to find memory leaks caused by retain cycles

## Initial Exercise (10 min)

In Pairs, discuss the following interview questions:

1. When and why would you use the keyword weak?
2. What is a retain cycle? Can you give examples of when a retain cycle might occur?
3. In Swift, memory management for value types is the same as memory management for reference types ?
4. Is the default attribute for properties declared as @IBOutlets weak or strong? Why?


## Memory Allocation/Deallocation  (15 min)
### Memory Leaks, Reference Counting, and Retain cycles

#### Memory Leaks
iOS keeps track of how much memory each app uses on a given device, and it is set up to kill apps that use too much.

A memory leak occurs when an instance of a reference type remains in memory even after its lifecycle has ended.

Leaked memory still counts as a portion of an app’s total memory, even though the objects causing the leaks are no longer needed or useful.

#### Value Types
When you create a new instance of a value type, the right amount of memory is set aside for it. Whenever you do anything with it — pass it to a function, store it as a property, etc — Swift creates a copy of the instance.

When that instance no longer exists, Swift automatically reclaims its allocated memory.

In Swift, you do not need to anything to manage memory used by value types.

#### Reference Types
But passing around an instance of a reference type (class, closure) or storing it as a property does not copy it — it creates an additional reference to the same instance of that reference type.

In other words, you are creating an additional reference to the same memory location on the heap.

**Q:** If there are multiple references to the same instance of a class (object), what is the impact to all its references if any single reference makes a change to it?

#### Reference Counting
Every class instance has a reference count — which is the number of references to the actual memory on the heap allocated for that instance.

As long as an instance’s reference count is greater than 0, the instance remains alive, and its memory will not be reclaimed.

As soon as its reference count becomes 0, its memory is deallocated, and its deinit() method will run.

***< TODO: need simple example code here ? diagram? >***


## In Class Activity I (20 min)

***< TODO: need to add starte app link >***

Part 1 - Individual
1. Download LeakyStarship starter app
2. Examine the 3 deinit() funtions in the app
3. Run the app, click the button on main scene, and examine what happens at each breakpoint

Part 2 - In Pairs
1. Discuss with your partner what occured at each deinit() breakpoint and why?

## Wrap Up (5 min)

- Complete challenges
- Begin first tutorial on closures.
- Read the content listed below if you need more clarity on closures.


## Additional Resources
1.
