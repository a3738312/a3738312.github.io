# TypeScript or JavaScript
* `() => {}` 方式定义的函数中 `this` 的指向固定为定义环境,`funtion(){}`方式定义的函数中 `this` 则是随着调用环境而变化，可以使用 `funtion(){}.bind(object)` 方式绑定 `this`
* 判断变量类型可以使用 `typeof obj ` 会返回变量类型的字符串
* es6中的Map可以使用 `list = Array.from(map.values());` 来只将value放入数组。或者使用 `list = [...map];` 来讲键值对都放入数组