___
title: JavaScript第一次小姐
toc: true
tags: -JavaScript
-学习笔记
-编程
calories: -JavaScript

## 一

## 、数据类型：

### 1.在JS中共有六种数据类型

1. ```javascript
   /* 在JS中有6中类型
    *        String
    *        Number
    *        Boolean
    *        Null  空值
    *        Undefined   未定义
    *        Object   对象（引用数据类型）
    */
    let str = '123';//string
    let num = 123;//number
    let boo = true;//boolean
    let a = null;//null
    let b;//undefined
   /*
    *可以创建一个变量而给他赋值null此时并未指向任何对象并且布尔值为0，
    *如果创建一个对象只是声明它并不给它赋值则它的数据类型为undefined那么它并不等同于null比如要比较两个变量那么就要考虑到是否会出现null和undefined的比较如果需要区分它们那么就需要使用严格比较符(===)以及严格不等符(!==)，否则使用普通的比较符
   ```

### 2.显示数据类型强制转换

#### 2.1数值转换

1. ```javascript
   //使用number()函数
   let str = '213';
   num = number(str);//num=213
   //typeof num =number
   str = '12px';
   num = number(str);//NaN
   num = number(null);//0
   num = number(false);//0 true = 1
   ```

2. ```javascript
   //使用parseInt() or parsefloat()
   //对于含有非数值类型的字符串可以转换
   //但是仅限于字符串开头是数字的的字符串
   //对于字符串开头不是数字的来说还是会返回NaN
   let str = '123';
   num = parseInt(str);//num=123,但是底层调用的还是number()函数
   num = parseInt('123.12e');//对于parseInt()返回的123，
   num = parsefloat('123.12e');//此时返回的是123.12
   ```

3. ```javascript
   /*
       *  parseInt(021, 8) return 15??;
       *  parseInt(15.99, 10)return 15??;
       * */
       let str = "015";
       str = parseInt(0o21, 8);//=parseInt("17",8);//parseInt(x,base)转换为不同进制的数字
       //let num = String(0o21);
       num = 0o21.toString();
       /*
           首先将八进制数字0o21通过toString()转换为字符串，而函数toString()对于纯数字的转换应当是
           按照十进制转换所以0o21(8)应该是17(10)，这个17(10)就会转换为'17'字符串，
           再通过parseInt()转换是指定的八进制所以就是17(8)=15(10)
       */
   ```

#### 2.2字符串转换

1. ```javascript
   //使用.toString()将非字符串数据转换为字符串数据
   /*调用被转换数据类型的toString()方法
       *       -不改变a的属性只是将转换的结果作为返回值
       *       -null undefined 没有toString()方法
    */
   let a =1;//number
   a = a.toString();//a='1'
2. ```javascript
   //与number数据类型转换一样使用string()
   //就是.toString 加上了 null和undefined的转换
   //直接输出字符串'null' 和 'undefined'
   let a = null;
   a = string(a);//a='null'
   let b;
   b = string(b);//b='undefined'
   ```
### 3.隐式数据类型强制转换(其实底层调用的依旧是显示转换的方法和函数)

1. ```javascript
   /*
    * 对于非数值类型的数据可以使用一些算数运算符和逻辑运
    * 算符进行数据类型转换，经常会需要将string和boolean
    * 类型的数据进行转换
    * 一元运算符，只要一个操作数
    *  + 正号  隐式数据类型转换 将任何非Number转换为Number类型数据
    *   我认为该方法底层依旧是调用了函数Number()
    *   函数Number()以及parseInt()
    *   包括函数parseInt()对于纯数字的字符串也是使用的函数Number();
    *  - 负号  对Number数据取反
    *         对非Number数据 先转换为NUmber再取反运算 比如Boolean数据
    *         纯数字的String*/
    let a = '123';
    a= +a;//a=123
    a = '123.12';
    a= +a//a=123.12
2. ```javascript
   /* 对于非string 的数据类型转换
    * 任意数据类型加上一个空串不能有空格
    * 或者加上null 转换为字符串
    * 是一种隐式转换
    * 实际上也是调用String()
    * /
    let a = 123;
    a = a + '';//a = a + null;
   ```
### 4.逻辑运算符
1. ```javascript
   // 逻辑与运算 &&
   // 逻辑或运算 ||
   // 逻辑非运算 !
   // 基础操作比如
   let a = true && false;
   a = false || 0;
   //都会将非布尔类型的数据转换为布尔数据
   //1 => true
   //0,null => false
   //任何与NaN进行逻辑运算都是NaN
   /*
    * ! 非
    *  -可以对一个值进行非运算
    *  -两次取反值不会变化
    *  -对非布尔值进行运算会转换为布尔值再取反运算
    * && 与
    * || 或*/
    let a = -10;
    let b = 0;
    let c = !!a && !!b;
    //c = true || alert("haello");//no alert
    //c = false || alert("hello");//alert
    //c = true && alert("hello");//alert
    c = false && alert("hello");//no alert
    //console.log("c = " + c);
    /*
    * && || 对于非布尔值的运算结果为布尔值但是返回的结果是原数据类型
    * */
    var result = 1 && 2;//return 2;
    result = 1 && 0;//return 2;
    result = 1 && undefined;
    //result = 1 || 2;//return 1;
    /*
    * 对于&& || 来说去判断该表达式的值返回的是
    * 判断结束时的最后判断的数据
    * && 判断到false就结束所以返回false的值，如果第一个
    * 值是true 则去判断第二个值所以返回的是第二个值*/
    console.log(result);
    /*
    * 可以利用&& 和 || 来表述一些逻辑关系的先后顺序
    * 通过这些逻辑运算符来直接关联但逻辑内容爆炸
    */
