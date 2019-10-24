<center><h1>Array.sort() 中的回调函数</h1></center>

### sort 的使用中的问题

**sort()** 方法用【原地算法】对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较它们的UTF-16代码单元值序列时构建的，

```javascript
let arr = [12, 43, 1, 2, 34, 5, 13];
arr.sort();
console.log(arr)
```

我们发现使用 sort() 排序后，仍然是乱序，但是仔细一看好像又有一些规律，比如看 第一位是按照数字从小到大的顺序排序的，**1**，**1**2，**1**3，**2**，**3**4，**4**3，**5**。

【原因】：元素按照转换为的字符串的各个字符的Unicode位点进行排序。

【人话版本】：

- 从左向右的第一位先比较
  - 比如 【12】 和【2】，【12】的第一位是 **1**，而【2】的第一位是 **2**，所以【12】被排到了【2】的前面。

- 如果第一位相同就比较第二位。
  - 比如【1】和【12】，第一位都是 **1**，于是比较第二位，【1】的第二位是 **空**，而【12】的第二位是 **2**，所以 【1】排在【12】的前面。 第二位为空的情况比 **0** 还要小。

### 解决方案
​		如果想要按照数字大小进行【升序】（从小到大）排序，需要在 **sort()** 中传入一个回调函数
```javascript
let arr = [12, 43, 1, 2, 34, 5, 13];
arr.sort( function( a, b ) {
    return a - b;
} );
console.log(arr);
```

​	如果想要按照数字大小进行【降序】（从大到小）排序，需要在 **sort()** 中传入一个回调函数

```javascript
let arr = [12, 43, 1, 2, 34, 5, 13];
arr.sort( function( a, b ) {
    return b - a;
} );
console.log(arr);
```



#### ES6 使用箭头函数简化写法

> ES6 的箭头函数简化写法并不是这里的重点，所以就简单写一下。

**升序**
```javascript
let arr = [12, 43, 1, 2, 34, 5, 13];
arr.sort((a, b) => a - b);
console.log(arr)
```

**降序**
```javascript
let arr = [12, 43, 1, 2, 34, 5, 13];
arr.sort((a, b) => b - a);
console.log(arr)
```
<br />
## 浅析 sort() 传参原理
​	sort() 排序使用的是一种 原地算法，我们这里使用一种简单的冒泡排序模拟一下 **sort()** ，重在理解传参的意义。
冒泡排序作为一种最基本的排序算法，代码如下
```javascript
let arr = [12, 43, 1, 2, 34, 5, 13];
for (let i = 0; i < arr.length - 1; i++) {
  for (let j = 0; j < arr.length - 1 - i; j++) {
    if (arr[j] > arr[j + 1]) {
      let temp = arr[j];
      arr[j] = arr[j + 1];
      arr[j + 1] = temp;
    }
  }
}
console.log(arr);
```
其中 `arr[j] > arr[j+1]` 时交换前后两个数据，也就是说前一项比后一项**大**就交换，最后的结果是升序排列的。
结果为 `[ 1, 2, 5, 12, 13, 34, 43 ]`

**如果降序呢？**
`arr[j+1] > arr[j]`时交换两个数据，也就是前一项比后一项**小**时就交换数据，那么最终的结果就是降序排列了。
```javascript
let arr = [12, 43, 1, 2, 34, 5, 13];
for (let i = 0; i < arr.length - 1; i++) {
  for (let j = 0; j < arr.length - 1 - i; j++) {
    if (arr[j + 1] > arr[j]) {
      let temp = arr[j];
      arr[j] = arr[j + 1];
      arr[j + 1] = temp;
    }
  }
}
bubbleSort(arr)
console.log(arr);
```

其结果为 `[ 43, 34, 13, 12, 5, 2, 1 ]`

### 封装函数
如果一段程序中需要很多次类似的排序，每一次重写冒泡排序显然不合理，所以封装成函数以保证去除冗余代码。
```javascript
// 升序
let arr = [12, 43, 1, 2, 34, 5, 13];

function bubbleSort(arr) {
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = 0; j < arr.length - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
}
bubbleSort(arr)
console.log(arr);
```

将冒泡排序封装成函数 bubbleSort 以后，目前只能进行【升序】排列，控制升序还是降序取决于 if 判断语句中的判定条件

```javascript
if(arr[j] > arr[j + 1])  // 升序
if(arr[j + 1] > arr[j])  // 降序
```

将判断条件稍微更改一下
`arr[j] > arr[j + 1]` 更改为
`arr[j] - arr[j + 1] > 0`
`arr[j + 1] > arr[j]` 更改为
`arr[j + 1] - arr[j] > 0`

也就是说 如果前一项 **减** 后一项大于 0 则升序，后一项 **减** 前一项大于 0 则降序
通过回调函数的形式将判断条件传入
```javascript
let arr = [12, 43, 1, 2, 34, 5, 13];

function bubbleSort(arr, callback) {
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = 0; j < arr.length - 1 - i; j++) {
      if (callback(arr[j], arr[j + 1]) > 0) {
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
}

// 升序调用
 bubbleSort(arr, function(a, b) {
     return a - b;
 })
// 降序调用
 bubbleSort(arr, function(a, b) {
     return b - a;
 })
```

#### 升序调用解析

```javascript
function(a, b) {
     return a - b;
 }
```

调用冒泡排序时将函数作为参数传入，使用形参 callback 接收函数参数，在条件判断时，将前一项作为实参传入回调函数的形参 a, 后一项作为回调函数的形参 b，

- 返回值为 a - b 
  - 如果前一项比后一项大时，a - b 大于 0，返回一个大于 0 的数字，满足条件，进行前后项的数据交换，
  - 如果前一项小于后一项时，a - b 小于 0，返回一个小于 0 的数字，不满足条件，不交换这两项的数据



#### 降序调用解析

```javascript
function(a, b) {
     return b - a;
 }
```

其中形参 a 为前一项， b 为后一项

- 返回值为 b - a 
  - 如果后一项比前一项大时，b - a 大于 0，返回一个大于 0 的数字，满足条件，进行前后项的数据交换，将数字大的交换到前面，数字小的交换到后面。
  - 如果后一项小于前一项时，b - a 小于 0，返回一个小于 0 的数字，不满足条件，不交换这两项的数据

#### 箭头函数调用

```javascript
// 升序调用 
bubbleSort(arr, (a, b) => {
     return a - b;
 })

// 降序调用
bubbleSort(arr, (a, b) => {
     return b - a;
 })
```



#### 总结

​	根据书写冒泡排序两种排序顺序的代码发现

```javascript
arr[j] - arr[j + 1] > 0	// 升序
arr[j + 1] - arr[j] > 0	// 降序
```

这两条是决定整个冒泡排序的关键，但我们不清楚用户需要做哪种排序，所以使用回调函数将用户项进行的排序条件传入。
 `a - b > 0 则升序`
 `b - a > 0 则降序`

**为什么传入回调函数就不会出现Unicode位点进行排序**
原因解析：我们再进行回调函数执行过程中 不论是 【a - b】 还是 【b - a】  都会将其隐式转换为数值类型进行运算，通过结果是否大于 0 来决定是否交换数据，而不是从左到右一位一位的比较，所以避免了这个问题。
<br />

#### 声明

这里只是使用冒泡排序模拟内置对象数组的 sort 方法，sort方法底层原理并不是这里所写的冒泡排序，因为冒泡排序效率太低，这里只是以简单的冒泡排序进行回调函数的理解。底层实现方式并不影响对于其回调函数传入的理解。

【😊 转载请注明出处链接】
