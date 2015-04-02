---
layout: post
title: Typed TableViewController回顾(Typed Table View Controllers Redux)
description: 在上一节中我们实现了一个绑定类型的tableView，这一节将会用更多的代码实现来尝试同样的事情。之前准备前一节的内容的时候，我们是根据一些手头的代码开始的。不过直到发布前，我们意识到应该让它更简单一些。经过一些重构之后，它编译正常，不幸的是它存在运行时崩溃，这一节用更多的代码来进行说明，开始吧...
keywords: Typed TableViewController回顾,Typed Table View Controllers Redux
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[Typed Table View Controllers Redux](http://www.objc.io/snippets/22.html)』

在上一节中我们实现了一个绑定类型的tableView，这一节将会用更多的代码实现来尝试同样的事情。之前准备前一节的内容的时候，我们是根据一些手头的代码开始的。不过直到发布前，我们意识到应该让它更简单一些。经过一些重构之后，它编译正常，不幸的是它存在运行时崩溃，这一节用更多的代码来进行说明，开始吧。

上一节的问题在于用想实现一个继承自非常规类的常规类。所以，我们这次实现一个非常规子类，并通过一个封装函数来保证类型安全。我们的tableViewController有两个变量：一个用来持有items，另一个是通过给定的item来配置cell的block；而必要的dataSource协议的实现很简单，跟前一节一样。

	class MyTableViewController: UITableViewController {
	    var items: [Any] = []
	    var configureCell: (UITableViewCell, Any) -> () = { _ in }
	    
我们通过一个常规方法来对该非常规类封装。它有两个入参：一个包含所有item的list和一个配置cell的block。像下边这样：

	func tableViewController<A>(items: [A],
	                            configure: (UITableViewCell, A) -> ())
	                         -> UITableViewController {
	                         
因为我们要处理一个任意类型的Array，我们需要box所有的item：

	var vc = MyTableViewController(style: UITableViewStyle.Plain)
	vc.items = items.map { Box($0) }
	
在cell的配置函数里，我们检查传入的数据是否是我们期望的类型，然后传入配置函数：

	vc.configureCell = { cell, obj in
	        if let value = obj as? Box<A> {
	            configure(cell, value.unbox)
	        }
	    }
	    return vc
	}

现在创建一个展示数字的简单tableViewController只需要一个函数调用：

	let rootViewController = tableViewController([1,2,3]) { cell, num in
	    cell.textLabel?.text = "Cell \(num)"
	}

如果我们想展示一个文件夹里的所有文件，可以这样：

	let fm = NSFileManager.defaultManager().
	if let files = fm.contentsOfDirectoryAtPath(path, error: nil) as? [String] ?? []
	let rootViewController = tableViewController(files) { cell, file in
	    cell.textLabel?.text = file
	}

尽管这种实现较之前逻辑稍多一些，不过我们还是通过一个类型安全的api对非类型安全的现存类进行封装，达到了同样的效果。全量的代码可以在[这里](https://gist.github.com/chriseidhof/892c1a574d1f3975c697)找到。