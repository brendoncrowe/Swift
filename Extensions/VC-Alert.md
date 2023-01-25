### Extension on UIViewController to make alerts easily accessible 

``` Swift 
extension UIViewController {
  func showAlert(title: String, message: String, completion: ((UIAlertAction) -> Void)? = nil) {
    let alertController = UIAlertController(title: title, message: message, preferredStyle: .alert)
    let okAction = UIAlertAction(title: "Ok", style: .default, handler: completion)
    alertController.addAction(okAction)
    present(alertController, animated: true, completion: nil)
  }
}


// completion is optional and set to nil
// make use of this if you want to do something after the showAlert is done, which is invoked when the ok is clicked.
// UIAlertAction: alert action

```
