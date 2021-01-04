# AsseBundle
* 2.4.x 版本的 `cc.assetManager.loadRemote` 远程下载单个文件可以通过在 `options` 中添加 `{onFileProgress:(loaded, total) => {} }` 来获取下载进度。
  ```typescript
    cc.assetManager.loadRemote(url, {
        onFileProgress: (loaded: number, total: number) => {
            console.log(loaded / total);//获取进度
        }
    }, (error: Error, asset: cc.Asset) => {
    });
  ```  
  但是因为下载远程文件用的 `XMLHttpRequest` 所以需要服务器添加对应设置才可以获取。如果没有办法添加则可以继续使用 `cc.loader.load` 来加载远程资源，截至2.4.3该API还没有移除。  
* 2.4.x 版本的 `cc.assetManager.loadBundle` 只会加载 Bundle 的配置文件，加载完毕会返回一个 Bundle 对象，可以通过这个对象来加载 Bundle 内的资源:
  ```typescript
  //cc.Asset 为指定类型，如果加载文件夹，将会只返回指定类型的资源
  bundle.loadDir(assetUrl, cc.Asset, (finish: number, total: number) => {
    }, (error: Error, asset: cc.Asset) => {
    });
  bundle.load(assetUrl, cc.Asset, (finish: number, total: number) => {
    }, (error: Error, asset: cc.Asset) => {
    });
  ```
* `cc.assetManager.loadBundle` 可以加载本地 Bundle ，开发时可以直接构建然后加载 build typescript AssetBundle 包进行测试
  ```Typescript
    cc.assetManager.loadBundle("./build/Bundle", null, (err: Error, bundle: cc.AssetManager.Bundle) => { 
    });
  ```
* 加载目录下所有资源会包含子目录，且加载目录下所有资源时，若指定了资源类型，则只会加载该类型的资源
* AssetBundle 可以包含代码，但是TS使用时不可以引用 Bundle 包里面的类，需要使用 `node.getComponent('className');` 的方式来获取 Component
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
* AssetBundle 使用 `loadDir` 加载文件夹时，如果文件夹下有子文件夹，会导致进度回调中的 `total` 字段随着加载增加，会出现 `finish/total` 获取的进度不准确
* `cc.assetManager.bundles` 可以查看当前已加载的所有 `bundle`  