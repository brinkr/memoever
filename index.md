###JavaScript高级程序设计(第3版) 读书笔记

#####第一章 Javascript简介

一个完整的JavaScript实现是由ECMAScript、文档对象模型(DOM)、浏览器对象模型(BOM)三部分组成的，其中ECMAScript是核心。

**ECMAScript**是对实现ECMA-262标准规定的这门语言的语法、类型、语句、对象、关键字、保留字、操作符等各方面内容的描述。JavaScript、Microsoft JScript和Adobe ActionScript都是对ECMAScript的实现。Web浏览器是ECMAScript最常见的宿主。宿主环境不仅提供基本的ECMAScript实现，同时也会提供该语言的扩展，以便语言与环境之间对接交互。

**DOM 文档对象模型**(**D**ocument **O**bject **M**ode)是针对XML但经过扩展用于HTML的应用程序编程接口。DOM把整个页面映射为一个多层节点结构，HTML或XML页面中的每个组成部分都是某种类型的节点，这些节点又包含着不同类型的数据。

- DOM1级(DOM Level 1)于1998年10月成为W3C的推荐标准。
  - DOM核心(DOM Core) 规定如何映射基于XML的文档结构，以便简化对文档中任意部分的访问和操作。
  - DOM HTML 在DOM Core的基础上扩展了针对HTML的对象和方法。

- DOM2级(DOM Level 2)在DOM Level1的基础上又扩充了鼠标和用户界面事件、范围、遍历(迭代)DOM文档的方法等细分模块，而且通过对象接口增加了对CSS的支持。

- DOM3级(DOM Level 3)进一步扩展了DOM，引入了以统一方式加载和保存文档的方法——在DOM加载和保存(DOM Load andSave)模块中定义；新增了验证文档的方法——在DOM验证(DOM Validation)模块中定义。DOM3级也对DOM核心进行了扩展，开始支持XML 1.0规范，涉及XML Infoset、XPath和XML Base。

**BOM 浏览器对象模型**(**B**rowser **O**bject **M**odel) 提供与浏览器显示页面以外的部分互访的方法和接口以及针对浏览器的JavaScript扩展。

**JavaScript版本**

在Netscape将源代码提交给开源的Mozilla项目的时候，JavaScript在浏览器中的最后一个版本号是1.3。基于Firefox 4将内置JavaScript2.0这一共识，2.0版之前每个递增的版本号，表示的是相应实现与JavaScript 2.0开发目标还有多大的距离。

#####第二章 在HTML中使用JavaScript

向HTML页面中插入JavaScript的主要方法，就是使用<script>元素，HTML 4.01为<script>定义了下列6个属性：

- async：异步，立即下载脚本，但不妨碍页面中其他操作，只对外部脚本有效。
- defer：延迟，脚本延迟到文档完全被解析和显示后再执行，只对外部脚本文件有效。
- charset：通过src属性指定的代码的字符集，被大多数浏览器忽略。
- language：表示编写代码使用的脚本语言，**已废弃**。   
- src：表示包含要执行代码的外部文件。
- type：可以看成是language的替代，表示编写代码使用的脚本语言的内容类型(MIME)。虽然text/javascript和text/ecmascript都已经不被推荐使用，但人们一直以来使用的都还是text/javascript。实际上，服务器在传送JavaScript文件时使用的MIME类型通常是application/x–javascript，但在type中设置这个值却可能导致脚本被忽略。另外，在非IE浏览器中还可以使用以下值：application/javascript和application/ ecmascript。考虑到约定俗成和最大限度的浏览器兼容性，目前type属性的值依旧还是text/javascript。若不指定，默认值为text/javascript。

几点注意：

- 在使用`<script>`嵌入JavaScript代码时，不要在代码中的任何地方出现`"</script>"`字符串。

  ```javascript
  <script type="text/javascript">    
  	function sayScript(){        
      	alert("</script>");     //当浏览器遇到字符串"</script>"时
  	}							//会认为那是结束的</script>标签，从而产生错误
  </script>
  ```

  ```javascript
  <script type="text/javascript">
      function sayScript(){
      	alert("<\/script>");	//用转义符将这个字符串分隔为两部分
  	}
  </script>
  ```

- 在XHTML中可以在单标签中闭合，即` <script type="text/javascript" src="example.js" />` 这种形式。但这不符合HTML的规范，也不能得到某些浏览器(IE)的正确解析。
- 带有src属性的`<script>`和`</script>`之间不宜再包含代码，否则只会执行脚本文件而忽略内嵌代码。
- 按照惯例，`<script>`元素应放在页面`<head>`元素中，但为了用户体验，一般放在页面内容后，`</body>`标签前。

#####第三章 基本概念

ECMAScript的语法大量借鉴了C及其他类C语言。

ES5引入了严格模式(strict mode)的概念。在严格模式下，ES3中的一些不确定的行为将得到处理，且对某些不安全的操作也会抛出错误。要在整个脚本中启用严格模式，在顶部添加`"use strict";`即可。在函数内部的上方包含这条编译指示，可指定该函数在严格模式下执行：

```javascript
function doSomething(){
    "use strict";        
    //……函数体    
}
```

**数据类型**

使用`var`操作符定义的变量将成为定义该变量的作用域中的局部变量。若省略`var`操作符，则定义为全局变量。

ECMAScript中有5种简单数据类型(基本数据类型)：`Undefined`、`Null`、`Boolean`、`Number`和`String`以及1种复杂数据类型`Object`。

- **Undefined类型**只有一个值，即特殊的undefined。在使用var声明变量但未对其加以初始化时，这个变量的值就是undefined。

  对未初始化的变量执行typeof操作符会返回undefined值，而对未声明的变量执行typeof操作符同样也会返回undefined值。

  ```javascript
  var message;// 这个变量声明之后默认取得了undefined值
  // var age	未被声明的变量
  alert(typeof message);// message有声明，未赋值，"undefined" 
  alert(typeof age);	  // age未声明 "undefined" 
  ```

- **Null类型**是第二个只有一个值的数据类型，这个特殊的值是null，表示一个空对象指针，这也是使用typeof操作符检测null值时会返回"object"的原因。

- **NaN**非数值(**N**ot **a** **N**umber)是一个特殊的数值，表示一个**本来要返回数值**的操作数未返回数值的情况。

  任何涉及NaN的操作(如NaN/10)都会返回NaN，且NaN与任何值都不相等，包括NaN本身。

  ```javascript
  alert(NaN == NaN);//结果为false
  ```

  ECMAScript定义了isNaN()函数，接收一个值，并尝试将此值转换为数值。若不能被转换为数值则返回true。

- **Boolean类型**类型只有两个字面值：true和false。就是说，True和False(以及其他的混合大小写形式)都不是Boolean值，只是标识符。

| 数据类型  |    转换为true的值    | 转换为false的值 |
| :-------: | :------------------: | :-------------: |
|  Boolean  |         true         |      false      |
|  String   |    任意非空字符串    |    空字符串     |
|  Number   | 任何非零数值(含无穷) |     0和NaN      |
|  Object   |       任意对象       |      bull       |
| Undefined | n/a(not applicable)  |    undefined    |

- **Number类型**使用IEEE754格式来表示整数和浮点数值。最基本的数值字面量格式是十进制整数，也可以表示八进制或十六进制。

  **八进制**字面值的第一位必须是零(0)，然后是八进制数字序列(0～7)。如果字面值中的数值超出了范围，那么前导零将被忽略，后面的数值将被当作十进制数值解析。

  ```javascript
  var octalNum1 = 070;// 八进制的56
  var octalNum2 = 079;// 无效的八进制数值——解析为79
  var octalNum3 = 08; // 无效的八进制数值——解析为8
  ```

  八进制字面量在严格模式下是无效的，会导致支持的JavaScript引擎抛出错误。

  **十六进制**字面值的前两位必须是0x，后跟任何十六进制数字(0～9及A～F)。其中，字母A～F可以大写，也可以小写。

  ```javascript
  var hexNum1 = 0xA; // 十六进制的10
  var hexNum2 = 0x1f;// 十六进制的31
  ```

  在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。

  **浮点数**需要的内存空间是整数值的两倍，故ECMAScript会不失时机地将浮点数值转换为整数值。

  ECMAScript能够表示的最小数值保存在Number.MIN_VALUE中，在大多数浏览器中，这个值是5e-324；能够表示的最大数值保存在Number.MAX_VALUE中，在大多数浏览器中，这个值是1.7976931348623157e+308。若计算结果超出JavaScript数值范围的值，这个值将被转换成特殊的Infinity值。若为负，即-Infinity(负无穷)，若为正，即Infinity(正无穷)。

  **数值转换**

  Number()可以用于任何数据类型，parseInt()和parseFloat()专门用于把字符串转换成数值。这3个函数对于同样的输入会有返回不同的结果。

  **Number()**函数的转换规则如下。

  > - 如果是Boolean值，true和false将分别被转换为1和0。   
  >
  > - 如果是数字值，只是简单的传入和返回。   
  >
  > - 如果是null值，返回0。   
  > - 如果是undefined，返回NaN。   
  > - 如果是字符串：   
  >   - 只包含数字(包括前面带加号或头号的情况)，则将其转换为十进制数值，即"1"会变成1，"123"会变成123，而"011"会变成11(忽略前导的零)；
  >   - 字符串中包含有效的浮点格式，如"1.1"，则将其转换为对应的浮点数值（忽略前导零）；
  >   - 字符串中包含有效的十六进制格式，例如"0xf"，则将其转换为相同大小的十进制整数值；
  >   - 如果字符串为空，则将其转换为0；
  >   - 如果字符串中包含除上述格式之外的字符，则将其转换为NaN。
  > - 如果是对象，则调用对象的valueOf()方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的toString()方法，然后再次依照前面的规则转换返回的字符串值。

  **parseInt()**函数在转换字符串时，主要看其是否符合数值模式。它会忽略字符串前面的空格，直至找到第一个非空格字符。如果第一个字符不是数字字符或者负号，parseInt()就会返回NaN；也就是说，用parseInt()转换空字符串会返回NaN（Number()对空字符返回0）。如果第一个字符是数字字符，parseInt()会继续解析第二个字符，直到解析完所有后续字符或者遇到了一个非数字字符。
  
  在ECMAScript 5JavaScript引擎中，parseInt()已经不具有解析八进制值的能力，为了消除在使用parseInt()函数时可能导致的上述困惑，可以为这个函数提供第二个参数：转换时使用的基数（即多少进制）。如果知道要解析的值是十六进制格式的字符串，那么指定基数16作为第二个参数，可以保证得到正确的结果，例如：
  
  ```Javascript
  var num = parseInt("0xAF", 16); //175
  var num1 = parseInt("AF", 16);  //175 指定了第二个参数16，可不带"0x"
  var num2 = parseInt("AF");	    //NaN
  ```
  
  **parseFloat()**也是从第一个字符(位置0)开始解析每个字符，且一直解析到字符串末尾，或者解析到遇见一个无效的浮点数字字符为止。即，字符串中的第一个小数点是有效的，而第二个小数点就是无效的了。除了第一个小数点有效之外，parseFloat()与parseInt()的第二个区别在于它始终都会忽略前导的零。parseFloat()可以识别前面讨论过的所有浮点数值格式，也包括十进制整数格式。但十六进制格式的字符串则始终会被转换成0。由于parseFloat()只解析十进制值，因此它没有用第二个参数指定基数的用法。最后还要注意一点：如果字符串包含的是一个可解析为整数的数（没有小数点，或者小数点后都是零），parseFloat()会返回整数。
  
  - String类型表示由零或多个16位Unicode字符组成的字符序列，即字符串。ECMAScript中单引号和双引号没有区别，但要成对使用。

##### 第四章 变量、作用域和内存问题

JavaScript变量松散类型的本质，决定了它只是在特定时间用于保存特定值的一个名字。由于不存在定义某个变量必须要保存何种数据类型值的规则，变量的值及其数据类型可以在脚本的生命周期内改变。

ECMAScript变量可能包含两种不同数据类型的值：基本类型值和引用类型值。基本类型值指的是简单的数据段，而引用类型值指那些可能由多个值构成的对象。

ECMAScript中所有函数的参数都是按值传递的。

基本类型值的传递如同基本类型变量的复制一样。

```javascript
function addTen(num) {
    num += 10;
    return num;
}
var count = 20;
var result = addTen(count);//20，没有变化
alert(count);//30
alert(result);
```

而引用类型值的传递，则如同引用类型变量的复制一样

```javascript
function setName(obj) {
    obj.name = "Nicholas";
    obj = new Object();		//如果person是按引用传递的
    obj.name = "Greg";		//person会被指向name为Grey的新对象
}					//实际上，在函数内部重写obj时，这个变量引用的也是一个局部对象
var person = new Object();//这个局部对象会在函数执行完毕后立即被销毁
setName(person);//"Nicholas"
alert(person.name);
```

**执行环境及作用域(execution context)**

执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的**变量对象**(*variable object*)，环境中定义的所有变量和函数都保存在这个对象中。

全局执行环境是最外围的一个执行环境。在Web浏览器中，全局执行环境被认为是window对象，所有全局变量和函数都是作为window对象的属性和方法创建的。某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁（全局执行环境直到应用程序退出——例如关闭网页或浏览器——时才会被销毁）。

每个函数都有自己的执行环境。当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。ECMAScript程序中的执行流正是由这个方便的机制控制着。当代码在一个环境中执行时，会创建变量对象的一个**作用域链**(*scope chain*)。

作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象（activation object）作为变量对象。活动对象在最开始时只包含一个变量，即arguments对象（这个对象在全局环境中是不存在的）。作用域链中的下一个变量对象来自包含（外部）环境，而再下一个变量对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象。标识符解析是沿着作用域链一级一级地搜索标识符的过程。搜索过程始终从作用域链的前端开始，然后逐级地向后回溯，直至找到标识符为止。

```javascript
var color = "blue";
function changeColor(){
    var anotherColor = "red";
    function swapColors(){
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;
        // 这里可以访问color、anotherColor和tempColor    
    }
        // 这里可以访问color和anotherColor，但不能访问tempColor    
        swapColors();
}
```

![img](https://cdn.cnbj1.fds.api.mi-img.com/book/images/b1fd3d7b7aea1fca186278ebca514a74?thumb=1&w=384&h=384)

内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。这些环境之间的联系是线性、有次序的。每个环境都可以向上搜索作用域链，以查询变量和函数名；但任何环境都不能通过向下搜索作用域链而进入另一个执行环境。

**延长作用域链**

>虽然执行环境的类型总共只有两种——全局和局部（函数），但还是有其他办法来延长作用域链。这么说是因为有些语句可以在作用域链的前端临时增加一个变量对象，该变量对象会在代码执行后被移除。在两种情况下会发生这种现象。具体来说，就是当执行流进入下列任何一个语句时，作用域链就会得到加长：   
>- try-catch语句的catch块；  
>- with语句。
>
>这两个语句都会在作用域链的前端添加一个变量对象。对with语句来说，会将指定的对象添加到作用域链中。对catch语句来说，会创建一个新的变量对象，其中包含的是被抛出的错误对象的声明。

**没有块级作用域**

```javascript
if (true) {
    var color = "blue";
}
alert(color);//"blue"
```

##### 第五章 引用类型

引用类型的值（对象）是引用类型的一个实例。在EC-MAScript中，引用类型是一种数据结构，用于将数据和功能组织在一起。它也常被称为类，但这种称呼并不妥当。尽管ECMAScript从技术上讲是一门面向对象的语言，但它不具备传统的面向对象语言所支持的类和接口等基本结构。引用类型有时候也被称为对象定义，因为它们描述的是一类对象所具有的属性和方法。
