# ES6-Basis
1.变量声明const和let
====================
我们都是知道在ES6以前，var关键字声明变量。无论声明在何处，都会被视为声明在函数的最顶部。这就是函数变量提升。<br>
>>例如：
```javascript
function fun() {
    if(bool) {
        var test = 'hello world'
    } else {
        console.log(test)
    }
  }
```
以上的代码实际上是：<br>
```javascript
function fun() {
    var test // 变量提升
    if(bool) {
        test = 'hello world'
    } else {
        //此处访问test 值为undefined
        console.log(test)
    }
    //此处访问test 值为undefined
  }
```
所以不用关心bool是否为true or false。实际上，无论如何test都会被创建声明。<br>
ES6中我们通常用let和const来声明，let表示变量、const表示常量。let和const都是块级作用域。<br>

for循环的计数器，就很合适使用let命令。<br>
```javascript
for (let i = 0; i < arr.length; i++) {}
console.log(i);
//ReferenceError: i is not defined
```
上面代码的计数器i，只在for循环体内有效<br>
下面的代码如果使用var，最后输出的是10。<br>
```javascript
var a=[];
for(var i = 0; i < 10; i++){
    a[i]=function(){
        console.log(i);
    }
}
a[6]();//10
```
上面代码中，变量i是var声明的，在全局范围内都有效。所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮的i的值。<br>
如果使用let，声明的变量仅在块级作用域内有效，最后输出的是6。<br>
```javascript
var a = [];
for (let i = 0; i < 10; i++) {
    a[i] = function () {
        console.log(i);
    };
}
a[6](); // 6
```
上面代码中，变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。<br>
```javascript
if (true) {
// TDZ开始
    tmp = 'abc'; // ReferenceError
    console.log(tmp); // ReferenceError
    let tmp; // TDZ结束
    console.log(tmp); // undefined
    tmp = 123;
    console.log(tmp); // 123
}
```
上面代码中，在let命令声明变量tmp之前，都属于变量tmp的“死区”。

```javascript
function f1() {
    let n = 5;
    if (true) {
        let n = 10;
    }
    console.log(n); // 5
}
```
再来说说const。<br>
const声明一个只读的常量。一旦声明，常量的值就不能改变。<br>
```javascript
const name = 'sw'<br>
name = 'vic' //再次赋值此时会报错<br>
```
说一道面试题<br>
```javascript
var funcs = []
for (var i = 0; i < 10; i++) {
    funcs.push(function() { console.log(i) })
}
funcs.forEach(function(func) {
    func()
})
```
这样的面试题是大家常见，很多同学一看就知道输出 10 十次<br>
但是如果我们想依次输出0到9呢？两种解决方法。直接上代码。<br>
```javascript
// ES5告诉我们可以利用闭包解决这个问题
var funcs = []
for (var i = 0; i < 10; i++) {
    func.push((function(value) {
        return function() {
            console.log(value)
        }
    }(i)))
}
// es6
for (let i = 0; i < 10; i++) {
    func.push(function() {
        console.log(i)
    })
}
```
达到相同的效果，es6简洁的解决方案是不是更让你心动！！！<br>

2.模板字符串
==================
第一个用途，基本的字符串格式化。将表达式嵌入字符串中进行拼接。用${}来界定。<br>
```javascript
//es5 
    var name = 'vic'
    console.log('hello' + name)
    //es6
    const name = 'vic'
    console.log(`hello ${name}`) //hello vic
```
第二个用途，在ES5时我们通过反斜杠(\)来做多行字符串或者字符串一行行拼接。ES6反引号(``)直接搞定。<br>
```javascript
// es5
var msg = "Hi \
    sw
"
// es6
const template = `<div>
        <span>hello world</span>
    </div>`
```
对于字符串es6当然也提供了很多厉害的方法。说几个常用的。<br>
```javascript
    // 1.includes：判断是否包含然后直接返回布尔值
    let str = 'hahay'
    console.log(str.includes('y')) // true
    // 2.repeat: 获取字符串重复n次
    let s = 'he'
    console.log(s.repeat(3)) // 'hehehe'
    //如果你带入小数, Math.floor(num) 来处理
```