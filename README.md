# e-js
JS在线学习笔记
 - 题外话：
 - 函数级作用域：js是函数级作用域，在内部的变量，内部都能访问，外部不能访问，内部能访问外部的。
 - 如何让外部能访问内部的作用域呢？闭包就能实现。


##### 闭包
 - 什么是闭包 ？
 - 简单理解， 函数嵌套函数
 - 例子解释：在函数outter_fn中又定义了函数inner_fn，并且，内部函数inner_fn可以引用外部函数outter_fn的参数和局部变量，当outter_fn返回函数inner_fn时，相关参数和变量都保存在返回的函数中，这种程序结构称为“闭包（Closure）”.
 - 白话解释：闭包就是能够读取其他函数内部变量的函数，在js中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成‘定义在一个函数内部的函数’
 ```
	function outter_fn(x){
		function inner_fn(){
			return x*100;
		}
		return inner;
	}
 ```
##### 闭包2个最大的用处：
  闭包2个最大的用处：
  1.可以读取函数内部的变量
  2.让这些变量的值始终保持在内存中
  3、避免污染全局变量
 
##### IE下会引发内存泄露
 - 当页面跳转的时候，变量不会释放，一直存放于内存当中，使cpu不断累加，只有关闭浏览器的时候才会释放
##### 闭包实战应用
- 场景一 避免全局变量的污染
```

```
- 场景二 用闭包巧妙实现私有成员的存在 （在java语言，同一个类里，其他方法可以调用私有方法和属性）
```
//匿名函数自动执行
var counter = (function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  };   
})();

console.log(counter.value()); // logs 0
counter.increment();
counter.increment();
console.log(counter.value()); // logs 2
counter.decrement();
console.log(counter.value()); // logs 1
```
```
//正常函数
var makeCounter = function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }  
};

var counter1 = makeCounter();
var counter2 = makeCounter();
alert(counter1.value()); /* Alerts 0 */
counter1.increment();
counter1.increment();
alert(counter1.value()); /* Alerts 2 */
counter1.decrement();
alert(counter1.value()); /* Alerts 1 */
alert(counter2.value()); /* Alerts 0 */
```
- 场景三 闭包解决循环通常问题
```
//多种闭包写法
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function makeHelpCallback(help) {
  return function() {
    showHelp(help);
  };
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
  }
}

setupHelp();
```
```
//匿名闭包写法
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    (function() {
       var item = helpText[i];
       document.getElementById(item.id).onfocus = function() {
         showHelp(item.help);
       }
    })(); // Immediate event listener attachment with the current value of item (preserved until iteration).
  }
}

setupHelp();
```