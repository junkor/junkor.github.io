---
layout: post
title: 配置参数(Configuration Values)
description: 当你处理一个配置对象时，一般都会处理多个配置属性。例如，当要配置一个TableView的时候，可以用一个配置对象来保存多个显示参数。假设我们的应用中有两类TableView：很普通的plain样式的（没有header、背景、footer）和高度定制的（有header和footer）.第一步我们需要声明一个结构来存储我们的自定义选项...
keywords: 配置参数,Configuration Values
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[Configuration Values](http://www.objc.io/snippets/20.html)』

当你处理一个配置对象时，一般都会处理多个配置属性。例如，当要配置一个TableView的时候，可以用一个配置对象来保存多个显示参数。假设我们的应用中有两类TableView：很普通的plain样式的（没有header、背景、footer）和高度定制的（有header和footer）.第一步我们需要声明一个结构来存储我们的自定义选项：

	struct TableViewConfiguration {
	    var headerView: UIView?
	    var backgroundView: UIView?
	    var footerView: UIView?
	}

因为我们没有TableView的源码，我们通过extension的方式用一个TableViewConfiguration来配置一个TableView.

	extension UITableView {
	    func configure(configuration: TableViewConfiguration) {
	       tableHeaderView = configuration.headerView
	       backgroundView = configuration.backgroundView
	       tableFooterView = configuration.footerView
	    }
	}

现在，我们可以创建两个配置对象来集中配置我们的自定义TableView了：

	let defaultConfiguration = TableViewConfiguration(headerView: nil, 
	                                                  backgroundView: nil,
	                                                  footerView: nil)
	let fancyConfiguration = TableViewConfiguration(headerView: myHeader, 
	                                                backgroundView: nil,
	                                                footerView: myFooter)
	                                                
这样就可以在我们的代码中随心使用了，因为我们声明的是let类型的常量，不可修改的。并且，它的属性是var修饰的，所以我们可以copy一份出来进行自定义配置：

	var myConfig = fancyConfiguration
	myConfig.backgroundView = ...