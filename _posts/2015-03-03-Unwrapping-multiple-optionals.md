---
layout: post
title: unwrap多个optional值(Unwrapping multiple optionals)
description: Swift1.2带来了很多新的变化，这一节我们来看一下unwrap多个optional值。在前边的Applicative Functors中，我们通过使用applicative functors来避免嵌套的if语句。如果你还有印象的话，我们大概是从下边的代码开始的...
keywords: Unwrapping multiple optionals
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[Caesar ciphers](http://www.objc.io/snippets/18.html)』

Swift1.2带来了很多新的变化，这一节我们来看一下unwrap多个optional值。在前边的『[Applicative Functors](http://junkor.github.io/2015/01/22/Applicative-Functors.html)』中，我们通过使用applicative functors来避免嵌套的if语句。如果你还有印象的话，我们大概是从下边的代码开始的：

	if let email = getEmail() {
	    if let pw = getPw() {
	        login(email, pw) { println("success: \($0)") }
	    } else {
	        // error...
	    }
	} else {
	    // error...
	}

通过自定义的<*>操作符和使用curry操作，我们它重构成了下边的单个if语句的形式：

	if let f = curry(login) <*> getEmail() <*> getPw() {
	    f { println("success \($0)") }
	} else {
	    // error...
	}

然而，在swift1.2中，我们可以通过在一个if语句中同时拆解多个optional值的方式取代自定义操作符和curry的方式：

	if let email = getEmail(), pw = getPw() {
	   login(email, pw) { println("success: \($0)") }
	}

在上边的代码中，很轻松就能读懂发生了什么，还不用了解applicative functor的语法，语言的一小半，编码的一大步。