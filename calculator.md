# Object.defineProperty 实现计算器

> 利用 object.defineProperty 的数据劫持实现简易的计算器



## 需求分析

1.  监听页面元素输入输出以及按钮的点击事件
2.  利用 object.defineProperty 的数据劫持
3.  改变视图中的数据展示





## HTML代码如下
<br />
```html
<style>
  .current {
    background-color: orangered;
    color: #fff;
  }
</style>

<body>
  <div class="result">0</div>

  <div class="ipt_group">
    <input type="text" class="fInput" value="0">
    <input type="text" class="sInput" value="0">
  </div>
  <div class="btn_group">
    <div class="btn_group">
      <button data-field="plus" class="current">+</button>
      <button data-field="minus">-</button>
      <button data-field="mul">x</button>
      <button data-field="div">/</button>
    </div>
  </div>
</body>
```

其中：

- result 为结果展示
- fInput 和 sInput 为输入第一个参数和第二个参数
- btn_group 下的 button 为四个按钮



## JS代码
<br />
```javascript
/*
 * @Author: 闲余
 * @Date: 2021-01-19 11:26:50
 * @LastEditTime: 2021-01-19 12:55:18
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \Calculator\calculator.js
 */
class Compute {
  plus (a, b) {
    return a + b
  }
  minus (a, b) {
    return a - b
  }
  mul (a, b) {
    return a * b
  }
  div (a, b) {
    return a / b
  }
}

class Calculator extends Compute {
  constructor(doc) {
    super()
    // 获取 dom 节点
    const iptGroup = doc.getElementsByClassName('ipt_group')[0]
    // 获取结果显示位置
    this.oResult = doc.getElementsByClassName('result')[0]
    // 获取输入框
    this.fInput = iptGroup.getElementsByTagName('input')[0]
    this.sInput = iptGroup.getElementsByTagName('input')[1]
    // 获取按钮
    this.oBtnGroup = doc.getElementsByClassName('btn_group')[0]
    this.oBtnItems = this.oBtnGroup.getElementsByTagName('button')
	// 标识点击哪一个按钮  后续设置 current 时使用
    this.btnIndex = 0

    // this.data = {
    //  fNumber: ''
    //  sNumber: ''
    //  field: ''
    // }
    this.data = this.defineData()
	// 初始化 绑定事件
    this.init()
  }
  init () {
   // 绑定 this 使 this 指向对象内部
    this.oBtnGroup.addEventListener('click', this.onBtnClick.bind(this), false)
	// 输入框输入事件
    this.fInput.addEventListener('input', this.onNumberInput.bind(this), false) 
    this.sInput.addEventListener('input', this.onNumberInput.bind(this), false)
  }

  // 声明数据
  defineData () {
    // 声明初始数据
    let _obj = {},
      fNumber = 0,
      sNumber = 0,
      field = 'plus'
    const self = this
    // 我们需要得到 this.data = {fNumber, sNUmber, field}
    Object.defineProperties(_obj, {
      fNumber: {
        get () {
          // 访问时 直接返回局部作用域中的数据即可
          console.log('get value :' + fNumber)
          return fNumber
        },
        set (newVal) {
          console.log(this)
          console.log('set new value :' + newVal)
		  // 为局部作用域赋值
          fNumber = newVal
          // 当值改变的时候应该调用计算方法 返回计算结果出去
          self.computeResult(fNumber, sNumber, field)
        }
      },
      sNumber: {
        get () {
          // 访问时 直接返回局部作用域中的数据即可
          console.log('get value :' + sNumber)
          return sNumber
        },
        set (newVal) {
          console.log('set new value :' + newVal)
		  // 为局部作用域赋值
          sNumber = newVal
          self.computeResult(fNumber, sNumber, field)
        }
      },
      field: {
        get () {
          // 访问时 直接返回局部作用域中的数据即可
          console.log('get value :' + field)
          return field
        },
        set (newVal) {
          console.log('set new value :' + newVal)
		  // 为局部作用域赋值
          field = newVal
          self.computeResult(fNumber, sNumber, field)
        }
      }
    })

    // 返回新对象
    return _obj
  }

  computeResult (fNumber, sNumber, field) {
    this.oResult.innerText = this[field](fNumber, sNumber)
  }

  onBtnClick (ev) {
    const e = ev || window.event, // 兼容 ie
      tar = e.target || e.srcElement, // 兼容火狐和ie
      targetName = tar.tagName.toLowerCase()
    targetName === 'button' && this.fieldUpdate(tar) // 更新field
    // console.log(this)
  }

  fieldUpdate (target) {
    // console.log(target)
	// 将原有的 current 去掉
    this.oBtnItems[this.btnIndex].className = ''
	// 返回被点击的索引
    this.btnIndex = [].indexOf.call(this.oBtnItems, target)
	// 返回被点击的索引
    // this.btnIndex = [...this.oBtnItems].indexOf(target)
	// 设置点击状态
    this.oBtnItems[this.btnIndex].className += ' current'
    // 修改 this.data 中的数据 执行 set 方法
    this.data.field = target.getAttribute('data-field')
  }

  onNumberInput (ev) {
    const e = ev || window.event,
      tar = e.target || e.srcElement,
	  // 获取输入框类名
      className = tar.className,
	  // 获取输入框的值 去空格 转化为 Number 类型   如果 NaN 则为 0
      val = Number(tar.value.replace(/s+/g, '')) || 0   

    switch (className) {
      case 'fInput':
        this.data.fNumber = val
        break
      case 'sInput':
        this.data.sNumber = val
        break
      default:
        break
    }
  }
}

// 实例化对象 传入 document 方便获取页面节点
new Calculator(document)
```



## 实现原理

**1、获取DOM节点**

- 实例化对象时	传入 document 对象
- 在构造器中将使用到的节点存储在私有属性中



**2、声明初始化数据**

- 为什么要声明 this.data 并且接受一个返回值?
  - 点击按钮以及两个输入框绑定事件，改变this.data 中属性的值
  - 使用 Object.defineProperty 监听 this.data 中属性的变化
  - this.data 中属性的变化在 set 时执行计算操作，更改页面数据



**3、defineData 方法中的初始化数据**

- 定义了几个局部变量，用来存储和访问数据
  - 当 修改 this.data 属性时,执行 set 方法，为相应的数据赋值
  - 调用 computeResult 方法将值传入
  - 第一次进入时 默认输入数值为 0 操作符为 plus
- 将声明的 _obj 返回给构造器中的 this.data 此时 this.data 中的数据被监听

**4、数据是如何改变的**

- 首先在构造器中调用了初始化方法 init()
  - init 方法为按钮和输入框绑定了不同事件
- 按钮点击事件
  - 判断是否点击了 button 标签
  - 将 this.btnIndex 索引对应的标签类名修改为空
  - 查找当前点击的索引值为 this.btnIndex 赋值
  - 修改当前点击的类名为 current
  - 获取被惦记的元素的自定义属性 将属性值赋值给 this.data.field
- 输入事件
  - 判断输入的是第一个值还是第二个值
  - 将输入框的 value 赋值给 this.data 中对应的 fNumber 或 sNumber
- 监听
  - 监听发生在赋值的过程中
  - 赋值时首先执行 set 将局部变量改变，调用 computeResult 将改编后的三个值传入

**4、Compute 类**

- 计算方法类
  - 使用四个按钮的自定义属性值作为方法名
  - 返回相应的计算结果
  - Calculator 类继承 并且执行 super()
- ComputeResult
  - 在 Calculator 类下的 ComputeResult 方法中直接调用父类的方法
  - 使用 this[field] 利用属性访问的方式执行相应的方法
  - 将返回值赋值给页面中的 result



## 总结

逻辑较为清晰简单，重要的是利用 Object.defineProperty 实现数据的劫持，从而达到简单的数据双向绑定



【😊 转载请注明出处链接】