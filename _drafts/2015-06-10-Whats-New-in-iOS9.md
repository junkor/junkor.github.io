###What's New in iOS9
####Contents

[iOS9](#iOS9)  
[iPad的多任务增强](#Multitasking)  
[搜索](#Search)  

* [使用NSUserActivity APIs使应用状态和活动可搜索]()  
* [使用Core Spotlight APIs让应用内容可搜索]()  
* [使用Web Markup使应用内容可搜索]()
* [使用通用链接使得你的应用可以处理你的网站链接]()

[游戏]()

* [GameplayKit]()
* [Model IO]()
* [MetaKit]()
* [MetalPerformanceShaders]()
* [Metal的新功能]()
* [SceneKit的新功能]()
* [SpriteKit的新功能]()

[App瘦身]()  
[支持Right-to-Left语言]()  
[App传输安全]()  
[拓展点]()  
[Contacts和Contacts UI]()  
[Watch Connectivity]()  
[Swift增强]()  
[其他的Framework变更]()  

* [AV Foundation Framework]()
* [AVKit Framework]()
* [CloudKit]()
* [Foundation Framework]()
* [HealthKit Framework]()
* [MapKit Framework]()
* [PassKit Framework]()
* [Safari Services Framework]()
* [UIKit Framework]()

[弃用APIs]()

###<span id="iOS9">iOS9.0</span>
这篇文章主要介绍了iOS9里跟开发相关一些主要功能，同时也列出了一些新功能的细节。
与iOS相关的最新的问题和发布信息可以参阅[iOS9 Release Notes](),iOS9中新增的全量API变更信息可以参阅[iOS 9.0 API Diffs]()。与新设备相关的信息可以参阅[ iOS Device Compatibility Reference]()。

####<span id="Multitasking">iPad的多任务增强</span>
iOS9通过Slide Over、Split View 和 Picture in Picture提升了用户的多任务操作体验。Slide Over功能允许用户打开第二个应用并快速与其交互。Split View功能可以使用户在iPad Air2中分屏使用两个app。Picture in Picture(PiP)功能使你可以打开一个视频的悬浮窗口，漂浮于其他应用之上。
你无法预知用户何时会在同一个屏幕上操作两个App，所以，为了保障用户有一个不错的多任务体验，你得做到以下两点：

* 在与其他应用共享系统资源的情况下，高效的利用系统资源是很有必要的。当产生内存的压力时,系统预先退出消耗最大内存的app。
* 确保你的应用已经兼容了size classes，当用户想用一部分屏幕展示你的app时，只有这样才能保证app的正常显示。
想了解更多关于良好的支持Split View和Slide Over的信息，可参阅[App Design for Multitasking]()

除了Split View 和 Slide Over,用户可能还想使用PiP在操作其他app的同时观看视频。如果视频播放不是你app的主要功能，你不需要为支持PiP额外做任何工作。

如果要支持PiP，请使用AVKit 或者 AV Foundation API。Media Player framework里的视频播放API将在iOS9中被起弃用，且不支持PiP。了解更多有关PiP的准备工作，请移步[Picture in Picture Quick Start]()

####<span id="Search">搜索</span>
iOS9中的搜索功能为用户提供了多种访问应用内信息的途径。当你兼容了iOS9的搜索功能以后，用户就可以通过Handoff、Siri Reminders 和 搜索结果访问到应用深处的内容。
兼容搜索功能以后，在提升了用户体验的同时，也降低了使用你的app的门槛，同时也可以在用户使用系统搜索或者网络搜索时轻松的展现你的应用内容。让我们举个例子来说明一下它是怎么工作的：假设你的app是帮助用户处理轻微病症,如晒伤或扭伤脚踝。你兼容过iOS9的搜索功能以后，当用户搜索“扭伤脚踝”，即便他们没有安装你的app也能看到有关你app的结果。当用户点击这条结果时，你的app就有机会被他们下载了。类似的，你可以关联关键字到你的web页面，这样用户可以在safari中搜索到相关结果。当用户点击时，Safari会引导用户到你的网站，用户可以通过你app的banner去下载应用。
兼容Search其实很简单：你不需要有任何关于搜索具体实现的经验，并且大多数开发者发现他们只用了几个教师就使得他们的内容可搜索了。iOS9提供了以下api供你使用：

* NSUserActivity包含了一些新的属性跟方法来帮助你索引app的状态以便在搜索结果中展示。几乎所有的应用程序可以利用NSUserActivity api来为用户提供有用的内容。具体详情，请移步[Use NSUserActivity APIs to Make App Activities Searchable]()
* CoreSpotlight.framework为你提供了内容索引和深层链接的功能。CoreSpotlight就是被用来处理持久化用户数据的，例如文档、照片还用用户创建的其他类型的内容。关于它的更多细节请移步[Use Core Spotlight APIs to Make App ContentSearchable]()
* 在你的网站端适当的添加web标记可以使相关的web内容可搜索且丰富用户的搜索结果。添加一个Smart App Banner可以方便的引导用户到你的app。更多关于web内容可搜索的信息，请移步[Use Web Markup to Make Web Content Searchable]()。关于Smart App Banner的使用，请移步[Promoting Apps with Smart App Banners]()

这三种搜索相关的api是协同工作的。如果你app的内容是与web端相对应的，你可以三种同时兼容；否则的话你可以只兼容NSUserActivity和CoreSpot light。
对这些api适当的兼容可以提升搜索结果与你的app的关联性和排名。为了给用户提供最好的搜索体验，系统将会权衡用户与检索到的app内内容和从Safari检索到的web内容的交互频度。iOS会根据用户与你的app交互的频率、用户看到检索结果并跳入到app内部的时间差、以及你网站的威望来综合计算关联性和排行。

>####重点：
>一定要避免app内容的过度索引或者添加不必要的关键字和参数来提升你在搜索结果中的排行。因为iOS会权衡用户参与搜索结果的级别，那些无用的信息最终将不被展示到搜索结果中。

有两点你可以用来判断用户检索与你app内容相关信息时体验是否完善：

1. 确保你的结果是丰富有用且描述性强的。当用户看到与他们的检索直接相关的信息时，他们会更乐意与你的app交互。
2. 当用户点击一条结果时，直接跳转到你应用里的相应区域。尽可能的不要有中间环节打断用户的操作，那样会阻止用户获取他们真正想要的内容。

###<span id="NSUserActivity">使用NSUserActivity使应用内容可搜索</span>
目前为止，你或许在使用NSUserActivity api来支持Handoff(更多关于handoff的功能可以参阅[Handoff Programming Guide ]())。在iOS9中，NSUserActivity添加了一些新的api允许你指定一些程序特定的活动或者状态是可搜索的














