Permutations
----
这一节，我们来看一下怎样为数组中的所有元素生成permutations。首先，我们需要声明一个辅助函数between。它用来计算在已知序列ys中插入元素x的所有排列方式。我们可以用一个for循环来实现，不过这里，我们用map、decompose和递归来实现：

	func between<T>(x: T, ys: [T]) -> [[T]] {
    	if let (head, tail) = ys.decompose {
       	 return [[x] + ys] + between(x, tail).map 	{ [head] + $0 }
    	} else {
       	 return [[x]]
    	}
	}
	
如果ys是一个空数组，就只有一种排列方式，即[[x]]。当ys不是空数组的时候，我们即可以将x插入在数组的首位([[x]+ys])或者数组尾部的其他地方。