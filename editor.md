# Editor 参数

-   **executeInEditMode**  
    允许当前组件在编辑器模式下运行。  
    默认情况下，所有组件都只会在运行时执行，也就是说它们的生命周期回调在编辑器模式下并不会触发。  
    值类型：Boolean  
    默认值：false  
    JS:

    ```Javascript
    cc.Class({
        extends: cc.Component,
        editor: {
            executeInEditMode: true
        }
    });
    ```

    TS:

    ```Typescript
    const { executeInEditMode } = cc._decorator;

    @executeInEditMode
    export default class Class extends cc.Component {
        ......
    }
    ```

-   **requireComponent**  
    requireComponent 参数用来指定当前组件的依赖组件。  
    当组件添加到节点上时，如果依赖的组件不存在，引擎将会自动将依赖组件添加到同一个节点，防止脚本出错。  
    该选项在运行时同样有效。  
    值类型：Function （必须是继承自 cc.Component 的构造函数，如 cc.Sprite）  
    默认值：null  
    JS:

    ```Javascript
    cc.Class({
        extends: cc.Component,
        editor: {
            requireComponent: cc.Sprite
        }
    });
    ```

    TS:

    ```Typescript
    const { requireComponent } = cc._decorator;

    @requireComponent(cc.Sprite)
    export default class Class extends cc.Component {
        ......
    }
    ```

-   **executionOrder**  
    脚本生命周期回调的执行优先级。  
    小于 0 的脚本将优先执行，大于 0 的脚本将最后执行。  
    该优先级只对 `onLoad`, `onEnable`, `start`, `update` 和 `lateUpdate` 有效，对 `onDisable` 和 `onDestroy` 无效。  
    值类型：Number  
    默认值：0  
    JS:

    ```Javascript
    cc.Class({
        extends: cc.Component,
        editor: {
            executionOrder: -1
        }
    });
    ```

    TS:

    ```Typescript
    const { executionOrder } = cc._decorator;

    @executionOrder(-1)
    export default class Class extends cc.Component {
        ......
    }
    ```

-   **disallowMultiple**  
    当本组件添加到节点上后，禁止同类型（含子类）的组件再添加到同一个节点，防止逻辑发生冲突。  
    值类型：Boolean  
    默认值：false  
    JS:

    ```Javascript
    cc.Class({
        extends: cc.Component,
        editor: {
            disallowMultiple: true
        }
    });
    ```

    TS:

    ```Typescript
    const { disallowMultiple } = cc._decorator;

    @disallowMultiple
    export default class Class extends cc.Component {
        ......
    }
    ```

-   **menu**  
    menu 用来将当前组件添加到组件菜单中，方便用户查找。  
    值类型：String （如 "Rendering/Camera"）  
    默认值：""  
    JS:

    ```Javascript
    cc.Class({
        extends: cc.Component,
        editor: {
            menu: "Menu/Class"
        }
    });
    ```

    TS:

    ```Typescript
    const { menu } = cc._decorator;

    @menu("Menu/Class")
    export default class Class extends cc.Component {
        ......
    }
    ```

-   **playOnFocus**  
    当设置了 `executeInEditMode` 以后， `playOnFocus` 可以用来设定选中当前组件所在的节点时，编辑器的场景刷新频率。  
    `playOnFocus` 如果设置为 true，场景渲染将保持 60 FPS，如果为 false，场景就只会在必要的时候进行重绘。  
    值类型：Boolean  
    默认值：false  
    JS:

    ```Javascript
    cc.Class({
        extends: cc.Component,
        editor: {
            playOnFocus: true
        }
    });
    ```

    TS:

    ```Typescript
    const { playOnFocus } = cc._decorator;

    @playOnFocus
    export default class Class extends cc.Component {
        ......
    }
    ```

-   **inspector**  
    自定义当前组件在 **属性检查器** 中渲染时所用的网页 url  
    值类型：String  
    默认值：""  
    JS:

    ```Javascript
    cc.Class({
        extends: cc.Component,
        editor: {
            inspector: "packages://inspector/inspectors/comps/button.js"
        }
    });
    ```

    TS:

    ```Typescript
    const { inspector } = cc._decorator;

    @inspector("packages://inspector/inspectors/comps/button.js")
    export default class Class extends cc.Component {
        ......
    }
    ```

-   **help**  
    指定当前组件的帮助文档的 url，设置过后，在 **属性检查器** 中就会出现一个帮助图标，用户点击将打开指定的网页。  
    值类型：String  
    默认值：""  
    JS:

    ```Javascript
    cc.Class({
        extends: cc.Component,
        editor: {
            help: "https://example.com/help.html"
        }
    });
    ```

    TS:

    ```Typescript
    const { help } = cc._decorator;

    @help("https://example.com/help.html")
    export default class Class extends cc.Component {
        ......
    }
    ```
