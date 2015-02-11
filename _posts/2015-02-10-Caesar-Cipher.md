---
layout: post
title: 凯撒密码(Caesar ciphers)
description: 今天我们对凯撒密码用swift做一个简短的实现。它是通过对字符串中所有字母加一个固定偏移实现的简单的加密技术。首先，我们要写一个工具函数对string进行map操作，作用于每一个字符的数值...
keywords: 凯撒密码,Caesar ciphers
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[Caesar ciphers](http://www.objc.io/snippets/17.html)』

今天我们对[凯撒密码](http://en.wikipedia.org/wiki/Caesar_cipher)用swift做一个简短的实现。它是通过对字符串中所有字母加一个固定偏移实现的简单的加密技术。首先，我们要写一个工具函数对string进行map操作，作用于每一个字符的数值：

	func mapScalarValues(s: String, f: UInt32 -> UInt32) -> String {
	    let scalars = Array(s.unicodeScalars)
	    let encrypted = scalars.map { x in
	        Character(UnicodeScalar(f(x.value)))
	    }
	    return String(encrypted)
	}

通过对每个字符的数值加7来实现一个简单的凯撒密码变体：

	func caesar(plainText: String) -> String {
    	return mapScalarValues(plainText) { x in x + 7 }
	}
	

我们还可以用这个函数简单的实现[ROT-13](http://zh.wikipedia.org/wiki/ROT13),ROT-13,是一种只对大写字母进行操作的加解密算法：

	func rot13(plainText: String) -> String {
	    let A: UInt32 = 65
	    let Z: UInt32 = 90
	    return mapScalarValues(plainText) { x in
	        if x >= A && x <= Z  {
	            return A + ((x + 13) % 26)
	        }
	        return x
	    }
	}

对于很多数据结构，map都是一种有效的操作，例如：array、optional，还有我们今天讲的对字符串中字符数值的操作。