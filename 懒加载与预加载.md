# 性能优化之图片懒加载

浏览器加载页面时需要从服务器获取资源。

每一次张图片都需要经过

- 发送请求--> 

- 服务器接收请求 --> 

- 响应 --> 

- 渲染到页面上

开发过程中可能会有页面中存在大量图片的情况，如电商网站等类似的多图网站。这些图片的存在导致了页面加载速度的缓慢。前端开发注重交互体验，短短的几秒钟就可能失去很多用户。同时浏览器的可视区域是有限的，没有必要将图片全部展示出来。所以遇到诸如此类的情况可以使用 Lazy Load 懒加载。

## 懒加载

懒加载实际上是先将用户可见区域中的图片展示出来，将未出现在可视区域内的图片先不做加载，使用一张默认的图片代替，等到滚动到可视区域后再加载真实图片。

![Snipaste_2021-02-19_10-17-38](C:\Users\49231\Desktop\Snipaste_2021-02-19_10-17-38.png)



![Snipaste_2021-02-19_10-20-50](C:\Users\49231\Desktop\Snipaste_2021-02-19_10-20-50.png)



### 实现原理

1. 将真实图片赋值到 img 标签的自定义属性中
2. 设置图片的默认图
3. 判断可视区域位置
4. 将自定义属性中的地址赋值到 img 标签的 src 属性中
5. 节流



```javascript
// html   
//  <div class="J_imgList">
//  	<img src="默认图片.png" class="list-img" data-src="真实地址">
//  	<img src="默认图片.png" class="list-img" data-src="真实地址">
// 	 	<img src="默认图片.png" class="list-img" data-src="真实地址">
//  	<img src="默认图片.png" class="list-img" data-src="真实地址">
//  	<img src="默认图片.png" class="list-img" data-src="真实地址">
//  	<img src="默认图片.png" class="list-img" data-src="真实地址">
//  	<img src="默认图片.png" class="list-img" data-src="真实地址">  
//  </div>

;(function (win, doc)) {
    var oImgs = doc.getElementsByClassName('list-img'); // 获取所有图片
	bindEvent();	// 绑定事件处理函数

    function bindEvent() {
      	// throttle 节流
        window.onload = window.onscroll = throttle(imgLazyLoad(oImgs), 500)
    }
	
	// 懒加载  
    function imgLazyLoad(image) {
		var imgLen = image.length,
             n = 0;   // 保存当前加载到的图片位置
      	var cHeight = document.documentElement.clientHeight,  // 可视区域高度
        	sTop = document.documentElement.scrollTop || document.body.scrollTop, // 滚动高度 兼容写法
        	imgItem;	// 声明图片每一项
      	
          for (var i = n; i < imgLen; i++) {
      		imgItem = image[i]; 
            // 元素高度出现在可视区域内
            if(imgItem.offsetTop < cHeight + sTop) {
              imgItem.src = imgItem.getAttribute('data-src')
              imgItem.removeAttribute('data-src')
              n++;
            }
      	}  
    }
})(window, document);


```



![Snipaste_2021-02-19_11-45-40](C:\Users\49231\Desktop\Snipaste_2021-02-19_11-45-40.png)

如图可见 当元素高度**小于**滚动高度与可视区域高度时 ，元素出现在浏览器可视区域内，因此加载图片，同时将 n++ 标识已结束此元素加载，下一次滚动条滚动事件处理时 i 从 n 开始执行，并不会影响之前的图片。



注意：

- n 通过闭包保存，并不会被系统回收。
- 节流操作是避免在滚动条高频触发事件处理函数导致data-src 已被清除但 n++ 尚未执行，响应未完全造成的页面报错。