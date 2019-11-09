# Web Storage

## HTTP 的无状态

 HTTP 是一个无状态协议，表示客户端所进行的每一次请求都是相对独立的，在前后端交互中是没有任何记忆的。

Cookie  一个保存在客户机中的简单的文本文件，Cookie 保存了客户端浏览器访问 Web 时的信息，并将信息存储到用户的机器上，当客户端再次访问这个 Web 服务时就可以使用文档中的信息。如果想实现在客户端保存少量数据，早期只能使用 Cookie 来实现，但是随着前端发展，Cookie 已经越来越无法满足开发者的需求，因为 Cookie 有几点不足

- Cookie 大小限制为 4KB
- Cookie 包含在每个 HTTP 请求中，导致多次发送冗余数据
- Cookie 在传输中未加密，存在安全隐患

HTML5 为了改善这种情况，新增了 Web Storage 功能，通过 Web Storage 可以让应用程序在客户端保存数据， 无须持续地将数据发回服务器。



## Web Storage

### 查看浏览器中的 Web Storage 

以谷歌示例：打开浏览器调试界面【F12】，在 Application 中就可以看到

<img src="D:\blog_upload\Blog\images\查看storage.gif" alt="查看storage" style="zoom:100%;" />

我们可以看到 Web Storage 分为 Local Storage 和 Session Storage 两种。



## Storage 

Storage 是 HTML5 新增的功能，有着比 Cookie 更强大的优点

###### 优点：

- 存储空间更大。
- 存储空间独立，每个域拥有独立的存储空间
- 存储内容在本地，对远端没有任何影响

凡事都有双面性，Storage 在带来强大的功能和便捷的同时，同样存在着一些问题

###### 缺点：

- 本地数据未加密



### local Storage

​	Local Storage 是保存在用户磁盘上的 Web Storage，因为保存在用户的磁盘上，所以通过 Local Storage 保存的数据生命周期很长，除非用户或程序主动的清除这些数据，否则数据会一直存在于用户的磁盘中。



###  Session Storage

Session Storage 也将数据保存在客户端中，Session Storage 与 Session 期限相同，用户从第一次访问某网站开始，到用户关闭浏览器离开网站中间的这段时间有效。



### Storage 提供了一些属性和方法

**setItem(key, value):  该方法向 Storage 中存入指定的键值对**

```javascript
sessionStorage.setItem('name', '闲余');
console.log( sessionStorage )		// 
```

![设置storage](D:\blog_upload\Blog\images\设置storage.gif)



**getItem(key):  该方法返回 Storage 中指定 属性名【key】 所对应的 值**

```javascript
sessionStorage.setItem('name', '闲余');
console.log( sessionStorage.getItem('name') ) 
// 闲余
```



**length:  该属性返回 Storage 里保存多少组键值对**

```javascript
sessionStorage.setItem('name', '闲余');
sessionStorage.setItem('age', 18);
sessionStorage.setItem('gender', '男');
console.log( sessionStorage.length )
//  3
```



**key(index):  该方法返回第 index 个 属性名【key】**

```javascript
sessionStorage.setItem('name', '闲余');
sessionStorage.setItem('age', 18);
sessionStorage.setItem('gender', '男');
console.log( sessionStorage.key(1) )
// age
```



**removeItem(key):  该方法从 Storage 中删除指定属性名的键值对**

```html
<body>
    <button onclick="fun()">清除 Storage</button>
</body>
<script>
    sessionStorage.setItem('name', '闲余')
    sessionStorage.setItem('age', 18)
    sessionStorage.setItem('gender', '男')

    function fun() {
        sessionStorage.removeItem('name');

    }
</script>
```

![remove](D:\blog_upload\Blog\images\remove.gif)



**clear():  删除 Storage 中所有键值对**

```html
<body>
    <button onclick="fun()">清除 Storage</button>
</body>
<script>
    sessionStorage.setItem('name', '闲余')
    sessionStorage.setItem('age', 18)
    sessionStorage.setItem('gender', '男')

    function fun() {
        sessionStorage.clear();

    }
</script>
```

![clear](D:\blog_upload\Blog\images\clear.gif)

