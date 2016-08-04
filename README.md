# ES6
# ES6 总结
### 首先，什么是ECMAScript？
js 包括 ES BOM DOM,而ES规定了js的语法规则。比如要用var声明一个变量。

### let关键字
let 声明不会出现变量提升。

    var a = 1;
    (function(){
        alert(a);
        let a = 2; 
    })();  【报错 a未被定义】
let 声明的变量只在自己的作用域内起作用。

    var arr = [];
    for(var i = 0;i < 10;i++){
        arr[i] = function(){
            alert(i);
        }
    }   
    arr[8]();  【输出10】
    
    var arr = [];
    for(let i = 0;i < 10;i++){
        arr[i] = function(){
            alert(i);
        }
    }   
    arr[8]();  【输出8】
- 用var声明的变量I的值会影响到各个作用域的i，循环完毕I=10,所有块的i 都变为10；

同一作用域不允许用let重复声明一个变量。


```
let a = 2;
let a = 3;
Uncaught SyntaxError: Identifier 'a' has already been declared
```

### const关键字
1.  不能修改

```
const a = 1;
a = 2;

Uncaught TypeError: Assignment to constant variable.
```
- const 为传址赋值

```
const Person = {"name":"lisi"};
Person.name = "zhangsan";
Person.age = 20;
console.log(Person);     {name:zhangsan,age:20}
```
**不可修改的是对象的地址而不是对象本身；**
2. 只在自己的作用域起作用

```
if(1){
    const a = 1;
}
alert(a);
Uncaught SyntaxError: Identifier 'a' has already been declared
```
3. 没有变量提升

```
    if(1){
        alert(a);
        const a = 1;
    }
ES6.html:25 Uncaught ReferenceError: a is not defined
```
4. 声明后必须赋值

```
const a;
ES6.html:28 Uncaught SyntaxError: Missing initializer in const declaration
```

### 解构赋值
赋值a=1,b=2,c=3;

```
传统的
var arr = [1,2,3];
var a = arr[0];
var b = arr[1];
var c = arr[2];
```
    ES6中
    var [a,b,c] = [1,2,3];

对象的解构赋值

```
var {a,b,c} = {"a":1,"b":2,"c":3};
```
- 作用
  - 方便的交换变量的值

```
  var x =1 ;
  var y =2;
  [x,y] = [y,x];
  传统的方法要有第三个临时变量temp来保存临时变量的值。
```
 - 
  - 提取函数返回的值

```
  function demo(){
    return {"name":"lisi","age":20}
}
var {name,age} = demo();
```
- 
 - 定义函数参数

```
 function demo({a,b,c}){
    console.log(a+b+c);
}
demo({a:"li",b:"1.8",c:"20kg",d:"5000"});
```
**方便的提取JSON对象中想要的参数**
- 
 - 函数参数的默认值

```
//判断参数是否传值，如果没传赋值"lisi"
 function demo(a){
    var name;
    if(a==undefined){
        name = "lisi";
    }else{
        name = a;
    }
}
```

```
//haha
function demo({name = "lisi"}){
    alert(name);
}
demo("haha");
```

```
//lisi
function demo({name = "lisi"}){
    alert(name);
}
demo({});
```
### 字符串新特性
- 模板字符串
```
//He is lisi,he is 20
反引号``
let name = "lisi";
let age = 20;
let str = `He is ${name},he is ${age}`;
alert(str);
```
- ${js表达式}

```
//the result is 3
var a = 1,b = 2;
var str = `the result is ${a+b}`;

var obj = {"a":1,"b":2};
var str = `the result is ${obj.a+obj.b}`;

function fn(){
    return 3;
}
var str = `the result is ${fn()}`;
```

### 箭头函数中的this指向定义时的this，而不是执行时的this

    //定义一个对象
    var obj = {
        x:100, //属性x
        show(){
        //延迟500毫秒，输出x的值
            setTimeout(
               //匿名函数
               function(){console.log(this.x);},
               500
           );
        }
    };
    obj.show();//打印结果：undefined  setTimeout是window对象的方法,打印输出的是window.x;

    //定义一个对象
    var obj = {
        x:100,//属性x
        show(){
            //延迟500毫秒，输出x的值
            setTimeout(
               //不同处：箭头函数
               () => { console.log(this.x)},
               500
            );
        }
    };
    obj.show();//打印结果：100

特点：
 - this 指向取决于箭头函数在哪儿定义，之后this不可变。

