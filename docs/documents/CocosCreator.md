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
## 关于游戏优化
* 加载场景时会把场景依赖的资源也一起加载，所以尽量不要把所有东西都放在场景内
* 关于内存占用，动态加载预制体也会把预制体依赖的资源一起加载进来，所以要及时释放掉
* 图片纹理占用内存大小是和图片的分辨率大小相关的，与图片本身文件大小无关，所以可以用小图的尽量用小图，能够显著降低该图片的内存占用
* 场景内层级尽量不要太深，否则可能会影响渲染效率
* 同一个界面的东西尽量在同一个层级，并且将同一个界面的图片做成图集，这样DC就只会有一次，在DC太高的时候可以这样进行优化
## 挺不错的文章
>[Cocos入门指引](https://forum.cocos.org/t/cocos/94728)  
[使用Creator三年的游戏开发总结](https://forum.cocos.org/t/creator/94747)  
[解读 Cocos Creator 引擎：让实例化快 50% 的原理，“拖节点”性能会更好吗？](https://forum.cocos.org/t/cocos-creator-50/92957)  
[[教程] | 物理挖洞/涂抹地形的几种实现讨论 [大集合整理篇]](https://forum.cocos.org/t/topic/91985)  
[[教程]Creator迷宫的生成: DFS与BFS算法实现](https://forum.cocos.org/t/creator-dfs-bfs/93906)  
[[ituuz分享]探照灯效果shader实现](https://forum.cocos.org/t/ituuz-shader/94180)  
[Cocos3D《病毒传播模拟器》游戏版本1 开发日志和总结](https://forum.cocos.org/t/cocos3d-1/94592)  
[游戏性能调优](https://forum.cocos.org/t/topic/95040)  
[Cocos Creator 性能优化：DrawCall（全面！）](https://forum.cocos.org/t/cocos-creator-drawcall/95043)  
[slg系列（地图篇）3d透视无限地图完成……其他补充中..](https://forum.cocos.org/t/slg-3d/95028)  
[slg 细分篇 ——— 无限地图原理](https://forum.cocos.org/t/slg/95269)  
[自定义渲染合批之自定义顶点格式（附 Demo 和引擎源码解读）](https://forum.cocos.org/t/demo/95087)  
[Cocos Creator 一种「无侵入」资源加密方案](https://forum.cocos.org/t/cocos-creator/95492)  
[Cocos Creator 通用框架设计 —— 资源管理](https://forum.cocos.org/t/cocos-creator/84793)  
[Cocos Creator 通用框架设计 —— 资源管理优化](https://forum.cocos.org/t/cocos-creator/93517)  
[自定义渲染合批之自定义顶点格式（附 Demo 和引擎源码解读）](https://forum.cocos.org/t/demo/95087)  
[Cocos Creator 水的浮力实现过程](https://forum.cocos.org/t/cocos-creator/96116)  
[制作一个可以将小游戏快速接入APP的安卓SDK-开篇](https://forum.cocos.org/t/app-sdk/95810)  
[解读 Cocos Creator 引擎：cc.AssetManager（1）重构后的 cc.loader](https://forum.cocos.org/t/cocos-creator-cc-assetmanager-1-cc-loader/92319)  
[Cocos Creator 微信小游戏平台启动与包体优化（首屏渲染耗时降低 50%）](https://forum.cocos.org/t/cocos-creator-50/94999)  
[如何巧用设计图提高UI的还原度](https://forum.cocos.org/t/ui/96354)  
[使用CocosCreator模拟书本翻页效果](https://forum.cocos.org/t/cocoscreator/96358)  
[Creator web平台setting.js 文件详细解析和资源加载解析](https://forum.cocos.org/t/creator-web-setting-js/78669)  
[cocos实现对ETC2的支持](https://forum.cocos.org/t/cocos-etc2/49061)  
[避免纹理抖动](https://forum.cocos.org/t/topic/91307/7)
[使用2种方式实现动画的动态蒙版](https://forum.cocos.org/t/topic/96372)