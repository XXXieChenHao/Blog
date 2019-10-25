# document.write() 和 innerHTML() 的异同

> 深化 DOM方法 操作文档的理解



## 一、document.write()

​	**write()** 是文档对象的一种方法，文档对象也就是当前的 HTML 页面，**write()** 方法会将括号中的内容渲染到页面上，但是docuemnt.write() 会分为以下集中情况

1、在任意位置写入script标签

write 添加的内容将被插入到文档流中

#### 1、页面未加载完成

```html
<body>
  <ul>
    <li>0</li>
    <li>1</li>
    <li>2</li>
  </ul>
  <script>
    document.write("我被书写到网页中了");
  </script>
  <p>abc</p>
</body>
```

效果



如果在 body 结束标签后写入则会添加到文档末尾

```html
<body>
  <ul>
    <li>0</li>
    <li>1</li>
    <li>2</li>
  </ul>
  <p>abc</p>
</body>

<script>
  document.write("我被书写到网页中了");
</script>

```

效果

#### 2、当页面加载完成后

> 使用入口函数 window.onload

```html
<body>
  <ul>
    <li>0</li>
    <li>1</li>
    <li>2</li>
  </ul>
  <p>abc</p>
</body>
<script>
  window.onload = function () {
    document.write("我被书写到网页中了");
  }
</script>
```

效果

#### 效果总结：

使用 window.onload 当页面加载完成后，才执行其中document.write() 方法，这时write() 会将整个页面清除再将内容渲染到页面中。原来的页面被新内容覆盖，也可以叫做 “重绘”。

## 二、innerHTML()

​	 **innerHTML()** 是 DOM 对象的一种方法，innerHTML() 有两个作用

- 获取某一指定元素的内容
- 将内容添加到某一指定元素中

### 获取内容：

#### 1、当页面未加载完成时

```html
<script>
  var p = document.getElementsByClassName('p')[0];
  console.log(p)
  console.log(p.innerHTML)
</script>

<body>
  <p class="p">abc</p>
</body>
```



**报错原因：**

浏览器执行顺序从上到下，当执行到 `<script>` 标签时页面还未加载完成，所以 输出 p 为undefined 表示未找到类名为 p 的标签，自然 innerHTML 无法使用。



#### 2、页面加载完成后

```html
<body>
  <p class="p">abc</p>
</body>
<script>
 var p = document.getElementsByClassName('p')[0];
 console.log(p.innerHTML);
</script>
```

使用 innerHTML 就可以获取标签的内容。

#### 添加内容：

1. 添加内容：

```html
<body>
  <p class="p">abc</p>
</body>
<script>
  window.onload = function () {
    var p = document.getElementsByClassName('p')[0];
    p.innerHTML = '<button>哇，我添加了一个按钮</button>';
  }
</script>
```



不难发现，原本 p 标签中的内容被innerHTML 添加的新内容替换掉了，但是整个页面并没有重绘，所以 innerHTML 只会修改指定的元素。

2. 清楚标签内容

> 有时候我们需要将标签中的内容做一个清除，我们就可以使用 innerHTML

```html
<body>
  <p class="p">abc</p>
</body>
<script>
  window.onload = function () {
    var p = document.getElementsByClassName('p')[0];
    p.innerHTML = '';
  }
</script>
```

利用 innerHTML 会覆盖掉指定元素内容的特性，使用空字符串覆盖掉原有的内容，以达到清除内容的作用。



## 三、总结

- 页面加载未完成时
  - document.write() 会按照 script 标签位置将内容插入到文档流中
  - innerHTML 则会因为获取不到标签而报错
- 页面加载完成时
  - document.write() 会将整个页面进行重绘，页面只会展示新的内容
  - innerHTML可以向指定标签添加内容
    - 添加具体内容 会将元素内原有的内容覆盖
    - 添加空字符串，清除标签的内容。
- 当添加的内容中包含标签的时候两种方法都可以识别标签。