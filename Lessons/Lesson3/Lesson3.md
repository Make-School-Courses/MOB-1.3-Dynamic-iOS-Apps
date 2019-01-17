# Protocols & Delegation

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**              |
| ----------- | --------- | ------------------------- |
| 0:00        | 0:05      | Objectives                |
| 0:05        | 0:05      | Initial Exercise          |
| 0:10        | 0:20      | Delegates                 |
| 0:30        | 0:10      | In Class Activity I       |
| 0:50        | 0:10      | BREAK                     |
| 1:00        | 0:20      | Creating our delegate     |
| 1:20        | 0:10      | In Class Activity II      |
| 1:30        | 0:10      | In Class Activity III     |
| 1:40        | 0:05      | Wrap Up                   |
| TOTAL       | 1:45      |                           |

# Why you should know this

The delegate pattern is widely used in iOS development to communicate an object back to its owner. We've seen it before in `UITableViews` and `UICollectionViews`. Most of the time is easy to implement the delegate methods without worrying much about how they work. If we understand how the delegate pattern works, we can implement our own and manage events, tasks and more in a simple way without compromising coupling.

## Class Learning Objectives/Competencies (5 min)

1. Construct and use protocols to define 'contracts' in code
1. Identify delegation in UIKit
1. Implement delegates in code

## Initial Exercise (5 min)

- Xcode tip: Layout stress testing by @twostraws <br>
When using IB in the assistant editor, change Automatic to Preview. Then change the language from English to Double-Length Pseudolanguage. This will cause every word in your UI to be repeated, letting you make sure it all fits.<br>

## Delegates (20 min)

Meaning in the real world.

*To delegate* as a verb means to give control.
*A delegate* as a noun means a person acting on behalf of another.

In software.

Delegation is a design pattern that enables a class or structure to hand off (or delegate) some of its responsibilities to an instance of another type.

In iOS development, delegation is used as a way for one class to communicate to another class. The example that looks the most familiar to us is UITableViewDelegate.

We can handle events.
```
tableView(_:didSelectRowAtIndexPath:)
tableView(_:willSelectRowAtIndexPath:)
```

They can also be used for customization.
```
tableView(_:heightForRowAtIndexPath:)
tableView(_:shouldHighlightRowAtIndexPath:)
```

**Q:** Where else have you seen delegates being used?

Simple example

#### Feeding your pet.
Pets expect their owners to feed them, preferably twice per day. Until they develop the ability to pour water in their bowls and refill their dry food, they need someone else to do it. They need to *delegate* these task. Yes, in reality we serve our pets :)

![Dog](https://www.dogster.com/wp-content/uploads/2017/10/A-hungry-dog-looking-up-near-his-food-and-water-bowl.jpg)

To continue with this example the first thing is creating a protocol for the one in charge of refilling he bowls.

```
protocol Feed{
    func refillWaterBowl()
}
```

Now we create the Owner struct that conforms to it.
```
struct Owner: Feed {
    func refillWaterBowl() { print("Bowl with fresh water now.") }
}
```
We can make an instance of it and make sure it can perform the task.
```
var owner = Owner()
owner.refillWaterBowl()
```
Now we create a pet. And since they need their owner to give them water, wee add the owner (delegate) to do it instead.
```
struct Pet {
    var delegate: Feed? // delegating the feeding task
}
```
Finally we create a pet and assign the owner as delegate.
```
var pet = Pet()
pet.delegate = owner
pet.delegate?.refillWaterBowl()
```

## In Class Activity I (20 min)

Part 1 - individual<br>
Using the example of feeding pets or other if it's easier for you, create a diagram to remember how delegates work.<br>
Try to include the following key words: **delegate**, **delegating object** and **delegate protocol**.

Part 2 - pairs<br>
Share and explain your diagram to a peer and compare both diagrams to see any similarities or differences

## Creating our own delegate (25 min)

Download the [starter files]() to try after the explanation.

We can take a systematic approach on creating delegates by identifying specific steps when implementing delegates.

Scenario: We have an app with two view controllers. `FirstViewController` has a button that takes us to `SecondViewController`. There we have 3 views of different colors. The goal is that by selecting one of the views, the app will take us back to the first view controller and change the background color to the one selected previously.

**Q:** What is the problem if we can't use unwind segues? Or if we're not even using storyboards?

We need a way of communication between both view controllers and pass information back from one to the other. Here's where delegates are useful.

For delegates to work we can already imagine we will need to setup a few things in both view controllers.

The first part will take place in `SecondViewController`.

### Step 1: Adding the protocol
```Swift
protocol BackgroundColorDelegate{
    func colorSelected(color:UIColor)
}
```
### Step 2: Creating a delegate property
```Swift
var delegate: BackgroundColorDelegate?
```
### Step 3: Adding the delegate method call
```Swift
//Dismiss view controller and call method
self.delegate?.colorSelected(color: someColor)
```

Now we move to `FirstViewController`

### Step 4: Adopting the protocol
Include `BackgroundColorDelegate` in the class declaration.

### Step 5: Creating a reference of SecondViewController specifying the delegate
```Swift
let secondVC = segue.destination as! SecondViewController
            secondVC.delegate = self
```
### Step 6: Use the method of the protocol
```Swift
func colorSelected(color: UIColor) {
  // set background color
}
```

Running the program should work as expected now.

## In Class Activity II  - individual (10 min)
Take the diagram you created earlier and see how you can fit the project to match the components of your diagram. Are there any things you need to change?

## In Class Activity III - pairs (20 min)

Remember closures and completion handlers? These can be used instead of delegates.<br>
Download [this working example](https://github.com/dmlebron/tutorial_closures) and look at the code with a partner. Notice how delegates are being replaced by closures and see how it leads to a similar result.
**Q:** What does the app do?<br>
**Q:** Where are closures being used?<br>
**Q:** How would the approach be with delegates?<br>

## Challenge

Change the implementation of the delegate to use a closure in the DelegateDemo project (the one with the colors). This approach is not the most common but it is a good idea to know there are more options to do the same task.

## Wrap Up (5 min)

- Finish implementing the `BackgroundColorDelegate` if you haven't done so making sure you understand all the steps.
- Complete the challenge if you want to get familiar with closures.

## Additional Resources

1. [Example for delegates](https://medium.com/@jamesrochabrun/implementing-delegates-in-swift-step-by-step-d3211cbac3ef)
1. [Closures as delegates - article](https://medium.com/@dmlebron/using-swift-closures-as-an-alternative-to-delegates-5c3c1a7f45d6)
1. [Understanding delegates - article](https://www.appcoda.com/swift-delegate/)
1. [Delegates simple analogy - article](https://blog.bobthedeveloper.io/the-meaning-of-delegate-in-swift-347eaa9674d)
