Flattening and mapping arrays
----
>文章翻译自[objc](http://www.objc.io)，原文连接：『[Flattening and mapping arrays](http://www.objc.io/snippets/4.html)』

在函数式编程中，在数组上map是个很常见的操作。一般情况下，那些需要flatten的数组map的结果是个数组。在很多函数式编程语言中，一般都会有一个操作符来实现flattening、然后mapping的操作。它map一个函数f，然后flatten结果数组到一个单一数组中：

	infix operator >>= {}
	func >>=<A, B>(xs: [A], f: A -> [B]) -> [B] {
    	return xs.map(f).reduce([], combine: +)
	}
	
这个操作符最酷的应用场景就是把多个列表进行组合。假设我们有一个列表表示扑克牌的牌码，一个列表表示扑克牌的花色：

	let ranks = ["A", "K", "Q", "J", "10",
             "9", "8", "7", "6", "5", 
             "4", "3", "2"]
	let suits = ["♠", "♥", "♦", "♣"]
	
现在，我们就可以通过遍历所以牌码，使其与可能组合的花色进行组合，就生成一个包含所以牌面的列表：

	let allCards =  ranks >>= { rank in
   		 suits >>= { suit in [(rank, suit)] }
	} 

	// Prints: [(A, ♠), (A, ♥), (A, ♦), 
	//

