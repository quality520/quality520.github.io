---
layout: post
title: Function类型
categories: [general, demo, sample]
tags: [javaScript, No.10, function, blogs]
<!-- fullview: false -->
---

<!-- ## {{ page.title }} -->
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

```javascript
    var addSomeNumber = function(num){
        return num + 100;
    };
    addSomeNumber = function(mum){
        return num + 200;
    };
    var result = addSomeNumber(100); //300
```

> 上述代码是怎么回事？在创建第二个函数时，实际上覆盖了引用第一个函数的变量addSomeNumber

#### 函数声明与函数表达式
> 解析器在向执行环境中加载数据时，会率先读取函数声明，并使其在执行任何代码之前可用；
> 至于函数表达式，则必须等到解析器执行到所在的代码行，才会真正被解析执行。

```javascript
    console.log(sum(10, 10));  //=>20;
    function sum(num1, num2){
        return num1 + num2;
    }
```

    上述代码执行之前，解析器就已经开始同一个名为函数声明提升（function declaration hoisting）
    的过程，读取并将函数声明添加到执行环境中。
    对代码求值时，JavaScript引擎在第一遍会声明函数并将它们放到源代码树的顶部。
    所以，即使声明函数的代码在调用它的代码后面，JavaScript引擎也能把函数声明提升到顶部。
```javascript
    console.log(sum(10, 20));
    var sum = function(num1, num2){
        return num1 + num2;
    }
```
    上述代码会报错，原因在与函数位于一个初始化语句中，而不是一个函数声明。换句
    话说，在执行到函数所在的语句之前，变量sum中不会保存有对函数的引用；而且，由于第一行代码就会导致“unexpected 
    identifier”（意外标识符）错误，实际上也不会
    执行到下一行。
    除了什么时候可以通过变量访问函数这一点却别之外，函数声明与函数表达式的语法其实是等价的。

#### 作为值的函数
    ECMAScript中的函数本身就是变量，所以函数也可以作为值来使用。
    可以像参数一样吧一个函数传递给另一个函数，而且还可以将一个函数作为另一个函数的结果返回。

```javascript
    function callSomeFunction(someFunction, someArgument){
        return someFunction(someArgument);
    }
```

> 上述函数接受两个参数。第一个参数应该是一个函数，第二个参数应该是要传递给函数的一个值。

```javascript
    function add10(num){
        return num + 10;
    }

    var result1 = callSomeFunction(add10, 10);

    console.log(result1); //=>20;

    function getGreeting(name){
        return "Hello," + name;
    }

    var result2 = callSomeFunction(getGreeting, 'white');
    console.log(result2); //=>"Hello, white"
```

    这里的callSomeFunction()函数是通用的，
    要访问函数的指针而不执行函数的话，必须去掉函数名后面的那对圆括号"()"。
    因此callSomeFunction()的是add10和getGreeting，而不是执行他们之后的结果。

    当然，可以从一个函数中返回另一个函数，而且这也是极为有用的一种技术。
    例如：
        假设有一个对象数组，我们想要根据某个对象属性对数组进行排序。而传递给数组sort()方法的
        比较函数要接收两个参数，即要比较的值。可是我们需要一种方式来指明按照那个属性来进行排序。
        要解决这个问题，可以定义一个函数，它接收一个属性名，然后根据这个属性名来穿件一个比较函数，
    下面就是这个函数的定义：

```javascript
    function createComparisonFunction(propertyName){
        return function(object1, object2){
            var value1 = object1[propertyName];
            var value2 = object2[propertyName];

            if(value1 < value2){
                return -1;
            } else if(value1 > value2){
                return 1;
            } else{
                return 0;
            }
        };
    }
```

    上述函数中嵌套了一个函数，而且内部函数前面加了一个return操作符。在内部函数
    接受到propertyName参数后，它会使用方括号表示法来取得给定属性的值。取得了想
    要的属性值之后，定义比较函数就非常简单了。
    调用方式：

```javascript
    var data = [{name: "Zachary", age: 28}, {name: "Nicholas", age: 29}];

    data.sort(createComparisonFunction("name"));
    console.log(data[0].name); //=>"Nicholas"

    data.sort(createComparisonFunction("age"));
    console.log(data[0].name); //=>"Zachary"
```

#### 函数内部属性
>在函数内部，有两个特殊对象：**arguments**和**this**，其中，arguments，是一个类数组对象包含这传入函数中的所有参数。虽然arguments的主要用途是保存函数参数，单这个对象还有一个名叫callee的属性，改属性是一个指针，指向拥有这个arguments对象的函数。
请查看下面的阶乘函数

```javascript
    function factorial(num){
        if(num <= 1){
            return 1;
        } else{
            return num * factorial(num - 1);
        }
    }
```

> 定义阶乘函数一般都是用到递归算法；如上面的代码所示，在函数有名字，而且名字以后也不会变得情况下，这样定义没有问题。但问题是这个函数的执行与函数名factorical紧紧耦合在一起。为了消除这种紧密的耦合现象，可以使用下面这样使用arguments.callee.

```javascript
    function factorial(num){
        if(num <= 1){
            return 1;
        } else {
            return num * arguments.callee(num - 1);
        }
    }
```

> 这个重写后的factorial()函数的函数体内，没有再引用函数名factorial。这样，无论引用函数时使用的是什么名字，都可以保证正常完成递归调用。
> 例如：

```javascript
    var trueFactorial = factorial;

    factorial = function(){
        return 0;
    }
    console.log(trueFactorial(15)); //=>120
    console.log(factorial(5)); //=>5
```

> 变量trueFactorial获得factorial的值，实际上是在另一个位置上保存了一个函数的指针，我们又将一个简单返回0的函数赋值给factorial变量。如果像原来的factorial()那样不适用arguments.callee，调用trueFactorial()就会返回0。可是，在解除了函数体内的代码与函数名的耦合状态之后，trueFactorial()仍然能够正常地计算阶乘；至于factorial()，它现在只是一个返回0的函数。

> 函数内部的另一个特殊对象是this，this引用的二十函数据以执行的环境对象--或者也可以说是this值(当在网页的全局作用域中调用函数时，tihs对象引用的就是window)。

```javascript
    window.color = 'red';
    var o = { color : 'blue'};

    function sayColor(){
        console.log(this.color);
    }

    sayColor(); //=>'red'
    o.sayColor = sayColor;
    o.sayColor(); //=>'blue'
```

> 上面的sayColor()函数是在全局作用域中定义的，它引用了this对象。由于在调用函数之前，this的值并不确定，因此this可能会在代码执行过程中引用不同的对象。在当在全局对象作用域中调用sayColor()时，this引用的是全局对象window;换句话说，对this.color求值会转换成对window.color求值，结果返回“red”。
> 而当吧这个对象赋值给对象o并调用sayColor()，this引用的是对象o，因此对this.color求值会转换成对o.color求值，结果返回“blue”

> tips:牢记，函数的名字仅仅是一个包含指针的变量而已。因此，即使是在不同的环境中执行，全局的sayColor()函数与o.sayColor()指向的仍然是同一个函数。

> ECMAScript5也规范化了另一个函数对象的属性：caller。这个属性中保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，它的值为null

```javascript
    function outer(){
        inner();
    }
    function inner(){
        console.log(arguments.callee.caller);
    }
    outer(); 
    /*
        function outer(){
            inner();
        }
    */
```

> 当行数在严格模式('use strict')下运行时，访问arguments.callee会导致错误。ECMAScript5还定义了arguments.caller属性，但在严格模式下访问它也会导致错误，而在非严格模式下这个属性始终是undefined。定义这个属性是为了分清arguments.caller和函数的caller属性。以上变化都是为了加强这门语言的安全性，这样第三方代码就不能在相同的环境里窥视其他代码了。

> 严格模式下，不能为函数的caller属性复制，否则会导致错误。

#### 函数属性和方法

> 函数是对象，因此函数也有属性和方法。每个函数都包含两个属性：length和prototype。其中，length属性表示函数希望接收的命名参数的个数。

```javascript
    function sayName(name){
        console.log(name);
    }
    function sum(num1, num2){
        return num1 + num2;
    }
    function sayHi(){
        console.log('hi');
    }

    console.log(sayName.length); //=>1
    console.log(sum.length); //=>2
    console.log(sayHi.length); //=>0
```

> prototype属性：prototype是保存他们所有实例方法的真正所在。换句话说，诸如toString()和valueOf()等方法世界上都保存在prototype名下，只不过是通过各自对象的实例访问罢了。在创建爱你自定义引用类型以及实现继承时，prototype属性的作用是极为重要的。在ECMAScript5中，prototype属性是不可枚举的，因此使用for-in无法发下。

> 每个函数都包含两个非继承而来的方法：apply()和call()。这两个方法的用途都是在特定的作用域中调用函数，世界上等于设置函数体内this对象的值。首先， 

> apply()方法接受两个参数：一个是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是Array实例，也可以是arguments对象

```javascript
    function sum(num1, num2){
        return num1 + num2;
    }
    function callSum1(num1, num2){
        return sum.apply(this,arguments);
    }
    functioin callSum2(num1, num2){
        return sum.apply(this,[num1, num2]);
    }
    console.log(callSum1(10, 20)); //=>30
    console.log(callSum2(10, 20)); //=>30
```

> callSum1()在执行sum()函数时传入了this作为this值(因为是全局作用域中调用，所以传入的就是window对象)和arguments对象。而callSum2()同样也调用了sum()函数，传入的是this和一个参数数组，这两个函数都会正常执行并返回正确的结果。
> **在严格模式下，为指定环境对象而调用函数，则this值不会转型为window。除非明确把函数添加到某个对象或者调用apply()或call()，否则this值将是undefined。**

>