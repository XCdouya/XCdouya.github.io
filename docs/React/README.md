---
typora-copy-images-to: images
---

# React学习笔记

## 一、React介绍

### 1.1.React是什么

* 将数据渲染成HTML视图的开源javas库



### 1.2.为什么要学React

* 使用原生js操作DOM繁琐，效率低（**DOM-API操作UI**）
* 使用原生js操作DOM,浏览器会进行大量**重绘重排**
* 原生js没有**组件化**编码方案，代码复用率低



### 1.3.使用React的好处

* React提供了一个虚拟DOM操作原生DOM，虚拟DOM只会重绘重排发生改变的元素



### 1.4.前提知识

* 学会判断this的指向
* class
* ES6语法规范
* npm包管理器
* 原型，原型链
* 数组常用方法
* 模块化



## 二、JS准备

* bable.min.js 用于将 ES6 ==> ES5 ，将 jsx ==> js
* react.development.js  React的核心库
* react.dom.development.js React的扩展库，让React帮我们操作DOM
* prop-types.js 暂时用不上，作用未知



## 三、虚拟DOM与真实DOM

### 3.1虚拟DOM的本质

* 虚拟DOM本质上就是一个对象
* 与真实DOM相比虚拟DOM比较“轻量级”，因为虚拟DOM是React在使用，无需真实DOM那么多属性
* 虚拟DOM最终会被React转化为真实DOM，呈现在页面上



## 四、jsx语法规则

* 定义虚拟DOM时不要写引号
* 在标签中想使用js表达式需要用{}括起来
* 样式的类名制定不要用class，要用className
* 内联样式，要用style={{key:value}的形式去写
* 只有一个根标签
* 标签必须闭合
* 标签首字母
  * 若小写字母开头，则将改标签转为htm1中同名元素，若htm1中无该标签对应的同名元素，则报错。
  * 若大写字母开头，react.就去渲染对应的组件，若组件没有定义，则报错。



## 五、js表达式和js语句

* 表达式：**一个表达式会产生一个值**，可以放在任何一个需要值的地方，下而这些都是表达式：
  * a					（单个元素）
  	 a+b					（算数表达式，逻辑表达式）	
  	demo(1)				（方法调用）
  	arr.map()			（数组的部分方法）
  	function test (){		（定义方法）
* 语句(代码)，下面这些都是语句（代码）：
  * if()					
  	 for()}				
  	switch(){case:xXXx}	
  * 流程控制语句





## 六、模块与组件、模块化与组件化的理解

### 6.1.模块

1. 理解：向外提供特定功能的js程序，一般就是一个js文件
2. 为什么要拆成模块：随着业务逻辑增加，代码越来越多且复杂。
3. 作用：复用js,简化js的编写，提高js运行效率



### 6.2.组件

1. 理解：用来实现局部功能效果的代码和资源的集合(html/css/js/image等等)
2. 为什么：一个界面的功能更复杂
3. 作用：复用编码，简化项目编码，提高运行效率~



### 6.3.模块化

* 当应用的js都以模块来编写的，这个应用就是一个模块化的应用



### 6.4.组件化

* 当应用是以多组件的方式实现，这个应用就是一个组件化的应用



### 6.5.React浏览器插件

![1660981097636](images\1660981097636.png)

开启后开发者模式中会多出两个与React相关的标签：

![1660981201489](images\1660981201489.png)

可以帮助我们进行编程；



## 七、组件

**React中组件分为**

* 函数式组件

* 类式组件

### 7.1.函数式组件

​	函数式组件顾名思义，是由函数定义的组件，将一个函数作为一个组件，根据jsx语法，函数名需要大写，否则该组件的标签会被浏览器认为HTML中的标签从而报错。

~~~jsx
//定义函数式组件
function Mycomponent(){
    //由于使用了babel的翻译，网页翻译成ES5后会开启严格模式，不允许this指向windon所以this指向为空
    return (
        <h1>这是函数式组件</h1>
    );
}

//React调用函数式组件
ReactDOM.render(<Mycomponent/>,document.getElementById('你HTML中的组件ID'))；

~~~

执行了ReactDOM.render(<MyComponent./>.....之后，发生了什么？

* React解析组件标签，找到了MyComponent组件。
* 发现组件是使用函数定义的，随后调用该函数，将返回的虚拟D0M转为真实D0M,随后呈现在页面中。



### 7.2.类式组件

​	类组件使用类定义拥有state，pops，refs，三大特性

~~~jsx
//定义类式组件
class Mycomponent() extends React.Component {
    
    render(){
        //render中的this是React帮我们创建的类实例，也就是Mycomponent的实例
        //render方法放在类的原型上供实例使用
        <h1>这是类式组件</h1>
    }
    
    f(){
        /*
        类中其他方法不会被React通过实例调用，会通过动态交互作为参数传递过去，调用时相当于直接调用f方法
        class A(){
        	f(){}
    	}
    	const a = new A();
    	a.f();//这里是实例调用，this指向实例
    	const test = a.f;
    	test();//这里是方法的直接调用，this为空，因为类中的方法会局部开启严格模式，所以不为window
    	当我们直接调用类中方法时会发生this的丢失
    	解决方法有两个：
    	1、通过bind改变方法的this
    	2、使用箭头函数
    	*/
    }
    
}

//React调用函数式组件
ReactDOM.render(<Mycomponent/>,document.getElementById('你HTML中的组件ID'))；

~~~



执行了ReactDOM.render(<MyComponent/>.....之后，发生了什么？

* React解析组件标签，找到了MyComponent组件。
* 发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render.方法。
* 将render.返回的虚拟DoM转为真实DoM,随后呈现在页面中。



## 八、组件状态—state

### 8.1.state是什么

|  人  | 状态  | 影响 | 行为 |
| :--: | :---: | :--: | :--: |
| 组件 | state | 驱动 | 页面 |

**组件的state存放页面相关数据，当数据发生改变时，React会将涉及到该数据的元素重新渲染。**

* state是组件对象最重要的属性，值是对象（可以包含多个key-value的组合）
* 组件被称为"状态机"，通过更新组件的state来更新对应的页面显示（重新渲染组件）



### 8.2.组件中添加state

向组件中添加state有两种方式：

* 使用构造器

  ~~~jsx
  class Clock extends React.Component {
    constructor(props) {
      super(props);
        //添加state
      this.state = {date: new Date()};
    }
  
    render() {
      return (
        <div>
          <h1>Hello, world!</h1>
          <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        </div>
      );
    }
  }
  
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(<Clock />);
  ~~~

  

* 直接创建state属性（简写）

  ~~~jsx
  class Clock extends React.Component {
    //直接添加名为state的属性
    state = {
        date: new Date();
    };
    
    render() {
      return (
        <div>
          <h1>Hello, world!</h1>
          <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        </div>
      );
    }
  }
  
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(<Clock />);
  ~~~



### 8.3.this丢失

在7.2中我们提到了this丢失的问题，当this丢失时我们无法获取到组件的state

* 组件中render方法中的this为组件实例对象

* 组件自定义的方法中this为undefined,如何解决？

  a. 强制绑定this:通过函数对象的bind（需要在this指向为实例的地方执行，如构造器，render方法）

  ~~~jsx
  
  class Clock extends React.Component {
    constructor(props) {
      //extends的语法要求
      super(props);
      //添加state
      this.state = {date: new Date()};
      //在当前原型中添加一个名为fun的属性，并将this中的test方法绑定this后赋值给test1
      //语句的右边，this因为在构造器中，所以this指向实例
      //this.test 会在原型中找到test方法，然后调用bind改变方法的this为当前的this也就是当前实例
      //然后通过赋值语句赋值给this的fun属性,此时在实例中会多出来一个fun属性
      //bind会创建一个新方法，并改变新方法的this指向
      //通常我们使用bind会将新方法名命名为原方法名，这里会有人产生误解，以为调用的是之前的方法
      //其实在方法被调用时，会先在实例上查找，没找到才会去原型中查找
      //请记住一句话，类中的方法放在原型上，供实例使用，而不是放在实例上！！！
      this.fun = this.test.bind(this);
  	
    }
  
    render() {
      return (
        <div>
              
          <h1 onClick={this.fun}>Hello, world!</h1>
          <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
          
        </div>
      );
    }
      
    test(){
          
    }
  }
  
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(<Clock />);
  ~~~

  

  b. 箭头函数

  ~~~jsx
  test= ()=>{
      console.log(this);
      //箭头函数本身没有this
      //箭头函数中的this继承外层函数调用的this
      //当render中调用到test方法时，test方法的this为render的this即组件的实例；
  }
  ~~~

  



### 8.4.修改state

* 状态数据，不能直接修改或更新

  ~~~jsx
  class Test extends React.Component{
      state = {
          flag:false
      }
  	
  changeState = ()=>{
      this.state.flag = !(this.state.flag);//错误的，直接修改和更新state，React并不能感知到
  }
  }
  ~~~

  

* 通过setState修改状态值

  ~~~jsx
  let flag = this.state.flag;
  flag = !flag;
  this.setState(flag);//正确，通过调用set方法告知React，state被更改了
  ~~~



## 九、组件属性—props

与HTML标签一样，在React中定义的标签也可以写上任意的属性

### 9.1.props的基本使用

~~~jsx
//跟HTML一样在组件创建的时候，写上所需要的值就可以了
<MyComponent name = "douya" age = {18}/>
~~~





