---
layout: default
title: Function类型
---

## {{ page.title }}
{{ page.date | date_to_string }}
> 函数实际上是对象。每个函数都是Function类型的实例，而且都与其他引用类
> 型一样具有属性和方法。由于函数是对象，因此函数名实际上也是一个指向函
> 数对象的指针，不会与某个函数绑定。    

#### 函数定义方法
- 函数声明
```javascript
    function sum(num1, num2){
        return num1 + num2;
    }
```
- 函数表达式
```javascript
   var sum = function(num1, num2){
        return num1 + num2;
   }; 
```
    函数表达式定义函数时，没有必要使用函数名--通过变量sum即可以引用函数，另外，还要注意函数末尾有一个分号，
    就像声明其他变量时一样。
- 构造函数--Function构造函数    
    Function构造函数可以接受任意数量的参数，单最后一个参数始终都被看成是函数体，而前面的参数则枚举出了新函数的参数
```javascript
    var sum = new Function('num1','num2','return num1 + num2'); //不推荐
```
    从技术角度讲，这是一个函数表达式。但是，我们不推荐使用这种方法定义函数，因为
    这种语法会导致解析两次代码（第一个是解析常规ECMAScript代码，第二次是解析传入构造函数中的字符串），
    从而影响性能。这种语法利于理解“函数是对象，函数名是指针”的概念。

> 函数名仅仅是指向函数的指针，因此函数名与包含对象的其他变量没有什么不同，换句话说，
> 一个函数可能会有多个名字。

```javascript
    function sum(num1, num2){
        return num1 + num2;
    }
    console.log(sum(10, 10));  //20

    var anotherSum = sum;
    console.log(anotherSum(10, 20)); //30

    sum = null;
    console.log(anotherSum(20, 20)); //40 
```
    以上代码定义了一个名为sum()的函数，用于求两个值的和。然后，又声明了变量anotherSum,将sum赋值给anotherSum。
    *注意，使用不带圆括号的函数名是访问函数指针，而非调用函数。*此时anotherSum和sum就都指向了同一个函数，
    因此anotherSum()也可以被调用并返回结果。即使将sum设置为null，让它与函数“断绝关系”，但仍然可以正常调用anotherSum()

#### 没有重载（深入理解）

    将函数名想象为指针，也有助于理解为什么ECMAScript中没有函数重载的概念。
- ECMAScript 3的例子
```javascript
    function addSomeNumber(num){
        return num + 100;
    }

    function addSomeNumber(num){
        return num + 200;
    }
    
    var result = addSomeNumber(100); //300
```

> 上述声明了两个同名函数，而结果则是后面的函数覆盖了前面的函数。
> 以上代码其实跟下面的代码没有声明区别

```
    var addSomeNumber = function(num){
        return num + 100;
    }
    addSomeNumber = function(mum){
        return num + 200;
    }
    var result = addSomeNumber(100); //300
```
