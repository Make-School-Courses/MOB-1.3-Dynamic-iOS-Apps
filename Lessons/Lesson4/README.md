<!-- Run this slideshow via the following command: -->
<!-- reveal-md README.md -w -->


<!-- .slide: class="header" -->

# Communication Patterns

## [Slides](https://make-school-courses.github.io/MOB-1.3-Dynamic-iOS-Apps/Slides/Communication-Patterns/README.html ':ignore')

<!-- > -->

# Why you should know this

If we think about how apps work, we can see they're usually multiple screens displaying or passing information.

In this last remark, there are several methods or patterns we can use to achieve communication between elements in an app.

You already know about delegates and callbacks. Others you have probably seen already and even used. Today we'll go over how they work.

<!-- > -->

## Learning Objectives

1. Identify and implement the **Target-Action** pattern for event handling.
1. Understand how Notifications work using **Notification Center**.
1. Analyze and decide which pattern to use based on the requirements of an app.

<!-- > -->

## Target-Action

Target-Action is a **design pattern** in which an object holds the information necessary to **send a message** to another object **when an event occurs**.

It's the pattern used to send messages in response to user-interface events, specifically from **UIControl**.

<!-- > -->

### Simplifying things

Sending a message (**the action**) to an object (**the target**).

**Target** - Where an action is fired, the object to receive the message.<br>
**Action** - A selector method to be called called in response of the event.

<!-- > -->

### An example: Buttons.

Buttons send a message when they have been tapped.

If the target is specified then the action message, it is sent to the object.

In case it is not specified (nil) then the action message goes up in the responder chain to look for an object that can receive it (not common). Only one object gets to handle the action.

<!-- > -->

### What are selectors?

**Unrecognized selector sent to instance**<br>
Who has encountered this error in which the app crashes?<br>
What was the problem?

<!-- > -->

A: Xcode couldn't find the method being called.

<!-- > -->

Selectors are the names of methods used to execute code at runtime. They are the way of telling objects like buttons "When tapped, send **this** message to **this** object".

The name of the method that will be called and the object to send the message to are **not known until runtime**.

Selectors are not part of the default Swift runtime behavior so we include `@objc` to our methods.

<!-- > -->

### How does this look like?

Creating a UIButton

```swift
let loginButton = UIButton(type: .roundedRect)
loginButton.setTitle("Login", for: .normal)
```

Adding the target

```swift
loginButton.addTarget(self, action: #selector(LoginViewController.login), for: .touchUpInside)
```

<!-- > -->

Created a UIButton and called `addTarget(_:action:for:)` with these parameters:

1. **target** is `self`
1. **action** is ``#selector(LoginViewController.login)``
1. **event** is `.touchUpInside`

When the event `.touchUpInside` occurs, send the message execute `login` to `self`

<!-- > -->

Here's the method called

```Swift
@objc func login(sender: UIButton){
  print("Button tapped")
  print(sender.titleLabel?.text)
}
```

<!-- > -->

## Example

[Playground example](https://www.ralfebert.de/ios-examples/uikit/uicatalog-playground/UIButton/)

Something to note is that the messages sent can't carry custom information. Which is why we can't send values in a UIButton's target action. They could send the sender that triggered the action as an argument, but that is as far as it goes.

<!-- > -->

## Passing arguments un the target action

There are times when we need to pass more arguments to a method call when tapping buttons. We know the target-action pattern can at most include the sender as a parameter.

One workaround is the use of **tags** in UIButtons. But there are other options. We could subclass `UIButton` and include any parameters we need there.

<!-- > -->

## Activity - Experimenting with UIButton

**Try this:**

1. Subclass UIButton.
2. Add a dictionary as a property that has strings as keys and any value as the values.
3. Include initializers

```swift
override init(frame: CGRect)
required init?(coder aDecoder: NSCoder)
```

<!-- > -->

4. Create a UIButton and use Target-Action to fire a method when tapped.
5. Include parameters in the button's dictionary `loginButton.params["firstValue"] = "Hello"`
6. Print these values when the method is called.

<!-- > -->

## Notifications

Notifications can broadcast messages between relatively unrelated parts of your code. They use the **Observer pattern** to inform registered observers when a notification comes in, using a central dispatcher called `Notification Center`.

The Notification Center deals with registering observers and delivering notifications.

<!-- > -->

Notifications can  include a payload in form of their property called `userInfo`, which is a dictionary.

The sender and the recipient don’t have to know each other so they are useful to send information between very distant modules.

<!-- > -->

### How the Notification Center works

Has 3 components
- An **observer** that listens for notifications
- A **sender** that sends notifications when something happens
- The **notification center** that keeps track of observers and notifications

<!-- > -->

![notification](assets/notification.png)

<!-- > -->

### Registering for notifications

```swift
print("Added Observer")
NotificationCenter.default.addObserver(self, selector: #selector(receivedNotification(_:)), name: Notification.Name("receivedNotification"), object: nil)
```

This adds an entry to the Notification Center. Every app has a default notification center property where objects register or post notifications. In this example 'self' will be listening for notifications with name 'receivedNotification' and when that event happens, the function 'receivedNotification' will be called.

<!-- > -->

Function that gets called.

```Swift
@objc func receivedNotification(_ notification:Notification) {
  // Do something here
  print("Received notification")
  self.view.backgroundColor = UIColor.purple       
}
```
Note: Observers need to be removed, or else you send a message to something that doesn't exist.

<!-- > -->

### Posting notifications

```Swift
print("Post Notification")
NotificationCenter.default.post(name: Notification.Name("receivedNotification"), object: self)
```
Needs the name of the notification and the object that posts the notification.

<!-- > -->

### Unsubscribing

```swift
deinit {
  print("Removed Observer")
  NotificationCenter.default.removeObserver(self, name: Notification.Name("receivedNotification"), object: nil)
}
```
*A deinitializer is called immediately before a class instance is deallocated. Swift automatically deallocates your instances when they are no longer needed, to free up resources.*

<!-- > -->

Tips

- When creating a notification, use **unique names** that are also helpful.
- It's best if you declare a **constant for the name**.
- Use the name of the Notification for the selector method. This is less confusing.
- Do not overuse this communication pattern or you'll end up with too many dependencies
- Use Notifications when you need to **communicate between 2 or more objects** in the app that don't know about each other. Or when using one-to-many or many-to-many communications that are consistent.

<!-- > -->

## Pomodoro App

Learn what the Pomodoro technique is: [go to video](https://youtu.be/V5l1NPYyH4k)

Then download [this](https://github.com/amelinagzz/pom-starter) starter app that serves as a time tracker for the Pomodoro technique.

<!-- > -->

Your task is to complete it by adding the functionality of buttons and the timer.

Both use the **Target-Action** pattern. Then every time the user completes a cycle of 4 Pomodoros, use a **notification** to update the count in the initial screen.

When the user leaves the timer screen they should see how many cycles they completed for the day.

<!-- > -->

## Lab - suggestions

- Complete classwork.
- Keep working on the Giphy tutorial due tonight.

<!-- > -->

## Additional Resources

1. [Selector and extensions](https://medium.com/@abhimuralidharan/selectors-in-swift-a-better-approach-using-extensions-aa6b0416e850)
1. [Target-Action](https://learnappmaking.com/target-action-swift)
1. [Notifications](https://learnappmaking.com/notification-center-how-to-swift/)
1. [Notifications](https://medium.com/@dmytro.anokhin/notification-in-swift-d47f641282fa)
