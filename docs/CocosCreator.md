[返回](../README.md)
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