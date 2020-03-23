- https://heybadsoul.github.io/notebook/
- https://www.cxymsg.com/
- ## vue列表渲染Key的作用
    Diff算法的作用是用来计算出 Virtual DOM 中被改变的部分，然后针对该部分进行原生DOM操作，而不用重新渲染整个页面。
-     提高diff算法的效率
## Tcp协议的三次握手
1. 客户端发送syn(序列号)到服务器，等待服务器确认（client => server）
2. 服务器接受到syn确认后将syn+ack（正确的信息）发送给客户端（server => client）
3. 客户端接受到信息 确认后发送确认包ack，两者真正建立了联系（client => server）
4. 四次握手就是中间多了一层等待服务器再一次响应回复相关数据的过程（关闭连接）
## vue中data为什么是函数（待补充）拷贝 （es6扩展运算是浅拷贝）
1. 引用类型
2. 深拷贝
2. 函数作用域
## 全局请求数据劫持
1. 请求地址携带token
2. 数据异常处理
## v-show与-v-for区别
1. v-show:消耗更低，css间切换适用于频繁切换使用
[区别](https://www.cnblogs.com/echolun/p/7803943.html)
## vue响应式原理

```
vue2：通过Object.defineProperty（自定义对象属性）给对象添加set，get属性，get部分收集数据依赖，数据改变时会触发set->更改对应的vdom，浅拷贝
vue3.0: proxy(代理)方式，深拷贝，不会更改原有的对象属性值
区别
    1：参数不同。
    2：深拷贝，浅拷贝
    3：对比2.0处理效率更高，2.0会遍历对象属性。（暂定原理还需继续理解）
    1）:数据转化为getter和setter
    2) :建立watcher并收集依赖
```
## vue3.0的diff优化
1.  不是在遍历所有的dom子节点进行对比，只对比动态数据的dom
2.  减少不需要的dom展示，提高页面渲染

## vue 动态路由加载
```
在vue-router对象中首先初始化公共路由，比如（404，login）等，然后在用户登陆成功，根据用户的角色信息，获取对应权限菜单信息menuList，并将后台返回的menuList转换成我们需要的router数据结构，然后通过vue-router2.2新添的router.addRouter(routes)方法，同时我们可以将转后的路由信息保存于vuex，这样我们可以在我们的SideBar组件中获取我们的全部路由信息，并且渲染我们的左侧菜单栏，让动态路由实现。

登录：当用户填写完账号和密码后向服务端验证是否正确，验证通过之后，服务端会返回一个token，拿到token之后（我会将这个token存贮到cookie中，保证刷新页面后能记住用户登录状态），前端会根据token再去拉取一个 user_info 的接口来获取用户的详细信息（如用户权限，用户名等等信息）。
权限验证：通过token获取用户对应的 role，动态根据用户的 role 算出其对应有权限的路由，通过router.addRoutes 动态挂载这些路由。

```
## vue项目优化
- keep-live
- cdn
## 浏览器输入url后发生了什么
- 域名解析->host文件->DNS
- 建立tcp链接。进过三次握手，连接服务器成功
- http请求发送数据包
- 服务器响应数据请求
- 关闭tcp连接
- 浏览器进行dom渲染
## vue生命周期
1. 先回创建vue实例（data,methods,computed）数据初始化
2. mounted解析模板，完成模板数据的渲染，挂载dom节点
3. updated 当数据发生改变是调用，模板数据重写替换，渲染
4. destroyed 实例销毁，清楚所有指令设事件。
```
数据初始化 ,vdom编译存放在内存当中,mounted将dom挂载完成，update更新数据，实例指令等摧毁
```
## 双向绑定
Object.defineProperty 通过 getter 和 setter 劫持了对象赋值的过程，在这个过程中可以进行更新 dom 操作等等

## 闭包
内部函数被外部函数变量引用 ，内部的函数称为闭包！

首先说明什么是闭包，闭包简单来说就是函数嵌套函数，内部函数引用来外部函数的变量，从而导致垃圾回收
机制没有把当前变量回收掉，这样的操作带来了内存泄漏的影响，当内存泄漏到一定程度会影响你的项目运行
变得卡顿等等问题。因此在项目中我们要尽量避免内存泄漏。
```javascript
function a(){
    var b = 0;
    return function(){
        b++;
        console.log(b);
    }
}
var d = a();
d();//1
d();//2
```
## 数组函数
```JavaScript
map
根据已有数组的每个元素生成一个长度相同的另一个元素。

[1, 2, 3].map(e => e * 2);	// [2, 4, 6]
reduce
遍历一个数组的所有元素与上一次遍历的结果，最终生成一个对象。

[1, 2, 3].reduce((acc = 0, e) => acc + e);	// 6
filter
根据条件过滤一个数组，将满足条件的元素存入一个新的数组中。

[1, 2, 3].filter(e => e % 2 === 1);	// [1, 3]   

```
## 设计模式（策略模式）
```javascript
<!----将业务规则写成函数，再讲目标参数与函数匹配进行运算，避免if的多重嵌套>
var calculateBouns = function(salary,level) {
    if(level === 'A') {
        return salary * 4;
    }
    if(level === 'B') {
        return salary * 3;
    }
    if(level === 'C') {
        return salary * 2;
    }
};
// 调用如下：
console.log(calculateBouns(4000,'A')); // 16000
console.log(calculateBouns(2500,'B')); // 7500
<!----策略模式写法>
var obj = {
        "A": function(salary) {
            return salary * 4;
        },
        "B" : function(salary) {
            return salary * 3;
        },
        "C" : function(salary) {
            return salary * 2;
        } 
};
var calculateBouns =function(level,salary) {
    return obj[level](salary);
};
console.log(calculateBouns('A',10000)); // 40000

```
## new运算
- 建立空对象
- 将实例的原型对象拷贝到新的原型链当中
- 改变this指向
## instanceof 原理
```javascript
检测实例的原型链是否和要比对的对象的原型对象中
var a = new Array();
console.log(a instanceof object)
```
## 前端路由分为 hash（带#号的，也成锚点），history
hashchange 事件

## 原型对象与原型链
[每个对象都可以有一个原型_proto_](https://www.jianshu.com/p/be7c95714586)，这个原型还可以有它自己的原型，以此类推，形成一个原型链。查找特定属性的时候，我们先去这个对象里去找，如果没有的话就去它的原型对象里面去，如果还是没有的话再去向原型对象的原型对象里去寻找...... 这个操作被委托在整个原型链上，这个就是我们说的原型链了。
### 继承
原型继承：父级的原型对象赋值给子级的实例，也就是对父级的引用
```JavaScript
原型继承
son.prototype = new father()
son.prototype.constructor = son;
改变this指向
Father.call(this)
-----------------------------------
function Person() {

}

Person.prototype.name = 'Kevin';

var person = new Person();

person.name = 'Daisy';
console.log(person.name) // Daisy

delete person.name;
console.log(person.name)
-----------------------------------
```
## router与route区别
router为全局的对象 
route为当前路由下的信息
```
vue路由传参
1:params 类似get方式，通过name匹配路由
2:query 类似post,由path匹配路由
```
## event loop
1：主线程执行完会执行任务队列当中的事件，过程之循环不断的
2：任务队列是存放事件的回调函数，先进先出

## webpack
[面试题](https://github.com/zhenzhencai/FontEndInterview/blob/master/topic/webpack.md)

## call,apply

```javascript
var a = 1
var obj1 = {
  a:2,
  fn:function(){
    console.log(this.a) //2
  }
}
obj1.fn.call(window,a) //1
```
**********
```javascript
原理实现
function func(){
console.log(this.name)
}
var a ={
  name : 'dudu'
}

Function.prototype.call = function(ctx){
var ctx = ctx || window //判断是否有参数 如果没有 赋予全局作用域
 
ctx.fn = this //将要调用的函数赋值给当前对象中
var array = [] //储存参数
for (i=1; i < arguments.length; i++){
array.push('arguments[' + i + ']')
}
//此处运用es6的语法
var result = ctx.fn(...array)
 
//es3的方法  var reslut = eval('ctx.fn(' + array + ')')  严格模式下不允许使用eval方法。
// delete ctx.fn
 
return result

}
func.call(a)
```
## 设备像素比
dpr 设备像素缩放比  dpr = dp/px
设备像素比  = 设备像素/逻辑像素(px)
## 函数节流 throttling
每隔一段时间去执行一次原本需要无时不刻地在执行的函数
```javascript
function throttle(fn, interval = 300) {
  let canRun = true;
  return function () {
    if (!canRun) return;
    canRun = false; // 300ms 之前则退出函数
    setTimeout(() => {
      fn.apply(this, arguments);
      canRun = true; // 设定 300ms 之后执行函数
    }, interval);
  };
}
```
场景：
- 滚动加载，加载更多或滚到底部监听
- 谷歌搜索框，搜索联想功能
- 高频点击提交，表单重复提交
## 函数防抖 debounce
在事件被触发n秒后再执行函数，如果在这n秒内又被触发，则重新计时
```javascript
function debounce(fn, interval = 300) {
  let timeout = null;
  return function () {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      fn.apply(this, arguments);
    }, interval);
  };
}
```
场景：
- 搜索框搜索输入。只需用户最后一次输入完，再发送请求
- 机号、邮箱验证输入检测
- 窗口大小Resize。只需窗口调整完成后，计算窗口大小。防止重复渲染。


## Git使用
- git branch name 创建分支
- git checkout name 进入分支
- git commit 提交当前分支
- git  merge dev ：dev分支的代码合并到master上
- git branch -d 删除分支
- git status 查看当前状态
- head指向当前分支。也可以进入其他分支
