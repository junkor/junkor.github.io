Typed Notification Observers
----
>文章翻译自[objc](http://www.objc.io)，原文连接：『[Typed Notification Observers](http://www.objc.io/snippets/16.html)』

这一节是建立在NSNotificationCenter之上的。我们要改善两点：第一，实现对notification center增加和移除观察的自动管理，使得添加观察的操作就像是创建一个对象；移除的操作就像是回收这个对象。第二，我们将应用前边讲到的[Phantom Types](http://junkor.github.io/2015/01/Phantom-Types)来实现我们自己的通知类型。

首先，我们创建一个结构体来描述notifiction。注意，这个结构体有一个phantom type：A。A将是存储到notifiction的userInfo中的类型。结构体里只存粗了notifiction的名称：

	struct Notification<A> {
    	let name: String
	}

我们需要对notifiction的userInfo进行简单的包装。由于NSNotificationCenter的工作机制，我们需要对value进行包装，防止value不是对象类型：

	func postNotification<A>(note: Notification<A>, value: A) {
    	let userInfo = ["value": Box(value)]
    	let center = NSNotificationCenter.defaultCenter()center.postNotificationName(note.name, object: nil, userInfo: userInfo)
	}
	
现在，我们可以创建我们的观察对象了。创建一个该对象的实例，然后向NSNotificationCenter添加观察者，当它需要销毁的时候，从NSNotificationCenter移除观察。这样我们就可以把它当作一个对象的属性来持有，一但对象的该属性被置为nil(例如当该对象被dealloc时),它就会自动从NSNotificationCenter中移除（省去了频繁编码实现的困扰）。

	class NotificationObserver {
	    let observer: NSObjectProtocol

	    init(notification: Notification<A>, block aBlock: A -> ()) {
	        let center = NSNotificationCenter.defaultCenter()
	        observer = center.addObserverForName(notification.name, 
	                                             object: nil, 
	                                             queue: nil) { note in
	            if let value = (note.userInfo?["value"] as? Box<A>)?.unbox {
	                aBlock(value)
	            }
	        }
	    }

	    deinit {
	        NSNotificationCenter.defaultCenter().removeObserver(observer)
	    }

	}

我们有各种需要使用我们定义的notification的场景。例如我们需要一个持有NSError对象的globalPanicNotification：
	
	let globalPanicNotification: Notification<NSError> = Notification(name: "Global panic")
	
直接调用postNotification:就可以发出通知：

	let myError: NSError = NSError(domain: "io.objc.sample", code: 42, userInfo: [:])
	postNotification(globalPanicNotification, myError)

需要注意的是，因为采用了[phantom types](http://junkor.github.io/2015/01/Phantom-Types)，声明时是NSError类型，所以调用时也只能传递NSError类型的参数。对该类型的通知建立观察，我们只需要在我们的类里创建一个这样的实例变量：

	let panicObserver = NotificationObserver(notification: globalPanicNotification) { err in
	     println(err.localizedDescription)
	}

这就是Typed Notification。现在，我们有了一个类型安全的notification，并且不用担心添加和移除观察的逻辑，它可以自动完成这些功能。我们可以利用框架本身提供的各种奇妙的基础结构，进行简单包装，便可大大提升安全性。所有的源码都可以从[这里](https://gist.github.com/chriseidhof/9bf7280063db3a249fbe)获取。