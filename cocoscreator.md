# Cocos Creator

## 综合
**替换服务器上资源一定要记得备份**
* 遇事不决先保存
* 一个需求完成后，先提交一次再继续做别的需求
* 废弃代码及时清理
* 空闲的时候看以前写的代码是否可以优化  
* 资源文件 ( 图片,预制体等 ) 名称不要有特殊符号和中文,可能会有问题  
* Cocos Creator 搭建安卓原生环境的时候要注意：Cocos Creator 安装目录和 sdk、ndk、ant 的路径都不能有中文和空格  
* Cocos Creator 编译安卓原生的时候有类似以下报错或找不到文件夹可能是因为项目路径太深  
    `fatal error: opening dependency file ... No such file or directory`
* CocosCreator 中 Java 和 JS 互相调用  
  > [如何在 Android 平台上使用 JavaScript 直接调用 Java 方法](https://docs.cocos.com/creator/manual/zh/advanced-topics/java-reflection.html?h=java)  
  >> _**\(特别注意String的方法签名 `Ljava/lang/String;` 后面的分号一定要加上去\)**_  
* 在 TS 中引用 JS `import js = require("./js")`
* 在 JS 中引用 TS `import ts from "./ts";`
* 在资源管理器里删除资源或者手动移动资源后如果有报错，把 `library` `local` `temp` 目录删掉重新打开
* TS 循环引用会报错  
* Git 同步场景可能会因为冲突导致无法解决的报错，这时可以放弃较少修改的部分，同步完后重新修改场景再提交
* `node._touchListener.setSwallowTouches(false);` 可以让去掉点击事件截断，非父子节点也可穿透，使用 `cc.macro.ENABLE_MULTI_TOUCH = false;` 关闭多点触碰，会导致点击事件被其他节点截断，哪怕层级本该被该层级遮挡
* Cocos Creator 默认的摄像机是透视模式的，如果需要用3D节点做倾斜文字，需要将摄像机设置为正交摄像机，不然因为透视会导致每个3D节点显示的角度不一样。
* `CCLabel` 的 `string` 修改后节点大小会在下一帧才刷新，2.2版本前可以使用 `label._updateRenderData()` 来手动刷新节点大小，之后可以使用 `label._forceUpdateRenderData()` 来刷新。刷新后获取节点大小就是修改内容后正确的大小
* ~~`cc.audioEngine.setFinishCallback(id, null);`~~//设置完成回调函数不能写null，会导致原生平台报错
* Spine 骨骼动画可以在不同轨道播放动画来实现动画混合效果，轨道动画播放完之后需要清除轨道动画，否则动画会一直覆盖在上面。使用 `setTrackCompleteListener` 来监听动画是否播放完毕
* 在项目中设置好布局之后，在项目的 `项目目录/local/layout.editor.json` 找到 `layout` 字段，将字段内容复制到 `编辑器目录/resources/static/layout` 中找到 `landscape.json` 并拷贝进去，即可替换
* 在2.4.3中发现，Collider 碰撞组件修改 `size` 或者 `offset` 之后调用 `apply()` 应用修改时，会让碰撞**组件启用**，效果**等同于** `enabled` 设置为 `true`；但实际上 `enabled` 的值**并不会发生变化**，所以如果碰撞组件在关闭的情况下修改大小位置等并应用，且在之后马上设置 `enabled` 为 `false` 会出现没有效果的问题，在 `enabled` 为 `false` 的情况下修改碰撞组件参数，不需要调用 `apply()`，在下次 `enabled` 值设置为 `true` 会应用
* 2.x 中，通过修改 `...\resources\static\template\new-script.ts` 可以自定义创建脚本模板
### Shader
[OpenGL ES](https://www.jianshu.com/p/99daa25b4573)  
[Learn OpenGL CN](https://learnopengl-cn.github.io/)  
* 大致流程  
  1. 获取图像数据流
  2. `顶点处理` 根据投影等矩阵变换改变顶点位置，并根据顶点位置来计算纹理坐标的位置
  3. `图元拼装` 根据处理好的顶点及纹理坐标信息，将纹理组装成图元
  4. `栅格化操作` 根据处理好的图元数据，分解成更小的对应缓冲区像素的片元
  5. `片元处理` 通过纹理坐标取得纹理中相对应的片元像素值,根据自己的业务处理来变换这个片元的颜色
  6. `帧缓冲操作` 将最终的像素值写入帧缓冲区
* `顶点着色器` 是用来**替代** `顶点处理` 阶段的，`片元着色器` 是用来**替代** `片元处理` 阶段的
* Cocos creator Effect 语法使用 `GLES语法(与C语言很相似)`，所以语法学习并不难，难点在于数学  

### 关于插件

* JS 文件可以勾选导入为插件，插件 JS 文件不可用 `ccclass` 类，会在游戏代码前加载，可以用于初始化一些全局变量、方法等

## 关于Tiled Map

* 可以在Tiled Map中新建对象层，使用多边形来设置不规则碰撞箱，在 Cocos Creator 内获得所有多边形的数组来动态生成多边形碰撞：  
  ```typescript
        let objects = map.getObjectGroup("collision").getObjects();//获取对象层内所有对象
        for (let i = 0; i < objects.length; i++) {
            let collider = new cc.Node("collision");
            this.node.addChild(collider);
            collider.setPosition(map.node.position.x + objects[i].x, map.node.position.y + objects[i].y);
            let body = collider.addComponent(cc.RigidBody);
            body.type = cc.RigidBodyType.Static;//设置刚体类型为静态
            body.allowSleep = true;//设置自动休眠为true
            body.gravityScale = 0;//设置受重力影响为0
            body.awakeOnLoad = true;//设置默认唤醒
            let polygon = collider.addComponent(cc.PhysicsPolygonCollider);
            let p = objects[i].points;
            let points: cc.Vec2[] = [];
            for (let i = 0; i < p.length; i++) {
                let v2 = cc.v2(p[i].x, p[i].y);
                points.push(v2);
            }
            polygon.points = points;
            polygon.apply();
        }
  ```

## 关于游戏优化

* 加载场景时会把场景依赖的资源也一起加载，所以尽量不要把所有东西都放在场景内  
* 关于内存占用，动态加载预制体也会把预制体依赖的资源一起加载进来，所以要及时释放掉  
* 图片纹理占用内存是根据色彩存储方式计算的，一般保存文件都是 `ARGB8888` 
  > 每个像素占四位，即A=8，R=8，G=8，B=8，那么一个像素点占8+8+8+8=32位
  
  所以一张 1024 x 1024 的图片，占用内存就是 1024 x 1024 x 32 = 33554432 位，也就是 `8.192MB`，但是如果换成 `ARGB4444` 
  >每个像素占四位，即A=4，R=4，G=4，B=4，那么一个像素点占4+4+4+4=16位

  同样 1024 x 1024 的图片，占用内存就是 1024 x 1024 x 16 = 16777216 位，也就是 `4.096MB`，但是相对的，也会有一定的失真  
* 开启动态合图的情况下，将可以参与动态合图的节点相邻即可合并 Draw call，Label 默认会打断合批，但是如果文本缓存模式设置为 `Bitmap` 或 `Char` 则也可以参与动态合图，从而降低 Draw call

  > [游戏性能调优](https://forum.cocos.org/t/topic/95040)  
  > [Cocos Creator 性能优化：DrawCall（全面！）](https://forum.cocos.org/t/cocos-creator-drawcall/95043)  
  > [Cocos Creator 微信小游戏平台启动与包体优化（首屏渲染耗时降低 50%）](https://forum.cocos.org/t/cocos-creator-50/94999)  
  > [自定义渲染合批之自定义顶点格式（附 Demo 和引擎源码解读）](https://forum.cocos.org/t/demo/95087)  
  > [解读 Cocos Creator 引擎：让实例化快 50% 的原理，“拖节点”性能会更好吗？](https://forum.cocos.org/t/cocos-creator-50/92957)  
  > [如何提升JSON.stringify\(\)的性能？](https://segmentfault.com/a/1190000019400854)

## 挺不错的文章

* 还未整理分类

  > [Cocos入门指引](https://forum.cocos.org/t/cocos/94728)  
  > [使用Creator三年的游戏开发总结](https://forum.cocos.org/t/creator/94747)  
  > [\[教程\] \| 物理挖洞/涂抹地形的几种实现讨论 \[大集合整理篇\]](https://forum.cocos.org/t/topic/91985)  
  > [\[教程\]Creator迷宫的生成: DFS与BFS算法实现](https://forum.cocos.org/t/creator-dfs-bfs/93906)  
  > [\[ituuz分享\]探照灯效果shader实现](https://forum.cocos.org/t/ituuz-shader/94180)  
  > [Cocos3D《病毒传播模拟器》游戏版本1 开发日志和总结](https://forum.cocos.org/t/cocos3d-1/94592)  
  > [slg系列（地图篇）3d透视无限地图完成……其他补充中..](https://forum.cocos.org/t/slg-3d/95028)  
  > [slg 细分篇 ——— 无限地图原理](https://forum.cocos.org/t/slg/95269)  
  > [自定义渲染合批之自定义顶点格式（附 Demo 和引擎源码解读）](https://forum.cocos.org/t/demo/95087)  
  > [Cocos Creator 一种「无侵入」资源加密方案](https://forum.cocos.org/t/cocos-creator/95492)  
  > [Cocos Creator 通用框架设计 —— 资源管理](https://forum.cocos.org/t/cocos-creator/84793)  
  > [Cocos Creator 通用框架设计 —— 资源管理优化](https://forum.cocos.org/t/cocos-creator/93517)  
  > [Cocos Creator 水的浮力实现过程](https://forum.cocos.org/t/cocos-creator/96116)  
  > [制作一个可以将小游戏快速接入APP的安卓SDK-开篇](https://forum.cocos.org/t/app-sdk/95810)  
  > [解读 Cocos Creator 引擎：cc.AssetManager（1）重构后的 cc.loader](https://forum.cocos.org/t/cocos-creator-cc-assetmanager-1-cc-loader/92319)  
  > [如何巧用设计图提高UI的还原度](https://forum.cocos.org/t/ui/96354)  
  > [使用CocosCreator模拟书本翻页效果](https://forum.cocos.org/t/cocoscreator/96358)  
  > [Creator web平台setting.js 文件详细解析和资源加载解析](https://forum.cocos.org/t/creator-web-setting-js/78669)  
  > [cocos实现对ETC2的支持](https://forum.cocos.org/t/cocos-etc2/49061)  
  > [避免纹理抖动](https://forum.cocos.org/t/topic/91307/7)  
  > [使用2种方式实现动画的动态蒙版](https://forum.cocos.org/t/topic/96372)