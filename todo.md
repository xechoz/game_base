1. 模块交互规范
    ```
    interface IFoo {
        val state: FooState
        
        fun upateX(x: String)
    }

    // state is immutable
    data class FooState(
        val x: String = ""
    )
    ```
2. 模块 api center implement

    api.ts 实现
    ```
    interface api {
        fun add(apiType: class<T>, apiTypeImple: T)
        
        fun use(apiType: Class<T>)
    }
    ```
3. 模块 api 注入&调用
    // inject implement  in app.ts or main.ts
    ```
    import 'api.ts'
    import 'IFoo.ts' // import interface
    import 'Foo.ts' // import implement

    // api is global usable 
    api.add(IFoo, FooImple()) // map interface to implement
    ```

    // use in bar module
    ```
    import 'api.ts'
    import 'IFoo.ts' // dependence only interface

    val foo = api.use(IFoo)

    foo.state.x // readonly

    // update by foo
    foo.updateX(x = newX)
    ```
4. add i18n 
5. add advertise module
6. add pinia
