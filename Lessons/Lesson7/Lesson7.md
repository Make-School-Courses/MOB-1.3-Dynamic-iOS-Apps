# Network Requests with URLSession

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**                        |
| ----------- | --------- | ----------------------------------- |
| 0:00        | 0:05      | Objectives                          |
|


## Why you should know this
Nearly every serious iOS app communicates at some point with an Internet component such as a remote web service.

Fetching and processing data between an app and a web service is an essential skill that all iOS developers must master.

And the most commonly-used protocol for communicating with web servers is **HTTP** (and its secure counterpart, HTTPS).

Since iOS 7, Apple has provided **URLSession** and its suite of related components as **a complete networking API** for uploading or downloading content via **HTTP** and HTTPS.

<!--
...and its family of related classes...

**HTTP** and **HTTPS** are robust and stable protocols. They have been widely used in web browsers for a long time. They offer several performance and security advantages, as well as a mature base of easy-to-use development and analysis tools.

to make GET and POST network requests...
-->


## Class Learning Objectives/Competencies (5 min)
At the end of this class, you should be able to...

1. xxx

<!-- use Xcode to find things out about foundation classes, JSONDeserialization, xxx -->


## Class Handouts (5 min)

### Structured Sharing Exercise - Part 1

By the end of class, write down your **three (3) most important** responses for each of the question sheets passed out.


## Initial Exercise (10 min)

What differentiates software developers from other engineers?

    Developers find things out.

We’ve learned that executing efficient **Internet searches** is a key skill every iOS developer needs.

We also have - at our fingertips — an extremely powerful tool for "finding things out": **Xcode**

### Xcode Techniques  

Xcode offers a wealth of insight into most of the Swift and iOS constructs we need to know to get the job done.

#### TODO: In Pairs (3 to 5 mins)

Resources needed:
* The [DailyPlanet](https://github.com/VanderDev1/DailyPlanet) starter app
* **Xcode**

1. Share your favorite Xcode techniques for finding out the capabilities, behaviors and requirements of these iOS networking constructs found in the **DailyPlanet** starter app:
* `URLSession`
* `URLRequest`
- `SessionDataTask`
- `.resume()`


<!-- Add graphic and/o code samples -->

2. Briefly share with each other any other tips on how to use built-in Xcode features to work more efficiently. Ideas could include:
* how to navigate around your code
* how to debug behaviors in your own code (i.e., using `p` or `po`, `print()` statements, and so on)

#### TODO: As A Class (3 to 5 mins)

1. Volunteers share the most useful techniques learned

##### Key Questions:
 - [ ] **Q:** What does `.resume()` do?</br>
 - [ ] **Q:** When does the `.resume()` function execute?

<br />


## URLSession - An Overview

`URLSession` is the **key object** responsible for sending and receiving HTTP requests. It natively supports the data, file, ftp, http, and https URL schemes.

It creates an object that coordinates a group of related network data transfer tasks.</br></br>

![syntax](assets/full_urlsession_suite.png) </br>


**Important Note:** *Like most networking APIs, the URLSession API is highly asynchronous. See* Additional Resources *below for more info.*


### URLSessionConfiguration

Every URLSession instance is initialized using an `URLSessionConfiguration object`, which defines the behavior and policies to use when uploading and downloading data.

URLSessionConfiguration will also let you configure additional HTTP headers, timeout values, caching policies, and other session properties.

#### Three types:

URLSessionConfiguration objects come in 3 flavors:

1. **.default** - Creates a default configuration object that uses the disk-persisted global cache, credential and cookie storage objects. Can save cache or cookies to disk, credentials to the Keychain.
2. **.ephemeral** - Similar to `.default`, but all session-related data is stored in memory and will be gone once the session terminates.
3. **.background** -  Allows the session to perform upload or download tasks in the background, even if the app is suspended.

Here is a simple example of a declaration of a `URLSession` instance using the `.default` `URLSessionConfiguration` type:

``` Swift
let defaultSession = URLSession(configuration: .default)
```

### URLSessionTask

To do the real work of fetching data or downloading/uploading files, a session creates one or more *tasks.*

`URLSessionTask` is the *abstract class* which contains predefined network task behaviors.

#### Three types:
The three concrete subclasses of URLSessionTask which you will employ most often are:

1. **URLSessionDataTask** - Intended for short, often interactive server requests such as fetching data by sending HTTP requests (GET, POST, etc). Data tasks send and receive data using `NSData` objects.
2. **URLSessionUploadTask** - Similar to data tasks, but also send data (such as a file) from disk to web service, and they support background uploads while the app is no longer running.
3. **URLSessionDownloadTask** - Data from a remote service is retrieved in the form of a file and stored in a temporary location. Supports background downloads/uploads while app the isn't running.

*Diagram illustrating the relationship between the abstract parent class, URLSessionTask, and its 3 concrete implementations, along with a few of the functions inherited from the abstract parent class:*

![syntax](assets/urlsessiontask_with_subclasses.png) </br>


<!--  TODO: code samples here -->


<br>

<!-- ### The `.resume()` Function -->

<!-- Response data? - can be accessed via delegate of completion block -->

### Making HTTP GET Requests Using URLSessionDataTask


<!-- Add graphic and/o code samples -->


## In Class Activity I (20 min)

For this exercise we will use these 3 resources:
1.  https://httpbin.org's [public API tester](https://httpbin.org/#/Request_inspection/get_headers)
2. Nasa's public [Astronomy Picture of the Day API](https://api.nasa.gov/api.html)
3. The [DailyPlanet](https://github.com/VanderDev1/DailyPlanet) starter app

**Part 1 - Individual**

Using the `Request inspection` endpoint on httpbin.org's tester page, we will send a `GET /headers` *request* that will return the `request's HTTP headers` in its `Response body`.

<!-- Add graphic and/o code samples -->


1. First, examine httpbin.org's exceptional, easy-to-use [web service testing interface](https://httpbin.org) in a web browser on your laptop.
2. Next, expand the *Request inspection* dropdown and its *GET /headers* function. Press the `Try it out` and `Execute` buttons. In the *Responses* fields returned, pay particular attention to the `"headers":` node in the *Response body* field. Also notice the HTTP status code returned (success = 200).

*(Feel free to experiment a little with this interface when you have time.)*

<!-- Add graphic and/o code samples -->


**Part 2 - Individual**

1. Download and run the [DailyPlanet starter app](https://github.com/VanderDev1/DailyPlanet).

*(Don't worry if the main scene is a blank screen - we'll improve on that later!)*
2. Study the construction of its `fetchHeaderData()` function.
3. Compare your debug output with the results of same *GET /headers* request executed from your web browser.

<!-- Add graphic and/o code samples -->


**Part 3 - Individual**

**TODO:** Using the URLSession implementation steps covered so far, complete the implementation of the `fetchNasaDailyImage()` in the starter app and present Nasa's Astronomy Picture of the Day to your users.

**NOTES:**

At the time of this writing, Nasa's pic of the day was:
https://apod.nasa.gov/apod/image/1902/FoxFur_new_color_2048px.jpg

To get the latest pic of the day:
- Launch the demo URL in your browser:
https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY
- find the `hdurl` node and copy it into your iOS project

*For clues, see the URLSession implementation details of `fetchHeaderData()` function.*

**Part 4 - As A Class**

  Briefly discuss..

**Q:** Looking at the results of either of the 2 fetch functions, what is your guess about where the `response` object came from?
**Q:** Why do you think the developers at httpbin.org bothered to spent time creating a service that would return HTTP headers?
**Q:** Why was the `DispatchQueue.main.async` statement needed?

``` Swift
    if let data = data, let image = UIImage(data: data) {
                 DispatchQueue.main.async {

                       //TODO: Insert downloaded image into imageView
                 }
```


## JSON Serialization of HTTP Responses


## In Class Activity II (xx min)


<!-- Give students simple Deserialization -->


## Challenges

<!-- xxx -->

## Wrap Up (xx mins)

### Structured Sharing Exercise - Part 2**

At the end of class, turn in all your question sheets. We will use them in Part 3 of this exercises in the next class session.

### About Next class

- Part 3 of Structured Sharing Exercise
- The last 40 mins of next class will be dedicated for questions on material so far and/or on student projects.


## Challenges

<!-- xxx -->


## Additional Resources

1. [Slides]
2. [URL Loading System -- from Apple](https://developer.apple.com/documentation/foundation/url_loading_system)
< URLSession>
< URLSession Configuration >
3. [Asynchronicity and URL Sessions](https://developer.apple.com/documentation/foundation/urlsession)
[](https://stackoverflow.com/questions/45463996/how-does-urlsessiontask-run)

<!-- xxx -->
