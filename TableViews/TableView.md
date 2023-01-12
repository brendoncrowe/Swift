# Table View

## Two primary elements for a Table View
There are two necessary methods needed to configure a table view...
1. numberOfRowsInSection - how many rows are needed for data
2. cellForRowAt - the configuration of a dequeueable cell

``` Swift
   override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return count
    }


      override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Food", for: indexPath)
       // remember to configure the cell content        
        return cell
    }
    
