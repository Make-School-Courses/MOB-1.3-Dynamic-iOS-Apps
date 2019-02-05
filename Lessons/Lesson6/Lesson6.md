# JSON and Swift

## Minute-by-Minute [OPTIONAL]

| **Elapsed** | **Time**  | **Activity**              |
| ----------- | --------- | ------------------------- |
| 0:00        | 0:05      | Objectives                |
| 0:05        | 0:05      | Initial Exercise          |
| 0:10        | 0:10      | JSON review               |
| 0:20        | 0:10      | In Class Activity I       |
| 0:30        | 0:10      | Reading data from a file  |
| 0:40        | 0:15      | Encoding and Decoding     |
| 0:55        | 0:10      | BREAK                     |
| 1:05        | 0:05      | In Class Activity II      |
| 1:10        | 0:10      | In Class Activity III     |
| 1:20        | 0:25      | Challenge                 |
| 1:45        | 0:05      | Wrap Up                   |
| TOTAL       | 1:50      |                           |

# Why you should know this

Every client application receives information from a common server. It doesn't matter how the implementation of handling the data happens, it has a standard format that can be recognized by every client app. Years ago XML(1996) was widely used for this purpose. But recent trends point that JSON(2002) has become the most popular data-interchange format and that it will continue to dominate the web.

## Class Learning Objectives/Competencies (5 min)

1. Identify JSON structures and create your own
1. Reading data from a file
1. Deserialize a JSON file
1. Turning data into objects using the Codable protocol

## Initial Exercise (5 min)

### Swift 5 Release Notes
- Raw strings
- Handling future enum cases
- Flattening nested optionals
- Checking for integer multiples
- compactMapValues()

[What's new in Swift 5?](https://www.hackingwithswift.com/articles/126/whats-new-in-swift-5-0)

## JSON quick intro/review (10 min)

JSON stands for JavaScript Object Notation. It is a lightweight data-interchange format that allows us to read and write data that machines can also generate and parse easily.

Two main structures:
- **Object:** Collection of name/value pairs.
- **Array:** Ordered list of values.

An object begins with a left brace and ends with a right brace. Each name is followed by colon and the pairs are separated by commas.

![object](assets/object.gif)

An array begins with a left bracket and ends with a right bracket. Values are separated by commas.

![array](assets/array.gif)

Values can be strings in double quotes, numbers, true, false, null, objects or arrays. Also, structures can be nested.

![value](assets/value.gif)

A string is a sequence of 0 or more characters in double quotes.

![string](assets/string.gif)

Numbers are regular numbers like any language.
![number](assets/number.gif)

In the end, JSON is a way of communicating using very specific rules that every entity involved can agree on.

### Example
```
{
   "festivals":[
      {
         "name":"Austin City Limits"
      },
      {
         "name":"Riot Fest"
      },
      {
         "name":"Coachella"
      }
   ]
}
```

## In Class Activity I (10 min)

Without using a format validator, create your own JSON file to represent a list of festivals. Include two festival entries. They do not have to be real.

Consider you have this structs as reference:

```Swift
struct Participant{
    let name: String
    let id: String
}

struct Country{
    let name: String
    let id: String
}

enum FestivalType{
    case music
    case food
    case cinema
}

struct Festival{
    let year: String
    let type: FestivalType
    let date: String
    let lineup : [Participant]
    let country: Country
}
```

After your'e done writing the structure use the [JSON validator](https://jsonformatter.curiousconcept.com) to make sure your format is correct. If not, do the necessary adjustments.

## Reading data from a file (10 min)

Data can sometimes be stored in text files inside our app bundle. To retrieve them there is a particular way of doing it. We can either look for a path or a URL. Today we're going to use the path. The call is the following:

```
func path(forResource name: String?, ofType extension: String?) -> String?
```
`name` is the name of the resource file
`extension` is the filename extension of the files to locate

The return value will be the full pathname for the resource or `nil` if the file could not be located.

```
let path = Bundle.main.path(forResource: "fileName", ofType: ".json")
if let path = path {
    let url = URL(fileURLWithPath: path)
    print(url)
}
```

## Getting the data from JSON

```
let contents = try? Data(contentsOf: url, options: .alwaysMapped)
let jsonResult = try? JSONDecoder().decode([].self, from: contents!)
// print(jsonResult)
```
We can now see all of the data we retrieved from the file.
If we were to create an object for each entry and save them in an array, before Swift 4, we would need to retrieve properties one by one. This is why external libraries like SwiftyJSON became popular, they made the job less complicated.

## Encoding and Decoding (15 min)

“Serializing a program’s internal data structures into some kind of data interchange format and vice versa is one of the most common programming tasks. Swift calls these operations encoding and decoding. One of the headline features of Swift 4 is a standardized design for encoding and decoding data that all custom types can opt into.”

Excerpt From: Chris Eidhof. “Advanced Swift.”

### The Codable system has 3 main goals:

##### Universality
Should work with structs, enums and classes.

##### Type safety
JSON is often weakly typed, but we're working with Swift so our code works with strongly typed data structures.

##### Reducing boilerplate
Writing less code, while the compiler generates what we need automatically.

Types conform to the **Encodable** and/or **Decodable** protocols to state their ability to encode itself and creating and instance from serialized data.

Most types will adapt both, so the standard library came up with a typealias for both: **Codable** protocol.

```
public typealias Codable = Encodable & Decodable
```

When we have a value that conforms to the Codable protocol we can:
- Create an *encoder* to serialize the value into JSON. (JSONEncoder is built-in in Swift)
- Create a *decoder* that takes the serialized data and turns it into an instance of the original type. . (JSONDecoder is built-in in Swift)

Note: Error handling is important during decoding. Many things can go wrong (missing fields, type mismatches, corrupted data, etc.)


```Swift
struct Location: Codable {
    var name: String
    var distance: Double
}

struct Park: Codable{
    var name: String
    var location: Location
}

let parks:[Park] = [Park(name: "Universal Studios", location: Location(name: "Florida", distance: 100.0)), Park(name: "Disneyland", location: Location(name: "California", distance: 200.0)), Park(name: "Cedar Point", location: Location(name: "Ohio", distance: 300.0))
]

//Encoding

do {
    let encoder = JSONEncoder()
    let jsonData = try encoder.encode(parks)
    let jsonString = String(decoding: jsonData, as: UTF8.self)
    print(jsonString)
} catch {
    print(error.localizedDescription)
}

//Decoding

do {
    let encoder = JSONEncoder()
    let jsonData = try encoder.encode(parks)

    let decoder = JSONDecoder()
    let decoded = try decoder.decode([Park].self, from: jsonData)

    for park in decoded{
        print(park.name)
    }

} catch {
    print(error.localizedDescription)
}
```
### Encoding
1. Initiate the encoding process
1. Encoder calls `encode(to:Encoder)` method passing itself as the argument.<br>
   Ex. `let jsonData = try encoder.encode(parks)`<br> which in result calls `parks.encode(to:self)``
1. In order to encode itself in the correct format, it will use *encoding containers* <br>The Encoder protocol returns these containers.
 ##### Types of containers
 - Keyed container: Encodes key value pairs. Will turn into JSON objects.
 - Unkeyed container: Encodes multiple values sequentially, omitting keys. Will turn into JSON arrays.
 - Single value container: Encodes a single value. Will turn into numbers, strings, booleans or null.
1. The compiler generates an enum named `CodingKeys`. It contains one case for every stored property of the struct.

Strongly typed keys lead to safety but they must eventually be able to turn into strings/integers. The CodingKey protocol handles this task.

### Decoding
1. call `try decoder.decode([Park].self, from: jsonData)`
1. Decoder creates an instance of the type passed in. What is it in this case?
1. Decoder manages a tree of *decoding continers* where each value travels down recursively in a container hierarchy while initializing values.

If at some point in the hierarchy, an error occurs, tje entire process fails.

### Coding Keys
We can control how a type encodes itself by writing a custom CodingKeys enum.

- Rename fields by giving string values
- Skip fields by omitting keys

## In Class Activity II (5 min)

How will the same `Park` struct look like if we're using custom Coding Keys and we're expecting the key of the `name` value to come with the label `state` in the JSON object?<br>
Write the struct.

How will the same `Park` struct look like if we're using custom Coding Keys and want to ignore the value for `distance`?
Write the struct.

## In Class Activity III (10 min)
Go back to the film locations example and musing the Codable protocol make sure to save this properties only:
```Swift
var firstActor: String
var location: String
var productionCompany: String
var releaseYear: String
var title: String
```

## Challenge (20 min)
Take the JSON file [Festivals]() and create a project where you decode the contents to display The name of the Festival, the date and the number of artists in the lineup in a TableViewCell.

## Wrap Up (5 min)
**Q:** How does the encoding process work?<br>
**Q:** What does the compiler handle when using Codable?<br>
**Q:** How does the decoding process work?<br>
- Start Product Hunt tutorial

## Additional Resources

1. [XML vs JSON](https://www.cs.tufts.edu/comp/150IDS/final_papers/tstras01.1/FinalReport/FinalReport.html)
1. [JSON documentation](http://www.json.org)
1. [JSON format validator](https://jsonformatter.curiousconcept.com)
1. [Film locations](https://data.sfgov.org/Culture-and-Recreation/Film-Locations-in-San-Francisco/yitu-d5am)
1. [Codable sheet](https://www.hackingwithswift.com/articles/119/codable-cheat-sheet)
1. (Book) “Advanced Swift.” by Chris Eidhof
