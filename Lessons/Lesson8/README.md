<!-- Run this slideshow via the following command: -->
<!-- reveal-md README.md -w -->


<!-- .slide: class="header" -->

# Requests with Authentication

## [Slides](https://make-school-courses.github.io/MOB-1.3-Dynamic-iOS-Apps/Slides/Lesson8/README.html ':ignore')

<!--

## Initial activity - 10 min

Open your project from last class: The Pokedex

- Show each other's final result.
- In pairs discuss how you coded your solution and share best practices or tips you found along the way.
- If you are blocked with the assignment see if your peer can unblock you.

-->

<!-- > -->

## Why you should know this

As the use of the **HTTP protocol** evolved, so have the efforts of hackers trying to exploit it.

In fact, HTTP is such an insecure protocol that Apple has all but prohibited its use in iOS apps.

Over the last few releases of iOS, Apple has redesigned iOS's networking frameworks to work natively with **HTTP's secure counterpart: HTTPS.**

<!-- > -->

![https](assets/https.png)

And the primary difference between HTTPS and HTTP?

- *HTTPS requires Authentication.*

Every iOS developer *must* know how to implement HTTPS-based network calls with some sort of authentication.

<!-- > -->

Most mobile apps implement some form of user authentication. This logic is performed by a backend service, but it's still an important part of an app's architecture.

<!-- > -->

## Learning Objectives

1. Make authenticated API requests.
2. Distinguish between OAUTH and API Key security models.
3. Design an authentication flow for a mobile app.

<!-- > -->

<!--## Initial Exercise (15 min)

**Structured Sharing Exercise - Part 3 from last class**

- Come up and review the sheets from last class.
- Select the one or two sheets you agree with most.

In Groups of 3 - (8 min)
- Share your thoughts about your selection.

As A Class - (6 min)
- We'll choose 3 review topics. Volunteers to explain each.
-->

## Network Authentication for iOS

There can be a variety of options for securing network communications.

Here a two options:

- **OAUTH** - Is an "authorization framework that enables third-party applications to obtain limited access to a web service."
- **API Keys**

<!-- > -->

## Collaborative activity - 15 min

[Let's compare both](https://docs.google.com/presentation/d/1V86TRQuRhsrRWlR_aZPI6KV0ZRhGlHbj40MpfUCPbG8/edit?usp=sharing)

<!-- > -->

## How can we authenticate users in a mobile app? 📱

- Embedded login screen
- External login screen

<!-- > -->

**Embedded login screens** have been the go to option for many years.

❌  Credentials are managed by the app = security issues

✅  No screen-switching or delays when logging in

<!-- > -->

![embedded](assets/embedded.jpeg)

<!-- > -->

**External login screens** delegate the tasks of authenticating to a different application (FB, Google, Safari, etc.)

❌  Not as seamless

✅  Handling credentials can be made by another entity that's more specialized and secure

<!-- > -->

![embedded](assets/external.jpeg)

<!-- > -->

## Let's talk about tokens

Modern authentication flows use tokens.

"Tokens are specially crafted pieces of data that carry just information to either **authorize the user to perform an action**, or **allow a client to get additional information about the authorization process** (to then complete it)" - Auth0

<aside class="notes">
In other words, clients need to request a token first to then get access to resources.
</aside>

<!-- > -->

## Types of tokens

- 🔑 **Access Tokens**: carry the necessary info to access a resource directly

- 🔄 **Refresh Tokens**: carry the information necessary to get a *new* access token

<!-- > -->

## JWT

A [JSON Web token](https://tools.ietf.org/html/rfc7519) is a common way to represent token information.

- The data format is JSON.
- Carries common fields such as subject, issuer, expiration time, etc.

<!-- > -->

## Design an auth flow

Use a flow chart to explain how you would design the authorization flow of a mobile app that uses Access Tokens and Refresh Tokens to get a user's profile information.

[Jamboard](https://jamboard.google.com/d/1B-saJkcl3qKdbBOKVOHfoSi5PoMpRpTe_fIXlfgxgYA/edit?usp=sharing)

<!-- > -->

<!--

## PhotoMatic App

Resources needed:

- Download [The PhotoMatic starter app](https://github.com/VanderDev1/PhotoMatic_Starter.git)
- An API Key for the Flickr photo web service
- The Flickr webservice endpoint URL and the name of the method required to access Flickr's free library of Interesting Photos.


### Part 1

For this activity, we are going to access the Flickr photo sharing and management web service.

But we'll need to obtain an API KEY from Flickr. Here's how:


#### FIRST STEP: Create A Flickr Account

1. Access the [Flickr API main page](https://www.flickr.com/services/api/)

2. Then click on the API Keys URL to bring up the Login screen:

![syntax](assets/Flickr_API_page.png)


3. Find and select the `Not a Flickr member? Sign up here.` link at the bottom of the Login dialog box

- Then, click the `I want to create a new Yahoo email address` and **use your Make School email address.**


4. After you create your account, you will need the API Keys URL again to apply for a key.

5. When prompted with the selection page, choose "Apply For Non-Commercial Key"

...and you should be presented with your brand-new, Flickr API Key!


### Part 2 - As A class

For this exercise, we need:

- [The PhotoMatic starter app](https://github.com/VanderDev1/PhotoMatic_Starter.git)
- A valid Flickr API Key
- The base url for the Flickr API:
https://api.flickr.com/services/rest
- The Flickr method to call to download Interesting Photos: `flickr.interestingness.getList`

...and one student volunteer to "drive"


1. **STEP 1** - Run the starter app
- it will fail. Why?

2. **STEP 2** - Let's fix it 🔨


### Part 3 - In Pairs

In 5 minutes, examine the code in the [The PhotoMatic starter app](https://github.com/VanderDev1/PhotoMatic_Starter.git)

**Q** With respect to MVC, what do you notice about the code in this app?<br>
**Q** What might you do restructure it to optimize its readability, adherence to MVC, and so on?


## Challenge - 20 min

Add an **Activity Indicator** (aka, a "spinner") to the project so that while every images is being downloaded the user is presented with a spinner indicating the network request for each image is in progress.

-->

## Authorization in practice

- Complete the Moviefy tutorial which will use OAUTH and tokens to authenticate users.

<!-- > -->

## Additional Resources

1. [HTTPS image](https://www.cloudflare.com/learning/ssl/why-is-http-not-secure/)
2. [OAUTH](https://hackernoon.com/mobile-api-security-techniques-682a5da4fe10)
3. [Apple Sign in](https://developer.okta.com/blog/2019/06/04/what-the-heck-is-sign-in-with-apple)
4. [Auth0 post on Authentication](https://auth0.com/blog/using-centralized-login-to-add-authentication-to-your-ios-apps/)
5. [Auth0 post on Refresh Tokens](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)
