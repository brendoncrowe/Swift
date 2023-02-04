# Table View

## 3 primary elements for a table view in a ViewController
There are two necessary methods needed to configure a table view as well as one variable
1. numberOfRowsInSection - how many rows are needed for data
2. cellForRowAt - the configuration of a dequeueable cell
3. Don't forget to set the view controller as the data source 

``` Swift
   override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return count
    }


      override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Food", for: indexPath)
       // remember to configure the cell content        
        return cell
    }
    
