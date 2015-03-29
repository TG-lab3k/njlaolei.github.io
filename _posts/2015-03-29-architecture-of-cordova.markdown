---
layout: post
title: "Cordova(PhoneGap)体系结构(Android)"
categories: Android
---
说明：

本文档只争对Cordova(PhoneGap)的Android端，基于Cordova2.1.0版本。

一．总体结构

Cordova的目标是用HTML，JS，来完成手机客户端的开发，并且是只开发一次可以在各种手机平台上跑，所以理想状态是用JS去控制所有事件。Cordova基于WebView组件。每个继承自DroidGap的Activity对应一个独立的CordovaWebView。Cordova提供了一些列的JS接口来访问Android的native（详细参见http://docs.phonegap.com/en/2.1.0/）。以插件（Plugin）的形式提供自定义接口给JS端访问。

二．一些疑问

1. Cordova框架是如何启动的？

2. 插件是怎么回事？如何工作的。

3. Cordova的官方文档都是说JS如何访问Android的native，那么在Android的native中是否可以访问JS的函数？如何访问？

三．结构解剖
1．Cordova的启动
1） 如何启动
Cordova提供了一个Class（DroidGap）和一个interface（CordovaInterface）来让Android开发者开发Cordova。一般情况下实现DroidGap即可，因为DroidGap类已经做了很多准备工作，可以说DroidGap类是Cordova框架的一个重要部分；如果在必要的情况下实现CordovaInterface接口，那么这个类中很多DroidGap的功能需要自己去实现。
继承了DroidGap或者CordovaInterface的Activity就是一个独立的Cordova模块，独立的Cordova模块指的是每个实现了DroidGap或者CordovaInterface接口的Activity都对应一套独立的WebView，Plugin，PluginManager，没有共享的。（我觉得这样是很不爽的，没有共享，如果plugin和Activity很多的情况下这样是相当的耗资源）
当在实现了DroidGap或者CordovaInterface接口的Activity的onCreate方法中调用DroidGap的loadUrl方法即启动了Cordova框架。
<!--excerpt-->

2）启动过程
{% highlight java %}
public class A extends DroidGap{}
{% endhighlight %}

a. 在A的onCreate方法中首先调用super. onCreate()，
这一步中创建了一个LinearLayout（LinearLayoutSoftKeyboardDetect.class）布局，以及计算这个Layout的大小及显示的一些方式。在后面的过程中会将一个WebView加到这个LinearLayout中。

b. 再次调用DroidGap 的loadUrl()方法
DroidGap. loadUrl()中首先判断A的CordovaWebView是否实例化，如果没有回实例化一个CordovaWebView对象并将该对象添加到父LinearLayout中去，同时将该LinearLayout添加的ContentView。见以下代码:

{% highlight java %}
public void onCreate(Bundle savedInstanceState) {
   …
   root = new LinearLayoutSoftKeyboardDetect(this, width, height);
   …
}
 
public void init() {
    CordovaWebView webView = new CordovaWebView(DroidGap.this);
    this.init(webView, 
    new CordovaWebViewClient(this, webView),
    new CordovaChromeClient(this, webView));
}
 
public void init(CordovaWebView webView, CordovaWebViewClient webViewClient, CordovaChromeClient webChromeClient) {
    this.appView = webView;
    this.appView.setId(100);
    …
    this.root.addView(this.appView);
    setContentView(this.root);
}
 
public void loadUrl(String url) {
   if (this.appView == null) {
      this.init();
   }
   …
}
{% endhighlight %}

这样就完成了我们在一般开发中使用的模式：
{% highlight java %}
public void onCreate(Bundle savedInstanceState) {
   super. onCreate(Bundle savedInstanceState);
   setContentView(resID);
   …
}
{% endhighlight %}

在初始化完CordovaWebView后调用CordovaWebView.loadUrl()。此时完成Cordova的启动。
c. 在实例化CordovaWebView的时候, CordovaWebView对象会去创建一个属于当前CordovaWebView对象的插件管理器PluginManager对象，一个消息队列NativeToJsMessageQueue对象，一个JavascriptInterface对象ExposedJsApi，并将ExposedJsApi对象添加到CordovaWebView中，JavascriptInterface名字为：_cordovaNative。

d. Cordova的JavascriptInterface

在创建ExposedJsApi时需要CordovaWebView的PluginManager对象和NativeToJsMessageQueue对象。因为所有的JS端与Android native代码交互都是通过ExposedJsApi对象的exec方法。在exec方法中执行PluginManager的exec方法，PluginManager去查找具体的Plugin并实例化然后再执行Plugin的execute方法，并根据同步标识判断是同步返回给JS消息还是异步。由NativeToJsMessageQueue统一管理返回给JS的消息。

e. 何时加载Plugin，如何加载

 Cordova在启动每个Activity的时候都会将配置文件中的所有plugin加载到PluginManager。那么是什么时候将这些plugin加载到PluginManager的呢？在b中说了最后会调用CordovaWebView.loadUrl()，对，就在这个时候会去初始化PluginManager并加载plugin。PluginManager在加载plugin的时候并不是马上实例化plugin对象，而是只是将plugin的Class名字保存到一个hashmap中，用service名字作为key值。

当JS端通过JavascriptInterface接口的ExposedJsApi对象请求Android时，PluginManager会从hashmap中查找到plugin，如果该plugin还未实例化，利用java反射机制实例化该plugin，并执行plugin的execute方法。



2．Cordova插件

在『1』中已经接触了些Cordova插件的东西，这里总结下Cordova的插件

Cordova插件只是一个普通的java类，没有什么特殊之处。插件中的execute方法只是Cordova框架的一个规定。

Cordova为了方便于插件的管理，所以引进了一个PluginManager来管理插件。在ExposedJsApi中，PluginManager起一个代理作用。

在原生态的WebView开发中，我们可以给WebView添加一个JavascriptInterface对象来使JS可以访问android的代码；在Cordova也有一个这样的对象ExposedJsApi。

在ExposedJsApi中，它将请求转发给我们的PluginManager，这个时候PluginManager会根据给的service的名字来查找具体的Plugin并执行。

所以，我认为Cordova插件是android端的API，提供给JS。

在Cordova虽然有JavascriptInterface对象ExposedJsApi，但在JS端并不是真正通过android提供的window.JavascriptInterface.request这种方式来请求。在JS端，Cordova是通过JS的prompt()函数触发ChromeClient中的onJsPrompt方法，通过onJsPrompt去获取到请求，再将请求转发给ExposedJsApi。

注：在Cordova中，Java端和JavaScript端都准备了代码来使用特殊URL（'http://cdv_exec/' + service + '#' + action + '#' + callbackId + '#' + argsJson;）的方式交互请求，但并没有真正使用。

3．Cordova的数据返回

   Cordova中通过exec()函数请求android插件，数据的返回可同步也可以异步于exec()函数的请求。

   在开发android插件的时候可以重写public boolean isSynch(String action)方法来决定是同步还是异步。

   Cordova在android端使用了一个队列(NativeToJsMessageQueue)来专门管理返回给JS的数据。

   1）同步

   Cordova在执行完exec()后，android会马上返回数据，但不一定就是该次请求的数据，可能是前面某次请求的数据；因为当exec()请求的插件是允许同步返回数据的情况下，Cordova也是从NativeToJsMessageQueue队列头pop头数据并返回。然后再根据callbackID反向查找某个JS请求，并将数据返回给该请求的success函数。

   

   2）异步

   Cordova在执行完exec()后并不会同步得到一个返回数据。Cordova在执行exec()的同时启动了一个XMLHttpRequest对象方式或者prompt()函数方式的循环函数来不停的去获取NativeToJsMessageQueue队列中的数据，并根据callbackID反向查找到相对应的JS请求，并将该数据交给success函数。

   注：Cordova对本地的HTML文件(file:// 开头的URL)或者手机设置有代理的情况下使用XMLHttpRequest方式获取返回数据，其他则使用prompt()函数方式获取返回数据。



4．Android代码访问JS

翻了Cordova的官方文档和Cordova代码，发现Cordova并未提供一种方式来让我们在Android中去访问JS。现在想来可能是这样的道理（个人观点），因为Cordova框架的性质就是一个用HTML+JS来开发APP的，相当于java用的虚拟机层（比喻不是很恰当），所以Cordova未提供Android访问JS的方式。

如果需要Android访问JS，只有使用原生态WebView开发的方式

loadUrl(“javascript:xxx”)

因为在Cordova框架中需要访问JS的时候也是使用的这种方式，如下(Activity的onResume事件)：
{% highlight java %}
public void handleResume(boolean keepRunning, boolean activityResultKeepRunning)
{
 
        // Send resume event to JavaScript
        this.loadUrl("javascript:try{cordova.fireDocumentEvent('resume');}catch(e){console.log('exception firing resume event from native');};");
 
        // Forward to plugins
        if (this.pluginManager != null) {
            this.pluginManager.onResume(keepRunning);
        }
 
        // Resume JavaScript timers (including setInterval)
        this.resumeTimers();
        paused = false;
}
{% endhighlight %}
