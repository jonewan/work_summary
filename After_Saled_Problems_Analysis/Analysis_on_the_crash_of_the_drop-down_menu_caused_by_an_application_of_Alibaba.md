# 关于阿里系某应用导致下拉菜单崩溃的问题分析

## 问题描述

有售后问题爆出阿里网盘在使用时出现崩溃的问题，导致整个界面卡死。

## 问题分析

根据现象可以看到，在使用阿里网盘时，进行下拉菜单时systemUI频繁crash，Crash的日志如下：

```
03-21 11:12:15.773  3336  3336 D StatusBar: panel: ACTION_DOWN at (1541.197266, 0.000000) mDisabled1=0x00000000 mDisabled2=0x00000000
03-21 11:12:15.784  3336  3336 I aaa     : isEthernetOpen------state>false
03-21 11:12:15.784  3336  3336 I aaa     : isEthernetOpen------state>false
03-21 11:12:15.793  3336  3336 D ccc     : blue   --setIconResource======isBlue=false
03-21 11:12:15.793  3336  3336 D ccc     : whiteBalance   --setIconResource======isWhiteBalance=false
03-21 11:12:15.805   470   470 W SurfaceFlinger: FB is protected: PERMISSION_DENIED
03-21 11:12:15.807  3336  3336 E SurfaceFlinger: captureScreen failed to readInt32: -1
03-21 11:12:15.807  3336  3336 W SurfaceControl: Failed to take screenshot
03-21 11:12:15.809  3336  3336 E InputEventReceiver: Exception dispatching input event.
03-21 11:12:15.810  3336  3336 E MessageQueue-JNI: Exception in MessageQueue callback: handleReceiveCallback
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI: java.lang.NullPointerException: Attempt to invoke virtual method 'android.graphics.Bitmap android.graphics.Bitmap.copy(android.graphics.Bitmap$Config, boolean)' on a null object reference
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at com.android.systemui.statusbar.phone.PanelView.onTouchEvent(PanelView.java:317)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at com.android.systemui.statusbar.phone.NotificationPanelView.onTouchEvent(NotificationPanelView.java:1077)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at com.android.systemui.statusbar.phone.PanelBar.onTouchEvent(PanelBar.java:148)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.View.dispatchTouchEvent(View.java:13463)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3175)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2831)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3181)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2788)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3181)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2788)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at com.android.systemui.statusbar.phone.StatusBarWindowView.dispatchTouchEvent(StatusBarWindowView.java:407)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.View.dispatchPointerEvent(View.java:13727)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$ViewPostImeInputStage.processPointerEvent(ViewRootImpl.java:5873)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$ViewPostImeInputStage.onProcess(ViewRootImpl.java:5668)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:5167)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.onDeliverToNext(ViewRootImpl.java:5220)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.forward(ViewRootImpl.java:5186)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$AsyncInputStage.forward(ViewRootImpl.java:5326)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.apply(ViewRootImpl.java:5194)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.vew.ViewRootImpl$AsyncInputStage.apply(ViewRootImpl.java:5383)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:5167)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.onDeliverToNext(ViewRootImpl.java:5220)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.forward(ViewRootImpl.java:5186)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.apply(ViewRootImpl.java:5194)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:5167)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl.deliverInputEvent(ViewRootImpl.java:7973)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl.doProcessInputEvents(ViewRootImpl.java:7942)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl.enqueueInputEvent(ViewRootImpl.java:7893)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.ViewRootImpl$WindowInputEventReceiver.onInputEvent(ViewRootImpl.java:8112)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.view.InputEventReceiver.dispatchInputEvent(InputEventReceiver.java:188)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.os.MessageQueue.nativePollOnce(Native Method)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.os.MessageQueue.next(MessageQueue.java:336)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.os.Looper.loop(Looper.java:174)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at android.app.ActivityThread.main(ActivityThread.java:7428)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at java.lang.reflect.Method.invoke(Native Method)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:492)
03-21 11:12:15.811  3336  3336 E MessageQueue-JNI:      at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:980)
03-21 11:12:15.812  3336  3336 D AndroidRuntime: Shutting down VM
--------- beginning of crash
03-21 11:12:15.820  3336  3336 E AndroidRuntime: FATAL EXCEPTION: main
03-21 11:12:15.820  3336  3336 E AndroidRuntime: Process: com.android.systemui, PID: 3336
03-21 11:12:15.820  3336  3336 E AndroidRuntime: java.lang.NullPointerException: Attempt to invoke virtual method 'android.graphics.Bitmap android.graphics.Bitmap.copy(android.graphics.Bitmap$Config, boolean)' on a null object reference
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at com.android.systemui.statusbar.phone.PanelView.onTouchEvent(PanelView.java:317)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at com.android.systemui.statusbar.phone.NotificationPanelView.onTouchEvent(NotificationPanelView.java:1077)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at com.android.systemui.statusbar.phone.PanelBar.onTouchEvent(PanelBar.java:148)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.View.dispatchTouchEvent(View.java:13463)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3175)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2831)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3181)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2788)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewGroup.dispatchTransformedTouchEvent(ViewGroup.java:3181)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewGroup.dispatchTouchEvent(ViewGroup.java:2788)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at com.android.systemui.statusbar.phone.StatusBarWindowView.dispatchTouchEvent(StatusBarWindowView.java:407)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.View.dispatchPointerEvent(View.java:13727)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$ViewPostImeInputStage.processPointerEvent(ViewRootImpl.java:5873)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$ViewPostImeInputStage.onProcess(ViewRootImpl.java:5668)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:5167)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.onDeliverToNext(ViewRootImpl.java:5220)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.forward(ViewRootImpl.java:5186)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$AsyncInputStage.forward(ViewRootImpl.java:5326)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.apply(ViewRootImpl.java:5194)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$AsyncInputStage.apply(ViewRootImpl.java:5383)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:5167)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.onDeliverToNext(ViewRootImpl.java:5220)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.forward(ViewRootImpl.java:5186)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.apply(ViewRootImpl.java:5194)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$InputStage.deliver(ViewRootImpl.java:5167)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl.deliverInputEvent(ViewRootImpl.java:7973)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl.doProcessInputEvents(ViewRootImpl.java:7942)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl.enqueueInputEvent(ViewRootImpl.java:7893)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.ViewRootImpl$WindowInputEventReceiver.onInputEvent(ViewRootImpl.java:8112)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.view.InputEventReceiver.dispatchInputEvent(InputEventReceiver.java:188)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.os.MessageQueue.nativePollOnce(Native Method)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.os.MessageQueue.next(MessageQueue.java:336)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.os.Looper.loop(Looper.java:174)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at android.app.ActivityThread.main(ActivityThread.java:7428)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at java.lang.reflect.Method.invoke(Native Method)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:492)
03-21 11:12:15.820  3336  3336 E AndroidRuntime:        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:980)
03-21 11:12:15.833  3336  3336 I Process : Sending signal. PID: 3336 SIG: 9
```

发现systemUI发生了空指针的操作，导致整个systemUI crash了，具体的操作是由于下拉菜单的背景模糊导致的，根据上面的log也可以看到，是由于在screenshot的时候发生了异常，由于目前的下拉菜单背景模糊的实现原理是对当前整个画面进行截图，之后进行blur后作为整个下拉菜单的背景，而截图发生异常后就导致获取到的bitmap为空，继而发生了该问题。

那到底是什么导致截图失败的呢？其实可以根据log中的如下打印进行继续深追：

```
03-21 11:12:15.805   470   470 W SurfaceFlinger: FB is protected: PERMISSION_DENIED
03-21 11:12:15.807  3336  3336 E SurfaceFlinger: captureScreen failed to readInt32: -1
03-21 11:12:15.807  3336  3336 W SurfaceControl: Failed to take screenshot
```

发现在SurfaceFlinger合成时有截图失败的相关打印，在如下的代码处寻找原因：

```c
status_t SurfaceFlinger::captureScreenImplLocked(const RenderArea& renderArea,
                                                 TraverseLayersFunction traverseLayers,
                                                 ANativeWindowBuffer* buffer,
                                                 bool useIdentityTransform, bool forSystem,
                                                 int* outSyncFd, bool& outCapturedSecureLayers) {
    ATRACE_CALL();

	// 在此处会遍历所有的layer，并找出layer是否可见并声明了secure。
    traverseLayers([&](Layer* layer) {
        outCapturedSecureLayers =
                outCapturedSecureLayers || (layer->isVisible() && layer->isSecure());
    });

	// 针对声明了Secure的Layer，若非系统应用，则没有权限进行截图。
	// const bool forSystem = uid == AID_GRAPHICS || uid == AID_SYSTEM;
    if (outCapturedSecureLayers && !forSystem) {
        ALOGW("FB is protected: PERMISSION_DENIED");
        return PERMISSION_DENIED;
    }
    renderScreenImplLocked(renderArea, traverseLayers, buffer, useIdentityTransform, outSyncFd);
    return NO_ERROR;
}
```

因此，可以发现，一定是截图时存在了声明为Secure的layer才会导致截图失败的，其实Secure是WindowManager::LayoutParams其中的一个Flag，声明如下：

```java
/** Window flag: treat the content of the window as secure, preventing
 * it from appearing in screenshots or from being viewed on non-secure
 * displays.
 *
 * <p>See {@link android.view.Display#FLAG_SECURE} for more details about
 * secure surfaces and secure displays.
 */
public static final int FLAG_SECURE             = 0x00002000;
```

Android表示，若图层声明了Secure的flag，则会进行截图保护，会看整个`dumpsys window`，发现阿里网盘的login界面确实声明了Secure的flag：

```
Window #11 Window{8db6bd6 u0 com.alicloud.databox/com.ali.user.mobile.login.ui.UserLoginActivity}:
    mDisplayId=0 stackId=60 mSession=Session{32b0dd3 32590:u0a10131} mClient=android.os.BinderProxy@8aca3f1
    mOwnerUid=10131 mShowToOwnerOnly=true package=com.alicloud.databox appop=NONE
    mAttrs={(0,0)(fillxfill) sim={adjust=resize forwardNavigation} ty=BASE_APPLICATION fmt=TRANSLUCENT wanim=0x10302fe surfaceInsets=Rect(15, 15 - 15, 15) (manual) (!preservePreviousSurfaceInsets)
      fl=LAYOUT_IN_SCREEN SECURE LAYOUT_INSET_DECOR SPLIT_TOUCH HARDWARE_ACCELERATED TRANSLUCENT_STATUS DRAWS_SYSTEM_BAR_BACKGROUNDS
      pfl=FORCE_DRAW_STATUS_BAR_BACKGROUND
      vsysui=LAYOUT_STABLE LAYOUT_FULLSCREEN LIGHT_STATUS_BAR}
    Requested w=700 h=1080 mLayoutSeq=7518
 canReceiveKeys()=true    mHasSurface=true isReadyForDisplay()=true mWindowRemovalAllowed=false
    WindowStateAnimator{be561d5 com.alicloud.databox/com.ali.user.mobile.login.ui.UserLoginActivity}:
      Surface: shown=true layer=0 alpha=1.0 rect=(0.0,0.0) 730 x 1110 transform=(1.0, 0.0, 1.0, 0.0)
    mForceSeamlesslyRotate=false seamlesslyRotate: pending=null finishedFrameNumber=0
    isOnScreen=true
    isVisible=true
```

并且，systemUI在Android系统中并不是SYSTEM_UID应用，所以无法对阿里网盘的界面进行截图，而供应商在实现整个下拉菜单的模糊效果时采用的截图方式会直接导致该空指针崩溃的发生：

`src/com/android/systemui/statusbar/phone/PanelView.java`

```java
switch (event.getActionMasked()) {
    case MotionEvent.ACTION_DOWN:
        mScreenBitmap = SurfaceControl.screenshot(rect, screenWidth, screenHeight + navHeight, 0);// 此处出现空指针崩溃
        mScreenBitmap = mScreenBitmap.copy(Bitmap.Config.ARGB_8888, true);
        notifyBarPanelExpansionChanged();
        setExpandedHeightInternal(wHeight);
        mStatusBar.getNotificationContainer().setBitmap(mScreenBitmap);
        break;
    case MotionEvent.ACTION_UP:
    case MotionEvent.ACTION_CANCEL:
        if(!mStatusBar.getNotificationContainer().isOpened() && mExpandedFraction == 1.0)
            mStatusBar.OnNotificationClose();
        break;
}

```

## 总结

总体而言，整个下拉菜单背景模糊的实现方式存在漏洞，若任何的window声明了Flag_secure的话均会导致下拉菜单无法截图而触发空指针崩溃，因此最稳妥的解决方案是在SurfaceFlager实现动态模糊，这也是行业内比较优秀的做法。
