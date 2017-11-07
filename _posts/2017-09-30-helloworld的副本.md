//
//  MasterController.swift
//  BodyBuilding
//
//  Created by 14110100912 on 2017/9/30.
//  Copyright © 2017年 xsh. All rights reserved.
//

import UIKit

class MasterController: UIViewController,UITableViewDelegate,UITableViewDataSource {
var tableView:UITableView!
var list:NSArray?

override func viewDidLoad() {
super.viewDidLoad()
viewConfig()

DispatchQueue.global(qos: .default).async {
self.getList()
}
}



func viewConfig(){
view.backgroundColor = UIColor.lightGray

let label = UILabel(frame: CGRect(origin: .zero, size: CGSize(width: 100, height: 20)))
label.font = UIFont.boldSystemFont(ofSize: label.bounds.height)
label.text = "大师列表"
label.center = CGPoint(x: view.center.x, y: 40)
label.textColor = UIColor(red: 0/255, green: 0/255, blue: 0/255, alpha: 1)
label.textAlignment = .center
view.addSubview(label)

tableView = UITableView(frame: CGRect(x: 15, y: 60, width: WIDTH-30, height: HEIGHT))
tableView.register(ClubCell.self, forCellReuseIdentifier: "cell")
tableView.delegate = self
tableView.dataSource = self
tableView.isScrollEnabled = false
tableView.contentInsetAdjustmentBehavior = .never
tableView.backgroundColor = view.backgroundColor
tableView.tableFooterView = UIView()
view.addSubview(tableView)

}

func getList(){
let parameter:NSDictionary = ["SessionID":LoginModel.sessionID,"page":"1","mod":"master"]
Networking.shared.getMasterList(parameter: parameter) { (json) in
self.list = json
self.tableView.reloadData()
}
}

func numberOfSections(in tableView: UITableView) -> Int {
return 1
}

func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
return list?.count ?? 0
}

func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath) as! ClubCell
let dict = list![indexPath.row] as! NSDictionary

cell.imageView?.sd_setImage(with: URL(string: BB_IMAGEURLPRE+(dict.value(forKey: "thumb") as! String)), placeholderImage: UIImage(named:"placeholder"))
cell.textLabel?.text = dict.value(forKey: "realname") as? String

return cell
}

func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
let controller = MasterDetailController()
controller.infoDict = list![indexPath.row] as! NSDictionary

hidesBottomBarWhenPushed = true
_ = navigationController?.pushViewController(controller, animated: true)
hidesBottomBarWhenPushed = false
}

func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
return (HEIGHT-69)/3
}

override func didReceiveMemoryWarning() {
super.didReceiveMemoryWarning()
}
}
