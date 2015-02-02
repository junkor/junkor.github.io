---
layout: post
title: Phantom Types
description: 有时，当在我们的app里处理重要的类型数据时需要额外的注意。假设你的应用核心是文件处理，并且想保证在打开并读取文件的时候，不能进行任何的写操作。第一步，我们可以对NSFileHandle进行包装，给他一个额外的参数...
keywords: Phantom Types
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[Phantom Types](http://www.objc.io/snippets/13.html)』

有时，当在我们的app里处理重要的类型数据时需要额外的注意。假设你的应用核心是文件处理，并且想保证在打开并读取文件的时候，不能进行任何的写操作。第一步，我们可以对NSFileHandle进行包装，给他一个额外的参数：

	struct FileHandle<A> {
    	let handle: NSFileHandle
	}
	
我们可以添加两个空的struct，虽然他们的数值没有任何意义，但我们可以用它们所代表的不同类型来存储一些信息：

	struct Read {}
	struct Write {}

用刚刚定义的这些类型，我们可以创建更多的wrapper，但这次，我们在初始化NSFileHandle时，我们会用到Read和Write类型，尽管FileHandle并未包含这样类型的数值。知道为什么参数A被称作Phantom Type了么？因为我们只他们来区分文件操作的类型（并不关心它具体的数值，只区分类型）。

	func openForReading(path: String) -> FileHandle<Read>? {
    	return NSFileHandle(forReadingAtPath: path).map { FileHandle(handle: $0) }
	}

	func openForWriting(path: String) -> FileHandle<Write>? {
    	return NSFileHandle(forWritingAtPath: path).map { FileHandle(handle: $0) }
	}
	
我们声明的FileHandle类型很漂亮的记录了发生了什么。现在，你可以写一个只用于文件读取的函数了：

	func readDataToEndOfFile(handle: FileHandle<Read>) -> NSData {
    	return handle.handle.readDataToEndOfFile()
	}

试图掉用该函数对一个用于写的文件执行读操作的时候，编译将无法通过。你也可以把这个技巧用于你的代码中比较重要的部分，总有些地方你希望错误发生在编译时而不是运行时。
