# TypeScript or JavaScript
* `() => {}` 方式定义的函数中 `this` 的指向固定为定义环境,`funtion(){}`方式定义的函数中 `this` 则是随着调用环境而变化，可以使用 `funtion(){}.bind(object)` 方式绑定 `this`
* 判断变量类型可以使用 `typeof obj ` 会返回变量类型的字符串
* es6中的Map可以使用 `list = Array.from(map.values());` 来只将value放入数组。或者使用 `list = [...map];` 来讲键值对都放入数组
* `for...in..` 可以用来遍历对象内部的属性及方法
* `for...of..` 专门用来遍历可迭代对象，比如数组，map，字符串等。如果数组中有部分下表没有赋值，则会跳过
* 可以使用以下方式在字符串中插入变量
    ``` javascript
    `Name:${name}` or "Name:" + name
    ```