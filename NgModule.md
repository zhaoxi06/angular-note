一、JavaScript模块 VS NgModule
1、它们的导入方式是不同的
```
/* These are JavaScript import statements. Angular doesn’t know anything about these. 
顶部这些是JavaScript的导入语句
*/
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

/* The @NgModule decorator lets Angular know that this is an NgModule.*/
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [     /* These are NgModule imports.  NgModule中的imports是angular特有的 */
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
2、它们的导出方式是相同的
```
export class AppComponent { }
```
3、它们关键性的不同
* NgModule 只绑定了可声明的类，这些可声明的类只是供 Angular 编译器用的。
* 与 JavaScript 类把它所有的成员类都放在一个巨型文件中不同，你要把该模块的类列在它的 @NgModule.declarations 列表中。
* NgModule 只能导出可声明的类。这可能是它自己拥有的也可能是从其它模块中导入的。它不会声明或导出任何其它类型的类。
* 与 JavaScript 模块不同，NgModule 可以通过把服务提供商加到 @NgModule.providers 列表中，来用服务扩展整个应用。

二、常用模块
* CommonModule贡献ngIf、ngFor这些指令。
* BrowserModule的提供商是面向整个应用的，所以它只能在根模块中使用，而不是特性模块。
* BrowserModule导入了CommonModule，另外，BrowserModule重新导出了CommonModule，以便它所有的指令在任何导入了BrowserModule的模块中都可以使用。

三、共享NgModule
```
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { CustomerComponent } from './customer.component';
import { NewItemDirective } from './new-item.directive';
import { OrdersPipe } from './orders.pipe';

@NgModule({
 imports:      [ CommonModule ],
 declarations: [ CustomerComponent, NewItemDirective, OrdersPipe ],
 exports:      [ CustomerComponent, NewItemDirective, OrdersPipe, CommonModule, FormsModule ]
})
export class SharedModule { }
```
通过重新导出CommonModule和FormModule，任何导入了SharedModule的其他模块，就都可以访问来自CommonModule的ngIf和ngFor等指令了，也可以绑定到来自FormModule中的`[(ngModule)]`的属性了。

四、其他
1、forRoot()方法
* 只能在应用的根模块AppModule中调用并导入.forRoot()的结果。在其他模块中导入它，特别是惰性加载模块中，是违反设计目标的并会导致一个运行时错误。
* 对于服务来说，除了可以使用 forRoot()外，更好的方式是在该服务的 @Injectable() 装饰器中指定 providedIn: 'root'，它让该服务自动在全应用级可用，这样它也就默认是单例的。
* RouterModule 也提供了静态方法 forChild，用于配置惰性加载模块的路由。
* forRoot() 和 forChild() 都是约定俗成的方法名，它们分别用于在根模块和特性模块中配置服务。
* Angular 并不识别这些名字，但是 Angular 的开发人员可以。 当你写类似的需要可配置的服务提供商时，请遵循这个约定。

2、AppModule和AppComponent
* 惰性加载模块机器组件可以注入AppModule中的服务，却不能注入AppComponent中的。
* 只有当改服务必须对AppComponent组件树之外的组件不可见时，才应该把服务注册进AppComponent的providers中。
* 在组件中提供服务可能导致这些服务出现多个实例。
* 所以，优先吧提供商注册进模块中，而不是组件中。