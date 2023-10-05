# js Date

```javascript
var now. = new Date();
now.getFullYear(); //n年
now.getMonth（）//月 0-11
now.getDate // 日
now.getHours() //时
now.getDay() //星期几
now.getMinutes() //分
now.getSeconds() //秒
now.getTime() // 时间戳 
console.log(new Date(时间戳)) //时间戳转为时间
now.toLocaleString()  //注意，调用是一个方法，不是一个属性
now.toGMTString()
```

# json

```javascript
var user = {
	name:"qinjiang",
  age:3,
  sex:'男'
}

//对象转化为json字符串
var jsonUser = JSON.stringify(user0)

//json字符串转化为对象
var obj = JSON.parse('{"name":"qinjiang","age":3,"sex":"男"}')

//很多人搞不清楚，json和js对象的区别
var obj = {a:'hello',b:'hellob'};
var json = '{"a":"hello","b":"hellob"}';
```

# Ajax

- 原生的js写法  xhr异步请求

- jQuey封装好的方法 $("#name").ajax("")

- Axis 请求

  

  # 面向对象编程 

  类：模版

  对象：具体的实例

  

  在javascript中这个需要大家换一下思维

  原型：

  ```javascript
  var user = {
    name：“qinjiang”,
    age :3,
    run :function() {
      console.log(this.name + "run....")
    }
  };
  
  var xiaoming = {
    name:"xiaoming"
  };
  xiaoming._proto_ = user;
  
  var Bird = {
    fly: function(){
      console.log(this.name + "fly....")
    }
  }
  
  xiaoming._proto_ = Bird;
  xiaoming.run //这个会报错，因为没有
  
  xiaoming._proto_ = user;
  xiaoming.run //这个时候小明可以run了
  xiaoming.age //小明的原型是user 
  
  ```

  

# class继承

class关键字，是在es6引入的

```javascript
function Student(name){
  this.name = name;
}

//给student新增一个方法
Student.proto.hello = function(){
  alter("hello")
}

//es6之后============
//定义一个学生类
class Student{
  constructor(name){
    this.name = name;
  }
  hello(){
    alter("hello")
  }
}

var xiaoming = new Student("xiaoming")
xiaoming.name
xiaoming.hello()
```



2.继承

````javascript
class Student{
  constructor(name){
    this.name = name;
  }
  hello(){
    alter("hello")
  }
}


class xiaoStudent extends Student{
  constructor(name,grade){
    super(name);
    this.grade = grade;
  }
  myGrade(){
    alter('我是一名小学生')
  }
}

var xiaoming = new Student("xiaoming");
var xiaohong = new Student("xiaohong",1)
````



```markdown
原型链
```

# 操作Bom(重点)

> 浏览器介绍
>
> Javascript 和浏览器的关系

javascript诞生就是为了能够让他在浏览器中运行！

B：浏览器对象模型

- IE
- chrome
- Safari
- FireFox
- Opera

三方

- QQ浏览器
- 360浏览器



> window（重要）

window代表浏览器的窗口

```javascript
window.alter(1)
undefined
window.innerHeight
windown.outerwidth

```

> Navigator（不建议使用）

Navigator,封装了浏览器的信息

```javascript
window.Navigator.appName
navigator.appName
navigator.appVersion
navigator.userAgent
navigator.platform
win32
```

大多时候，我们不会使用`navigator` 对象·，因为会被人人为修改！

不建议使用这些属性来编写代码



> Screen

```javascript
screen.width
screen.height
```



> location(重要)

location 代表当前页面的URL信息

```javascript
host:"www.baidu.com"
href:"https://www.baidu.com/"
protocol:"https:"
reload:f reload() //刷新网页
//设置新的地址
location.assign（'https://blog.kuangstudy.com/'）
```



> Document

document。代表当前页面 html dom文档数

```javascript
document.title
"百度一下，你就知道"
document.title = '狂神说'
'狂神说'
```



获取具体的文档树节点

```javascript
<dl id="app">
  <dt>Java</dt>
	<dt>JavaEE</dt>
	<dt>JavaSE</dt>
</dl>

<script>
  var dl = document.getElementById('app');
</script>
```

获取cookie

```javascript
document.cookie
```

劫持cookie原理

www.taobao.com

```html
<script src="aa.js"></script>>
<!--恶意人员：获取你的cookie上传到他的服务器-->
```

服务器端可以设置 cookie:httpOnly



> history（不建议使用）

```javascript
history.back //后退
history.forward() //前进
```

# 操作DOM对象(重要)

DOM:文档对象模型，

> 核心

浏览器网页就是一个DOM属性结构

- 更新Dom节点
- 遍历DOM节点：得到DOM节点
- 删除：删除一个DOM节点
- 添加：添加一个新的节点

要操作一个DOM节点，就必须要获得这个DOM节点

```javascript
```

