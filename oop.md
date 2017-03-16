##### 标准对象
- 为了区分对象的类型，我们用typeof操作符获取对象的类型，它总是返回一个字符串
```
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'

```
##### 总结一下，有这么几条规则需要遵守：
- 不要使用new Number()、new Boolean()、new String()创建包装对象；
  - 用parseInt()或parseFloat()来转换任意类型到number；
  - 用String()来转换任意类型到string，或者直接调用某个对象的toString()方法；
  - 通常不必把任意类型转换为boolean再判断，因为可以直接写if (myVar) {...}；
  - typeof操作符可以判断出number、boolean、string、function和undefined；
  - 判断Array要使用Array.isArray(arr)；
  - 判断null请使用myVar === null；
  - 判断某个全局变量是否存在用typeof window.myVar === 'undefined'；
  - 函数内部判断某个变量是否存在用typeof myVar === 'undefined'

##### 正则表达式 一种用来匹配字符串的强有力的武器
 - 它的设计思想是用一种描述性的语言来给字符串定义一个规则，凡是符合规则的字符串，我们就认为它“匹配”了，否则，该字符串就是不合法的。
 ```
  \d 可以匹配一个数字
  \w 可以匹配一个字母或数字
  .  可以匹配任意字符
  *  表示任意个字符（包括0个）
  +  表示至少一个字符
  ?  表示0个或1个字符
  {n}表示n个字符
  {n,m}表示n-m个字符
  \s可以匹配一个空格（也包括Tab等空白符）
  '-'是特殊字符，在正则表达式中，要用'\'转义，所以，上面的正则是\d{3}\-\d{3,8}
  ```
  ##### JavaScript有两种方式创建一个正则表达式： 
  - 第一种方式是直接通过/正则表达式/写出来
  - 第二种方式是通过new RegExp('正则表达式')创建一个RegExp对象
  ```
	var re1 = /ABC\-001/;
	var re2 = new RegExp('ABC\\-001');

	re1; // /ABC\-001/
	re2; // /ABC\-001/
	
  ```
  //注意，如果使用第二种写法，因为字符串的转义问题，字符串的两个\\实际上是一个\。
  RegExp对象的test()方法用于测试给定的字符串是否符合条件。


##### 切分字符串

##### 进阶 []表示范围

##### 分组

##### 贪婪匹配
- 加个?就可以让\d+采用非贪婪匹配
- i标志，表示忽略大小写，
- m标志，表示执行多行匹配
- 最常用的是g，表示全局匹配
```
var s = 'JavaScript, VBScript, JScript and ECMAScript';
var re=/[a-zA-Z]+Script/g;
var tempArr = [];
do{
	var temp = re.exec(s);
	if(temp){
		tempArr.push(temp[0]);
	}
	
}while(temp);
console.log(tempArr);
```
```
var s = 'JavaScript, VBScript, JScript and ECMAScript';
var re=/[a-zA-Z]+Script/;
var tempArr = re.exec(s);
console.log(tempArr);
```