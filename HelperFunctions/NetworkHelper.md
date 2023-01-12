### A Network Helper function that assits with a URLSession 


Newtork Helper is a class built around URLSession. The NetworkHelper wrapper class returns a Data object given a url String. This String can be an image url string or and API endpoint url String or any given GET url.

``` Swift 
enum NetworkError: Error {
    case badURL(String)
    case networkClientError(Error)
    case noResponse
    case noData
    case badStatusCode(Int)
    case decodingError(Error)
}

struct JokeAPIClient {
    static func getJoke(with urlString: String, completion: @escaping (Result<Joke, NetworkError>) ->()) {
        guard let url = URL(string: urlString) else {
            completion(.failure(NetworkError.badURL(urlString)))
            return
        }
        let dataTask = URLSession.shared.dataTask(with: url) { (data, response, error) in
            if let error = error {
                completion(.failure(.networkClientError(error)))
            }
            guard let urlResponse = response as? HTTPURLResponse else {
                completion(.failure(.noResponse))
                return
            }
            switch urlResponse.statusCode {
            case 200...299: break
            default:
                completion(.failure(.badStatusCode(urlResponse.statusCode)))
                return
            }
            guard let data = data else {
                completion(.failure(.noData))
                return
            }
            do {
                let joke = try JSONDecoder().decode(Joke.self, from: data)
                completion(.success(joke))
            } catch {
                completion(.failure(.decodingError(error)))
            }
        }
        dataTask.resume()
    }
}
