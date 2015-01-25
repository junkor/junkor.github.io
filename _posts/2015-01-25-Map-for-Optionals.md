Map for Optionals
----
在之前我们已经讲过对Array执行map操作。map 操作把一个数组转换成一个具有同样数目元素的新数组，每个元素都经过了转换函数的处理，如：

	let urls: [NSURL] = [ /* a bunch of image URLs */ ]
	let images: [NSImage?] = urls.map { NSImage(contentsOfURL: $0) }
	
但map操作对象并不紧紧局限于数组。从一个更抽象的层面上讲，map是简单的从一个集合中取出一个数值，再对其执行转换操作，然后再放回到一个集合中去。例如，我们可以为一个optional类型数据定义一下操作（它已经存在于标准库中，但你还是可以很轻松的做自己的实现）：

	func map<A, B>(x: A?, f: A -> B) -> B? {
    if let x1 = x {
        return f(x1)
    }
    return nil
	}
	
假设我们想把一个oprional类型的URL转换成一个NSImage，一种方式是使用swift的optional binding:
	
	let url: NSURL? = NSURL(string: "image.jpg")
	var image: NSImage?
	if let url1 = url {
    	image = NSImage(contentsOfURL:url1)
	}

通过使用map，我们就有了一种更简洁的方式：

	let url: NSURL? = NSURL(string: "image.jpg")
	let image = map(url) { NSImage(contentsOfURL: $0) }

map操作定义作用在很多类型上，包括 Dictionary、Tuple、Function 或者你自己封装的类型。