---
layout: default
title: RegExp类型
---
## {{ page.title }}
{{ page.date | date_to_string }}  
		ECMAScript通过RegExp类型来支持正则表达式。使用下面类似Perl的语法，就可以创建一个正则表达式

```
var expression = / pattern / flags;
```

		模式（pattern）部分可以是任何简单或复杂的正则表达式，可以包含 `字符类、限定类、分组、向前查找以及反向引用` 
		每个正则表达式都可带有一或多个标志（flags），用以标明正则表达式的行为。以下为三个标志
		* g :   表示全局(global)模式，即模式将应用于所有字符串，而非在发现第一个匹配项时立即停止
		* i：表示不区分大小写（case-insensitive）模式，即在确定匹配项时忽略模式与字符串的大小写；
		* m：表示多行（multiline）模式，即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项   

#### Example

```
/*
*匹配字符串中所有"at"的实例
*/
var pattern1 = /at/g;

/*
*匹配第一个"bat"或"cat",不区分大小写
*/
var pattern2 = /[bc]at/;

/*
* 匹配所有以"at"结尾的 3 个字符的组合，不区分大小写
*/
var pattern3 = /.at/g;
```

		正则比到达时模式中使用的所有元字符都必须转义。元字符包括：
`( [ { \ ^ $ | ? * + . } ] )`

#### Example

```
/*
* 匹配第一个"bat"或"cat"，不区分大小写
*/
var pattern1  = /[bc]at/;

/*
* 匹配第一个" [bc]at"，不区分大小写
*/
var pattern2 = /\[bc\]at/;

/*
* 匹配所有以"at"结尾的 3 个字符的组合，不区分大小写
*/
var pattern3 = /.at/gi

/*
* 匹配所有".at"，不区分大小写
*/
var pattern4 = /\.at/gi
```

#### RegExp构造函数
		前面的例子都是通过字面量形式来定义正则表达式。    
		另外一种创建正则表达式的方式是使用RegExp构造函数，它接受两个参数：一个是要匹配的字符串模式，另一个是可选的标志字符串。

```
var pattern1 = /[bc]at/i;

var pattern2 = new RegExp("[bc]at","i");
/*pattern1与pattern2是两个完全等价的正则表达式*/

```

##### tips
		传递给RegExp构造函数的两个参数都是字符串（不能把正则表达式字面量传递给RegExp构造函数）。
		由于RegExp构造函数的模式参数是字符串，所以在某些情况下要对字符进行双重转义。

|     字面量模式     |   等价的字符串 |
| :-------: | :-------:|
| /\[bc\]at/    |   "\\[bc\\]at" |
|  /\.at/  |  "\\.at"  |
|  /name\/age/  |  "name\\age"  |
|  /\d.\d{1,2}/  |  "\\d.\\d{1,2}"  |
|  /\w\\hello\\123/  |  "\\w\\\\hello\\\\123"  |

		使用正则表达式字面量和使用RegExp构造函数创建的正则表达式不一样。在ECMAScript3中，
		正则表达式字面量始终共享同一个RegExp实例，而使用构造函数创建的每一个新RegExp实例都是一个新实例

```
var re = null,i;

for(i = 0;i < 10;i++){
  re = /cat/g;
 re.test('catastrophe')
}

for(i = 0;i < 10;i++){
  re = new RegExp('cat','g');
  re.test('catastrophe');
}
```

		在第一个循环中，即使是循环体中指定的，但实际上只为 /cat/ 创建了一个 RegExp 实例。由于实例属性（下一节介绍实例属性）
		不会重置，所以在循环中再次调用 test() 方法会失败。这是因为第一次调用 test() 找到了 "cat" ，但第二次调用是从索引为 3 的字符（上一次匹配的末尾）开始的，所以就找不到它了。由于会测试到字符串末尾，所以下一次再调用 test() 就又从开头开始了。    
		第二个循环使用 RegExp 构造函数在每次循环中创建正则表达式。因为每次迭代都会创建一个新的RegExp 实例，所以每次调用
		 test() 都会返回 true 。    
		***ECMAScript5明确规定，使用正则表达式字面量必须像直接调用RegExp构造函数一样，每次都创建新的RegExp实例***

#### RegExp实例属性
		RegExp的每个实例都具有下列属性，通过这些属性可以获取有关模式的各种信息：
		* global：布尔值，表示是否设置了g标志
		* ignoreCase：布尔值，表示是否设置了i标志
		* lastIndex：整数，表示开始搜索下一个匹配项的字符位置。从0算起
		* multiline：布尔值，表示是否设置了m标志
		* source：正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回。    
		***通过这些属性可以获知一个正则表达式的各方面信息，单却没有多大用处，因为这些信息全部包含在模式声明中***

```
var pattern1 = /\[bc\]at/i;
console.log(pattern1.global); //false
console.log(pattern1.ignoreCase); //true
console.log(pattern1.multiline); //false
conosle.log(pattern1.lastIndex); //0;
conosle.log(pattern1.source); /"\[bc\]at"/

var pattern2 = new RegExp('\[bc\]at','i');
console.log(pattern2.global); //false
console.log(pattern2.ignoreCase); //true
console.log(pattern2.multiline); //false
conosle.log(pattern2.lastIndex); //0;
conosle.log(pattern2.source); /"\[bc\]at"/
```

		上述代码中第一个模式使用的是字面量，第二个模式使用了RegExp构造函数，但他们的source属性是相同的，
		可见，source属性保存的是规范形式的字符串，即字面量形式使用的字符串。

#### RegExp实例方法

##### exec方法

		exec()，该方法是专门为捕获组而设计的。exec()接受一个参数，即要应用模式的字符串，然后返回包含第一个匹配项信息的
		数组；或者在没有匹配想的情况下返回null。返回的数组虽然是Array的实例，单包含两个额外的属性：index和input。其中，
		index表示匹配项在字符串中的位置，而input表示应用正则表达式的字符串。在数组中，第一项是整个模式匹配的字符串，其
		他项是与模式中的捕获组匹配的字符串（如果模式中没有捕获组，则该数组只包含一项）。

```
var text = 'mom and dad and baby';
var pattern = /mom( and dad( and baby)?)?/gi;
var matches = pattern.exec(text);
console.log(matches);
//[
	"mom and dad and baby", 
	" and dad and baby", 
	" and baby", 
	index: 0, 
	input: "mom and dad and baby"
]
```

##### test方法
		test()，它接受一个字符串参数。在模式与该参数匹配的情况下返回true；否则，返回false。在只想知道目标字符串与某个模式是否
		匹配，但不需要知道其文本内容的情况下，使用这个方法比较方便。

```
var text = '000-00-0000';
var pattern = /\d{3}-\d{2}-\d{4}/;
if(pattern.test(text)){
	console.log('The pattern was matched');
}
```

##### toLocaleString 、toString方法

```
var pattern = new RegExp('\\[bc\\]at','gi');
console.log(pattern.toLocaleString());
// /\[bc\]/gi
console.log(pattern.toLocaleString();
// /\[bc\]/gi
```

		即使上例中的模式是通过调用RegExp构造函数创建的，但toLocaleString()和toString()方法仍然会像它是以字面量形式创建的
		一样显示字符串表示
		*tips:正则表达式的valueOf()方法返回正则表达式本身*

#### RegExp构造函数属性
		RegExp构造函数包含一些属性，这些属性适用于作用域中
