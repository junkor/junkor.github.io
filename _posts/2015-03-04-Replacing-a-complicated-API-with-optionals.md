---
layout: post
title: 用optionals取代复杂的API(Replacing a complicated API with optionals)
description: 有些时候对top-level的函数进行轻量的封装是很nice的，或者完成像在《Functional Programming in Swift》中描述的那样进行完全封装。这一节，我们来给NSScanner做一个拓展...
keywords: 用optionals取代复杂的API,Replacing a complicated API with optionals
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[Replacing a complicated API with optionals](http://www.objc.io/snippets/19.html)』

有些时候对top-level的函数进行轻量的封装是很nice的，或者完成像在《Functional Programming in Swift》中描述的那样进行完全封装。这一节，我们来给NSScanner做一个拓展。

如果细想一下NSScanner,它的swift API有点不对劲。例如想要扫描一个Int值，我们要调用下边函数：

	func scanInteger(result: UnsafeMutablePointer<Int>) -> Bool

是objective-c的话，这个API是有意义的(传入一个integer指针，如果返回值是YES，然后去读取对应的数值)。在swift里，可以通过使用optional来对原接口进行封装：

	extension NSScanner {
	    func scanInteger() -> Int? {
	        var result: Int = 0
	        return scanInteger(&result) ? result : nil
	    }
	}

细看一下返回类型，是不是简单多了。函数要返回integer或者nil。这样就不需要去翻文档看参数指针或是返回值bool的使用方法了。