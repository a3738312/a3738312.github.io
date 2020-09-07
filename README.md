# 日常工作遇到的一些问题的笔记

# 替换服务器上资源一定要记得备份
# cocos creator
* 资源文件(图片,预制体等)名称不要有特殊符号和中文,可能会有问题  
* CocosCreator需要动态加载的在resources图片的图集，不要使用自动图集，否则会每个图片都生成一个json文件(没卵用)  
* CocosCreator搭建安卓原生环境的时候要注意：CocosCreator安装目录和sdk、ndk、ant的路径都不能有中文和空格  
* CocosCreator编译安卓原生的时候有类似以下报错或找不到文件夹可能是因为项目路径太深  
    ``` 
    fatal error: opening dependency file  
    ...No such file or directory
    ```
* CocosCreator中Java和JS互相调用
    > [如何在 Android 平台上使用 JavaScript 直接调用 Java 方法](https://docs.cocos.com/creator/manual/zh/advanced-topics/java-reflection.html?h=java)  
    > ***(特别注意String的方法签名 `Ljava/lang/String;` 后面的分号一定要加上去)***
* 在TS中引用JS `import js = requrie("./js")` 无代码提示
* 在JS中引用TS `import ts from "./ts";`
* 在资源管理器里删除资源或者手动移动资源后如果有报错，把 `library` `local` `temp` 目录删掉重新打开
* 两个ts文件互相引用编辑器会报错
* git同步场景可能会因为冲突导致无法解决的报错，这时可以放弃较少修改的部分，同步完后重新修改场景再提交
* `node._touchListener.setSwallowTouches(false);`可以让去掉点击事件截断，非父子节点也可穿透
* CC默认的摄像机是透视模式的，哪怕是2d节点，如果需要用3d节点做倾斜文字，需要将摄像机设置为正交摄像机，不然因为透视会导致每个3d节点显示的角度不一样。
# 原生
* 通过使用PowerManager里面的WakeLock可以使游戏不息屏
    > [Android-WakeLock(唤醒锁与CPU休眠/屏幕常亮)](https://blog.csdn.net/qq_32115439/article/details/80169222)
* 接入一些需要设置某些值的SDK，要判断SDK和游戏都初始化完毕了再做后面的事情
* Google play servers 接入的时候要注意创建OAth2.0客户端，一个调试客户端，一个正式客户端
* 安卓Facebook分享需要base64图片或bitmap，可以在JS里处理图片(截图、拼图等)然后调用jsb.saveImageData()保存在本地，安卓根据路径读取图片直接以bitmap传入即可(会比较块)
* Cocos Creator 生成的配置文件里，主Acticity的任务关联是空字符串，会导致其他的Actictiy独立于App显示在最近任务中，比如激励视频广告
* **android:taskAffinity:**  
    * 与 `Activity` 有着相似性的任务。从概念上讲，具有同一相似性的 `Activity` 归属同一任务（从用户的角度来看，则是归属同一“应用”）。任务的相似性由其根 `Activity` 的相似性确定。（可以用来制作小程序这样独立于app的任务）
    * 相似性确定两点内容 — `Activity` 更改父项后的任务（请参阅 allowTaskReparenting 属性），以及通过 `FLAG_ACTIVITY_NEW_TASK` 标记启动 `Activity` 时，用于容纳该 `Activity` 的任务。
    * 默认情况下，应用中的所有 `Activity` 都具有同一相似性。您可以设置该属性，以不同方式将其分组，甚至可以在同一任务内放置不同应用中定义的` Activity`。如要指定 `Activity` 与任何任务均无相似性，请将其设置为空字符串。(若主`Activity`设置为空字符串则所有任务都没有相似性)
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
    ```
    adb shell am broadcast -a com.android.vending.INSTALL_REFERRER -n "包名/Receiver完整地址" --es "referrer" "utm_source%3DtestSource%26utm_medium%3DtestMedium%26utm_term%3DtestTerm%26utm_content%3DtestContent%26utm_campaign%3DtestCampaign"
    ```
* 安卓JAVA和webview内部js交互
     ```Java
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
    ```Typescript
    // ts调用java
    public sendMsgToJava(__data:string) {
        let __msg = JSON.stringify(__data);
        if (window.gameViewAPI && window.gameViewAPI.sendMsgToJava) {
            LogComponent.Log("sendMsgToJava: " + __msg);
            window.gameViewAPI.sendMsgToJava(__msg);
        }
    }
    
    //接收java调用,要在全局的地方定义
    window["revMsgToJava"] = (data: string) => {
         //处理数据
    }
    ```
* [安卓获取设备ID](https://www.jianshu.com/p/671e1da50b33)
* 写effect的时候要注意参数格式不能有boolean，因为安卓底层转换格式没有boolean
# QQ小游戏
* 远程加载资源有不支持的文件格式，下载不支持的格式会报错 Download Fielded
* CocosCreator在QQ小游戏里获取环境是安卓，要加判断
* CocosCreator发布QQ小游戏流程：发布微信小游戏，用VS Code打开微信小游戏文件夹，全局搜索'wx.'替换成'qq.'  saveFile前需改成fs(和微信不一样)
* qq小程序开发者工具打开工程会报错时更新到最新版即可
# 字节跳动小游戏
* CocosCreator直接发布微信小游戏即可
# OPPO小游戏
* OPPO小游戏发布需要将录屏和更多游戏按钮去掉，在结算页添加结算页互推
* 远程音频下载后用本地地址或者直接使用远程地址的音效不能用cocos自己的音频引擎播放，需要使用oppo的`InnerAudioContext`来播放
    ```typescript
    //官方示例
    var audio = qg.createInnerAudioContext()
    audio.loop = true
    audio.volume = 0.7
    audio.autoplay = false
    var playSound = function() {
    audio.play()
    audio.offCanplay(playSound)
    }
    audio.onCanplay(playSound)
    audio.src = 'res/demo.mp3'
    ```
* 音频播放在切后台的时候调用暂停音频的话，在切前台的时候有可能音频还会自动播放，但是音频当前状态还是暂停，这种时候可以在且前台先调用`audio.play()`再调用`audio.puase()`来暂停
# VIVO小游戏
* VIVO小游戏的远程音频需同OPPO一样处理
* VIVO小游戏在游戏里播放`cc.AudioClip`会出现没有声音的问题，需要修改源码：
    ``` js
    //调整下 [creator目录]/resource/builtin/vivo-adapter/engien/jsb-audio.js 的 接口 cc.audioEngine.playEffect 为
    cc.audioEngine.playEffect = function (filePath, loop) {
        return cc.audioEngine.play(filePath, loop || false, _effect.volume);
    };
    ```
# 小程序SDK接入
* main.js如果开了md5的话不能设置模板，所以有引用js的话每次出包都要记得加上
# 工作注意
* 遇事不决先保存
* 一个需求完成后，先提交一次再继续做别的需求
* 废弃代码及时清理
* 空闲的时候看以前写的代码是否可以优化
