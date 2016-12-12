# Object 总结

- [创建对象](#创建对象)
- [属性的查询和设置](#属性的查询和设置)
- [删除属性](#删除属性)
- [检测属性](#检测属性)
- [枚举属性](#枚举属性)
---
#### 创建对象
- 对象直接量

```
var a = {},
    b = {x: 0, y: 1},
    c = {x: b.x, y: b.y},
    d = {
        name: 'UMEI',
        year: 2,
        member: {
            'CEO': 'xu',
            'CTO': 'yonnie'
        }
    }
```

- **new**

> 通过new运算符创建一个对象，这里称作构造函数，构造函数用以初始化一个新创建的对象

```
var a = new Object(),
    b = new Array(),
    c = new Date()
```

- **原型**

**每一个javascript对象（null除外）都和一个原型对象相关联，每一个对象都从原型继承属性。**

所有通过对象直接量创建的对象都具有同一个原型对像--**Object.prototype**。

通过new创建的对象的原型就是构造函数的prototype属性的值。因此通过new Object()创建的对象继承Object.prototype，通过new Array()创建的对象继承Array.prototype，通过new Date()创建的继承Date.prototype。

Object.prototype不继承任何属性，没有原型对象。

所有的其他的对象、原型对像都继承自Object.prototype。因此new Date()创建的对象同时继承自Date.prototype和Object.prototype。这一系列链接的原型对象就是所谓的**“原型链”**

- Object.create()

Object.create()可以传入两个参数，第一个参数是这个原型的对象，还有第二个可选参数。

```
var a = Object.create({x: 1, y: 1});
a继承了x和y属性
```
可以通过传入null创建一个没有原型的新对象，但通过这种方式创建的不会继承任何方法和属性。

如果想创建一个普通的空对象，需要传入Object.prototype

可以通过任务原型创建新对象（可以使任意对象可继承）


```
//inherit()返回了一个继承自原型对像p的属性的新对象
//这里使用es5中的Object.create()函数
//如果不存在Object.create()则使用其他方法

function inherit(p){
    if(p == null) throw TypeError(); //p如果是null或者undefined 抛出类型错误
    if(Object.create) //如果存在create方法，则使用create
        return Object.create(p);
    var t = typeof p;
    if(t !== "object" && t !== "function"){
        throw TypeError();
    }
    function f(){};
    f.prototype = p;
    return new f();
}

```
inherit()函数的作用就是防止库函数无意间修改那些不受你控制的对象。当函数读取继承对象的属性时，实际上读取的是继承来的值。如果给继承对象的属性赋值，不会影响原始对象

---

#### 属性的查询和设置

> 可以通过(.)和([])来获取属性的值。如果一个对象的属性名是保留字，必须使用[]来访问

##### 继承

> Javascript对象具有“自有属性”，也有一些属性是从原型对象继承而来。

查询 对象o的属性x，如果o中不存在x，那么就会沿着o的原型链查，直到找到x或者找到一个原型是null的对象位置。

```
var o = {};
o.x = 1;

var p = inherit(o);
p.y = 2;

var q = inherit(p);
p.z = 3;

var s = q.toString();//toString继承自Object.prototype

q.x + q.y; //--> 3, x和y分别继承自o和p

q.x = 4; //会覆盖q继承的x属性，o的x属性不会变
```

属性赋值操作首先检查原型链。例如，如果o继承自一个只读属性x。那么赋值操作是不允许的。如果允许赋值操作，它也总是在原始对象上创建属性或对已有的属性赋值，不会去修改原型链。

在js中，只有在查询属性时才会体会到继承的存在，而设置属性则和继承无关。

##### 属性访问错误

访问一个不存在的属性不会报错，单数如果访问一个不存在的对象的属性就会报错

内置构造函数的原型是只读的。
```
Object.prototype = 1; //不会报错，但是没被修改为1
```

在严格模式中，任何失败的属性设置操作都会抛出一个类型错误。

#### 删除属性

delete运算符可以删除对象的属性，delete只是断开属性和宿主对象的联系，而不会去操作属性中的属性。
```
var a = {p: {x: 1}},
    b = a.p;
    
delete a.p;
console.log(b.x); //--> 删除了a.p属性，但是b.x的值还是1
```

已经删除的属性的引用依然存在，delete只能删除自有属性。

delete不能删除那些可配置性为false的属性。某些内置对象的属性是不可配置的，比如通过变量声明和函数声明创建的全局对象的属性。在严格模式中，删除一个不可配置属性会报一个类型错误。在非严格模式中，delete操作失败会返回false

```
delete Object.prototype     //不能删除，属性不可配置
var x = 1;
delete this.x;              //返回false
delete x;                   //返回false

function f(){

}

delete this.f;              //不能删除全局函数
```

#### 检测属性
> 可以通过in运算符、hasOwnPreperty() 和 propertyInEnumerable()方法来判断某个属性是否存在于某个对象中。

in 运算符，如果对象的自由属性或继承属性中包含这个属性则返回true
```
var o = {x: 1};
'x' in o;           //true
'y' in o;           //false
'toString' in o;    //true 继承的属性
```

hasOwnProperty()方法用来检测是否是自有属性，如果是继承的属性会返回false
```
var o = {x: 1}
o.hasOwnProperty('x');      //true
o.hasOwnProperty('y');      //false
o.hasOwnProperty('toString');   //false
```

propertyIsEnumerable()是hasOwnProperty()的增强版，只有检测到自有属性且这个属性可枚举时才返回true。有些内置属性时不可枚举的

```
var o = inherit({y: 2});
o.x = 1;
o.propertyIsEnumerable('y');        //false
o.propertyIsEnumerable('x');        //true
Object.prototype.propertyIsEnumerable('toString');      //false  toString不可枚举
```

有一种情况只能用in运算符来判断，就是存在并且值为undefined的属性
```
var o = {x: 1, y: undefined};
'x' in o;   //true
'y' in o;   //true
'z' in o;   //false
delete o.x
'x' in o;   //false
```
#### 枚举属性
> for/in 循环可以在循环体中遍历对象中所有可枚举的属性（**包括自有和继承的**），把属性名称赋值给循环变量。**对象继承的内置方法不可枚举**，但在代码中给对象添加的属性都是可枚举的。

```
var o = {
    x: 1,
    y: 2,
    z: 3,
    test: function(){
        console.log('test')
    }
},
    p = Object.create(o);
    
for(i in p){
    console.log(i);     //--> x,y,z,test; 继承自o的属性都是可枚举的, 没有toString
}
    
```

在es5之前，在Object.prototype添加的方法会被枚举出来。

定义一个extend()函数来枚举属性

```
function extend(o, p){
    for (prop in p){
        o[prop] = p[prop];  //遍历p中的所有属性，将属性添加至o中
    }
    return o;
}

/*
*将p中的可枚举属性复制到o中，并返回o
*如果o和p中有同名的属性，o中的属性将不受影响。
*这个函数并不处理getter和setter以及复制属性
*
*/
function merge(o, p){
    for(prop in p){
        if(o.hasOwnProperty(prop)) continue; //过滤掉已经在o中存在的属性
        o[prop] = p[prop]
    }
    
    return o
}

/*
*如果o中的属性在p中没有同名属性，则从o中删除这个 属性
*返回o
*/
function restrict(o, p){
    for(prop in o){
        if(!(prop in p)) delete o[prop]; //如果在p中不存在，则删除之
    }
    return o;
}

/*
*如果o中的属性在p中存在同名属性，则从o中删除这个属性
*返回o
*/
function(o, p){
    for(prop in p){
        delete o[prop]
    }
    return o;
}

/*
*返回一个新对象，这个对象同时拥有o的属性和p的属性
*如果o和p有重名属性，使用p中的属性值
*/
function union(o, p){
    return extend(extend({}, o), p);
}

/*
*返回一个新对象， 这个对象拥有同时再o和p中竖线的属性
*很想求o和p的交集，但p中属性的值被忽略
*/
function intersection(o, p){
    return restrict(extend({}, o), p);
}

/*
*返回一个数组，这个数组包含的是o中可枚举的自有属性的名字
*/
function keys(o){
    if(typeof o !== 'object') throw TypeError();
    var result = [];
    for(var prop in o){
        if(o.hasOwnProperty(prop))
        result.push(prop);
    }
    return result;
}

```