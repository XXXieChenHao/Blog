# 浅谈 Ajax
> 当你想与后台检测用户名正确的时候。
> 当你不只想改变部分页面的时候。
> 当你想要想搜索引擎一样数据联想的时候
> 当数据的存入和展出都在同一页面的时候

本文不考虑跨域问题，跨域相关技术文档：
【[https://www.cnblogs.com/wangpenghui522/p/6284355.html](https://www.cnblogs.com/wangpenghui522/p/6284355.html "https://www.cnblogs.com/wangpenghui522/p/6284355.html")】
【[https://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html](https://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html "https://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html")】
Ajax 相关技术文档
【[廖雪峰老师所写的相关博客](https://www.liaoxuefeng.com/wiki/1022910821149312/1023022332902400 "https://www.liaoxuefeng.com/wiki/1022910821149312/1023022332902400")】
【[W3Cschool中的Ajax基础学习](https://www.w3school.com.cn/ajax/index.asp "https://www.w3school.com.cn/ajax/index.asp")】

————————————————————————————————————————————

##  Ajax 简介
  Ajax    (Asynchronous JavaScript and XML 的缩写) 是一种异步请求，所谓的异步请求，就是在代码在执行过程中，遇到异步代码，不等待异步代码执行的结果，直接进行代码执行，待运行环境中代码执行完毕后再回到运行环境中。

  Js 是设计时为了避免用户对页面的操作产生冲突，所以被设计成一种单线程，或者说是只有一个主线程的语言，这样在用户操作时任务按顺序执行，一个任务执行完毕后才会执行下一个任务，这样就不会存在任何矛盾。

### 单线程是如何实现异步的呢？

  可以简单的理解为当 js 引擎解析同步代码时按顺序执行，解析到异步代码时就会将异步任务放到另外一处排队等待，等到同步任务执行完毕后，再读取这些异步任务。

————————————————————————————————————————————


### 为什么使用 Ajax 请求？

  当我们使用同步任务时，当页面向后台请求数据，如果数据很大，就会造成页面阻塞，在此期间用户必须等待无法进行任何操作，这对用户体验度来说并不是很好。又比如说我们在使用百度、谷歌等搜索引擎的时候，输入一些文字会有相应的下拉提示，如果使用同步任务，那么每输入一个字都需要请求后台返回数据，但必须刷新页面才能完成相应的渲染，这显然也是不合理的。所以才有了 Ajax 展露的舞台。

### Ajax 的优点

  Ajax 是一种**无需重新加载页面**的情况下，与后台服务器进行数据交换，更新网页中一部分内容的技术手段。Ajax 最显著的优点就是改变页面一小部分时，或者提交数据时，不需要重新加载整个页面。减少用户等待时间，效率更高，用户体验度更好。


## Ajax 请求

  Ajax 有两种常见的请求方式 **get** 和 **post** 请求，

结合W3C官方说法和我个人的理解总结以下几点区别
| 区别     | GET                                 | POST                           |
| -------- | ----------------------------------- | ------------------------------ |
| 参数传递 | 请求参数是在 url 中发送，以 ? 分割  | 请求参数在 Http 消息主体中发送 |
| 大小     | GET 请求长度受到 url 限制           | POST 请求对数据长度没有要求    |
| 可见性   | 由于参数在url中，所以对所有人都可见 | 不在 url 中，不会被轻易看到    |
| 安全性   | 安全性差，数据是 url 的一部分       | 安全性比 GET 好一些            |

**安全性：**
GET 和 POST 请求其实都不安全, POST 相对于 GET 请求来讲只是稍微的遮挡了一点，实际上在发送过程中是在 Http 消息主体中发送。

**使用场景：** 
一般获取一些数据传参时使用 GET 请求，而在传送一些账号密码时使用 POST 请求，因为不在 url 地址栏中显示，所以避免一眼被人看见。


### jQuery 中的 Ajax 请求

  为什么先写 jQuery 中的 ajax 请求呢？ 因为 jQuery 库已经将 Ajax 封装好，引用 jQuery 库后直接使用即可，非常方便，非常的爽。所以先学会使用 jQuery 中的 Ajax 后再学习原生写法。

  代码如下
```javascript
$.ajax({             // $.ajax()
  url: 'http://......请求地址'，
  type: 'get'，      // 或者是post
  data: {
    name: '闲余',
    age: 18
  },
  success: function (res) {
    console.log(res);
  }
})
```
结合上面代码进行详细讲解。
> 第1行  $.ajax()  调用 jQuery 对象的 ajax 方法，并且传入了一个对象
> 第2行  url  请求地址，是后端工作人员书写开发文档或共同商议的请求地址
> 第3行 type  请求方式，一般这个也会在数据接口文档中竖写清楚
> 第4行 data  也是一个对象，是你需要传递的参数，属性名由开发文档执行，属性值是你要传得值。
> 第8行 success  作为执行成功的回调函数，当请求成功时就会从后台返回一个数据，res 就是那个数据，function 中的代码就是要进行的操作。

是不是觉得超级简单？
下面是 原生的 Ajax 请求。

### XMLHttpRequest 对象
&emsp&emsp原生 Ajax 请求依赖于 XMLHttpRequest 对象，XMLHttpRequest 对象用于在后台与服务器交换数据，并且所有现代的浏览器都支持 XMLHttpRequest 对象。

#### Ajax 的 GET 请求
```javascript
var xhr = new XMLHttpRequest();          // 新建 XMLHttpRequest 对象 
xhr.open('GET', '请求地址 ? urlencoded'); // 在请求地址后拼接请求参数
xhr.send();                              // 发送请求
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {            // 请求成功了
    // 请求成功了你可以做一些事情了
    console.log(xhr.responseText);
    // xhr.responseText 是后台传回来的数据
  }
}
```

第1行 实例化 XMLHttpRequest 对象 存储到变量 xhr 中  var xhr= new XMLHttpRequest();    新建 XMLHttpRequest 对象
> xhr 是一个变量名 随便起的你想叫 cat dog 只要符合规范 叫什么都行，xhr 为 XMLHttpRequest 的首字母缩写 所以多数使用 xhr 为变量名

第2行 open 方法设置和服务器的交互信息设置为 GET 请求类型，url?urlencoded 格式的参数
 		urlencoded 是一种参数格式， 格式为 参数1=值1&参数2=值2...

第3行 send() 方法，将请求发送，这个过程是异步的，所以会继续执行第4行的代码，而不会在这一行等待。

第4行 xhr 有一个属性 readyState , 监听这个属性的变化，每次变化就会执行后面的事件执行程序

第5行 判断 xhr 的 readyState 属性的值

- readyState 属性的值共有五种
  - 0   未初始化——对象已经创建但还没有调用 open 方法;
  - 1   已建立连接——对象已经创建并初始化，但未调用 send 方法;
  - 2   已发送请求——已经调用 send 方法，但该对象正在等待状态码和头的返回
  - 3   正在接收——已经接收了部分数据，不完整，不能使用该对象的属性和方法
  - 4   已加载——所有数据接收完毕

> 这里有一个简化在 IE9 以后 可以使用 xhr.onload 代替 xhr.onreadystatechange, 如果使用 onload 则不需要对 xhr.readyState 状态进行判断，当状态码为 **4** 时，表示信息接收完毕并且调用函数。

#### Ajax 的 POST 请求
```javascript
var xhr = new XMLHttpRequest();    // 新建 XMLHttpRequest 对象
xhr.open('POST', '请求地址');      // 请求地址
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');    // 设置请求头
xhr.send(参数);                   // 发送请求
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {       // 请求成功了
    // 请求成功了你可以做一些事情了
    console.log(xhr.responseText);
  }
}
```

第1行 与 GET 请求一样，实例化对象

第2行 设置 POST 请求，地址只写请求地址，不拼接参数，因为 POST 请求不在 url 中传参

第3行 设置请求头 setRequestHeader('Content-Type','application/x-www-form-urlencoded') 不写会报错
> 这是因为使用 POST 传送数据的时候要对传输的数据进行编码处理，将参数变为 urlencoded 编码格式，如果传送的数据时 **FormData** 就不用设置请求头，因为 **FormData** 不是 urlencoded 格式。

第4行 send() 方法，将请求发送，将数据写在括号中。

第4行 监听这个状态码的变化

第5行 判断 xhr 的 readyState 属性的值

————————————————————————————————————————————

## 原生 Ajax 的封装
  有时候我们为了方便使用，我们可以将原生 Ajax 封装。
```javascript
  // options 形参接收一个对象
function ajax(options) {
  // 如果传入请求类型就按照请求类型，否则默认 get
  var type = options.type || 'get';

  // 传入请求地址 url
  var url = options.url;

  //数据是否存在如果存在调用 dataFormat 函数,
  var data = options.data ? getUrlencoded(options.data) : null;

  var success = options.success;      // 传入回调函数

  var xhr = new XMLHttpRequest();     // 实例化对象

  // 使用三元，如果请求类型为 GET 则拼接参数
  xhr.open(type, type === 'get' ? url + '?' + data : url);

  if (type === 'get') {
    xhr.send();
  } else {
    // 如果请求参数不是 GET 则需要设置请求头，未考虑 FormDate 情况
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
    xhr.send(data);
  }

  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4) {
      // 将 xhr.responseText 作为实参传到 success 中函数
      success && success(xhr.responseText);
      // 如果 success 存在 执行 success()
    }
  };

};

// 参数处理程序
function getUrlencoded(data) {
  var arr = [];
  for (var k in data) {
    // 将参数拼接成 [参数1=值1，参数2=值2，...的数组]
    arr.push(k + '=' + data[k]);
  }
  // 调用数组 join 方法将数组拼接成  参数1=值1&参数2=值2...的字符串
  var result = arr.join('&');
  return result;
}
```
**重点内容详解**
**ajax 的形参接收了一个对象，使用对象的好处：**
- 无序，对象的属性是无序的，所以调用 ajax 函数时传参不需要按照顺序，
- 可选参数  如果使用非对象的形式，写多少形参就必须传多少实参，有时有些参数是可选的，就会出现问题
  - 比如传三个形参 a b c ，b 是一个可选参数，传实参时只想传 a 和 c，那么系统就会将实参分配到 a 和 b 上，这显然是不合理的
  

**success && success(xhr.responseText)**
- 这里牵扯到 非布尔类型的逻辑判断
- 如果第一个参数可以判断整个表达式，则不再向下执行，如果判断不了则判断第二个参数
- 如果 success 存在，&& 中必须两个参数都为 true 才能判断整个表达式，所以向后执行，当代码读取 success(xhr.responseText) 就已经执行了这个函数。

到这里原生 Ajax 封装基本就已经完成了，下面是调用!
```javascript
ajax({
  // 传入一个对象
  type: 'get',        // 【可选参数】 get 或者 post 如果不传，默认 get  大小写无所谓
  url: '请求地址',     // 【必传参数】
  data: {             // 【可选参数】 以对象的方式传入
    参数1: 值1,
    参数2: 值2,
      ...
  },
  success: function (res) {   // 【可选参数】  传入回调函数，形参为 res
    // 进行想要的操作
  }
})
```
【😊 转载请注明出处链接】
