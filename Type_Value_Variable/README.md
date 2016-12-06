## 基本类型和引用类型的值
  基本类型值指的是简单的数据段，而引用类型指的是可能有多个值构成的对象。

##### 5个基本类型值:
- [Number](#number)
- String
- Boolean
- Null and Undefined

---
##### 动态属性

引用类型的值，我们可以添加、改变和删除其属性和方法，如果对象的不不背销毁或者属性不被删除，则这个属性将一直存在。

```
var a = {name: 'James'}
a.age = 18;
console.log(a.age); //-->18
delete a.age;
console.log(a.age); //-->undefined
```
**无法给直接类型值添加属性**

```
var a = 'String';
a.age = 18;
console.log(a.age); //-->undefined
```

---

##### 复制变量值
复制基本类型值时，原先的变量改变后不影响复制的值

```
var a = 1,
    b = a;
console.log(a, b); //--> 1,1
a = 2;
console.log(a, b); //--> 2,1 b复制a后是完全独立的
```
复制引用类型值时，复制的是指向引用类型值得**指针**，这个指针**指向存储在堆中的一个对象**。

```
var a = {},
    b = a;
a.name = 'James';
console.log(b.name); //-->James
//改变b，a也会改变
b.name = 'Jack';
console.log(a.name); //-->Jack
```
---

### Number
在javascript中能够表示的最大整数范围是 **-2^53 ~ 2^53**
##### javascript 中的算术运算

```
Math.pow(2,53);     //--> 2的53次方
Math.round(.6);     //--> 1.0 四舍五入，返回的还是浮点型
Math.ceil(.6);      //--> 1.0 向上取整
Math.floor(.6);     //--> 0.0 向下取整
Math.abs(-1);       //--> 1 取绝对值
Math.max(2, 5, 6);  //--> 6 取最大值
Math.min(-1, -2, 1);//--> -2 取最小值
Math.random();      //--> 取0-1的随机数
Math.PI             //--> 圆周率π
Math.E              //--> e: 自然对数的底数
Math.sqrt(3);       //--> 3的平方根
Math.pow(3, 1/3);   //--> 3的立方根
Math.sin(0);        //--> 三角函数： 还有Math.cos(), Math.atan()等
Math.log(10);       //--> 10的自然对数
Math.log(100)/Math.LN10; //-->以10为底100的对数
Math.exp(3);        //--> e的三次幂

```
当数字运算返回的结果超过最大整数时，结果是Infinity,负数则是-Infinity;
可以通过isFinite()方法判断;
NaN也有isNaN()判断;

##### 浮点运算和四舍五入错误

```
var x = .3 - .2,
    b = .2 - .1;
x == b; //--> false

```

---
### String

##### \转义符
```
//常用的转义符
\n  //换行
\t  //水平制表
\r  //回车
\'  //单引号转义
\"  //双引号转义
\\  //\转义

```

##### String方法和属性