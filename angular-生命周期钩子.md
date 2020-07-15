[Angular生命周期钩子](https://angular.cn/guide/lifecycle-hooks#peek-a-boo)
[对Angular中的生命周期钩子的理解](https://juejin.im/post/597dd3546fb9a03c5945699a)
[Angular4学习笔记——生命周期钩子](https://segmentfault.com/a/1190000012180793)

### `*ngChanges()`
* 当组件中使用@Input()定义了一个输入属性时，只要这个输入属性的值发生了改变，就会触发ngOnChange()。
* 这个生命周期钩子接收一个参数，这个参数是一个SimpleChanges对象，这个对象中包含了输入属性当前值和上一值。
* ngOnChanges()首次调用发生在ngOnInit()之前。
* 用处：
```
@Input()
public name: string;

ngOnChanges(changes: SimpleChanges): void {
  console.log(changes); // name:SimpleChange {previousValue: "a", currentValue: "ab", firstChange: false}
}
```
**!注意:** 输入属性定义为引用类型和基本类型的时候其表现结果是不同的。当输入属性是基本类型时，每一次改变都会触发ngOnChanges()生命周期钩子,而当输入属性是引用类型时,改变引用类型当中的属性时,并不会触发ngOnChanges()生命周期钩子.只有当把引用类型数据的指针指向另一块内存地址的时候才会触发ngOnChanges()生命周期钩子.


### `*ngOnInit()`
* 在ngOnChanges()第一次调用完成后调用，只调用一次。
* 用处： 在这里放置复杂的初始化逻辑

**注意：** 应该让构造函数保持简单，只做简单的初始化操作。

### `*ngDoCheck()`
* 在每个angular变更检测周期中调用，ngOnChanges()和ngOnInit()之后调用。
* 用处：检测Angular自身无法捕获的变更并采取行动。

**注意：** ngDonCheck在每次变更检测周期之后，被非常频繁地调用，发生了变化的每个地方都会调用它。所以在使用时，一定要加上判断，避免不必要的触发。

### `*ngAfterContentInit()`
* 当父组件向子组件投影内容，在子组件内初始化父组件的投影内容时会调用。
* 在第一次ngDoCheck()之后调用，只调用一次。
* 只适用于组件。

### `*ngAfterContentChecked()`

### `*ngAfterViewInit()`

### `*ngAfterViewChecked()`

### `*ngOnDestory()`
