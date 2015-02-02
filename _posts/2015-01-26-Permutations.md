---
layout: post
title: 排列(Permutations)
description: 这一节，我们来看一下怎样生成一个数组所有元素的排列组合。首先，我们需要声明一个辅助函数between。它用来计算在已知序列ys中插入元素x的所有排列方式。我们可以用一个for循环来实现，不过这里，我们用map、decompose和递归来实现...
keywords: 排列,Permutations
---

>文章翻译自[objc](http://www.objc.io)，原文连接：『[Permutations](http://www.objc.io/snippets/10.html)』

这一节，我们来看一下怎样生成一个数组所有元素的排列组合。首先，我们需要声明一个辅助函数between。它用来计算在已知序列ys中插入元素x的所有排列方式。我们可以用一个for循环来实现，不过这里，我们用map、decompose和递归来实现：

	func between<T>(x: T, ys: [T]) -> [[T]] {
    	if let (head, tail) = ys.decompose {
       	 return [[x] + ys] + between(x, tail).map 	{ [head] + $0 }
    	} else {
       	 return [[x]]
    	}
	}
	
如果ys是一个空数组，就只有一种排列方式，即[[x]]。当ys不是空数组的时候，我们即可以将x插入在数组的首位([[x]+ys])或者数组尾部的其他地方。下边是一个简单的调用示例--调用递归函数between(x, tail) 并且将x添加到每个decompose出来的tail数组前，从而组成所有x可插入的组合：

	between(0, [1, 2, 3])

	> [[0, 1, 2, 3], [1, 0, 2, 3], [1, 2, 0, 3], [1, 2, 3, 0]]

为了定义permutation操作，我们需要用到同样的模式：decompose数组，然后递归数组decompose出来的tail：

	func permutations<T>(xs: [T]) -> [[T]] {
    	if let (head, tail) = xs.decompose {
        	return permutations(tail) >>= { permTail in
            	between(head, permTail)
       	}
    	} else {
        	return [[]]
   	 	}
	}

如果xs为空，则只有一种排列[[]].如果xs不为空，我们先计算xs的tail的所有可能的排列，然后将原数组的head，即第一个元素，插入到通过调用between(head, permTail)生成的排列中。我们也可以使用map来实现，但它可能会返回[[[T]]]而不是[[T]]。最后，我们使用之前章节中自定义的flatmap操作符(>>=)将结果联合起来。

	permutations([1, 2, 3])

	> [[1, 2, 3], [2, 1, 3], [2, 3, 1],
   		[1, 3, 2], [3, 1, 2], [3, 2, 1]]
   		