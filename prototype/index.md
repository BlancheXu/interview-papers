1. 请写出一下console的结果。
    ```javascript
    function A() {}
    function B() {}
    A.prototype = {
        fun: function() {}
    }
    var a = new A();
    console.log(a.constructor === A); // false, 应该是Object。是由于A这个类使用了他自身的一个新对象重写与定义的A.prototype对象。这个新定义的原型对象不含有constructor属性。因此，A的实例也不含有constructor属性。

    console.log(A.prototype.constructor === A); // false, 应该是Object

    console.log(a.hasOwnProperty('constructor')); // false


    a instanceof A; // true

    A.prototype = new B();
    var b = new A();

    console.log(b.constructor === A); // false, 应该是B 
    


    console.log(B.prototype.constructor === A); // false，应该是B

    console.log(b.constructor.prototype.constructor === A); // false，应该是B


    console.log(b.hasOwnProperty('constructor')); // false

    console.log(b instanceof A); // true
    


    console.log(b instanceof B); // true
    ```
2. 写出以下console结果
    ```javascript
    var b = {x: 4};
    function fn2(o) {
        this.x = o.x;
    }
    fn2.prototype = {
        init: function() {
            return this.x;
        }
    }
    var fn3 = new fn2({x: 5});
    console.log(fn3.init());
    console.log(fn3.init === fn2.init);
    console.log(fn3.init.call(b));
    var c = fn3.init;
    console.log(c());
    
    setTimeout(function() {
        console.log(1);
    }, 0);
    new Promise(function(resolve) {
        console.log(2);
        for(var i=0; i<10; i++) {
            i ==9 && resolve();
        }
        console.log(3);
    }).then(function() {
        console.log(4);
    });
    console.log(5);
    ```
    答案：
    5
    false
    4
    undefined
    5
    2
    4
    3
    1
    正确答案：
    5
    false
    4
    undefined
    2
    3
    5
    4
    1
    原因：Event Loop。js的脑子只是一根筋，是的，他是单线程的。他虽然是单线程的，但是可以模拟单线程，而且不会懵逼，这项技能就是Event Loop。[详情点我](https://juejin.im/entry/5b6967e2518825620f5803e3)

    异步任务分为微任务、宏任务，执行的顺序是：执行栈中的代码 =》微任务 =》宏任务。

    微任务：promise、MutationObserver……
    宏任务：setTimeout、setInterval、setImmediate（IE专用）、messageChannel……