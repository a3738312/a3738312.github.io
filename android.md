# 安卓原生

* 通过使用PowerManager里面的WakeLock可以使游戏不息屏

  > [Android-WakeLock\(唤醒锁与CPU休眠/屏幕常亮\)](https://blog.csdn.net/qq_32115439/article/details/80169222)

* 接入一些需要设置某些值的SDK，要判断SDK和游戏都初始化完毕了再做后面的事情
* Google play servers 接入的时候要注意创建OAth2.0客户端，一个调试客户端，一个正式客户端
* 安卓Facebook分享需要base64图片或bitmap，可以在JS里处理图片\(截图、拼图等\)然后调用 `typescriptb.saveImageData()` 保存在本地，安卓根据路径读取图片直接以bitmap传入即可\(会比较块\)
* Cocos Creator 生成的配置文件里，主Acticity的任务关联是空字符串，会导致其他的Actictiy独立于App显示在最近任务中，比如激励视频广告
* **android:taskAffinity:**  
  * 与 `Activity` 有着相似性的任务。从概念上讲，具有同一相似性的 `Activity` 归属同一任务（从用户的角度来看，则是归属同一“应用”）。任务的相似性由其根 `Activity` 的相似性确定。（可以用来制作小程序这样独立于app的任务）
  * 相似性确定两点内容 — `Activity` 更改父项后的任务（请参阅 allowTaskReparenting 属性），以及通过 `FLAG_ACTIVITY_NEW_TASK` 标记启动 `Activity` 时，用于容纳该 `Activity` 的任务。
  * 默认情况下，应用中的所有 `Activity` 都具有同一相似性。您可以设置该属性，以不同方式将其分组，甚至可以在同一任务内放置不同应用中定义的`Activity`。如要指定 `Activity` 与任何任务均无相似性，请将其设置为空字符串。\(若主`Activity`设置为空字符串则所有任务都没有相似性\)
  * 如果未设置该属性，则 `Activity` 会继承为应用设置的相似性（请参阅[`<application>`](https://developer.android.com/guide/topics/manifest/application-element)元素的 taskAffinity 属性）。应用默认相似性的名称为[`<manifest>`](https://developer.android.com/guide/topics/manifest/manifest-element)元素所设置的软件包名称。
* 安卓查询最近系统发的广播：[`adb shell dumpsys | grep BroadcastRecord`](https://blog.csdn.net/g19920917/article/details/38032413)
* 谷歌的广告归因需要将firebase接入到项目中
* 谷歌支付如果查询不到商品信息，可能是应用没有上架谷歌商店
* 谷歌支付如果闪一下返回错误6，有可能是谷歌商店没有允许后台运行和后台开启
* 或许会用到的东西：
  * 使用外部存储：  

    [关于获得安卓外部存储读写权限](https://www.cnblogs.com/zanzg/p/9129375.html)  

    [Android 文件外/内部存储的获取各种存储目录路径](https://blog.csdn.net/csdn_aiyang/article/details/80665185)
* 接入Facebook Banner广告可以新建一个Activity然后把AdView添加到该Activity上
* 谷歌广告归因测试： 

  ```text
    adb shell am broadcast -a com.android.vending.INSTALL_REFERRER -n "包名/Receiver完整地址" --es "referrer" "utm_source%3DtestSource%26utm_medium%3DtestMedium%26utm_term%3DtestTerm%26utm_content%3DtestContent%26utm_campaign%3DtestCampaign"
  ```

* 安卓JAVA和webview内部js交互

  ```java
    //给webview注册监听接收js调用
    webView.addJavascriptInterface(new gameViewAPI(), "gameViewAPI");
    //接受js调用的方法
     static class gameViewAPI {
        @JavascriptInterface
        public String sendMsgToJava(final String __msg) {
            //处理数据
            return "";
        }
    }

    public static void sendMsgToGame(final String __msg) {
        if (webView != null && activity != null) {
            //发送消息到游戏必须在ui线程调用
            activity.runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    //发送消息到游戏
                    //window.revMsgFromLobby在js中全局定义
                    webView.loadUrl(
                        "javascript:window.revMsgToJava(" + __msg + ")");
                }
            });
        } else {
             Log.d(TAG, "webview or activity is null");
        }
    }
  ```

  ```typescript
    // js调用java
    if (window.gameViewAPI && window.gameViewAPI.sendMsgToJava) {
        LogComponent.Log("sendMsgToJava: " + __msg);
        window.gameViewAPI.sendMsgToJava(__msg);
    }

    //接收java调用,要在全局的地方定义
    window["revMsgToJava"] = (data: string) => {
         //处理数据
    }
    or
    window.revMsgToJava = (data) => {

    }
  ```

* [安卓获取设备ID](https://www.jianshu.com/p/671e1da50b33)
* 写effect的时候要注意参数格式不能有boolean，因为安卓底层转换格式没有boolean
* 使用 Android studio 打开构建出的安卓项目时，若出现一下报错，可能是 NDK 路径配置或 NDK 版本有问题
  ``` java
  A problem occurred configuring project ':app'.
    java.lang.NullPointerException (no error message)
  ```
* 亚马逊与谷歌同时使用AdMob广告需要使用不同的包名

* ![图片](.\image\android1.png)
此方式引入会导致引入所有的 `firebase` 相关包
* 播放音频后同一帧或极短时间内调用stop停止音频，可能会引发系统死锁进而导致闪退。报错日志可能包含此关键词 `pthread_mutex_lock`