# Star Wars API

Resources needed:
* [The Star Wars API (aka, SWAPI)](https://swapi.co)


## PART 1

Use the DailyPlanet app as a "canvas" to practice deserializing JSON from an HTTP request:

- Create and call a new function that fetches data from SWAPI's `/starships/` endpoint: https://swapi.co/api/starships/
- In your data tasks' completion handler, **convert** the returned `data` object to JSON, and **print** your converted `jsonObject` to the debug console.
- If time permits, handle the HTTP `error` object returned.

## PART 2

**Create A Paginating Table View App with JSON Data:**

Using the `/people/` endpoint on the [SWAPI](https://swapi.co) web service, create a **table view** app with **pagination** that:
- Uses a custom cell to present the `"name"` and 2 other properties/items (i.e.. `"height"`) from the JSON response returned
- When scrolled to the end of the currently available data, the app must present the user with the option to imageView the `next` or `previous` set of data (i.e., pagination)
