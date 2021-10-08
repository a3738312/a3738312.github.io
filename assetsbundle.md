# AsseBundle
* 2.4.x 版本的 AssetBundle 为了**兼容旧版本**，也保留有 `cc.resources`，但 `resources` 也是一个内置 Bundle，会在**引擎加载时**加载
* 2.4.x 版本的 `cc.assetManager.loadBundle` 只会加载 Bundle 的**配置文件**(`config.json`)及该 Bundle 中的所有脚本(`index.js`)，加载完毕会返回一个 Bundle 对象，可以通过这个对象来加载 Bundle 内的资源:
  ```typescript
  //cc.Asset 为指定类型，如果加载文件夹，将会只返回指定类型的资源
  //loadDir 会加载文件夹下所有资源，包括子文件夹
  bundle.loadDir(assetUrl, cc.Asset, (finish: number, total: number) => {
    }, (error: Error, asset: cc.Asset) => {
    });
  bundle.load(assetUrl, cc.Asset, (finish: number, total: number) => {
    }, (error: Error, asset: cc.Asset) => {
    });
  ```
* 2.4.x 版本的 `cc.assetManager.loadRemote` 远程下载单个文件可以通过在 `options` 中添加 `{onFileProgress:(loaded, total) => {} }` 来获取下载进度。
  ```typescript
    cc.assetManager.loadRemote(url, {
        onFileProgress: (loaded: number, total: number) => {
            console.log(loaded / total);//获取进度
        }
    }, (error: Error, asset: cc.Asset) => {
    });
  ```  
  但是因为下载远程文件用的 `XMLHttpRequest` 所以必须在服务器端做好相关配置。如果没有办法添加则可以继续使用 `cc.loader.load` 来加载远程资源，截至2.4.3该API还没有移除。  
* `cc.assetManager.loadBundle` 可以加载本地 Bundle ，开发时可以直接构建然后加载  AssetBundle 包进行测试
  ```Typescript
    cc.assetManager.loadBundle("D:/build/Bundle", null, (err: Error, bundle: cc.AssetManager.Bundle) => { 
    });
  ```
* AssetBundle 可以包含代码，但是使用时不可以直接引用 Bundle 包里面的类， `Bundle` 包里的脚本也不可以直接引用外部脚本，否则会导致脚本被打包到主包，使用时需要使用 `node.getComponent('className');` 的方式来获取脚本实例
* AssetBundle 的版本号就是打包出来之后中间的这段字符串 `config.版本号.json` `index.版本号.js` 若勾选md5，则会自动添加md5字符串，也可以手动填写，如 `index.1.0.js` 加载时版本号填写 `{ver: '1.0'}` 即可  
* 可以通过以下代码获取bundle中所有资源路径，且不需要加载资源  
  ```typescript
  //_config为私有属性，不推荐使用，但当前官方并未提供获取api
  let map = bundle._config.paths._map;
  let tmpArr = [];
  for (var item in map) {
      tmpArr.push(item);
  }
  console.log(tmpArr);//["path1","path2","path3/path"];
  ```
* 通过 `bundle` 对象加载的资源，需要自己管理释放；资源加载后默认引用计数为0，使用 `asset.addRef()` 可以增加资源引用，使用 `asset.decRef()` 可以减少资源引用，当引用计数减少到0时，会自动尝试释放资源
* AssetBundle 使用 `loadDir` 加载文件夹时，如果文件夹下有子文件夹，会导致进度回调中的 `total` 字段随着加载增加，会出现 `finish / total` 获取的进度不准确
* `cc.assetManager.bundles` 可以查看当前已加载的所有 `bundle`  
* AssetBundle 加载的代码资源无法清除缓存，加载的 Bundle 内若有与现有脚本同名的脚本则会报错
* AssetBundle 打包的时候需要设置目标平台为对应平台，否则可能出现资源加载问题