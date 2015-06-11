###What's New in iOS9
####Contents

[iOS9](#iOS9)  
[iPad的多任务增强](#Multitasking)  
[搜索](#Search)  

* [使用NSUserActivity APIs使应用状态和活动可搜索](#NSUserActivity)  
* [使用Core Spotlight APIs让应用内容可搜索](#CoreSpotlight)  
* [使用Web Markup使应用内容可搜索](#WebMarkup)
* [使用Universal Links使得你的应用可以处理你的网站链接]()

[游戏](#Gaming)

* [GameplayKit](#GameplayKit)
* [Model IO]()
* [MetaKit]()
* [MetalPerformanceShaders]()
* [Metal的新功能]()
* [SceneKit的新功能]()
* [SpriteKit的新功能]()

[AppThinning]()  
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
与iOS相关的最新的问题和发布信息可以参阅[iOS9 Release Notes](https://developer.apple.com/library/prerelease/ios/releasenotes/General/RN-iOSSDK-9.0/index.html#//apple_ref/doc/uid/TP40016202),iOS9中新增的全量API变更信息可以参阅[iOS 9.0 API Diffs](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222)。与新设备相关的信息可以参阅[iOS Device Compatibility Reference](https://developer.apple.com/library/prerelease/ios/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599)。

####<span id="Multitasking">iPad的多任务增强</span>
iOS9通过Slide Over、Split View 和 Picture in Picture提升了用户的多任务操作体验。Slide Over功能允许用户打开第二个应用并快速与其交互。Split View功能可以使用户在iPad Air2中分屏使用两个app。Picture in Picture(PiP)功能使你可以打开一个视频的悬浮窗口，漂浮于其他应用之上。
你无法预知用户何时会在同一个屏幕上操作两个App，所以，为了保障用户有一个不错的多任务体验，你得做到以下两点：

* 在与其他应用共享系统资源的情况下，高效的利用系统资源是很有必要的。当产生内存的压力时,系统预先退出消耗最大内存的app。
* 确保你的应用已经兼容了size classes，当用户想用一部分屏幕展示你的app时，只有这样才能保证app的正常显示。
想了解更多关于良好的支持Split View和Slide Over的信息，可参阅[Adopting Multitasking Enhancements on iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)

除了Split View 和 Slide Over,用户可能还想使用PiP在操作其他app的同时观看视频。如果视频播放不是你app的主要功能，你不需要为支持PiP额外做任何工作。

如果要支持PiP，请使用AVKit 或者 AV Foundation API。Media Player framework里的视频播放API将在iOS9中被起弃用，且不支持PiP。了解更多有关PiP的准备工作，请移步[Picture in Picture Quick Start](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14)

####<span id="Search">搜索</span>
iOS9中的搜索功能为用户提供了多种访问应用内信息的途径。当你兼容了iOS9的搜索功能以后，用户就可以通过Handoff、Siri Reminders 和 搜索结果访问到应用深处的内容。
兼容搜索功能以后，在提升了用户体验的同时，也降低了使用你的app的门槛，同时也可以在用户使用系统搜索或者网络搜索时轻松的展现你的应用内容。让我们举个例子来说明一下它是怎么工作的：假设你的app是帮助用户处理轻微病症,如晒伤或扭伤脚踝。你兼容过iOS9的搜索功能以后，当用户搜索“扭伤脚踝”，即便他们没有安装你的app也能看到有关你app的结果。当用户点击这条结果时，你的app就有机会被他们下载了。类似的，你可以关联关键字到你的web页面，这样用户可以在safari中搜索到相关结果。当用户点击时，Safari会引导用户到你的网站，用户可以通过你app的banner去下载应用。
兼容Search其实很简单：你不需要有任何关于搜索具体实现的经验，并且大多数开发者发现他们只用了几个教师就使得他们的内容可搜索了。iOS9提供了以下api供你使用：

* [NSUserActivity](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSUserActivity_Class/index.html#//apple_ref/occ/cl/NSUserActivity)包含了一些新的属性跟方法来帮助你索引app的状态以便在搜索结果中展示。几乎所有的应用程序可以利用NSUserActivity api来为用户提供有用的内容。具体详情，请移步[Use NSUserActivity APIs to Make App Activities Searchable](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-SW2)
* CoreSpotlight.framework为你提供了内容索引和深层链接的功能。CoreSpotlight就是被用来处理持久化用户数据的，例如文档、照片还用用户创建的其他类型的内容。关于它的更多细节请移步[Use Core Spotlight APIs to Make App ContentSearchable](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-SW3)
* 在你的网站端适当的添加web标记可以使相关的web内容可搜索且丰富用户的搜索结果。添加一个Smart App Banner可以方便的引导用户到你的app。更多关于web内容可搜索的信息，请移步[Use Web Markup to Make Web Content Searchable](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-SW4)。关于Smart App Banner的使用，请移步[Promoting Apps with Smart App Banners](https://developer.apple.com/library/prerelease/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html#//apple_ref/doc/uid/TP40002051-CH6)

这三种搜索相关的api是协同工作的。如果你app的内容是与web端相对应的，你可以三种同时兼容；否则的话你可以只兼容NSUserActivity和CoreSpot light。

对这些api适当的兼容可以提升搜索结果与你的app的关联性和排名。为了给用户提供最好的搜索体验，系统将会权衡用户与检索到的app内内容和从Safari检索到的web内容的交互频度。iOS会根据用户与你的app交互的频率、用户看到检索结果并跳入到app内部的时间差、以及你网站的威望来综合计算关联性和排行。

>####重点：
>一定要避免app内容的过度索引或者添加不必要的关键字和参数来提升你在搜索结果中的排行。因为iOS会权衡用户参与搜索结果的级别，那些无用的信息最终将不被展示到搜索结果中。

有两点你可以用来判断用户检索与你app内容相关信息时体验是否完善：

1. 确保你的结果是丰富有用且描述性强的。当用户看到与他们的检索直接相关的信息时，他们会更乐意与你的app交互。
2. 当用户点击一条结果时，直接跳转到你应用里的相应区域。尽可能的不要有中间环节打断用户的操作，那样会阻止用户获取他们真正想要的内容。

###<span id="NSUserActivity">使用NSUserActivity使应用内容可搜索</span>
目前为止，你或许在使用NSUserActivity api来支持Handoff(更多关于handoff的功能可以参阅[Handoff Programming Guide ](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338))。在iOS9中，NSUserActivity添加了一些新的api允许你指定一些程序特定的活动或者状态是可搜索的,当这些内容被出现在用户的搜索结果或者Safari结果中时，用户可以点击相应的结果跳转到你的app中来。

NSUserActivity也提供了一些属性以便你在搜索结果中展示丰富的内容，例如你可以指定搜索结果的标题、描述或者缩略图。

通过创建一个NSUserActivity对象来描述应用活动和状态的可搜索性。使用NSUserActivity的属性可以完整的描述这些信息并使其具备被搜索的资格。Listing1中的代码展示了如何配置一个activity：
>Listing 1 Creating a new activity

	NSUserActivity *userActivity = [[NSUserActivity alloc]    initWithActivityType:@“com.mycompany.activity-type”];	// Set properties that describe the activity and that can be used in search.	userActivity.title = @"...";	userActivity.keywords = [NSSet setWithArray:@[...]];
	// Set values needed to restore state	userActivity.userInfo = @{ ... };	// Enable the activity to participate in search results.	[userActivity.eligibleForSearch = YES];

当一个用户执行这个activity或者携带着与你创建的NSUserActivity对象一致的状态进入应用，你的app需要调用[userActivity becomeCurrent] 来确保这个activity就是当前的activity。当前的activity才有资格自动出现到索引结果中(也就是CSSearchableIndex)。

当用户点击一条与一个状态或者活动有关的结果时，你的app通过NSUserActivity的api来继续用户的操作并记录用户的信息。Listing 2展示了怎样继续用户的行为：
>Listing 2 Continuing an activity

	- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler: {	    NSString *activityType = userActivity.activityType;    	if ([activityType isEqual:@“com.mycompany.activity-type”]) {        	// Handle restoration for values provided in userInfo			return YES; 
		}		return NO;
	}

同样的，它适用于所有用户。例如，Health应用索引它的分组信息使得他们对于所有用户都是可用的。当用户搜索“steps”时，搜索结果中会显示用户当前的记步信息，点击该条目时，会自动跳转到Health应用的Steps内容区域。因为“step”被标记为一个公共搜索条目，即便是用户的设备没有跟踪其记步信息，当用户搜索“step”时，Health应用也会收到一个跟记步相关的link。

被标记为public的activity可被展示给那些未安装你app的用户并引导他们来使用你的app。默认情况下activity都是私有的，只有当一个activity对其他用户也是有意义的时候你才能把它标记未public。一般情况下，用户自己创建的内容都不会对别人有太大用途。标记activity为public的话，直接设置[eligibleForPublicIndexing](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSUserActivity_Class/index.html#//apple_ref/occ/instp/NSUserActivity/eligibleForPublicIndexing)属性为YES即可。

###<span id="CoreSpotlight">使用Core Spotlight使应用内容可搜索</span>
CoreSpotlight提供了类似数据库操作的api，允许你对描述可搜索内容的条目进行增删改查。当你使用CoreSpotlight来检索这些条目的时候,用户可以轻松的搜索到他们想要的内容。
为了使应用内内容可搜索，首先你要做的是创建一些参数用来存放你想在结果列表中展示的元数据。Listing 3中展示了如何创建一个CSSearchableItemAttributeSet对象并且配置它的属性。
>Listing 3 Creating an attribute set for a searchable item

	// Create an attribute set for an item that represents an image.	CSSearchableItemAttributeSet* attributeSet = 	[[CSSearchableItemAttributeSet alloc] 	initWithItemContentType:(NSString*)kUTTypeImage];	// Set properties that describe attributes of the item such as title, description, and image.	[attributeSet setTitle:@"..."];	[attributeSet setContentDescription:@"..."];

然后创建CSSearchableItem对象来描述这个item并把它加到Index中。Listing 4展示了如何创建一个CSSearchableItem对象并加到index中：
>Listing 4 Creating a searchable item

	// Create a searchable item, specifying its ID, associated domain, and attribute set.	CSSearchableItem* item;	item = [[CSSearchableItem alloc] initWithUniqueIdentifier:@"..."	domainIdentifier:@"..." attributeSet:attributeSet];	// Index the item.	[[CSSearchableIndex defaultSearchableIndex] indexSearchableItems:@[item] completionHandler: ^(NSError * __nullable error) {		NSLog(@"Search item indexed";	}];

当用户点击一条你添加到index中的搜索结果时，你的app需要被打开，并且恢复到相应条目的上下文。为了实现这样的功能，你需要在你的app delegate中实现application:continueUserActivity:restorationHandler:，可以通过判断activity的类型来检测app是否打开。Listing 5展示了该函数的一个实现：
>Listing 5 Implementing continueUserActivity... in the app delegate

	- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void(^)(NSArray *restorableObjects))restorationHandler
	{    	
    	if ([[userActivity activityType] isEqualToString:CSSearchableItemActionType])
    	{			// This activity represents an item indexed using Core Spotlight, so restore the context related to the unique identifier.    		// The unique identifier of the Core Spotlight item is set in the activity’suserInfo for the key CSSearchableItemActivityIdentifier        	NSString *uniqueIdentifier = [activity.userInfo objectForKey:CSSearchableItemActivityIdentifier];		} 
	}

如果你的app允许用户创建并存储数据，建议你同时兼容NSUserActivity和CoreSpotlight。对两者同时兼容的话可以使activity和检索条目展示同样的内容，这样可以提升用户体验。Listing 6中展示了如何通过一个unique ID来把activity和item关联起来：
>Listing 6 Relating a user activity and a searchable item

	// Create an attribute set that specifies a related unique ID for a Core Spotlight item.  	CSSearchableItemAttributeSet *attributes = [[CSSearchableItemAttributeSet alloc] initWithItemContentType:@"public.image"];	attributes.relatedUniqueIdentifier = coreSpotlightUniqueIdentifier;
	  	// Use the attribute set to create an NSUserActivity that's related to a Core Spotlight item.  	NSUserActivity *userActivity = [[NSUserActivity alloc] initWithActivityType:@“com.mycompany.viewing-message”];
    userActivity.contentAttributeSet = attributes;

iOS9中还包含了一个Core Spotlight的拓展，它允许系统在你的app没有运行的时候与你的app进行通信，为你的app提供更新index或者校验item可用性的时机。

###<span id="WebMarkup">使用Web Markup使应用内容可搜索</span>
如果你app的内容有对应的web版本，你可以通过web markup使得用户可以在搜索结果中直接访问你的app内容。因为Apple索引了web内容并且使其在Search和safari中都是可用的，添加markup的支持并帮助Apple发现和索引你的内容对于展示丰富的搜索结果是很重要的。

 采用Smart App Banners是帮助你的web用户发现你的app的最好方式。在你的Smart App Banner标记中添加app-argument来允许Apple索引到你网站的内容。
 
除了使用Smart App Banners以外，你还可以选择苹果支持的其他开放标准。

用开放标准声明(如[Schema.org](http://schema.org))的结构来标记你的网站内容，使得用户可以搜索到丰富的结果。例如，一个食谱网址或许会使用Listing 7中关于食谱的标记信息：
>Listing 7 Using markup to provide more information

	<div itemscope itemtype="http://schema.org/Recipe">  		<span itemprop="name”>Apple Pie</span>  		<span itemprop="description">This yummy recipe uses healthy Gala apples!</span>  		Prep Time: <meta itemprop="prepTime" content="PT30M”>30 minutes  		Cook Time: <meta itemprop="cookTime" content="PT2H”>2 hours  		Yields: <span itemprop="recipeYield”>8 servings</span>  		Ingredients:  			- <span itemprop="recipeIngredient”>6-8 Gala apples, diced</span>  			- <span itemprop="recipeIngredient”>1/4 cup of flour egg</span>  			- <span itemprop="recipeIngredient”>1 pie dough</span>	</div>

除了使用[Schema.org](http://schema.org)的标准，你还可以支持Open Graph标记来提供一张具体的图片，或者使用标题和描述的方式来组织内容。

###<span id="WebMarkup">使用Universal Links使得你的应用可以处理你的网站链接</span>
在iOS9里，你的app可以直接注册并打开一个web链接(http或者https),而不是通过Safari打开。这种web和app之间的关联方式可以帮助Apple在搜索列表中呈现你的app的内容。

支持universal links是建立于在浏览器和app之间使用Handoff的类似机制之上的，并且可以共享web证书(关于技术的细节可移步至[ Web Browser–to–Native App Handoff](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW10)和[Shared Web Credentials Reference](https://developer.apple.com/library/prerelease/ios/documentation/Security/Reference/SharedWebCredentialsRef/index.html#//apple_ref/doc/uid/TP40014989))。一个信任的链接关系是通过在一个存在app中添加一个叫做com.apple.developer.associated-domains的entitlement和在网站端添加一个叫做apple-app-site-association的文件来建立的。

在你的entitlement中，需要包含你想要app处理的universal links列表，例如：

	applinks:developer.apple.com

在你的apple-app-site-association文件中，你可以指定哪些路径需要当做universal links来处理。例如：

	{
	    "applinks": {
	        "apps": [],
	        "details": {
	            "9JA89QQLNQ.com.apple.wwdc": {
	                "paths": [
	                    "/wwdc/news/",
	                    "/videos/wwdc/2015/*"
	                ]
	            }
	        }
	    }
	}

当你把上边说的两个文件分别部署到你的app和网站端以后，当用户点击了某个web端的链接，你的app就会被唤醒并处理这个链接。你的app需要实现UIApplicationDelegate中为支持Handoff提供的函数(尤其是application:continueUserActivity:restorationHandler:)，才能确保接收到链接并得到处理。

强烈建议使用universal links方式取代URL scheme方式。主要有以下好处：

* 安全 - universal links 不会像URL schemes那样被另外一个app劫持。
* 当用户未安装你的app时，会回调到Safari中去。
* 一个链接，app和web同样适用，不管你的用户在哪一端。

###<span id="Gaming">游戏</span>
iOS9中包含了多项有关游戏绘制和音频播放的技术提升。你可以使用高层api轻松开发，也可以使用底层api来充分发挥GPU的力量。

####<span id="GameplayKit">GameplayKit</span>
GameplayKit.framework提供了游戏开发的基础支持。使用GameplayKit来开发游戏机制，并结合高级图形引擎(例如SceneKit或SpriteKit)可以创建一个完整的游戏上。这个framework提供了


###<span id="AppThinning"> AppThinning </span>
App thinning

App thinning包含以下几点：

* Slicing。根据设备只下载当前设备需要的资源。(比如iphone6，只下载@3x的图片资源)
* On-Demand Resources。允许app在需要时再异步下载对应的资源文件(比如游戏，玩到第3关再下载第3关的地图等资源),更多细节到[On-Demand Resources Guide](https://developer.apple.com/library/prerelease/ios/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/Chapters/Introduction.html#//apple_ref/doc/uid/TP40015083)。
* Bitcode。归档你的app并提交编译的中间文件到App Store，apple会将中间文件编译成指定的64位或者32位的可执行文件供相应的设备下载。

更多关于app thinning的信息，移步[App Thinning (iOS, watchOS)](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AppThinning/AppThinning.html#//apple_ref/doc/uid/TP40012582-CH35)



