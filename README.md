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
3.函数
==============================
在ES5我们给函数定义参数默认值是怎么样？<br>
```javascript
 function action(num) {
        num = num || 200
        //当传入num时，num为传入的值
        //当没传入参数时，num即有了默认值200
        return num
    }
```
但有的同学肯定会发现，num传入为0的时候就是false， 此时num = 200 与我们的实际要的效果明显不一样<br>
ES6为参数提供了默认值。在定义函数时便初始化了这个参数，以便在参数没有被传递进去时使用。<br>
```javascript
function action(num = 200) {
        console.log(num)
    }
    action() //200
    action(300) //300
```
箭头函数
-----------
箭头函数最直观的三个特点。

1.不需要function关键字来创建函数<br>
2.省略return关键字<br>
3.继承当前上下文的 this 关键字<br>
```javascript
//例如：
    [1,2,3].map( x => x + 1 )

//等同于：
    [1,2,3].map((function(x){
        return x + 1
    }).bind(this))
```
当你的函数有且仅有一个参数的时候，是可以省略掉括号的。当你函数返回有且仅有一个表达式的时候可以省略{}；例如:<br>
```javascript
 var people = name => 'hello' + name
    //参数name就没有括号
```
如下：<br>
```javascript
 var people = (name, age) => {
        const fullName = 'h' + name
        return fullName
    } 
    //如果缺少()或者{}就会报错
```
4.拓展的对象功能
==================================
对象初始化简写
<br>
ES5我们对于对象都是以键值对的形式书写，是有可能出现键值对重名的。例如：<br>
```javascript
 function people(name, age) {
        return {
            name: name,
            age: age
        };
    }
```
键值对重名，ES6可以简写如下：<br>
```javascript
    function people(name, age) {
        return {
            name,
            age
        };
    }
```
ES6 同样改进了为对象字面量方法赋值的语法。ES5为对象添加方法：<br>
```javascript
const people = {
        name: 'lux',
        getName: function() {
            console.log(this.name)
        }
    }
```
ES6通过省略冒号与 function 关键字，将这个语法变得更简洁<br>
```javascript
const people = {
        name: 'lux',
        getName () {
            console.log(this.name)
        }
    }
```
ES6 对象提供了Object.assign()这个方法来实现浅复制。Object.assign()<br>
可以把任意多个源对象自身可枚举的属性拷贝给目标对象，然后返回目标对象。<br>
第一参数即为目标对象。在实际项目中，我们为了不改变源对象。一般会把目标对象传为{}<br>
```javascript
 const obj = Object.assign({}, objA, objB)
```
5.更方便的数据访问--解构
==============================
数组和对象是JS中最常用也是最重要表示形式。为了简化提取信息，ES6新增了解构，这是将一个数据结构分解为更小的部分的过程<br>

ES5我们提取对象中的信息形式如下：<br>
```javascript
const people = {
        name: 'lux',
        age: 20
    }
    const name = people.name
    const age = people.age
    console.log(name + ' --- ' + age)
```
是不是觉得很熟悉，没错，在ES6之前我们就是这样获取对象信息的，一个一个获取。<br>
现在，解构能让我们从对象或者数组里取出数据存为变量，例如<br>
```javascript
 //对象
    const people = {
        name: 'lux',
        age: 20
    }
    const { name, age } = people
    console.log(`${name} --- ${age}`)
    //数组
    const color = ['red', 'blue']
    const [first, second] = color
    console.log(first) //'red'
    console.log(second) //'blue'
```
6.Spread Operator 展开运算符
==========================================
ES6中另外一个好玩的特性就是Spread Operator 也是三个点儿...接下来就展示一下它的用途。<br>

组装对象或者数组<br>

```javascript
//数组
    const color = ['red', 'yellow']
    const colorful = [...color, 'green', 'pink']
    console.log(colorful) //[red, yellow, green, pink]

    //对象
    const alp = { fist: 'a', second: 'b'}
    const alphabets = { ...alp, third: 'c' }
    console.log(alphabets) //{ "fist": "a", "second": "b", "third": "c"
}


```
有时候我们想获取数组或者对象除了前几项或者除了某几项的其他项<br>
```javascript
//数组
    const number = [1,2,3,4,5]
    const [first, ...rest] = number
    console.log(rest) //2,3,4,5
    //对象
    const user = {
        username: 'lux',
        gender: 'female',
        age: 19,
        address: 'peking'
    }
    const { username, ...rest } = user
    console.log(rest) //{"address": "peking", "age": 19, "gender": "female"
}
```
对于 Object 而言，还可以用于组合成新的 Object 。(ES2017 stage-2 proposal) 当然如果有重复的属性名，右边覆盖左边<br>
```javascript
const first = {
        a: 1,
        b: 2,
        c: 6,
    }
    const second = {
        c: 3,
        d: 4
    }
    const total = { ...first, ...second }
    console.log(total) // { a: 1, b: 2, c: 3, d: 4 }

```
7.import 和 export
==================================
import导入模块、export导出模块<br>
```javascript
//全部导入
import people from './example'

//有一种特殊情况，即允许你将整个模块当作单一对象进行导入
//该模块的所有导出都会作为对象的属性存在
import * as example from "./example.js"
console.log(example.name)
console.log(example.age)
console.log(example.getName())

//导入部分
import {name, age} from './example'

// 导出默认, 有且只有一个默认
export default App

// 部分导出
export class App extend Component {};

```
以前有人问我，导入的时候有没有大括号的区别是什么。下面是我在工作中的总结：<br>
```javascript
1.当用export default people导出时，就用 import people 导入（不带大括号）

2.一个文件里，有且只能有一个export default。但可以有多个export。

3.当用export name 时，就用import { name }导入（记得带上大括号）

4.当一个文件里，既有一个export default people, 又有多个export name 或者 export age时，导入就用 import people, { name, age } 

5.当一个文件里出现n多个 export 导出很多模块，导入时除了一个一个导入，也可以用import * as example

```

8. Promise
============================

在promise之前代码过多的回调或者嵌套，可读性差、耦合度高、扩展性低。<br>
通过Promise机制，扁平化的代码机构，大大提高了代码可读性；<br>
用同步编程的方式来编写异步代码，保存线性的代码逻辑，极大的降低了代码耦合性而提高了程序的可扩展性。

