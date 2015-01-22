Applicative Functors
----
>文章翻译自[objc](http://www.objc.io)，原文连接：『[Applicative Functors](http://www.objc.io/snippets/7.html)』

在之前的[『Currying』](http://junkor.github.io/2015/01/Currying/)我们有这样一个用于登录验证的例子：

	func login(email: String, pw: String, success: Bool -> ())
	
有可能经常碰到这种情况，作为可选的输入的参数，需要做一些必要的验证跟提示，例如：

	if let email = getEmail() {
    	if let pw = getPw() {
        	login(email, pw) { println("success: \($0)") }
    	} else {
        	// error...
    	}
	} else {
    	// error...
	}

有一种更优雅的方式，就是使用applicative functors，其实swift中的optional类型就是applicative functors的一种实现(we can make use of the fact that optionals in Swift are an instance of applicative functors.)，听起来有点费解，看例子。像applicative functors，例如optional类型，我们可以声明<*>操作符：

	infix operator <*> { associativity left precedence 150 }
	func <*><A, B>(lhs: (A -> B)?, rhs: A?) -> B? {
    	if let lhs1 = lhs {
        	if let rhs1 = rhs {
            	return lhs1(rhs1)
        	}
    }
    	return nil
	}
	
简而言之，如果操作符左右两侧元素都不为nil的话，该操作符作用于右侧的元素，并把它传递给左侧的函数。用这个操作符并配合我们之前讲过的[『Currying』](http://junkor.github.io/2015/01/Currying/)可以重写上边的例子：

	if let f = curry(login) <*> getEmail() <*> getPw() {
   		 f { println("success \($0)") }
	} else {
    	// error...
	}
更漂亮简洁了，对吧？当然，刚开始看上去可能还是比较古怪，不过你可以多想想怎样把它应用到其他的场景。