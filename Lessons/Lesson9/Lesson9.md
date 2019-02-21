# Building a Networking Domain

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**                        |
| ----------- | --------- | ----------------------------------- |
| 0:00        | 0:05      | Objectives                          |
| 0:05        | 0:20      | Initial Exercise                    |

| 0:x        | 0:xx      | BREAK                               |
| 0:x        | 0:x      | In Class Activity I                 |
| 1:20       | 0:15      | xxx |
| 1:35        | 0:20      | In Class Activity II                |
| 1:45        | 0:05      | Wrap Up                             |
| TOTAL       | 1:50      |                                     |



## Why you should know this

xxx

<!-- puts request logic into a separate object, and we call this object a store (Figure 28.4). Using a store object minimizes redundant code and simplifies the code that fetches and saves data. Most importantly, it moves the logic for dealing with an external source into a tidy class with a clear and focused goal. This makes code easier to understand, which makes it easier to maintain and debug, as well as share with other programmers on your team.

-->


## Class Learning Objectives/Competencies (5 min)
At the end of this class, you should be able to...

1. xxx
2. xxx


## Initial Exercise (20 min)

### As A Class

**Quizlet Game**


## Overview






### Domain Model

**A Definition**</br>
In software engineering, a **domain model** is a conceptual model of the domain that ___incorporates both behavior and data.___ <sup>[1](#footnote1)</sup>

**Implementation is in Layers**</br>
A domain model is commonly implemented as an **[object model](https://en.wikipedia.org/wiki/Object_model)** - *a collection of objects or classes through which a program can examine and manipulate specific parts of its world.*<sup>[1](#footnote1)</sup>

It is typically comprised of:<sup>[1](#footnote1)</sup>
1. A lower-level **persistence layer**
2. A higher-level **API layer** to gain access to the data and behavior of the model.



![syntax](assets/mvc_with_network_service_layer.png)

In this class, we will expand our proficiency with MVC by implementing an ___API Layer___ designed to manage access to the data and functionality of the *Model* layer.
</br>

### Key Application Design Principles

As a design pattern, MVC seeks to promote two important design principles fundamental to OOP:

1. **Separation of Concerns (SoC)** - Represents a ___modular___ approach to constructing an application in which code can be separated into logical sections, each addressing separate areas of functional behavior (concerns). SoC results in higher degrees of freedom for because it hides the need for a given section to know particular information addressed by other sections.

 - MVC can separate content from presentation and data-processing (model) from content.
 - Service-oriented design can separate concerns into services.

 2. **Reusability** - If correctly implemented, view and model layers can easily be composed of reusable, modular components (though controllers are seldom reusable).


### MVC's Current State

As the complexity of iOS app development evolves, design patterns have emerged to address shortcomings of the standard MVC model. The hype of many promising new iOS architectures can be misleading.

As a developer, you need to be aware of the pros and cons of emerging iOS design patterns such as:

1. **Model-View-ViewModel pattern (MVVM)** - Like MVC, but it adds a fourth component - the ___view model.___ The view model is responsible for managing the model and passing data to view controllers in a format ready to be displayed on the screen, relieving controllers of these responsibilities.

2. **View-Interactor-Presenter-Entity-Routing (VIPER)** - Expands MVC to include intermediary components which handle specific tasks and reduces interaction with the model, view and controller to a single point of contact. For example, views and view controllers now only detect user interaction and display data; the Presenter responds to user interaction information received from the controller, fetches and prepares data for presentation.

3. **Model-View-Controller-Store (MVCS)** - An expansion on MVC in which data request logic is moved out of the controller and into separate modules/objects specifically designed to fetch the data, regardless of where the data comes from (from a server, local file, a database, etc).
- This could also be thought of as ___"Model-View-Controller-Service,"___ where a *service layer* handles fetching and processing data.


But behind them all is simply the principle of **Separation of Concerns** being applied in specific ways to MVC...


### Project Organization

Structure and organization are key contributors to effective modularized architecture.

Creating groups folder is a great place to start.

*Note that controllers, views, and model are only suggestions - feel free to name yours whatever makes sense in your project.*


![syntax](assets/project_folders.png)


### Service Objects

The core component of our API Layer is the **Service** or **API object.**

It's the job of this object to:

- Fetch, post and process data to and from the target web services
- Serialize JSON data for manipulation and presentation
- Provide constructs for handling the successful or failed state of web service requests and responses


### The Model Object

Sometimes called a *Transfer Object* (aka, a TO), a *Value Object,* or a *Data Transfer Object (DTO),* model objects are an important component of an efficient network service layer.

They represent a single, simplified data instance that can be passed around your code and used in multiple ways. They can be used in data fetch and retrieval operations, data storage/persistence, presentation to the user (i.e., to populate a table cell’s data, for example), and more.

A simple example of a `user` represented (modeled) as a Codable struct with 3 properties:

```Swift
struct User:Codable {
    var first_name:String
    var last_name:String
    var country:String
}
```


## In Class Activity I (20 min)

Required resources:
1. [the XXX Starter App]()
**Individual**

__Scenario:__
- You just got hired as an app developer at a new firm, but you "inherited" code created by several previous developers who no longer work at the company.
- The app was written in an older version of Swift.
- It was originally a quick “prototype” app developed by engineers new to both iOS and Swift.

**TODO:** Using what you’ve learned so far about networking in iOS, your assignment is to:
- Refactor the code so that it (a) is scalable, and (b) adheres to the tenets of MVC
- Where you see quick and practical opportunities, update the code to Swift 4 constructs, including implementing model objects with the `Codable` interface, properly handling Optionals and errors, and so on...



<!-- Use the App from Lesson 8, add stubs, have students expand the model -->




<!-- Insert code here -->
## HTTP Post Requests

To add a new item to a web service, we use the HTTP protocol's **POST method.**

Implementing a POST request is a bit like performing a GET request in reverse, except that for a POST you will need to supply additional parameters to the URLRequest object. Commonly required parameters include:

- The __httpMethod type__ (i.e., “POST”)
- The __content type__ (JSON, in our case)
- Or any other ___headers___ required by the web service API (a valid API Key, for example)


### STEP 1: Set Up the Session and Requests

Just as we did with our HTTP GET request, we first need to create and configure a **URLSession** and a **URLRequest** object that points to our target web service **URL.**

```Swift
  let session = URLSession.shared
  let url = URL(string: "https://<your_web_service_url>")
  var request = URLRequest(url: url!)
```


<!-- Insert code sample here -->

### STEP 2: Configure the Request

#### Specify the httpMethod type

For any web service request other than GET (i.e., POST, PUT, PATCH, DELETE), we need to specify the httpMethod to invoke.

Since we are performing a POST here, we will set httpMethod property to `urlRequest = "POST"`.

```Swift
  request.httpMethod = "POST"
```

#### Specify Headers

Next, use the URLRequest `setValue(_:forHTTPHeaderField:)` method to set the values of any HTTP headers you want to provide (except the `Content-Length` header. The session automatically figures out content length  from the size of your data).

___`Content-Type`___
We use `Content-Type` header to indicate to the web service API the type of data we are sending.

In our case, we want to set the `Content-Type` to `JSON`.

```Swift
  request.setValue(“application/json”, forHTTPHeaderField: “Content-Type”)
```

___`Accept`___

The `Accept` request header field is used to specify certain **media types** that are acceptable for the **response** object returned by the web service.

We want our response to be returned as JSON, so we set the `Accept` request header field to return JSON.

```Swift
  request.setValue(“application/json”, forHTTPHeaderField: “Accept”)
```

____Other Header Fields___

Follow the same process of using the URLRequest `setValue(_:forHTTPHeaderField:)` method to supply all header fields required for communicating with your target web service.

A valid API Key is commonly required for the "Authorization" header field:

```Swift
    request.setValue("<insert_valid_API-KEY_here>", forHTTPHeaderField: “Authorization”)
```


<!-- Insert code sample here -->


### STEP 3:



<!-- Insert code sample here -->

### STEP x:


<!-- Insert code showing PUTTING IT ALL TOGETHER here -->


## The Request Builder

The **Builder** design pattern is a type of **Creational** design pattern that is used to create complex objects step-by-step.

It offers:
- ___Flexibility___ - Easily create different representations of the same complex object
- ___Simplicity___ - Simplifies the creation of a complex object.

HTTP request methods GET, POST, DELETE, and so on, are constructed using parameters common to them all. Thus, HTTP requests offer a prime opportunity to employ the Builder pattern to create different types of request objects with commonly shared parameters.

Instead of rewriting the parameters for a separate request object for each HTTP method, a more efficient design is to design a single RequestBuilder object that reuses the commonly shared reqeust parameters, then call a separate function on the RequestBuilder object to create a request for a GET or a POST, respectively.

<!-- Insert code sample here -->



## Best Practices

<!-- Best Practices for Network Architecture
keep controllers light and focused on its job
when adding anything, ask yourself “does this functionality really belong in the controller
keep model out of controllers
API Keys:
store API keys in Plist
add to headers is safer than appending
TODO: research why?
code reuse
simple, modular building blocks of specialized objects
model objects
Codable interface
TODO: research why?

https://github.com/futurice/ios-good-practices
futurice/ios-good-practices
-->



## Challenges

1.
<!-- apply best practices (saving API Key as a plist) -->




## Wrap Up (5 mins)


## Additional Resources

1. [Slides]
2. <a name="footnote1"><sup>1</sup></a>[Domain model - A Wikipedia article](https://en.wikipedia.org/wiki/Domain_model)
3. [xxx]()


<!-- xxx -->


https://en.wikipedia.org/wiki/Separation_of_concerns
https://medium.com/yay-its-erica/intro-to-the-viper-design-pattern-swift-3-32e3574dee02
