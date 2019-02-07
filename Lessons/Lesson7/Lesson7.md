# Network Requests with URLSession

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**                        |
| ----------- | --------- | ----------------------------------- |
| 0:00        | 0:05      | Objectives                          |
|

## Why you should know this
Nearly every professional-level iOS app communicates at some point with an Internet component such as a remote web service.

Fetching and processing data between an app and a web service is an essential skill that all iOS developers must master.

And the most commonly-used protocol for communicating with web servers is **HTTP** (and its secure counterpart, HTTPS).

Since iOS 7, Apple has provided **URLSession** and its family of related classes as **a complete networking API** for uploading or downloading content via HTTP/S.

<!--
**HTTP** and **HTTPS** are robust and stable protocols. They have been widely used in web browsers for a long time. They offer several performance and security advantages, as well as a mature base of easy-to-use development and analysis tools.

to make GET and POST network requests...
-->

## Class Learning Objectives/Competencies (5 min)
At the end of this class, you should be able to...

1. xxx

## Initial Exercise (5 min)
xxxx


## URLSession - An Overview


<!-- Add graphic and/o code samples -->


## In Class Activity I (20 min)

For this exercise we will use these 3 resources:
1.  https://httpbin.org's [public API tester](https://httpbin.org/#/Request_inspection/get_headers)
2. Nasa's public [Astronomy Picture of the Day API](https://api.nasa.gov/api.html)
3. The [DailyPlanet](https://github.com/VanderDev1/DailyPlanet) starter app

**Part 1 - Individual**

Using the `Request inspection` endpoint on httpbin.org's tester page, we will send a `GET /headers` *request* that will return the `request's HTTP headers` in its `Response body`.

1. First, examine httpbin.org's exceptional, easy-to-use [web service testing interface](https://httpbin.org) in a web browser on your laptop.
2. Next, expand the *Request inspection* dropdown and its *GET /headers* function. Press the `Try it out` and `Execute` buttons. In the *Responses* fields returned, pay particular attention to the `"headers":` node in the *Response body* field. Also notice the HTTP status code returned (success = 200).

*(Feel free to experiment a little with this interface when you have time.)*

**Part 2 - Individual**

1. Download and run the [DailyPlanet starter app](https://github.com/VanderDev1/DailyPlanet).

*(Don't worry if the main scene is a blank screen - we'll improve on that later!)*
2. Study the construction of its `fetchHeaderData()` function.
3. Compare your debug output with the results of same *GET /headers* request executed from your web browser.

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

1. Where did the `response` object come from?
2. Why was the `DispatchQueue.main.async` statement needed?

``` Swift
    if let data = data, let image = UIImage(data: data) {
                 DispatchQueue.main.async {

                       //TODO: Insert downloaded image into imageView
                 }
```
