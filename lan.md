# 关于JS和TS

* `() => {}` 方式定义的函数中 `this` 的指向固定为定义环境,`funtion(){}`方式定义的函数中 `this` 则是随着调用环境而变化，可以使用 `funtion(){}.bind(object)` 方式绑定 `this`
* 判断变量类型可以使用 `typeof obj` 会返回变量类型的字符串
* es6中的Map可以使用 `list = Array.from(map.values());` 来只将value放入数组。或者使用 `list = [...map];` 来讲键值对都放入数组
* `for...in..` 可以用来遍历对象内部的属性及方法
* `for...of..` 专门用来遍历可迭代对象，比如数组，map，字符串等。如果数组中有部分下表没有赋值，则会跳过
* 可以使用以下方式在字符串中插入变量

  ```typescript
    `Name:${name}` or "Name:" + name
  ```

* Typescript可以这样重载方法

  ```typescript
    let a: any[] = [1, "ss", 2, false, new Object()];
    function toString(target: number): string;
    function toString(target: string): string;
    function toString(target: boolean): string;
    function toString(target: object): string;
    function toString(target): any {
        if (typeof target == "number")
            return "number is " + target;
        if (typeof target == "string")
            return "string is " + target;
        if (typeof target == "boolean")
            return "boolean is " + target;
        if (typeof target == "object")
            return "object is " + JSON.stringify(target);
    }
    for (let obj of a) {
        cc.log(toString(obj));
    }
    //output number is 1
    //output string is ss
    //output number is 2
    //output boolean is false
    //output object is {}
  ```

* 如果想打印 Typescript 中定义的枚举名，可以这样
    ```typescript
    enum Type {
      Type1 = 0,
      Type2 = 1
    }
    console.log(Type[Type.Type1]);//output: Type1
    console.log(Type[1]);//output: Type2
    ```

