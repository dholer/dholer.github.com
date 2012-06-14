---
layout: page 
title: Objective-c
---


###教程
- [iOS开发](http://my.oschina.net/plumsoft/blog?catalog=145903)
- [Xcode4实现基于Webservice用户登录的iphone程序](http://www.cnblogs.com/fengxiang/archive/2011/05/15/2045719.html)
- [Storyboard全解析-第一部分](http://www.iteye.com/topic/1122979)
- [Storyboard全解析-第二部分](http://www.iteye.com/topic/1122984)
- [Using Xcode Storyboards to Build Dynamic iPad TableViews with Prototype Table View Cells](http://www.techotopia.com/index.php/Using_Xcode_Storyboards_to_Build_Dynamic_iPad_TableViews_with_Prototype_Table_View_Cells)
- [iOS iPhone iPad eBooks](http://www.techotopia.com/index.php/IOS_iPhone_iPad_eBooks)
- [ios good tutorial](http://www.raywenderlich.com/tutorials)
###Libraries
- [SBJSON](http://stig.github.com/json-framework/api/3.0/index.html)




##由init、loadView、viewDidLoad、viewDidUnload、dealloc的关系说起
###init方法
在init方法中实例化必要的对象（遵从LazyLoad思想）
‍init方法中初始化ViewController本身
 loadView方法
当view需要被展示而它却是nil时，viewController会调用该方法。不要直接调用该方法。
如果手工维护views，必须重载重写该方法
如果使用IB维护views，必须不能重载重写该方法
loadView和IB构建view
你在控制器中实现了loadView方法，那么你可能会在应用运行的某个时候被内存管理控制调用。 如果设备内存不足的时候， view 控制器会收到didReceiveMemoryWarning的消息。 默认的实现是检查当前控制器的view是否在使用。 如果它的view不在当前正在使用的view hierarchy里面，且你的控制器实现了loadView方法，那么这个view将被release, loadView方法将被再次调用来创建一个新的view。
 
###viewDidLoad方法
viewDidLoad 此方法只有当view从nib文件初始化的时候才被调用。
重载重写该方法以进一步定制view
在iPhone OS 3.0及之后的版本中，还应该重载重写viewDidUnload来释放对view的任何索引
viewDidLoad后调用数据Model
‍
###viewDidUnload方法‍
当系统内存吃紧的时候会调用该方法（注：viewController没有被dealloc）
内存吃紧时，在iPhone OS 3.0之前didReceiveMemoryWarning是释放无用内存的唯一方式，但是OS 3.0及以后viewDidUnload方法是更好的方式
在该方法中将所有IBOutlet（无论是property还是实例变量）置为nil（系统release view时已经将其release掉了）
在该方法中释放其他与view有关的对象、其他在运行时创建（但非系统必须）的对象、在viewDidLoad中被创建的对象、缓存数据等 release对象后，将对象置为nil（IBOutlet只需要将其置为nil，系统release view时已经将其release掉了）
一般认为viewDidUnload是viewDidLoad的镜像，因为当view被重新请求时，viewDidLoad还会重新被执行
viewDidUnload中被release的对象必须是很容易被重新创建的对象（比如在viewDidLoad或其他方法中创建的对象），不要release用户数据或其他很难被重新创建的对象
###dealloc方法
viewDidUnload和dealloc方法没有关联，dealloc还是继续做它该做的事情