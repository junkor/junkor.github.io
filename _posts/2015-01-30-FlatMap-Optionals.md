---
layout: post
title: FlatMap Optionals
description: 之前我们讲过给数组绑定函数（前边的『Flattening and Mapping Array』）的知识,有时称作flatMap。我们可以对optional做同样的事情，让我们来看看下边的两个dictionary...
keywords: FlatMap Optionals
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[FlatMap Optionals](http://www.objc.io/snippets/14.html)』

之前我们讲过给数组绑定函数（前边的[Flattening and Mapping Array](http://junkor.github.io/2015/01/Flattening-and-mapping-arrays/)）的知识,有时称作flatMap。我们可以对optional做同样的事情，让我们来看看下边的两个dictionary：

	let populations = ["Paris": 2243, "Madrid": 3216, "Amsterdam": 881, "Berlin": 3397]
	let capitals = ["France": "Paris", "Spain": "Madrid","The Netherlands": "Amsterdam", "Sweden": "Stockholm"]

如果我们想从dictionary中查找一个值，我们需要这样操作：

	func populationOfCapital(country: String) -> Int? {
    	if let capital = capitals[country] {
        	return populations[capital]
    	}
    	return nil
	}

我们也可以用[Map for Optionals](http://junkor.github.io/2015/01/Map-for-Optionals/)来实现，我们只需要一个单一的返回值，不过它却返回了一个嵌套的optional：

	func populationOfCapital2(country: String) -> Int?? {
    	return capitals[country].map { populations[$0] }
	}

对于这种使用函数链并需要一个optional返回值的场景，用flatMap是非常得心应手的：

	func flatMap<A,B>(x: A?, y: A -> B?) -> B? {
    	if let value = x {
        	return y(value)
    	}
    	return nil
	}
	
现在就可以重构我们的populationOfCapital为只有一个单一返回值了：

	func populationOfCapital3(country: String) -> Int? {
    	return flatMap(capitals[country]) { populations[$0] }
	}

此外，我们也可以用自定义操作符>>=，像我们之前在[]()中用的那样，用于链接调用多个函数。当你想在单行语句里使用多个flatMap操作的时候，使用操作符的优势就立刻彰显了：

	func populationOfCapital4(country: String) -> Int? {
    	return capitals[country] >>= { populations[$0] }
	}


