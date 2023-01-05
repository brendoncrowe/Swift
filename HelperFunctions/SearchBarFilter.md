# Search bar filter helper function


The first thing to do in the function is to make sure that there is text in the search field. If there is none, the function will exit.

``` Swift
  guard !searchText.isEmpty else { return }
  ```

The second thing to do is to filter the headlines object, which is a property of the current view controller. </br> getHeadlines() is a static function that returns an array that can be passed to a class objet.

``` Swift 
func filterData(by searchText: String) {
  guard !searchText.isEmpty else { return }
  headlines = HeadlinesData.getHeadlines().filter { $0.title.lowercased().contains(searchText.lowercased()) }
  }

```

The final code falls under the search bar delegate method "textDidChange"

``` Swift 
  func searchBar(_ searchBar: UISearchBar, textDidChange searchText: String) {
        guard !searchText.isEmpty else {
            // if search text is empty here, original headlines are presented using loadData() method
            loadData()
            return
        }
        filterData(by: searchText)
    }
