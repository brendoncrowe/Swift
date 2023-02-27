# Get Request 
One of the most common HTTPMethods is a **GET** request, which is often used for a rest api (getting data from an online endpoint). 
REST is commonly used by people on web systems to interact with each other. For example, retrieving and updating account information in a social media application.

<ins>The URLSession class and related classes provide an API for downloading data from and uploading data to endpoints indicated by URLs.</ins>

Using the endpoint for a random joke, a get request is made in the following way...

### Code Overview in Playgrounds 
``` Swift
import UIKit

struct Joke: Decodable {
    let type: String
    let setup: String
    let punchline: String
    let id: Int
}

var dataTask: URLSessionDataTask?
var jokes = [Joke]()

let endpoint = "https://official-joke-api.appspot.com/jokes/programming/ten"
let url = URL(string: endpoint)!


dataTask = URLSession.shared.dataTask(with: url) { data, response, error in
    guard let data = data else {
        return
    }
    do {
        let jokeResults = try JSONDecoder().decode([Joke].self, from: data)
        jokes = jokeResults
        print(jokes)
    } catch {
        print("Error decoding jokes: \(error)")
    }
}
dataTask?.resume()

extension Joke: CustomStringConvertible {
    var description: String {
        return "\(setup)? \(punchline)!"
    }
}

```
### Output

[What goes after USA?? USB.!, What's the best part about TCP jokes?? I get to keep telling them until you get them.!, Why dot net developers don't wear glasses?? Because they see sharp.!, What is the most used language in programming?? Profanity.!, A male developer often gets called as a Dev, then what would you call a female developer?? Devi.!, There are 10 types of people in this world...? Those who understand binary and those who don't!, If you put a million monkeys at a million keyboards, one of them will eventually write a Java program? the rest of them will write Perl!, Why do programmers always mix up Halloween and Christmas?? Because Oct 31 == Dec 25!, Lady: How do I spread love in this cruel world?? Random Dude: [...💘]!, What did the router say to the doctor?? It hurts when IP.!]

