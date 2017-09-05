
# 尝试在JavaScript中构建一个"Maybe"检测器

>翻译原文出处：[Building a Maybe in JavaScript](http://developingthoughts.co.uk/building-a-maybe-in-javascript/)
 鄙人翻译略差且略有出入，别见笑。

很多时候我们会碰到：Uncaught TypeError: Cannot read property 'x' of undefined（无法读取未定义的属性“x”）。  
我猜，如果你正好看到这个你以前不单只碰过还历历在目的东西，可能有那么一刻想把显示器给砸了。
这里想起了我们尊敬的计算机领域的爵士——托尼·霍尔；他在Infoq办的大会演讲时，用到的主题是：“Null References: The Billion Dollar Mistake”（Null 引用：一个十亿美元级别的错误），讲演摘要中这样写的：  
“我把Null引用称为自己的十亿美元错误。它的发明是在1965年，那时我用一个面向对象语言( ALGOL W )设计了第一个全面的引用类型系统。我的目的是确保所有引用的使用都是绝对安全的，编译器会自动进行检查。但是我未能抵御住诱惑，加入了Null引用，仅仅是因为实现起来非常容易。它导致了数不清的错误、漏洞和系统崩溃，可能在之后40年中造成了十亿美元的损失。近年来，大家开始使用各种程序分析程序，比如微软的PREfix和PREfast来检查引用，如果存在为非Null的风险时就提出警告。更新的程序设计语言比如Spec#已经引入了非Null引用的声明。这正是我在1965年拒绝的解决方案。”

## 那十亿美元级别的错误

幸运的是，我们可以使用一些功能性的编程技术，以清洁、简洁和可靠的方式缓解这带来疼痛。让我们想象一下，我们要从下面的对象中提取属性“c”的值，并附加字符串“is great”。

```js
const a = {
    b: {
        c: "fp"
    }
};
```

我们使用的简单方法可能是：

```js
const appendString = (obj) =>
    obj.b.c + " is great";
    
appendString(a);

```

这样的写法很棒，但可悲的是`a`对象并非是一成不变的。因此，我们收回的数据有时会采取到以下的形式：

```js
const a = {
    b: {}
};

// or

const a = {};

```

当这个时候我们调用了appendString函数时，整个宇宙将会爆炸的...

## 无法读取未定义的属性“c”

这个时候可能要我们对函数的传参进行空检查：

```js
const appendString = (obj) => {
    if (!obj || !obj.b || !obj.b.c || !) return null;
    return obj.b.c + " is great";
}
```

这是有效的，但它看起来很丑陋和很容易出错。我们必须对传参进行每种类型的对象定义特定的（正确的）空检查，这是不是很有趣（复杂）。
哈哈哈，这个时候可能`Maybe`就派得上场了。

## `Maybe`的基本用法

基本上，我们都会将要构建的对象封装其值可能为null的概念，并且考虑到随之而来的复杂性。在学习`Elm`(一门专注于Web前端的纯函数式语言)之后，我会在Maybe上的封装了两个概念状态`Maybe.just`和`Maybe.nothing`。对于初学者，我们简单地定义一个返回一个布尔值的`isNothing`方法，告诉我们Maybe是否不包含任何内容：

```js
const isNullOrUndef = (value) => value === null || typeof value === "undefined";

const maybe = (value) => ({
    isNothing: () => isNullOrUndef(value)
});
```

甚至使用一个简单的工厂函数来创建我们的`Maybe` - 考虑到往后可能会添加更多的方法，我们将使用一个对象来定义它：

```js
const Maybe = {
    just: maybe,
    nothing: () => maybe(null)
};

```

所以现在我们可以这样做：

```js
const maybeNumberOne = Maybe.just("a value");
const maybeNumberTwo = Maybe.nothing();

maybeNumberOne.isNothing(); // false
maybeNumberTwo.isNothing(); // true
```

一切都很好，但到目前为止还不是很实用。编程是关于转换数据的，所以我们需要一种改变我们的`Maybe`的方法 - 一个`map`函数。这个`map`函数将使用一个表示我们希望进行转换的函数参数，并返回一个包含转换结果的新参数。重要的是，如果`maybe`不包含任何内容，那么该函数将不会被应用，我们将返回一个新的`maybe.nothing`方法。

```js
const maybe = (value) => ({
    isNothing: () => isNullOrUndef(value),
    map: (transformer) => !isNullOrUndef(value) ? Maybe.just(transformer(value)) : Maybe.nothing()
});
```

现在我们可以这样来调用`maybe`实现：

```js
const maybeOne = Maybe.just(5);
maybeOne.map(x => x + 1); // Maybe.just(6);

const maybeTwo = Maybe.nothing();
maybeTwo.map(x => x + 1) // Maybe.nothing();
```

关键一点的是`maybe.map`返回一个新的`maybe`，所以我们可以将这些操作链接在一起。回到我们现在可以做的最初的问题：

```js
const a = {
    b: {
        c: "fp"
    }
};

const maybeA = Maybe.just(a)
    .map(a => a.b)
    .map(b => b.c)
    .map(c => c + " is great!");
```

这里的好处是，如果链中的任何步骤返回null，我们仍然会得到结果Maybe.nothing的结果，而不是运行时错误。

好了，在`Github`上面有个`maybe.js`库: [A Maybe monad implementation in JavaScript](https://github.com/stewart/maybe.js):   

它比`Haskell`的实现更加灵活，而且还附带了一些额外的功能，考虑到
JavaScript的类型系统限制和语言的一般灵活性。

## Point-free 链式函数

如果你看过我以前发布的文章[柯里化函数](http://developingthoughts.co.uk/curried-functions-and-point-free-programming/)，那么你就会想我们可以弄得比这更好。我们可以创建从对象中提取命名属性的高阶函数，以及用于追加字符串：

```js
const prop = (propName) => (obj) => obj[propName];
const append = (appendee) => (appendix) = appendee + appendix;
```

这里可以参考知乎上的[JavaScript函数式编程（一）](https://zhuanlan.zhihu.com/p/21714695)里面的知识点。

所以现在我们可以在我们的`map`链式中使用这个功能：

```js
const a = {
    b: {
        c: "fp"
    }
};

const maybeA = Maybe.just(a)
    .map(prop("b"))
    .map(prop("c"))
    .map(append(" is great!"));
```

好了，我们现在终于到了这一步， 我们已经处理了空检查，并将其重构为`point-free`形式。接下来需要做的就是使逻辑可重用性；我们想要的是能够将我们的功能传递给一个功能，并应用所有步骤，以便我们可以在许多不同的`Maybe`上重新使用提取器：

```js
   const extractor = // what we're about to make
    extractor(Maybe.just(a)); // Maybe.just("fp is great")
```

我们需要的是一个需要我们的步骤的函数，并且每个步骤依次调用我们`Maybe.map`方法。我们将调用函数`Maybe.chain`，我们可以用reducer来实现：

```js
const Maybe = {
    just: maybe,
    nothing: () => maybe(null),
    chain: (...fns) => (input) => fns.reduce((output, curr) => output.map(curr), input)
};
```

我们现在可以构建一个可以应用于`maybe`的可用功能：

```js
const appendToC = Maybe.chain(
    prop("b"),
    prop("c"),
    append(" is great!")
);
```

并将其用于各种输入：

```js
const goodInput = Maybe.just({
    b: {
        c: "fp"
    }
});

const badInput = Maybe.just({});

appendToC(goodInput); // Maybe.just("fp is great!")
appendToC(badInput); // Maybe.nothing()
```

虽然我在学习`Elm`之前不习惯`Maybe`的价值观概念，但是他们在JavaScript中不想落后啊。如果我们简简单单地去使用它，就只会使用到基础方法而已，所以我们要在它的基础上添加更多的功能函数。所以我将在稍后阅读关于使用`Maybe`的后续文章。

：：完毕：：

## 更多阅读

- [Elm入门实践（一）——基础篇](https://segmentfault.com/a/1190000005701562)
- [函数式编程入门教程](http://www.ruanyifeng.com/blog/2017/02/fp-tutorial.html)
- [JavaScript函数式编程（一）](https://zhuanlan.zhihu.com/p/21714695)
- [JavaScript函数式编程（二）](https://zhuanlan.zhihu.com/p/21926955)
- [JavaScript函数式编程（三）](https://zhuanlan.zhihu.com/p/22094473)
- [函数式JavaScript（2）：如何打造“函数式”编程语言？](http://blog.jobbole.com/77078/)
- [A Maybe monad implementation in JavaScript](https://github.com/stewart/maybe.js)
- [Functional Programming in Javascript](http://reactivex.io/learnrx/)













