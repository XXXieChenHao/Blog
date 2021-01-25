# 编程语言 JavaScript
> 四种基本元素
1. 变量
2. 运算能力
3. 数据结构
4. 函数


## 变量
> 变量是一个容器，可以存储数据后续使用

### 1. 声明变量
  - 声明变量的过程是向系统申请一个存储空间，也可以说声明的过程是为变量来分配一个存储空间
  - " = " 不是相等，而是将 " = " 后面的值赋予给变量的存储空间中

JavaScript 声明变量使用 var (variable 变量)

<br />

#### 2. 变量赋值

```javascript
  var a;  // 变量声明
  a = 3;  // 变量赋值

  var a = 3;  // 变量声明并赋值
  // var a = 3; 是由两个部分组成 先声明 再赋值 的过程

  var x = 1,  // 单一声明方式 一个 var 声明多个变量
      y = 2;  // 变量之间以 "," 隔开
```
#### 3. 重复赋值

<br />

JavaScrip 中在变量声明后，可以通过赋值的方式改变变量存储的数据

<br />

```javascript
  var a = 1;
  a = 2;
   document.write(a); // 此时输出 a = 2
```

#### 4. 命名规范
  - 不能以数字作为首字母，可以使用字母、_、$ 作为首字母
  - 除首字母外，可以由 字母、_、$、数字组成
  - 不能以关键字、保留字命名
    - 关键字：表示控制语句的开始或结束,或者用于执行特定操作
    - 保留字：ECMA 认定以后可能会用到的，暂时还未使用的命名
  - 语义化
    - 命名一定要有意义 避免 abc
    - 尽可能不适用拼音以及拼音缩写
  - 结构化
    - 小驼峰：首写字母是小写，其他单词首页大写 myExample
    - 大驼峰：每一个单词首字母大写 MyExample

#### 5. 优先级

变量赋值的优先级与运算优先级的对比

<br />

```javascript
  var x = 3,  // 声明一个变量 x 赋值 3
      y = 4;  // 声明一个变量 x 赋值 3

  var z = x + y;
  // 声明一个变量 z ，先计算 x + y 的，再将结果赋值给 z
  // 运算的优先级大于赋值

  document.write(z)
```

<br />

## JavaScript 的值
> 1. 原始值
  2. 引用值

### 语言类型
JavaScript 是一门弱类型语言，在定义是不确定类型，通过变量存储的值来确定类型
JS 也可以成为 动态语言，当对一个变量赋值时,是不需要考虑它的类型
- 动态语言一定是脚本语言，脚本语言一定是解释型语言，解释型语言一定是弱类型语言
- 静态语言一定是编译型语言，编译型语言一定是强类型语言


### 原始值
在 JavaScript 中 原始值又被称为**基本类型**
- JavaScript 中一共有五种原始值（基本类型）
  - Number （数字）
  - String （字符串）
  - Boolean （布尔值）
  - Undefined （undefined）
  - Null  （null）

**Number**
数字类型

```javascript
  var a = 1;
  var b = 3.14
  var c = 1e+22
  var d = -1
  // 以上在 JavaScript 中都属于数字类型
```

**String**
在单引号或双引号之间的字符叫做字符串类型

```javascript
  var str = 'My name is 汐潮';
  var str = '1'
```

**Boolean**
布尔值只有两个值 `true` 和 `false`

```javascript
  var a = true
  document.write(a) // 输出 true
```

在计算机中只有 1 和 0，也可以说 非真既假，非假既真

**Undefined**
undefined 表示一个未声明的变量，或已声明但没有赋值的变量

<br />

```javascript
  var a = undefined
  document.write(a) // 输出 undefined
  var b;
  document.write(b) // 输出 undefined
```

**Null**
null 标识空值，一般用于初始化、占位。

<br />

```javascript
  var a = null
  document.write(a) // 输出 null
```




### 引用值
> 比较常用的引用值
  1. Object
  2. Array
  3. Function
  4. Date
  5. RegExp

### 内存
> 原始值与引用值在内存当中的存储方式不同

引用值的值并不存在栈内存中，而是存在堆内存中，实际上引用值在栈内存中存储了一个地址，地址记录了引用值的值存储在堆内存中的位置。
```javascript
  var a = 1,
      b = a;
  a = 2;
  console.log(a)  // 输出 2
  console.log(b)  // 输出 1

  var arr = [1, 2 , 3, 4]
  var arr2 = arr;
  arr.push(5)
  console.log(arr)  // 输出 [1,2,3,4,5]
  console.log(arr2)  // 输出 [1,2,3,4,5]
```

基本值的值存在栈内存中，`b = a` 的操作过程中，系统将 a 的值赋值给了 b， 所以 a 与 b 在栈内存中处于两个空间，不会相互影响
引用值在存储时在栈内存中存储一个地址，地址指向堆内存中存储数据的位置，而在 `arr2 = arr` 操作时只是将 arr 在栈中所存储的地址赋值给了 arr2 实际上此时两个变量所指向同一位置，所以无论修改哪一个数据都会影响另外一个








## JavaScript 的基本语法
> 浏览器右键检查可以打开调试工具，在 Elements 中查看 HTML 节点，在 Console 中查看控制台，以及一些其他的
### 语法
### 规范
### 错误
### 运算符
### 判断分支
### 注释