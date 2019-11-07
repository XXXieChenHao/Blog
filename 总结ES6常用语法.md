<center><h1>总结 ES6 常用语法</h1></center>

> ES6， 全称 ECMAScript 6.0 ，是 JavaScript 的下一个版本标准，2015.06 发版。 

## 前言

​	JavaScript 正式的名字其实是  ECMAScript ，JavaScript 虽然名字中带有 Java- 但实际上与 Java 没有什么关联，1996 年网景公司将 JavaScript 提交到 ECMA(一个欧洲组织) 机构进行管理，于是陆续发行了很多版本，目前来讲ES5 的使用较多，因为 ES6 仍存有一些浏览器的兼容问题，目前 ES6 只能兼容 IE10 以上。但在移动端兼容性很好。

## 变量声明

### var 关键字

​		原本的 Js 中声明变量使用 var 关键字，Js 是一门弱类型语言，所以 var 的使用也相对于其他强类型如 Java 等灵活许多，var 关键字声明存在一些缺陷：

- 可以重复声明
- 无法限制删除和修改
- 只有全局作用域和函数作用域

使用 var 关键字可能会出现一些开发者无法预见的错误，如变量覆盖，降低代码可读性等。当然不包括【严格模式】



### 使用 let 声明变量

#### 不存在变量提升

- 预解析

  - 在当前作用域下,js运行之前，会把带有 var 和 function 关键字的事先声明，（可以理解成提升到执行顺序的最顶端）并在内存中安排好。然后再从上到下执行js语句。
  - **let**  则不会像 var 那样发生变量提升现象，所以变量必须声明后使用

```javascript
// var first； 预解析

consol.log(first); //  输出undefined
consol.log(second); //  报错ReferenceError

var first = 2;
//first = 2； 赋值

let second = 2;

//  使用var，会发生变量提升，脚本开始运行，变量first就已经被声明，但是没有值，所以输出 undefined
//  let不会变量提升，所以输出的时候变量second是不存在的，所以报错
```



#### 禁止重复声明

```javascript
var a = 10;
var a = 20;
console.log(a);	//  20

let b = 10;
let b = 20;
console.log(b)  //  Identifier 'b' has already been declared
```



#### 代码块有效

ES6 中引入了代码块的概念，后面会详细讲解代码块

```javascript
{
    let a = 10;
    var b = 1;
}

console.log(a)		// 	ReferenceError: a is not defined
console.log(b)		//	1
//	a有效  b报错 说明 let 声明只在它所在代码块有效
```



常用场景：for循环，防止变量泄露

```javascript
for (let i = 0; i < 10; ++i) {}
console.log(i);		//	ReferenceError: i is not defined

for (var j = 0; j < 10; ++j) {}
console.log(j)		//	10
// 使用let只在for循环体内有效
```





### const 关键字

> 常量声明

#### 不可修改

使用 const 声明后不允许修改

```javascript
const PI = 3.1415926;
console.log(PI) 	//  3.1415926

PI = 3.14;
console.log(PI) 	//  Assignment to constant variable
```



#### 声明必须赋值

ES6 中规定声明的同时就必须赋值 【const 变量名 = 变量值;】

```javascript
const PI;
PI = 3.1415926;

console.log(PI);	// Missing initializer in const declaration

```



#### 此外

不存在变量提升和禁止重复声明与 **let** 相同



## 块级作用域

在 ES5 中只有两种作用域，全局作用域和函数作用域，只有函数才能分割出一块作用域，在 ES6 中引入了块级作用域的概念，js 在保留全局作用域和函数作用域的基础上增加了块级作用域，

#### ES5 和 ES6 作用域对比

#### ES5

**变量泄露**

在 for 循环中的数据可以在外部使用

```javascript
for (var i = 0; i < 50; i++) {
   // 进行了一些操作
}
console.log(i) 		// 输出 50
```

**变量覆盖**

```javascript
var value = 10;

(function(){
    console.log(value);		// 输出 undefined
    if(false) {
         var value = '20'
    }
})()
```

if 程序块中存在变量提升，变量声明覆盖全局变量，但未赋值，所以输出 undefined



#### ES6 — 块级作用域 

**每一个代码块都是一个块级作用域**

```javascript
if(true) { // 块级作用域 }
    
for(...) { // 块级作用域 }
    
{
    // 一对花括号也是一个块级作用域
}
```



**相互独立**

```javascript
{
    {
        let inner = 'Inner'
    }
    console.log(inner);   //  inner is not defined
};
```

**不同作用域变量名不覆盖**

```javascript
let n = 5; 
{
  let n = 10;
  console.log(n) // 10
}
console.log(n); // 5
```



## 箭头函数

### 基本书写

​	将 function 去除，在 ( ) 后加上 => 

```javascript
function () { 
    // 一段程序
}

() => {
    // 一段程序
}
```

举例

```javascript
setInterval(() => {
    // 一段程序
},1000)

element.onclick = () => {
    // 事件执行程序
}
```

### 参数传递

#### 无参数和两个以上参数

```javascript
// 无参数
setInterval(() => {
    // 一段程序
},1000)

// 多个参数
setInterval((a,b,c) => {
    // 一段程序
},1000)
```

#### 仅有一个参数

当只有一个参数时，可以省去括号

<span style="color:red">注意：</span>多也不可以，少也不可以

```javascript
// 数组遍历
arr.forEach((item) => {
    
})
或
arr.forEach( item => {
    
})
```



### 返回值

#### 简单语句

简单语句可以省去花括号 { }

```javascript
setTimeout(() => {
    console.log(123)
}, 1000);

// 简写
setTimeout(() => console.log(123),1000);
```



简单语句并且只有一个 return 时可以省去花括号并且必须删除 return

```javascript
let a = () => {
    return 20
}
console.log(a())

// 简写
let a = () => 20
console.log(a())
```



## 字符串模板

### ES5 中字符串拼接

书写繁琐，

```javascript
// 使用字符串拼接
var str = 'http://www.nicexch.cn' + '?index.html';
```

可能会出现换行问题

```javascript
let a = '插入'
let str = '<div> 
    <p></p>
    </div>'
console.log(str)
```

原因：换行时会产生一个看不见的换行符，导致错误

解决方案  在每一行后面加上反斜线 \ 将换行符转义

```javascript
let a = '插入'
var str = '<div>\
    <p>' + a + '</p>\
    </div>'
console.log(str)
```



### ES6 中字符串模板

使用 ``  键盘 ESC 键下面

```javascript
let a = '插入'
let str = `<div> 
    <p>${a}</p>
    </div>`
console.log(str)
```

使用 `` 包裹字符串，使用 ${ } 插入字符串拼接