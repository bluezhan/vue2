
# 你需要知道面试中的10个JavaScript概念

>翻译原文出处：[10 JavaScript concepts you need to know for interviews](https://dev.to/arnavaggarwal/10-javascript-concepts-you-need-to-know-for-interviews)

之前不是闹得沸沸扬扬的大漠穷秋文章《为什么只会Vue的都是前端小白？》；甚至大多数回头看了，也就会jQuery和Vue这两个库；也就大部分在运用着这两个库。我这里不是吐槽和开骂什么的；在之前jQuery年代，很多面试官都会问除了用jQuery来实现，能不能改写原生JavaScript来处理。也大部分人在看jQuery源码，甚至穷出不尽的底层库。

## 自我学习

目前有成千上万的年轻人在学习JavaScript和Web开发，希望获得一份工作。通常，自我学习的年轻人对JavaScript语言本身不够深入了解，在这方面留下了一片空白。

实际上令人惊讶的是，只需要了解非常小的一部分语言就可以来制作复杂的网页。在自己的网站上创建网站的人往往不太了解JavaScript的基本原理。大多数年轻人基本都是通过Bootstrap、jQuery及插件、Backbone或Angular等库和框架直接就搞定，而且还能构建复杂应用。

使用基本技能来避免复杂的主题和实现功能是相当容易的。在不理解被复制的代码的情况下，通过依赖[Stack Overflow](https://stackoverflow.com/)、github等网站放出的demo，甚至一些建站网站来创建自己的网站是比较轻松的。

如果您想要掌握更多的JavaScript面试相关资讯，请查看“ [提升你的JS：中级JavaScript的权威指南](https://www.educative.io/collection/5679346740101120/5707702298738688?authorName=Arnav%20Aggarwal)”

## 面试

那么问题来了，测试您对JavaScript深浅理解的问题，正是许多科技公司在面试中所要求的。当一个求职者只是刚好能通过面试，但如果不够深入了解该语言的本质，这是很槽糕的。

以下是Web开发中常见的概念需要重要的，前提是你已经了解了循环、函数和回调等基础知识。

## 概念

1、[值和引用](https://www.educative.io/collection/page/5679346740101120/5707702298738688/5685265389584384/) — 了解对象、数组和函数是通过引用进行复制和传递的；了解原始元素是按值复制和传递的。
2、[作用域](https://scotch.io/tutorials/understanding-scope-in-javascript#toc-scope-in-javascript) — 了解全局作用域，函数作用域和块作用域之间的差异。了解哪些变量在哪里可以用。了解JavaScript引擎如何执行变量查找。新出的ES6语法中申明变量关键字let、const对变量作用域的影响。
3、[变量提升](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/) — 了解变量和函数声明被提升到可用范围的顶部；了解函数表达式没有提升。
4、[闭包](http://javascriptissexy.com/understand-javascript-closures-with-ease/) — 知道闭包是指可以访问其他函数作用域内变量的函数。知道这样做可以使我们做什么，例如创建私有变量，动态函数生成等。
5、[this](https://www.educative.io/collection/page/5679346740101120/5707702298738688/5676830073815040) — 知道this的绑定规则。知道它是如何工作的，知道如何找出它在函数中与之相等的，并且知道为什么它是有用的。
6、[new](https://codeburst.io/javascripts-new-keyword-explained-as-simply-as-possible-fec0d87b2741) — 知道new如何与面向对象编程有关，知道使用new调用的函数会发生什么，通过函数的prototype属性了解如何使用new继承生成的对象。
7、[apply，call，bind](https://codeplanet.io/javascript-apply-vs-call-vs-bind/) — 知道这几个函数如何工作的，知道如何使用它们，知道它们做了什么。
8、[原型和继承](https://codeburst.io/master-javascript-prototypes-inheritance-d0a9a5a75c4e) — 了解JavaScript中的继承通过prototype链进行工作，了解如何通过函数和对象设置继承，以及new函数帮我们来实现它。知道__proto__和原型属性是什么以及它们的作用。
9、[异步JS](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=948s) — 了解事件循环。了解浏览器如何处理用户输入，Web请求和一般事件；知道如何识别并正确实现异步代码。了解JavaScript是异步单线程的。
10、[高阶函数](https://www.sitepoint.com/higher-order-functions-javascript/) — 了解函数是JavaScript中的一级对象，这意味着什么；知道从另一个函数返回函数是完全合法的。了解闭包和高阶函数允许我们使用的情况。

## 更多资源

如果上面的知识点包含的链接还不够，那么你可以上其它网站找资源，可以帮助您学习这些概念。

我个人创建了 [提升你的JS：中级JavaScript的权威指南](https://www.educative.io/collection/5679346740101120/5707702298738688?authorName=Arnav%20Aggarwal)，以帮助开发者提高他们的知识；它涵盖了所有这些概念和更多。

这里是我已经阅读或看过的资源，至少有一些可以推荐。

- [You Don’t Know JS](https://github.com/getify/You-Dont-Know-JS)
- [JavaScript is Sexy](http://javascriptissexy.com/16-javascript-concepts-you-must-know-well/)
- [javascript.com](https://www.javascript.com/resources)
- [Frontend Masters](https://frontendmasters.com/)
- [Eloquent JavaScript](http://eloquentjavascript.net/)

Good luck for your interviews!!!!(这句你懂得)

如果你发现这很有用，就请您点个赞，转发给其他人也看到它（这是博主原话）。

随时查看我最近的一些写的文章：

- [提升你的JS：中级JavaScript的权威指南](https://www.educative.io/collection/5679346740101120/5707702298738688?authorName=Arnav%20Aggarwal)
- [我从参加一个编码开机画面中学到的东西，并实现了一个](https://codeburst.io/what-i-learned-from-attending-a-coding-bootcamp-and-teaching-another-one-65addec715fd)
- [反应生态系统设置 - 分步演练](https://codeburst.io/react-ecosystem-setup-step-by-step-walkthrough-721ff45a7fc1)

## 参考

- [你们认为学习JavaScript难点在那里？](https://www.zhihu.com/question/34262554)
- [10个JavaScript难点](http://www.cnblogs.com/fundebug/p/7193369.html)
- [你有必要知道的 25 个 JavaScript 面试题](https://segmentfault.com/a/1190000004180569)
- [谈谈javascript语法里一些难点问题（一）](http://www.cnblogs.com/sharpxiajun/p/4133462.html)
- [谈谈javascript语法里一些难点问题（二）](http://www.cnblogs.com/sharpxiajun/p/4133984.html)
- [javascript技术难点（三）之this、new、apply和call详解](http://www.cnblogs.com/sharpxiajun/p/4148932.html)









