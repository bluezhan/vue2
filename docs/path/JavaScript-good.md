
# JavaScript-good


在写函数的时候

无法就是：
- 健全性
- 可读性
- 可扩展
- 健壮性

那么里面还有函数的参数验证(这就涉及到基础类型和引用类型应用和检查、还有拷贝、转变啊)


## 关于new的一切

new操作的过程如下：
1.创建一个空对象{}，并将this指向该空对象；
2.将this的__proto__指向构造函数的prototype；
3.执行构造函数，如果构造函数返回值为对象，那么就返回该对象；如果构造函数没有显式返回值或者返回值为基本数据类型，那么直接忽略这些返回值，而是返回this。

Animal 本身是一个普通函数，但当通过new来创建对象时，Animal 就是构造函数。  
JS引擎执行这句代码时，在内部做了很多工作，用伪代码模拟其内部流程如下：
```js
var cat = new Animal("cat");
new Animal("cat") = {
    var obj = {};
    obj.__proto__ = Animal.prototype;
    var result = Animal.call(obj,"cat");
    return typeof result === 'object'? result : obj;
}
```

new操作有几个特点：

构造函数（里面定义变量、函数和外面的区别）
函数对象
基本数据类型和引用类型
__proto__ 和 prototype 、constructor
call apply
new 后面一定是构造函数 （not a constructor）

#### 构造函数

JavaScript中构造函数与new操作符的实例详解
｛…｝语法允许创建一个对象，但如果需要创建多个类似的对象，则我们需要使用构造函数和“new”操作符。

构造函数技术上就是正常的函数，但一般有两个约定： 
1、他们的名称第一个字母大写。 
2、他们应该仅仅使用new操作符执行。

```js
function User(name) {
  // this = {};  (implicitly)
 
  // we add properties to this
  this.name = name;
  this.isAdmin = false;
 
  // return this;  (implicitly)
}
```

通常构造器无需返回值语句。它的任务是往this对象中写一些必要内容，然后自动返回之。 
但如果有return语句，那么规则很简单： 
- 如果return返回一个对象，那么则代替this被返回。 
- 如果return返回原始类型，则被忽略，仍然返回this。

技术上，任何函数都可以用作构造器，即任何函数都可以使用new调用。
首字母大写只是一个常规约定，使其更清晰说明其为构造函数，应该使用new调用。

双重使用构造器: new.target

函数内部，我们可以检查其调用方式是否使用new方式。使用一个特殊的属性new.target可以。 
普通调用其值为空，通过new调用其值为该函数。


```js
function User() {
  console.log(new.target);
}
 
User(); // undefined
new User(); // function User { ... }

function User(name) {
  if (!new.target) { // if you run me without new
    return new User(name); // ...I will add new for you
  }
 
  this.name = name;
}
 
let john = User("John"); // redirects call to new User
alert(john.name); // John
```


有一个经典的例题：
```js
function Animal(name){
    this.name = name;
}
 Animal.color = "black";
 Animal.prototype.say = function(){
    console.log("I'm " + this.name);
 };
 var cat = new Animal("cat");
 
 console.log(
     cat.name,  //cat
     cat.height //undefined
 );
 cat.say(); //I'm cat
 
 console.log(
     Animal.name, //Animal
     Animal.color //back
 );
 Animal.say(); //Animal.say is not a function
```

代码解读如下：

  L1-3： 创建了一个函数Animal，并在其 this 上定义了属性：name，name的值是函数被执行时的形参。
  L4 ： 在 Animal 对象（Animal本身是一个函数对象）上定义了一个静态属性:color,并赋值“black”
  L5-7：在 Animal 函数的原型对象 prototype 上定义了一个 say() 方法，say方法输出了 this 的 name 值。
  L8： 通过 new 关键字创建了一个新对象 cat
  L10-14： cat 对象尝试访问 name 和 color 属性，并调用 say 方法。
  L16-20： Animal 对象尝试访问 name 和 color 属性，并调用 say 方法。


参考
JavaScript中构造函数与new操作符的实例详解 http://www.php.cn/js-tutorial-376246.html  
深入理解 new 操作符 http://www.cnblogs.com/onepixel/p/5043523.html


## 解析URL并生成对象


## 两道简单题目

=========
function f5() {
        f = ff();
        return f;
        function ff() {
            return "f" in window;
        };
       
}
console.log(f5()); //返回的为什么不是true呢
==========
var obj={           
        name:name,      
        age:f3()        
    };                  
    var name="QQ";      
    var age=18;         
    function f3() {     
        return ++age    
}                                     
console.log(obj); //浏览器首次打印和刷新页面后打印的结果不一样，为什么呢？
==========

考的最多就是：this\new\变量提升\作用域\return\引用类型\setTimeout

## 参考

- [为什么你的前端工作经验不值钱？](http://mp.weixin.qq.com/s/6X8peCZXUWrroVBMBD5eyg) 
- [理解构造函数与原型对象](https://mp.weixin.qq.com/s/egP8jkUDLSUknwu1Ms__jg)
- [【第1050期】前端校招面试该考察什么？](http://mp.weixin.qq.com/s/GtPwOzlKZFAP2-oFTF5nNQ)
- [web图片响应式自适应结合懒加载的最佳方案](http://mp.weixin.qq.com/s/E7jrI56qafxmXSDdK4F3pA)
- []()




