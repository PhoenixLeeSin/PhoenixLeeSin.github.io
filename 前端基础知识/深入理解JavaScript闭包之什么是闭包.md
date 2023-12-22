# [深入理解JavaScript闭包之什么是闭包](https://segmentfault.com/a/1190000023356598)



### [原文链接](https://segmentfault.com/a/1190000023356598)

## 前言

在看本篇文章之前，可以先看一下之前的文章 [深入理解JavaScript 执行上下文](https://link.segmentfault.com/?enc=jR3rmDETdn6TOLHIIBGKkw%3D%3D.7I%2Fctl493aZG8ZdrdrNqcG6z3MALFFR%2FN%2BZKWiAZ1jS4UPf7sYSG6jcoPLxc9q0d%2BgGjZX7%2F22L%2BfOPrQ2kLAw%3D%3D) 和 [深入理解JavaScript作用域](https://link.segmentfault.com/?enc=N3MVP66xw73OvPg7opHFbw%3D%3D.k8phABABjfoz7gpJXpP%2FI5w16iWpWfeLo%2FVkc5pbRta9Bf2YSqeb0KMVpew1P0ojlfvb%2BXoE4NooGhsTlhhtyg%3D%3D)，理解执行上下文和作用域对理解闭包有很大的帮助。

需要回忆的一些知识点:

1. **作用域和词法作用域**，作用域就是查找变量(去哪儿找，怎么找)的一套规则。词法作用域在你写代码的时候就确定了。`JavaScript`是基于词法作用域的语言，通过变量定义的位置就能知道变量的作用域。
2. **作用域链**：当某个函数第一次被调用时，会创建一个执行环境及相应的作用域链，并把作用域链赋值给一个特殊的内部属性 `[[Scope]]` 。然后，使用 `this`、`arguments` 和其他命名参数的值来初始化函数的活动对象。但在作用域中，外部函数的活动对象始终处于第二位，外部函数的外部函数的活动对象处于第三位，...直至作用作用域链终点的全局执行环境。

## 一个真实的面试场景

- A: 什么是闭包
- B: 函数 foo 内部声明了一个变量 a， 在函数外部是访问不到的，闭包就是可以使得在函数外部访问函数内部的变量
- A：额，不太准确，那你说一下闭包有什么用途吧
- B: ...，不好意思，一下子想不起来了
- A：今天面试就到这儿了，有后续再通知你。

闭包差不多是面试必问的一个知识点了，记得几年前刚出来找实习的时候问的是这个，现在出去面试还是一直在问这个。很有必要好好学习一下，不仅仅是因为面试，更是因为它在代码中也非常常见。

## 什么是闭包

**当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行的**。

```javascript
function foo() {
    var a = 1; // a 是一个被 foo 创建的局部变量
    function bar() { // bar 是一个内部函数，是一个闭包
        console.log(a); // 使用了父函数中声明的变量
    }
    return bar();
}
foo(); // 1
```

foo() 函数中声明了一个内部变量 a , 在函数外部是无法访问的，bar() 函数是 foo() 函数内部的函数，此时 foo 内部的所有局部变量，对 bar 都是可见的，反过来就不行，bar 内部的局部变量，对 foo 就是不可见的。这就是javaScript特有的”作用域链“。

```javascript
function foo() {
    var a = 1; // a 是一个被 foo 创建的局部变量
    function bar() { // bar 是一个内部函数，是一个闭包
        console.log(a); // 使用了父函数中声明的变量
    }
    return bar;
}
const myFoo = foo();
myFoo();
```

这段代码和上面的代码运行结果完全一致，其中不同的地方就是在于内部函数 bar 在执行前，从外部函数返回。`foo()` 执行后，将其返回值（也就是内部的 `bar` 函数）赋值给变量 `myFoo` 并调用 `myFoo()`, 实际上只是通过不同的标识符引用调用了内部的函数 `bar()`。

`foo()` 函数执行后，正常情况下 `foo()` 的整个内部作用域被销毁，占用的内存被回收。但是现在的 `foo`的内部作用域 `bar()` 还在使用，所以不会对其进行回收。bar() 依然持有对改作用域的引用，这个引用就叫做闭包。这个函数在定义的词法作用域以外的地方被调用。闭包使得函数可以继续访问定义时的词法作用域。

用一句话描述：**闭包是指有权访问另一个函数作用域中变量的函数**。创建闭包最常见的方式就是，在一个函数内部创建另一个函数。

### 常见的一些闭包

```javascript
function foo(a) {
    setTimeout(function timer(){
        console.log(a)
    }, 1000)
}
foo(2);
```

`foo`执行`1000ms` 后，它的内部作用域不会消失，`timer`函数依然保有 `foo` 作用域的引用。timer函数就是一个闭包。

定时器，事件监听器，`Ajax`请求，跨窗口通信，`Web Workers`或者其他异步或同步任务中，只要使用回调函数，实际上就是闭包。

### 循环和闭包

```javascript
for(var i = 0; i < 5; i++) {
    setTimeout(() => {
        console.log(i);
    }, i * 1000);
}
```

上面的这段代码，预期是每隔一秒，分别输出 `0, 1, 2, 3, 4`, 但实际上依次输出的是 `5, 5, 5, 5, 5`。首先解释5是从哪里来的，这个循环的终止条件是 `i` 不再 `< 5`，条件首次成立时 `i` 的值是`5`，因此，输出显示的是循环结束时 i 的最终值。

**延迟函数的回调会在循环结束时才执行**。事实上，当定时器运行时即使每个迭代中执行的都是 `setTimeout(.., 0)`,所有的回调函数依然是在循环结束后才会被执行。因此每次输出一个 5来。

我们的预期是每个迭代在运行时都会给自己 "捕获" 一个 `i` 的副本。但是实际上，根据作用域的原理，尽管循环中的五个函数都是在各自迭代中分别定义的，但是他们都封闭在一个共享的全局作用域中，因此实际上只有一个 `i`。即所有函数共享一个 `i` 的引用。

```javascript
for(var i = 0; i < 5; i++) {
    (function(j){
        setTimeout(() => {
            console.log(j);
        }, j * 1000);
    })(i)
}
```

代码改成上面这样，就可以按照我们期望的方式进行工作了。这样修改之后，在每次迭代内使用 IIFE（立即执行函数）会为每个迭代都生成一个新的作用域，使得延迟函数的回调可以将新的作用域封闭在每个迭代内部，每个迭代内部都会含有一个具有正确值的变量可以访问。

```javascript
for(let i = 0; i < 5; i++) {
    setTimeout(() => {
        console.log(i);
    }, i * 1000);
}
```

使用 ES6 块级作用域的 let 替换 var 也可以达到我们的目的。

> 为什么总是 JavaScript 中闭包的应用都有着关键词 “return”, javaScript 秘密花园 中有一段话解释到：闭包是JavaScript 一个非常重要的特性，这意味着当前作用域总是能够访问外部作用域的变量。 因为函数是 JavaScript 中唯一拥有自身作用域的结构，因此闭包的创建依赖于函数。

### 需要注意的点

**容易导致内存泄漏**。
闭包会携带包含它的函数作用域，因此会比其他函数占用更多的内存。过度使用闭包会导致内存占用过多，所以要谨慎使用闭包。

## 关于this的情况

在闭包中使用 `this` 对象。

> this对象是运行时基于函数的执行环境绑定的。全局函数中，this指向 window，当函数被作用某个对象的方法调用时，this指向这个对象，不过匿名函数的执行环境具有全局性，因此其this对象通常指向window。之前这篇[一文理解this、call、apply、bind](https://link.segmentfault.com/?enc=pLNYImNkm%2FK0%2F9HeG3ba7g%3D%3D.uJUnkr7VBRxDC6PMmWLd%2Bq%2BkvY6ERwdOm1VdT3ToXbBF9EGcSv17EwpvEsPvgpgcoj51zomLfAgCySXLdMxAQw%3D%3D)文章中也专门讲了this。

```javascript
var name = 'The window';

var object = {
    name: 'my Object',
    getName: function() {
        return function() {
            return this.name;
        }
    }
}
console.log(object.getName()()); // The window 非严格模式下
```

1. 上面代码创建了一个全局变量 name, 又创建了一个包含 name 属性的对象，这个对象还包含了一个方法 `getName()`，它返回一个匿名函数，而匿名函数又返回 `this.name`。
2. 由于`getName`返回一个函数，因此调用 `object.getName()()` 会立即调用它返回的函数。结果就是返回字符串 “The window ”，即全局 name 变量的值。

为什么匿名函数没有取得包含作用域的this对象呢？每个函数在被调用时会自动获取两个特殊的变量： `this`, `arguments`。内部函数在搜索这两个变量时，只会搜索到其活动对象为止，因此永远不可能直接访问外部函数的这两个变量。

不过把外部作用域中的 this对象保存在一个闭包能够访问到的变量里，就可以让闭包访问该对象了。

```javascript
var name = 'The window';

var object = {
    name: 'my Object',
    getName: function() {
        var that = this; // 把this对象赋值给了 that变量
        return function() {
            return that.name;
        }
    }
}

console.log(object.getName()()); // my Object
```

上面代码中把this对象赋值给了 `that`变量，`that`变量时包含在函数中的，即时函数返回之后，that也仍然引用这 object，所以调用 `object.getName()()` 返回 “my Object”

> arguments 和 this存在相同的问题，如果想访问作用域中的 arguments 对象，必须将对该对象的引用保存到另一个闭包能够访问的变量中。

有几种特殊情况下，this的值可能会意外地发生改变。比如下面的代码是修改其前面例子的结果。

```javascript
var name = 'The window';

var object = {
    name: 'my Object',
    getName: function() {
        return this.name
    }
}

console.log(object.getName()); // my Object
console.log((object.getName)()); // my Object
console.log((object.getName = object.getName)()); // The window 非严格模式下
```

1. 第一个就是正常的调用，打印 `“my Object”`
2. 第二个就是在调用这个方法前先给它加上了括号，但是和 object.getName 是一样的，所以打印为 `"my Object"`
3. 第三个是先执行了一个赋值语句，然后再调用赋值后的结果。因为这个赋值表达式是函数本身，所以此时调用，`this` 指向的是 `window`，打印的是 `"The window"`

关于什么是闭包就大概说到这里，下一篇文章会讲一下闭包的应用场景。

## 总结

- 闭包是指有权访问另一个函数作用域中变量的函数。
- 闭包通常用来创建内部变量，使得 这些变量不能被外部随意修改，同时又可以通过指定的接口来操作。

## 参考

- [破解前端面试（80% 应聘者不及格系列）：从闭包说起](https://link.segmentfault.com/?enc=JtzaJrSvrCVfpLFGBPJyxA%3D%3D.SqClSHoM7QUqZ6w1U0UIQlJymoHDNy84x2BmKBUZDAqSk96QaysasP39mRM9G%2FXP)
- [MDN - 闭包](https://link.segmentfault.com/?enc=WXwLai5GzWB78NLUsce%2BEw%3D%3D.eqztpFQw69UPOK3y2y6gYgaYFZz6ExEc6k1QtfCJIvXVOBVYC3JLxEj9k7av06YmrrDJjzCFCojyvgxDn2fLRiHwb%2Bhu8p4MkeLN%2BF2zZQo%3D)
- [学习Javascript闭包（Closure）](https://link.segmentfault.com/?enc=JuRpEF630mlgPOZ06OdCTg%3D%3D.Ye67iykq0q9R5GcTc7o%2BLKBSMxWXLF%2Fog9STIUEm9E%2BcUmzQQkDhtstmJw4D9HMjheOZE1Y4ZGLoNHASf%2FrlLkM3E%2BtSOOjQ1SBFRFaQ5Ko%3D)
- [闭包详解一](https://link.segmentfault.com/?enc=rzRYbKeVCCPYf1ZjXq3gtw%3D%3D.GDEjHi0ca2BjbEQpT4rCEYQCazvLWhTfazw%2BHw1yMZGiBCHQ6EQp3Sf25Og%2Fu8Ba)
- [搞懂闭包](https://link.segmentfault.com/?enc=k5GaQEc%2F3d%2B944zxeDwu7Q%3D%3D.c9IdkUbZK%2F3bC7w3GXspnHSOBVYavNqFzD58PQ8ITpXIMPRMDOuM3n47styykZd4)
- [我从来不理解JavaScript闭包，直到有人这样向我解释它](https://link.segmentfault.com/?enc=I5hEXrzUn27Iz0P%2BVmFF1Q%3D%3D.NVNiq3oBGdiz9TqfK0uAjdk9ySiBDMxFe3kXWaCrEAXcfaOtqaNsNfl7vYYUK2%2By)

## 其他

最近发起了一个100天前端进阶计划，主要是深挖每个知识点背后的原理，欢迎关注 微信公众号「牧码的星星」，我们一起学习，打卡100天。同时也会分享一些自己学习的一些心得和想法，欢迎大家一起交流。