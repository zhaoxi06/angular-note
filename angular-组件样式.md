1、样式表、选择器、规则以及媒体查询等，都可以直接用于angular程序中。
2、组件样式的实现方式之一，是在组件的元数据中设置styles属性。styles属性可以接受一个包含css代码的字符串数组，通常指给它一个字符串就行了。
```
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styles: ['h1 { font-weight: normal; }']
})
export class HeroAppComponent {
/* . . . */
}
```
作用于规则：这个组件的样式，既不会被模板中嵌入的组件继承，也不会被通过内容投影(ng-content)嵌尽量的组件继承。它只对自身这个组件有效。
3、特殊选择器
* :host
选择组件宿主元素**也即自定义元素**中的元素。因为组件定义的样式规则都只在组件内部才能生效，不进不出，所以angular提供了在组件内部的样式定义选择宿主元素的:host选择器。
```
:host {
  display: block;
  border: 1px solid black;
}
//把宿主样式做为条件进行筛选。只有当它同时带有active css类的时候才会生效
:host(.active) {
  border-width: 3px;
}
```
* :host_context
在当前组件宿主元素的祖先节点中查找css类。
```
//只有当某个宿主元素的祖先元素有css类theme-light时，才会把background-color样式应用到组件内部的所有h2上
:host-context(.theme-light) h2 {
  background-color: #eef;
}
```
4、把样式加载进组件中
* 设置styles或styleUrls元数据
* 内联在模板的HTML中
```
@Component({
  selector: 'app-hero-controls',
  template: `
    <style>
      button {
        background-color: white;
        border: 1px solid #777;
      }
    </style>
    <h3>Controls</h3>
    <button (click)="activate()">Activate</button>
  `
})
```
* 通过CSS文件导入
    * 模板中的link标签
    ```
    @Component({
    selector: 'app-hero-team',
    template: `
        <!-- We must use a relative URL so that the AOT compiler can find the stylesheet -->
        <link rel="stylesheet" href="../assets/hero-team.component.css">
        <h3>Team</h3>
        <ul>
        <li *ngFor="let member of hero.team">
            {{member}}
        </li>
        </ul>`
    })
    ```
    * css @imports
    ```
    //css文件中导入
    @import './hero-details-box.css';
    ```
第2点中的作用域规则对上述所有这些加载模式都适用。

5、视图封装模式
* 可选的视图封装模式有以下几种
    * ShadowDom 模式使用浏览器原生的 Shadow DOM 实现（参见 MDN 上的 Shadow DOM）来为组件的宿主元素附加一个 Shadow DOM。组件的视图被附加到这个 Shadow DOM 中，组件的样式也被包含在这个 Shadow DOM 中。(译注：不进不出，没有样式能进来，组件样式出不去。)
    * Native 视图包装模式使用浏览器原生 Shadow DOM 的一个废弃实现 —— 参见变化详情。
    * Emulated 模式（默认值）通过预处理（并改名）CSS 代码来模拟 Shadow DOM 的行为，以达到把 CSS 样式局限在组件视图中的目的。(译注：只进不出，全局样式能进来，组件样式出不去)
    * None 意味着 Angular 不使用视图封装。 Angular 会把 CSS 添加到全局样式中。而不会应用上前面讨论过的那些作用域规则、隔离和保护等。 从本质上来说，这跟把组件的样式直接放进 HTML 是一样的。(译注：能进能出。)
* 通过组件元数据中的encapsulation属性来设置组件封装模式
```
encapsulation: ViewEncapsulation.Native
```

6、生成的css
当使用默认的仿真模式时，Angular 会对组件的所有样式进行预处理，让它们模仿出标准的 Shadow CSS 作用域规则。在启用了仿真模式的 Angular 应用的 DOM 树中，每个 DOM 元素都被加上了一些额外的属性。
```
<hero-details _nghost-pmm-5>
  <h2 _ngcontent-pmm-5>Mister Fantastic</h2>
  <hero-team _ngcontent-pmm-5 _nghost-pmm-6>
    <h3 _ngcontent-pmm-6>Team</h3>
  </hero-team>
</hero-detail>
```
生成出的属性分为两种：
* 一个元素在原生封装方式下可能是 Shadow DOM 的宿主，在这里被自动添加上一个 _nghost 属性。 这是组件宿主元素的典型情况。
* 组件视图中的每一个元素，都有一个 _ngcontent 属性，它会标记出该元素属于哪个宿主的模拟 Shadow DOM。