
import UIKit

class ExpandTableViewController: UIViewController , UITableViewDelegate,UITableViewDataSource {
    
    var inputTextField: UITextField?
    
    var tableViewData =
        ["One","Two","Three","Four","Five"]
        

    var array2 = [String]()
    var buttonArray = [Int]()
    
    var textData: String?
    var hiddenSections = Set<Int>()

    @IBOutlet var expandTableView: UITableView!
    override func viewDidLoad() {
        super.viewDidLoad()
       
        
        expandTableView.delegate = self
        expandTableView.dataSource = self
       
    }
    func numberOfSections(in tableView: UITableView) -> Int {
        
        return 2
    }
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    
        if self.hiddenSections.contains(section)
        {
                return 0
        }
        
            return tableViewData.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
       
        let cell = tableView.dequeueReusableCell(withIdentifier: "expandCell", for: indexPath) as! ExpandTableViewCell
        cell.label.text = tableViewData[indexPath.row]
       
        cell.editButton.tag = indexPath.row
        cell.editButton.addTarget(self, action: #selector(editButtonAction), for: .touchUpInside)
        return cell
    }
    
    @objc func editButtonAction(_sender: UIButton){
        let yotube = tableViewData[_sender.tag]
        let alert = UIAlertController(title: "Data Replace", message: "enter data below,\(yotube)", preferredStyle: .alert)
        let done = UIAlertAction(title: "Done", style: .default) { (action) in
            
            let textData1 = self.inputTextField?.text
                self.tableViewData[_sender.tag] = textData1!
                self.expandTableView.reloadData()
        }
        let cancel = UIAlertAction(title: "Cancel", style: .cancel, handler: nil)
        alert.addAction(cancel)
        alert.addAction(done)
        alert.addTextField(configurationHandler: inputTextField)
          self.present(alert, animated: true, completion: nil)
        
        
    }

    func tableView(_ tableView: UITableView, viewForHeaderInSection section: Int) -> UIView?
    {
      
        //header view
        let header = UIView(frame: CGRect(x: 0, y: 0, width: view.frame.size.width , height: view.frame.size.height))
        header.backgroundColor = .blue
        //section button
        let sectionButton = UIButton(frame: CGRect(x: 5, y: 10, width: 150, height: 40))
        
        sectionButton.setTitle("Button", for: .normal)
        sectionButton.backgroundColor = .clear
        //sectionButton.titleLabel?.textAlignment = .left
        sectionButton.tag = section
        sectionButton.addTarget(self,action: #selector(self.hideSection(sender:)), for: .touchUpInside)
        header.addSubview(sectionButton)
        //add button
        let add = UIButton(frame: CGRect(x: view.frame.size.width - 50 , y: 10, width: 40, height: 40))
        add.setTitle("Add", for: .normal)
        add.backgroundColor = .clear
        add.addTarget(self, action: #selector(addAction), for: .touchUpInside)
        header.addSubview(add)
      
        return header
    }
    
     func tableView(_ tableView: UITableView, heightForHeaderInSection section: Int) -> CGFloat {
        return 60
    }
    
    
    //Add Action Button
    @objc func addAction()
   {
        
        let alert = UIAlertController(title: "ADD", message: "Enter Below", preferredStyle: .alert)
        let submit = UIAlertAction(title: "Submit", style: .default) { (action) in
            print("hello")
            
            if let textData = self.inputTextField?.text , !textData.isEmpty
            {
                self.tableViewData.insert(textData, at: 0)
                self.expandTableView.reloadData()
               
            }
        
        }
        let cancel = UIAlertAction(title: "Cancel", style: .cancel , handler: nil)
        alert.addAction(submit)
        alert.addAction(cancel)
        alert.addTextField(configurationHandler: inputTextField)
        present(alert, animated: true, completion: nil)
    }
    func inputTextField(textField: UITextField) {
        inputTextField = textField
        inputTextField?.placeholder = "enter"
        inputTextField?.clearButtonMode = .whileEditing
    }
   
    func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {

        if editingStyle == .delete
        {
            tableViewData.remove(at: indexPath.row)
            expandTableView.reloadData()

        }

    }
  
   
   
    //create tag section
    @objc private func hideSection(sender: UIButton) {
        
        let section = sender.tag
       
        func indexPathsForSection() -> [IndexPath]
        {
               var indexPaths = [IndexPath]()
               
               for row in 0..<tableViewData.count
               {
                   indexPaths.append(IndexPath(row: row, section: section))
               }
               
               return indexPaths
        }
       
        if self.hiddenSections.contains(section) {
                self.hiddenSections.remove(section)
                self.expandTableView.insertRows(at: indexPathsForSection(),with: .fade)
            } else {
                self.hiddenSections.insert(section)
                self.expandTableView.deleteRows(at: indexPathsForSection(),with: .fade)
            }
        }
    
}
                                            
