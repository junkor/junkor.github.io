---
layout: post
title: What's New in iOS9
description: 这篇文章主要介绍了iOS9里跟开发相关一些主要功能，同时也列出了一些新功能的细节。与iOS相关的最新的问题和发布信息可以参阅iOS9 Release Notes,iOS9中新增的全量API变更信息可以参阅iOS 9.0 API Diffs。与新设备相关的信息可以参阅iOS Device Compatibility Reference。...
keywords: whats-new-in-iOS9 iOS9 iOS new
---

<!--####Contents-->

[iOS9](#iOS9)  
[iPad的多任务增强](#Multitasking)  
[搜索](#Search)  

* [使用NSUserActivity APIs使应用状态和活动可搜索](#NSUserActivity)  
* [使用Core Spotlight APIs让应用内容可搜索](#CoreSpotlight)  
* [使用Web Markup使应用内容可搜索](#WebMarkup)
* [使用Universal Links使得你的应用可以处理你的网站链接](#UniversalLinks)

[游戏](#Gaming)

* [GameplayKit](#GameplayKit)
* [Model IO](#ModelIO)
* [MetaKit](#MetalKit)
* [MetalPerformanceShaders](#MetalPerformanceShaders)
* [Metal的新功能](#NewFeaturesinMetal)
* [SceneKit的新功能](#NewFeaturesinSceneKit)
* [SpriteKit的新功能](#NewFeaturesinSpriteKit)

[AppThinning](#AppThinning)  
[支持Right-to-Left语言](#RightToLeft)  
[App传输安全(ATS)](#ATS)  
[插件拓展(Extension Points)](#ExtensionPoints)  
[Contacts和Contacts UI](#Contacts)  
[Watch Connectivity](#WatchConnectivity)  
[Swift增强](#Swift)

[其他的Framework变更](#AdditionalChanges)  

* [AV Foundation Framework](#AVFoundation)
* [AVKit Framework](#AVKit)
* [CloudKit](#CloudKit)
* [Foundation Framework](#Foundation)
* [HealthKit Framework](#HealthKit)
* [Local Authentication Framework](#LocalAuthenticationFramework)
* [MapKit Framework](#MapKit)
* [PassKit Framework](#PassKit)
* [Safari Services Framework](#SafariServices)
* [UIKit Framework](#UIKit)

[弃用APIs](#DeprecatedAPIs)

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
目前为止，你或许在使用NSUserActivity api来支持Handoff(更多关于handoff的功能可以参阅[Handoff Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338))。在iOS9中，NSUserActivity添加了一些新的api允许你指定一些程序特定的活动或者状态是可搜索的,当这些内容被出现在用户的搜索结果或者Safari结果中时，用户可以点击相应的结果跳转到你的app中来。

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

###<span id="UniversalLinks">使用Universal Links使得你的应用可以处理你的网站链接</span>
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
GameplayKit.framework提供了游戏开发的基础支持。使用GameplayKit来开发游戏机制，并结合高级图形引擎(例如SceneKit或SpriteKit)可以创建一个完整的游戏上。这个framework为创建游戏提供了很多模型化的架构，包括：

* 在不影响调试的情况下给游戏引入不可预知性的Randomization tools
* Entity-component式的架构设计，提升gameplay代码的复用性
* 降低gameplay系统状态机维护的复杂性

GameplayKit也包含了对通用gameplay算法的标准实现，所以你不必花费太多精力去阅读白皮书，而是把更多的时间用在创建独一无二的游戏上。GameplayKit主要实现了下列几种标准算法：

* 敌对的回合制游戏minmax人工智能。
* 用来描述行为的高级自动追踪目标的模拟代理。
* 构建数据驱动的游戏规则系统：logic、fuzzy reasoning和emergent behavior。

了解更多GameplayKit地信息，可以查阅[GameplayKit Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172)和[GameplayKit Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199)。具体的，可以从看一些demo工程开始：FourInARow(使用GameplayKit Minmax Strategist实现的Opponent AI)、AgentsCatalog(使用了GameplayKit的Agents System)还有DemoBots(使用SpriteKit和GameplayKit创建跨平台的游戏)。

####<span id="ModelIO">Model IO</span>
ModelIO.framework提供对3D资源和相关素材的系统级别的理解。使用这个框架你可以：

* 引入mesh数据、资源描述、光线和camera的配置，或者从其他当下主流的绘制软件或者游戏引擎支持的格式文件中引入场景信息。
* 访问或生成下列数据——如：bake光照信息到mesh中，或者创建程序式的sky textures。
* 与MetalKit、GLKit和SceneKit配合，完成向GPU缓存高效的加载资源数据进行渲染。
* 导出正在访问的或者生成的资源数据到多个或者多种格式的文件中。

了解更多关于Model IO的细节，看看[Model I/O Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)。

####<span id="MetalKit">MetalKit</span>
MetalKit.framework提供了很多使用的类和函数来降低创建一个Metal应用的成本。MetalKit对开发主要提供了3方面的支持：

* Texture loading帮助你的应用轻松且移步的从不同格式的资源中加载纹理。常见的格式有PNG、JPEG，还支持纹理的特有格式如KTX和PVR。
* Model handling提供了Metal特用的功能，使得Metal和Model IO之间的接口更简单。使用这些类和函数在Model IO的meshes和Metal的buffers之间高效的传输数据。
* View management对Metal view的标准实现，这样彻底降低了创建一个图形渲染app的代码成本。

了解更多MetalKit APIs，这里[MetalKit Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356).Metal相关的，[Metal Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)、[Metal Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161)还有[Metal Shading Language Guide](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)。

####<span id="MetalPerformanceShaders">MetalPerformanceShaders</span>
MetalPerformanceShaders.framework提供了高度优化的计算和图形着色器，可以轻松高效地集成到你的Metal应用中。这些data-parallel着色器将充分发挥那些支持Metal的iOS设备CPU的硬件特性。

使用MetalPerformanceShader类可以在支持的硬件上，在不改变你原有着色器代码的情况下提升性能。MetalPerformanceShader对象。MetalPerformanceShader objects可以无缝地融入你的Metal应用中配合Metal resource objects如buffers、或者textures使用。

MetalPerformanceShaders.framework支持的常用着色器有：

* Gaussian blur - 由MPSImageGaussianBlur类实现
* Image histogram - 由MPSImageHistogram类实现
* Sobel edge detection - 由MPSImageSobel类实现

####<span id="NewFeaturesinMetal">New Features in Metal</span>
Metal.framework添加了一些使你的应用图像渲染性能更多、效果更好地新特性。例如：

* Metal Shading Language和Metal标准库的改进
* 着色计算现在支持更大范围的像素格式
* OSX中添加私有的深度模板文理对齐(The addition of private and depth stencil textures to align with OS X)
* 为了改善阴影效果新增的depth clamping和前后分离的模板引用值(stencil reference values)

####<span id="NewFeaturesinSceneKit">New Features in SceneKit</span>
SceneKit.framework在iOS9中包含的新特性有：

* 支持Metal渲染。[SCNView](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SCNView_Class/index.html#//apple_ref/occ/cl/SCNView)和[SCNSceneRenderer](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SCNSceneRenderer_Protocol/index.html#//apple_ref/occ/intf/SCNSceneRenderer)类可以在支持Metal的设备上执行高性能的Metal渲染。创建游戏或者交互式的3D应用时，使用场景编辑器可以减少一些编码成本和时间成本（这里有个相关的事例工程，这里下载[Building a SceneKit Game with the Xcode Scene Editor](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)）。
* 音频定位(Positional audio)。[SCNAudioPlayer](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SCNAudioPlayer_Class/index.html#//apple_ref/occ/cl/SCNAudioPlayer)和[SCNNode](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SCNNode_Class/index.html#//apple_ref/occ/cl/SCNNode)类中添加了音频占位效果，可以自动追踪场景中的人物坐标。(这个翻译的有点儿屎)

了解这方面的更多详情或者其他特性，移步[SceneKit Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html)

####<span id="NewFeaturesinSpriteKit">New Features in SpriteKit</span>
SpriteKit.framework在iOS9中包含的新特性有：

* 支持Metal渲染。在支持Metal的设备上，metal渲染是自动启用的，除非你用的是自定义的OpenGL ES着色器。
* Xcode集成了新的场景编辑器和Action Editor。创建游戏或者交互式的2D应用时，使用场景编辑器可以减少一些编码成本和时间成本（这里有个相关的事例工程，这里下载[Building a Cross Platform Game with SpriteKit and GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)）。
* Camera nodes([SKCameraNode](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SKCameraNode/index.html#//apple_ref/occ/cl/SKCameraNode)类实例)使得scrolling类型的游戏实现更简单。只用拖拽一个camera node到你的场景中，然后配置到场景的camera属性中就ok了。
* Positional audio。想要了解更多关于在场景中追踪玩家坐标自动配置音效的内容，看看[SKAudioNode Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SKAudioNode/index.html#//apple_ref/doc/uid/TP40015249)。

了解这方面的更多详情或者其他特性，移步[SpriteKit Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041)

###<span id="AppThinning"> AppThinning </span>
App thinning包含以下几点：

* Slicing。根据设备只下载当前设备需要的资源。(比如iphone6，只下载@3x的图片资源)
* On-Demand Resources。允许app在需要时再异步下载对应的资源文件(比如游戏，玩到第3关再下载第3关的地图等资源),更多细节到[On-Demand Resources Guide](https://developer.apple.com/library/prerelease/ios/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/Chapters/Introduction.html#//apple_ref/doc/uid/TP40015083)。
* Bitcode。归档你的app并提交编译的中间文件到App Store，apple会将中间文件编译成指定的64位或者32位的可执行文件供相应的设备下载。

更多关于app thinning的信息，移步[App Thinning (iOS, watchOS)](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AppThinning/AppThinning.html#//apple_ref/doc/uid/TP40012582-CH35)

####<span id="RightToLeft">支持Right-to-Left Languages</span>
iOS9中对Right-to-Left Languages做了广泛支持，使得实现翻转类的交互更加简单。例如：

* 标准的UIKit控件在right-to-left的context中可以自动翻转。
* [UIView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView)定义了content attributes的语法使得你可以指定特定的View出现在right-to-left的context中。
* [UIImage](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/cl/UIImage)提供了image[FlippedForRightToLeftLayoutDirection](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/instm/UIImage/imageFlippedForRightToLeftLayoutDirection)方法，使得用程序的方式翻转图片变得更加简单。

了解更多flip方式的交互，移步[Supporting Right-to-Left Languages](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17)

####<span id="ATS">App Transport Security</span>
ATS允许一个app在Info.plist中声明一个指定的域名，来标示与其进行安全通讯。ATS为了防止信息泄露，提供了简单易用的默认的安全处理。你需要尽快采用ATS，不管你是创建新的app或者是已经现存的一个app。

如果你是新开发一个app，你需要使用HTTPS。如果是现存的app，你最好立刻切换成https的方式，并尽快制定一个迁移方案。

####<span id="ExtensionPoints">Extension Points</span>
iOS9中引入了一些新的Extension Points(一个Extension Point的意思就是为指定的功能点定义一些策略或者提供一些api，以便你开发相应的插件)

* 网络的extension points:
	* 使用Packet Tunnel Provider来实现自定义的VPN数据封装协议
	* 使用App Proxy Provider实现一个客户端的自定义的透明的网络代理协议
	* 使用Filter Data Provider和Filter Control Provider实现一个动态的、设备端的网络过滤器。  

	>每一个网络的extension points都需要Apple特定的许可。

* Safari extension points:
	* 使用Shared Links可以在Safari的分享列表中展现你的内容
	* 使用Content Blocking，通过给Safari提供一个描述屏蔽内容的列表就可以在用户浏览web内容时屏蔽指定的内容。

* Spotlight extension points:
	* 使用app indexing extension point 来索引你应用内的数据
	* 使用Index Maintenance extension point使得在不加载应用的情况下re-indexing  
* Audio Unit extension point 允许你的应用拥有像GarageBand、Logic等的乐器、音效、声音合成功能。这个extension point不仅给iOS带来了插件式的音频处理体验并且允许你在App Store中售卖。

想了解更多关于App extension的信息，移步[App Extension Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

####<span id="Contacts">Contacts and Contacts UI</span>
iOS9引入了Contacts和Contacts UI框架(Contacts.framework和 ContactsUI.framework)，提供了Address Book和Address Book UI的oop替代方案。了解更多可以到[Contacts Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/Contacts/Reference/Contacts_Framework/index.html#//apple_ref/doc/uid/TP40015328)或者[ContactsUI Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/ContactsUI/Reference/ContactsUI_Framework/index.html#//apple_ref/doc/uid/TP40016207)

####<span id="WatchConnectivity">Watch Connectivity</span>
Watch Connectivity框架(WatchConnectivity.framework)提供了两种iPhone与已配对Apple Watch的通讯方式。使用这个框架来协调你的iOS app与Watch app直接的互动。框架为两个应用提供了针对运行时（不是runtime而是两个app both running的时候）的及时消息(immediate messaging)和非运行时的后台消息(background messaging)。了解更多，请移步[Watch Connectivity Framework Reference](https://developer.apple.com/library/prerelease/ios/documentation/WatchConnectivity/Reference/WatchConnectivity_framework/index.html#//apple_ref/doc/uid/TP40015269)


####<span id="Swift">Swift Enhancements</span>
想了解关于Swift的更新？看看这个[Swift Language](https://developer.apple.com/library/prerelease/ios/documentation/DeveloperTools/Conceptual/WhatsNewXcode/Articles/xcode_7_0.html#//apple_ref/doc/uid/TP40015242-SW2)

###<span id="AdditionalChanges">Additional Framework Changes</span>
除了上述的一些主要变更外，iOS9还做了很多其他方面的改进。

####<span id="AVFoundation">AV Foundation Framework</span>
AVFoundation.framework包含了一个新的class —— [AVSpeechSynthesisVoice](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)，允许你通过一个identifier指定voice，也可以使用name或者quality属性获取voice的信息。

####<span id="AVKit">AVKit Framework</span>
AVKit.framework引入了[AVPictureInPictureController](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController)和[AVPlayerViewController](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController)来帮助支持画中画(pip).更多关于 Picture in Picture的信息，看这里[Multitasking Enhancements for iPad](#Multitasking)。

####<span id="CloudKit">CloudKit</span>
如果你在应用中使用了CloudKit，你可以使用CloudKit web services或者CloudKit JS(一个js库)提供的web接口来为用户提供访问数据。你需要创建相应的schema以实现现有数据库操作增、删、改、查、订阅等web接口的功能。更多内容请查阅[CloudKit JS Reference](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359),[CloudKit Web Services Reference](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240)还有[CloudKit Catalog: An Introduction to CloudKit](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599)。

####<span id="Foundation">Foundation Framework</span>
Foundation.framework包含了以下增强：
* [NSBundle](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)资源的按需加载(on-demand loading)APIs
* Strings文件支持context-dependent variable width strings
* [NSProcessInfo](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)提供了对电量和热量的管理APIs

####<span id="HealthKit">HealthKit Framework</span>
HealthKit.framework主要有以下改进：
* 支持胜利健康和紫外线等领域的健康跟踪。更详细的内容，可以参考[HealthKit Constants Reference](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html)
* 支持删除信息的查询和追踪(New support for bulk-deleting entries and tracking deleted entries),更多信息可以参考[HKHealthStore Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708)中的[HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject)，[HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery)，[deleteObjects:withCompletion:](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/occ/instm/HKHealthStore/deleteObjects:withCompletion:)还有[deleteObjectsOfType:predicate:withCompletion:](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/occ/instm/HKHealthStore/deleteObjectsOfType:predicate:withCompletion:)。

####<span id="LocalAuthenticationFramework">Local Authentication Framework</span>
LocalAuthentication.framework包含以下改进：

* 新增的当前登记指纹信息的描述，使得当一个指纹登记或者移除的时候，应用可以修改相应的行为。
* 支持代码方式取消用户提示(prompt)。
* 支持对keychain访问的control lists的评估和在keychain调用中使用authentication context（Support for evaluating keychain access control lists and the use of an authentication context in keychain calls）。
* 支持Touch ID匹配重用。通过evaluateAccessControl: 和 [evaluatePolicy:localizedReason:reply:](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:)，可以复原当前手机的前一次指纹校验。

####<span id="MapKit">MapKit Framework</span>
MapKit.framework引入了一些新的特性来提升用户体验。特别是：

*  MapKit支持交通路线搜索和路线导航(and launching Maps into transit directions)
*  Map views支持3D立交桥模式
*  注释支持完全自定义
*  MapKit和[CLGeocoder](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder)的搜索结果支持时区

####<span id="PassKit">PassKit Framework</span>
PassKit.framework主要增加了一些Apple Pay的支持，例如：

* iOS9中Apple pay支持Discover cards、store debit 和信用卡。更多信息
* 发卡机构和支付系统可以在他们的应用中直接向apple pay中添加卡。有关更多信息,请参见[PKAddPaymentPassViewController](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832)。
* 新的API避免了ApplePay的自动展示，只有当应用在前台且靠近NFC或者RF reader的时候才会自动触发。更多信息可以参考[requestAutomaticPassPresentationSuppressionWithResponseHandler:](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116)。

####<span id="SafariServices">Safari Services Framework</span>
SafariServices.framework包含了以下改进： 
SFSafariViewController 可以用来在你的应用内展示web内容，并且可以共享Safari的cookie和其他网站信息，同时也支持很多Safari的其他主要功能：自动填充、Reader等。SFSafariViewController并不像Safari那样，它只展示一页，和一个“完成”按钮，以 便用户可以回到原来应用的场景中。 
如果你应用中展示的web内容没有太多自定义的需求，可以考虑把原来的[WKWebView](https://developer.apple.com/library/prerelease/ios/documentation/WebKit/Reference/WKWebView_Ref/index.html#//apple_ref/occ/cl/WKWebView)或者基于[UIWebView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIWebView_Class/index.html#//apple_ref/occ/cl/UIWebView)的内置浏览器替换为SFSafariViewController。

####<span id="UIKit">UIKit Framework</span>
UIKit.framework包含了很多新的提升，例如：

* [UIStackView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/index.html#//apple_ref/occ/cl/UIStackView)可以帮助你管理一些横向或者纵向的子view。
* 为了使布局更简单，为[UIView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView)(例如[leadingAnchor](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/instp/UIView/leadingAnchor)和[widthAnchor](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/instp/UIView/widthAnchor))、[NSLayoutAnchor](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor)和[NSLayoutDimension](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension)提供了新的anchors。
* 新的布局引导在你采用readable content时，提供合适的边距和定义一个view中内容的绘制区域，详情请见[UILayoutGuide](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide)
* [UIApplicationDelegate](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate)中提供了一个新的函数用来打开(或者编辑)一个文档而不是处理一个文档的副本。应用想要支持open-in-place功能，还需要在Info.plist中配置LSSupportsOpeningDocumentsInPlace的值为yes或者true。
* UITextInputAssistantItem类用来来布局shortcuts bar中的bar buttons。
* touch events有部分改进，比如你可以获取到最后一次刷新显示和触摸预测之间的toch值。
* UIKit dynamics的升级有，支持不规则边界的碰撞检测、新的UIFieldBehavior类支持自定义多种field types，而[UIAttachmentBehavior](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior)则支持附加的attachment types。
* [UIUserNotificationAction](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIUserNotificationAction_class/index.html#//apple_ref/occ/cl/UIUserNotificationAction)增加的behavior属性，允许你在用户输入时得到通知。
* 新的NSDataAsset类使得从内存或者绘图中获取资源更加轻松
* 所有标准UIKit组件都适的当支持从右到左的语言。此外,导航,手势,collection views和 table cell布局也有相应的支持。

###<span id="DeprecatedAPIs">Deprecated APIs</span>
下列APIs是被弃用的：

* AddressBook和AddressBookUI，可以使用Contacts和Contacts UI替代。
想获取完整的弃用API列表，可以在这里找到[iOS 9.0 API Diffs](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222)。 








