## UIImageView Extension

``` Swift 
import UIKit

extension UIImageView {
    
    // instance method, works on an instance of UIImageView
    
    func getImage(with urlString: String,
                  completion: @escaping (Result<UIImage, AppError>) ->()) {
        
        // configure UIActivityIndicatorView
        let activityIndicator = UIActivityIndicatorView(style: .large)
        activityIndicator.color = .systemBlue
        activityIndicator.center = center // center implies center of UIImageView
        addSubview(activityIndicator)
        // this line of code adds the UIActivityIndicatorView to the UIImageView
        activityIndicator.startAnimating()
        
        // because NetworkHelper is capturing activityIndicator, you must declare a weak reference
        NetworkHelper.shared.performDataTask(with: urlString) { (result) in
            DispatchQueue.main.async {
                activityIndicator.stopAnimating()
            }
            switch result {
            case .failure(let appError):
                completion(.failure(.networkClientError(appError)))
            case .success(let data):
                if let image = UIImage(data: data) {
                    completion(.success(image))
                }
            }
        }
    }
}
```
