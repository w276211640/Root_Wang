---
title: JavaScript第二次小姐
toc: true
tags: 
- JavaScript
- 学习笔记
- 编程
categories: JavaScript
---

## 一、作用域
### 1、全局作用域

  作用域指的是一个变量的作用范围，在JS中有两种作用域
  1.全局作用域
    -直接编写在scrip标签里的JS代码就是在全局作用
    -在页面打开时创建，在页面关闭时销毁
    -在全局作用域中有一个全局对象window，代表的是浏览器的窗口可以直接使用
    -在全局创建的变量都会作为window对象的属性保存创建的函数都作为window的方法保存

1.1.1

```javascript
var a =10;//保存在window的对象属性中
console.log(window.a);//10

function fun(){
	
}
window.fun();
//变量声明提前
// 	-使用关键字var 声明变量，会在所有代码执行前执行
//   但是只会提前声明为变量并不会赋值 所以会是undefined
//   如果不使用关键字var 不会提前
console.log("a = "+a);
var a =10;//会提前声明并赋值
console.log("b = "+b);
var b;//会提前声明但是不会赋值所以是undefined

//函数的提前声明
fun();//可以正常调用
fun2();//函数未被初始化 错误
//函数声明形式创建函数
//会提前声明并初始化可以直接使用
function fun1(){

}

//函数表达式形式创建函数
//函数会提前声明
//但是不会初始化所以不能在声明之前调用
 var fun2 = function(){
 
 }
```
### 2、函数作用域

	-函数在调用时创建函数作用域，函数执行完毕后函数作用域销毁，所以作用域内的变量会销毁
	-每调用一次函数都会创建一次函数作用域每次作用域相互独立
	-函数作用域内可以访问全局变量，可以使用window对象来访问全局变量
	在函数内部使用变量时会寻找变量的声明优先在函数的内部，也就是函数定义域内部寻找，如果没有，就去构造函数的原型链上去寻找变量，这时会一直寻找到Object上的null那么最后才会在全局作用域中寻找，也就是window的属性，所以这个寻找顺序也就是变量的优先顺序，比如在原型链上定义了一个方法，但同时在函数内部也有一个在函数定义域的一个属性名一样，那么就会优先使用这个属性所以，下面的例子里有一个会提示我该函数方法不是一个function 是因为我在函数定义了同名的属性
	在构造函数里的属性名和方法名使用this.来命名，这样对于使用构造函数new出来的对象会自动连接到这个this从而导致指向的对象是这个创立的新对象
1.2.1

```javascript
function fun(){
	console.log(this.name);
}
var name = "13"
//== window.name = "13"
fun();//"13"
//使用对象来调用全局作用域的函数
var obj1 = {
  name : "123",
  sayName : fun
}
obj1.sayName();//"123"
```

## 二、对象
### 1.对象分类

#### 1.1 内建对象

	-由ES标准中定义的对象，在任何ES的实现中都可以使用
	-比如：Math String number Boolean Function Object
#### 1.2 宿主对象

	-主要指浏览器提高的对象
	-比如BOM DOM
#### 1.3 自定义对象

	-有开发人员自己创建的对象
2.1.1

```javascript
console.log();//BOM对象
var obj = new Object();
obj.name = "bob";
obj.age = 12;
//对象的属性值也可以是函数
//那么调用该对象的属性函数时称之为
//使用该对象的方法
obj.sayName = function(){
		console.log(this.name);
		//this.name这里代替的时该对象的属性name 
}
obj.sayName();//调用改对象的方法
```
	对于需要大量数量生产对象的时候如果将对象的批量的使用上面的方法生产就会很麻烦，那么对于同一类的对象，只是他们的参数值不一样，对于javascript来说没有类class只有原型继承模型，那么可以考虑将其写成一个函数，生产对象时就往函数里扔实参，然后返回一个对象，由于只是实参不一样所以此方法可行

2.1.2
```javascript
function Person(name, age, gender){//函数名字第一个字母必需大写
		this.name = name;
		this.age = age;
		this.gender = gender;
		this.sayName = function (){
			console.log("i am "+ this.name);
		}
}

var per = new Person("bob",23,"man");//i am bob
//实际上就是构造函数，但是这里对象的方法每次创建对象时都会
//创建一个新的函数，所以占资源
```
## 三、原型

	我们每次创建的函数解析器都会在函数中添加一个原型属性 prototype 每个函数都有自己的原型对象 当函数作为普通函数调用时，这个原型对象没有任何作用，只有在函数作为构造函数时，才会有原型对象的作用。
	创建的对象比如
3.1.1
```javascript
function Student(name, age, gender){
	this.name = name;
	this.age = age;
	this.gender = gender;
}
//构造函数存在一个公共区域通过使用
// ---Student.prototype来访问，这是这个公共区域就是原
//型对象的属性 那么通过该构造函数创立的新的对象都可以访问
//到这个公共区域，这也就解决了每次通过创立对象时需要创建一
//个新的函数
function sayName(){
	return "i am "+ this.name;
}
//这种创建方法占用了全局作用域的命名空间，污染了全局作用域
//所以要使得对象的方法既不能每次都创建大量相同的函数
//又不能使用全局作用域导致污染命名空间
//所以使用原型对象 将每次创建对象的公共方法放在这里所以同
//一构造函数创建的对象都可以使用该方法

Student.prototype.sayName = function(){
	return "i am "+this.name;
}
var stu1 = new Student();
console.log(stu1.sayName());
```
请假系统上一个随机人名和学院生成的函数实例
3.1.2

```javascript
function Name(){
  this.FirstName = [
    '赵','钱','孙','李','周','吴','郑','王','冯','陈',
    '褚','卫','蒋','沈','韩','杨','朱','秦','尤','许',
    '何','吕','施','张','孔','曹','严','华','金','魏',
    '陶','姜','戚','谢','邹','喻','柏','水','窦','章',
    '云','苏','潘','葛','奚','范','彭','郎','鲁','韦',
    '昌','马','苗','凤','花','方','俞','任','袁','柳',
    '酆','鲍','史','唐','费','廉','岑','薛','雷','贺',
    '倪','汤','滕','殷','罗','毕','郝','邬','安','常',
    '乐','于','时','傅','皮','卞','齐','康','伍','余',
    '元','卜','顾','孟','平','黄','和','穆','萧','尹',
    '姚','邵','湛','汪','祁','毛','禹','狄','米','贝',
    '明','臧','计','伏','成','戴','谈','宋','茅','庞',
    '熊','纪','舒','屈','项','祝','董','粱','杜','阮',
    '蓝','闵','席','季','麻','强','贾','路','娄','危',
    '江','童','颜','郭','梅','盛','林','刁','钟','徐',
    '邱','骆','高','夏','蔡','田','樊','胡','凌','霍',
    '虞','万','支','柯','咎','管','卢','莫','经','房',
    '裘','缪','干','解','应','宗','宣','丁','贲','邓',
    '郁','单','杭','洪','包','诸','左','石','崔','吉',
    '钮','龚','程','嵇','邢','滑','裴','陆','荣','翁',
    '荀','羊','於','惠','甄','麴','加','封','芮','羿',
    '储','汲','邴','糜','松','井','段','富','巫','乌',
    '焦','巴','弓','牧','隗','山','谷','车','侯','宓',
    '蓬','全','郗','班','仰','秋','仲','伊','宫','宁',
    '仇','栾','暴','甘','钭','厉','戎','祖','武','符',
    '刘','景','詹','束','龙','叶','幸','司','韶','郜',
    '黎','蓟','薄','印','宿','白','怀','蒲','台','从',
    '鄂','索','咸','籍','赖','卓','蔺','屠','胥','能',
    '苍','双','闻','莘','党','翟','谭','贡','劳','逄',
    '姬','申','扶','堵','冉','宰','郦','雍','郤','璩',
    '桑','桂','濮','牛','寿','通','边','扈','燕','冀',
    '郏','浦','尚','农','温','别','庄','晏','柴','瞿'
  ]
  this.LastName = [
    "涛","昌","进",'林','有','坚','和','彪','博','诚','先','敬','震',
    '振','壮','会','群','豪','心','邦','承','乐','绍','功','松','善',
    '厚','庆','磊','民','友','裕','河','哲','江','超','浩','亮','政',
    '亨','奇','固','之','轮','翰','朗','伯','宏','言','若','鸣','朋',
    '斌','梁','栋','维','启','克','伦','翔',"旭",'鹏','泽','晨','辰',
    '士','以','建','家','致','树','炎',"德",'行','时','泰','盛','雄',
    '冠','策','腾','伟','刚','勇','毅','俊','峰','强','军','平','保',
    '东','文','辉','力','明','永','健','世','广','志','义','兴','良',
    '海','山','仁','宁',"贵",'福','生','龙','元','全','国','胜','学',
    '祥','才','发','成','康','光','天','达','安',"岩",'中',"茂",'武',
    '新','利','清','飞','彬','富','顺','信','子','杰','楠','榕','风',
    '航','弘'
  ]
  this.AcademyName = [
    '教育实验学院',
    '电子信息学院',
    '计算机学院',
    '网络安全',
    '航空学院',
    '航天学院',
    '航海学院',
    '材料学院' ,
    '机电学院',
    '力学与土木建筑学院',
    '动力与能源学院',
    '电子信息学院',
    '自动化学院',
    '数学与统计学院',
    '物理科学与技术学院',
    '化学与化工学院',
    '管理学院',
    '公共政策与管理学院',
    '软件学院',
    '生命学院',
    '外国语学院',
    '微电子学院',
    '网络空间安全学院',
    '生态环境学院'
  ]
  this.date = new Date();
  this.num1 = this.date.getHours();
  this.num5 = this.date.getMinutes()
  this.num2 = Math.round(this.num5*this.num1)%this.FirstName.length;
  this.num3 = Math.round(this.num5*this.num1)%this.LastName.length;
  this.num4 = Math.round(this.num2*this.num1)%this.LastName.length;
  this.num6 = Math.round(Math.random()*this.num1)%this.AcademyName.length;
  /*this.FirstName = function(){
    return FirstName[this.num2];
    // return this.num2;
  };
  this.LastName1 = function(){
    return LastName[this.num3];
    // return this.num2;
  };
  this.LastName2 = function(){
    return LastName[this.num4];
    // return this.num2;
  }
  this.DisplayFull = function(ele){
    ele.innerHTML = this.FirstName()+this.LastName1()+this.LastName2();
  }
  this.DisplayHalf = function(ele){
    ele.innerHTML = this.LastName1()+this.LastName2();
  }
  this.DisplayAcademe = function (ele){
    ele.innerHTML = AcademyName[this.num6];
  }*/
}
Name.prototype.FullName = function (){
  return this.FirstName[this.num2]+this.LastName[this.num3]+this.LastName[this.num4];
}

Name.prototype.HalfName = function (){
  return this.LastName[this.num3]+this.LastName[this.num4];
}
/*Name.prototype.AcademyName =function (){
  return this.AcademyName[this.num6];
}*/
//对于重复的命名会有优先选择构造函数的属性名已经存在 所以会提示name.AcademyName is not a function
//应该重新命名原型链上的函数
Name.prototype.Academy = function (){
  return this.AcademyName[this.num6];
}
var name = new Name();
console.log(name.FullName());
console.log(name.HalfName());
console.log(name.Academy());
```