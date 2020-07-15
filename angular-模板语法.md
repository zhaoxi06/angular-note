1、 `HTML`
* `<script>`元素被禁用了，以阻止脚本注入共计的风险
* `<html>`、`<body>`、`<base>`元素在angular中是没有意义的

2、插值与模板表达式。
* 使用场景
	* HTML元素标签之间 
	`<P>{{title}}</p>`
	* 属性复制语句内的文本中
	`<div><img src="{{itemImageUrl}}"></div>`
* 区别
	* 插值表达式：{{...}}
	* 模板表达式：就是出现在双花括号中的内容。angular先对它求值，再把它转换成字符串。
* 模板表达式一般是组件属性的名字，也可以调用宿主组件的方法。

3、模板表达式
>angular执行模板表达式，并把它复制给绑定目标的属性，这个绑定目标可能是 **HTML元素** 、 **组件** 或 **指令**。

4、表达式上下文
表达式中的上下文变量是由 **模板变量**、指令的 **上下文变量** 和组件的 **成员** 叠加而成的。
```
//模板变量
<h4>{{recommended}}</h4>
<img [src]="itemImageUrl2">
//指令的上下文变量
<ul>
  <li *ngFor="let customer of customers">{{customer.name}}</li>
</ul>
//模板引用变量
<input #customerInput>{{customerInput.value}}</label>
```


