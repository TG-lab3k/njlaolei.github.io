"<1> main@830013584504" prio=5 runnable
  java.lang.Thread.State: RUNNABLE
	  at com.armterla.cleaner.widget.CstRelativeLayout.onMeasure(CstRelativeLayout.java:69)
	  at android.view.View.measure(View.java:12911)
	  at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:4805)
	  at android.widget.FrameLayout.onMeasure(FrameLayout.java:297)
	  at android.support.v7.internal.widget.ContentFrameLayout.onMeasure(ContentFrameLayout.java:124)
	  at android.view.View.measure(View.java:12911)
	  at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:4805)
	  at android.support.v7.internal.widget.ActionBarOverlayLayout.onMeasure(ActionBarOverlayLayout.java:444)
	  at android.view.View.measure(View.java:12911)
	  at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:4805)
	  at android.widget.FrameLayout.onMeasure(FrameLayout.java:297)
	  at android.view.View.measure(View.java:12911)
	  at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:4805)
	  at android.widget.LinearLayout.measureChildBeforeLayout(LinearLayout.java:1399)
	  at android.widget.LinearLayout.measureVertical(LinearLayout.java:676)
	  at android.widget.LinearLayout.onMeasure(LinearLayout.java:557)
	  at android.view.View.measure(View.java:12911)
	  at android.view.ViewGroup.measureChildWithMargins(ViewGroup.java:4805)
	  at android.widget.FrameLayout.onMeasure(FrameLayout.java:297)
	  at com.android.internal.policy.impl.PhoneWindow$DecorView.onMeasure(PhoneWindow.java:2097)
	  at android.view.View.measure(View.java:12911)
	  at android.view.ViewRootImpl.performTraversals(ViewRootImpl.java:1064)
	  at android.view.ViewRootImpl.handleMessage(ViewRootImpl.java:2442)
	  at android.os.Handler.dispatchMessage(Handler.java:99)
	  at android.os.Looper.loop(Looper.java:137)
	  at android.app.ActivityThread.main(ActivityThread.java:4441)
	  at java.lang.reflect.Method.invokeNative(Method.java:-1)
	  at java.lang.reflect.Method.invoke(Method.java:511)
	  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:823)
	  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:590)
	  at dalvik.system.NativeStart.main(NativeStart.java:-1)


measure 过程
1. 根据widthMeasureSpec&heightMeasureSpec计算缓存key
2. 判断是否forceLayout
3. 判断是否是Exactly
4. 判断是否matchingSize：Exactly模式下当前的measureWidth和measureHeight与View的measureWidth和measureHeight相当
5. 如果是forceLayout 或者是 （非matchingSize并且当前MeasureSpec不等于OldMeasureSpec）进行measure工作：
   measure过程：
   1）查找Measure缓存
   2）如果没在缓存中找到或者IgnoreMeasureCache调用onMeasure
   3）如果在缓存中找到，取出并设置为当前View的measureWidth和measureHeight， 并且标记layout的时候measure一次。
6. 将当前widthMeasureSpec和heightMeasureSpec保存为old值，将MeasuredWidth和MeasuredHeight存入缓存。



"<1> main@830013584504" prio=5 runnable
  java.lang.Thread.State: RUNNABLE
	  at com.armterla.cleaner.widget.ScanButton.setFrame(ScanButton.java:74)
	  at android.view.View.layout(View.java:11438)
	  at com.armterla.cleaner.widget.ScanButton.layout(ScanButton.java:69)
	  at android.widget.RelativeLayout.onLayout(RelativeLayout.java:930)
	  at com.armterla.cleaner.widget.CstRelativeLayout.onLayout(CstRelativeLayout.java:75)
	  at android.view.View.layout(View.java:11444)
	  at android.view.ViewGroup.layout(ViewGroup.java:4331)
	  at android.widget.FrameLayout.onLayout(FrameLayout.java:443)
	  at android.view.View.layout(View.java:11444)
	  at android.view.ViewGroup.layout(ViewGroup.java:4331)
	  at android.support.v7.internal.widget.ActionBarOverlayLayout.onLayout(ActionBarOverlayLayout.java:493)
	  at android.view.View.layout(View.java:11444)
	  at android.view.ViewGroup.layout(ViewGroup.java:4331)
	  at android.widget.FrameLayout.onLayout(FrameLayout.java:443)
	  at android.view.View.layout(View.java:11444)
	  at android.view.ViewGroup.layout(ViewGroup.java:4331)
	  at android.widget.LinearLayout.setChildFrame(LinearLayout.java:1666)
	  at android.widget.LinearLayout.layoutVertical(LinearLayout.java:1524)
	  at android.widget.LinearLayout.onLayout(LinearLayout.java:1429)
	  at android.view.View.layout(View.java:11444)
	  at android.view.ViewGroup.layout(ViewGroup.java:4331)
	  at android.widget.FrameLayout.onLayout(FrameLayout.java:443)
	  at android.view.View.layout(View.java:11444)
	  at android.view.ViewGroup.layout(ViewGroup.java:4331)
	  at android.view.ViewRootImpl.performTraversals(ViewRootImpl.java:1489)
	  at android.view.ViewRootImpl.handleMessage(ViewRootImpl.java:2442)
	  at android.os.Handler.dispatchMessage(Handler.java:99)
	  at android.os.Looper.loop(Looper.java:137)
	  at android.app.ActivityThread.main(ActivityThread.java:4441)
	  at java.lang.reflect.Method.invokeNative(Method.java:-1)
	  at java.lang.reflect.Method.invoke(Method.java:511)
	  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:823)
	  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:590)
	  at dalvik.system.NativeStart.main(NativeStart.java:-1)

