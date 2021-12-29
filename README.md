# CS50-final-project
My final project for EDx CS50 is a to do list swift
####Video Demo: https://youtu.be/ft5ygnvrvK8 
####Description: 
my chosen project is a to do app. I hoped to learn a new skill (coding) throughout this course, which I believe I was able to achieve to a certain extent. I found out about this course and was thrilled that this was accessible to anyone, whether a beginner or not. Unfortunately, I did not completely allocate my time effectively and chose a not so perfect time to start this project (nearly the end of the year and a start to alevels). I am grateful for the skill I have acquired throughout this course and I hope to further advance my knowledge in coding throught the next year. Maybe next year I will even apply to Harvard for a computer science degree ;)


//
//  ViewController.swift
//  To Do CS50
//
//  Created by Dara Shubay on 21/12/2021.
//

import UIKit
//aa help i have never coded before
class ViewController: UIViewController, UITableViewDataSource {

    private let table: UITableView = {
        let table = UITableView ()
        table.register(UITableViewCell.self,
                    forCellReuseIdentifier: "cell")
        return table
    }()
    
    var items = [String]()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        self.items = UserDefaults.standard.stringArray(forKey: "items") ?? []
            title = "To Do List"
        view.addSubview(table)
        table.dataSource = self
        navigationItem.rightBarButtonItem = UIBarButtonItem(barButtonSystemItem: .add,
                                                            target: self, action:
                                                                #selector(didTappAdd))
    }
    
    
    @objc private func didTappAdd() {
        let alert = UIAlertController(title: "New Item", message: "Enter new To Do list item", preferredStyle: .alert)
        alert.addTextField { field in
            field .placeholder = "Enter item..."
        }
        alert.addAction(UIAlertAction(title: "Cancel", style: .cancel, handler: nil))
        alert.addAction(UIAlertAction(title: "Done", style: .default, handler: { [weak self]
            (_) in
            if let field = alert.textFields?.first {
                if let text = field.text, !text.isEmpty {
                    
                    // Enter new To Do list sentence
                    DispatchQueue.main.async {
                        var currentItems = UserDefaults.standard.stringArray(forKey: "items")
                            ?? []
                        currentItems.append(text)
                        UserDefaults.standard.setValue(currentItems, forKey: "items")
                        self?.items.append(text)
                        self?.table.reloadData()
                    }
                }
            }
        }))
        
        present(alert, animated: true)
    }
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        table.frame = view.bounds
    }
    

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return items.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        
        cell.textLabel?.text = items[indexPath.row]
        return cell
        
    }
    
    func tableView(_ tableView: UITableView, editingStyleForRowAt indexPath: IndexPath) -> UITableViewCell.EditingStyle {
        return .delete
    }
    
    func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {
        if editingStyle == .delete {
            tableView.beginUpdates()
            items.remove(at: indexPath.row)
            tableView.deleteRows(at: [indexPath], with: .fade)
            tableView.endUpdates()
        }
    }
    
}
