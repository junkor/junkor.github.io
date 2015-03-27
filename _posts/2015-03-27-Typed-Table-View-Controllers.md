---
layout: post
title: Typed TableViewController(Typed Table View Controllers)
description: 这周我们来品尝一下iOS开发的黄油和面包：TableViewController。第一步，我们创建一个UITableViewController的子类，并且包含一个普通的类型参数A...
keywords: Typed TableViewController
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[Typed Table View Controllers](http://www.objc.io/snippets/21.html)』

这周我们来品尝一下iOS开发的黄油和面包：TableViewController。第一步，我们创建一个UITableViewController的子类，并且包含一个普通的类型参数A：

	class MyTableViewController<A>: UITableViewController {

现在,我们可以添加一个arrsy型属性来存储列表的数据元素，当一个新值被设置时，tableView就会reload:
	
	var items: [A] = [] {
	    didSet { self.tableView.reloadData() }
	}

我们还需要一个配置cell的函数，入参是一个cell和一个A类型数据元素：

	var configureCell: (UITableViewCell, A) -> () = { _ in () }
	
最后，我们重写一下初始化函数(作为子类化的实现一般都需要这么做)，并对cellForRowAtIndexPath和numberOfRowsInSection做一个简单实现：

	override init(style: UITableViewStyle) {
        super.init(style: style)
    }

    override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: UITableViewCellStyle.Default, reuseIdentifier: nil)
        configureCell(cell, items[indexPath.row])
        return cell
    }

    override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return items.count
    }

欧了。现在我们可以在确保类型安全的情况下用一个tableView来展示A类型的数据列表了~

Update：不幸的是，上边的代码在运行时无法正常工作。具体的可用实现见[下一节](#)。
