### this 指向问题
[微信收藏](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651552403&idx=1&sn=9f3ebb90573742a9506875ab6b2c2a5b&chksm=8025ad52b7522444b61cbb18ac593fc1b523e1bd383d10614e3ab9a6ee5e02ef3a2a745175f6&scene=0&key=cf0fa2bf8e1a08acddf806280dc4c2d91f38725fc6d6f7183a29d35633f58db07da6ff35b0f44e823a7e47706f05e74b8c7e039fec9f781b4022c9a60d87e66fa746bfd17da94a6b11f8bcac60942cd5&ascene=0&uin=MTc1MTY2NDg2MQ%3D%3D&devicetype=iMac+MacBookPro13%2C2+OSX+OSX+10.12.5+build(16F73)&version=12020810&nettype=WIFI&fontScale=100&pass_ticket=ReTDcqRl%2BHxWX%2ByvtUlqiBXKCKX%2B8lS0nDTRpSWS0vm0DaAJhCmwldF19J%2B%2BR3DC)
1.
```javascript
function a() {
    var user = '追梦子';
    console.log(this.user);  //undefined
    console.log(this); //window
}
a();
```


2
```javascript
function a () {
    var user = '追梦子';
    console.log(this.user);  //undefined
    console.log(this);  //window
}
window.a();
```


3
```javascript
var o = {
    user: '追梦子',
    fn: function () {
        console.log(this.user);   //追梦子
        console.log(this);        // o
    }
}
o.fn();
```


4
```javascript
var o = {
    user: '追梦子',
    fn: function () {
        console.log(this.user); //追梦子
        console.log(this);  // o
    }
}
window.o.fn();
```


5
```javascript
var o = {
    a: 10,
    b: {
        a: 12,
        fn: function () {
            console.log(this.a); // 12
            console.log(this); // b
        }
    }
}
o.b.fn();
```


* 情况1：
> 如果一个函数中有this，但是它没有被上一级的对象所调用，
那么this指向的就是window，
这里需要说明的是在js的严格版中this指向的不是window


* 情况2：
> 如果一个函数中有this，这个函数有被上一级的对象所调用，
那么this指向的就是上一级的对象。


 * 情况3：
> 如果一个函数中有this，这个函数中包含多个对象，
尽管这个函数是被最外层的对象所调用，
this指向的也只是它上一级的对象


6
```javascript
var o = {
    a: 10,
    b: {
        // a: 12,
        fn: function () {
            console.log(this.a); //undefined
            console.log(this); //b
        }
    }
}
o.b.fn();
```


7  特例
this永远指向的是最后调用它的对象，
也就是看它执行的时候是谁调用的
```javascript
var o = {
    a: 10,
    b: {
        a: 12,
        fn: function () {
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();
```


8.构造函数中的this
> 这里用变量a创建了一个Fn的实例（相当于复制了一份Fn到对象a里面）
此时仅仅只是创建，并没有执行，而调用这个函数Fn的是对象a，
那么this指向的自然是对象a
```javascript
function Fn() {
    this.user = '追梦子';
    console.log(this);  // 追梦子
}
var a = new Fn();
console.log(a.user); // 追梦子
console.log(a); // Fn
```

9
```javascript
function fn () {
    this.user = '追梦子';
    return {};  // return 一个空对象
}
var a = new fn;   // *这里没有括号
console.log(a.user); //undefined
```


10
```javascript
function fn () {
    this.user = '追梦子';
    return function () {}; // return 一个函数
}
var a = new fn;
console.log(a.user); //undefined
```


11
```javascript
function fn () {
    this.user = '追梦子';
    return 1;   // ruturn 一个数
}
var a = new fn;
console.log(a.user); // 追梦子
```


12
> 如果返回值是一个对象，那么this指向的就是那个返回的对象，
如果返回值不是一个对象那么this还是指向函数的实例。
```javascript
function fn() {
    this.user = '追梦子';
    return undefined;
}
var a = new fn;
console.log(a.user); // 追梦子
console.log(a); //fn {user: '追梦子'}
```


13 特例
> return null
```javascript
function fn () {
    this.user = '追梦子';
    return null;
}
var a = new fn;
console.log(a.user); // 追梦子
```

在严格模式中，默认的this不再指向window，而是undifined


new操作符会改变函数this的指向问题
首先new关键字会创建一个空的对象，
然后会自动调用一个函数apply方法，
将this指向这个空对象，
这样的话函数内部的this就会被这个空的对象替代。
