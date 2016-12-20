##### 声明一个数组的方式有：
```javascript
var a = []; 
var b = new Array(); // --> []
var c = new Array(6); // --> length为6的数组，每个元素都是undefined
var d = new Array(1,2,3); // --> [1,2,3]
```
##### 还有不常见的稀疏数组， 数组中不连续，会有undefined存在



##### 可以通过Object.defineProperty()让数组的length属性变成只读的，如：
```javascript
var a= [1, 2, 3];
Object.defineProperty(a, 'length', {writable: false});
a.length = 1; //--> length属性不会改变
```


##### 数组的元素的增加和减少 posh pop shift unshift

```javascript
var a = [1, 2, 3];
a.push(4); //--> [1, 2, 3, 4]
a.pop(); //--> a = [1, 2, 3]   ; pop 方法会删除并返回最后一个元素，会改变原数组
a.shift(); //--> 和pop方法相同，不过是第一个元素
a.unshift(4); //--> 和push方法相同，从前面插入
```
##### 数组的方法：
- **splice**

    splice方法  **删除或者添加**数组元素 splice(a, b), 从第a个开始删除b个元素，并返回删除的元素或数组，会改变原来的数组
    
```
var a = [1,2,3,4,5];
a.splice(0, 1); //--> 删除第0个开始的1个元素 
a; // --> [2,3,4,5]
```
    

```
//如果省略第二个参数，后面的全删除
var a = [1,2,3,4], b = ['a', 'b', 'c', 'd'];
a.splice(2); // --> 返回[3,4], a 为 [1, 2]
```
    

```
//spilce除了删除还能添加！！
var a = [1,2,3], b = ['a', 'b', 'c', 'd'];
a.splice(2,0, 'a', 'b'); //--> [1,2,'a', 'b', 3]
b.splice(2,1,[1,2], 'e') ; // --> 返回['c'], b为 ['a', 'b', [1,2], 'e', 'd']
```


    

- **concat**

    合并两个数组 a.concat(b) 并返回合并后的数组， 原数组不会改变
    
```
var a = [1,2,3,4,5], b = [1, 2];
a.concat(b) ; //--> [1, 2, 3, 4, 5, 1, 2]
console.log(a); // --> [1, 2, 3, 4, 5]
console.log(b); // --> [1, 2]  a b 数组都不会变
```

- **sort** 和 **reserve**

     
     
```
var a = [{value: 2}, {value: 1}, {value: 5}, {value: 4}];
a.sort(function(obj1, obj2){
    return obj1 - obj2;  //正序-->[{value: 1}, {value: 2}, {value: 4}, {value: 5}];
})
```

-  **join**
  
    将数组中所有元素化为字符串并连接在一起，并返回生成的字符串。同时可以指定一个字符来作为连接符
    

```
var a = [1,2,3];
//a.join();  -->'123'
//a.join(' '); --> '1 2 3'
//a.join('-'); --> '1-2-3'
```

- **slice** 

    返回元素或子数组， slice(a, b), 返回从第a个开始到第b个元素的集合, a和b可以是负数, 负数则从末尾开始算
    

```
var a = [1, 2, 3, 4];
a.slice(0, 2); //[1, 2]
a.slice(0, -1); //[1, 2, 3]
```

- **forEach**
> forEach 方法遍历整个数组，并对数组的每个元素执行方法, 可以传递3个参数： 元素 序列 数组本身


```
var a = [1, 2, 3, 4],
    b = 0;
a.forEach(
    function(item, index, self){
        b += item; //循环结束b=10
    }
);

```

需要注意的是**forEach不能停止循环**，没有for循环中的break语句。如果需要提前终止循环，**必须放在try块中，并抛出异常**。抛出异常的时候会触发**foreach.break** 事件

- **map** 
> 将调用的数组的每个元素传递给指定的函数，并返回新的数组，包含该函数的返回值

```
var a = [1, 2, 3, 4, 5];
var b = a.map(function(item){return item*2 }); //[2, 4, 6, 8, 10]
var c = [{val: 1}, {val: 2}, {val: 3}];
var d = c.map(function(i){return (i.val + 1)});//[2, 3, 4]
```
map遍历和forEach 不一样，map返回的是新数组，不会改变原数组

- **filter**
- **every和some**
> every和some是数组的判断， 返回true或者false

如果一个数组的每个元素都满足every中的函数，则返回true；而some只要有符合条件的就返回true。

**every相当于每个元素的 & , some 相当于 |**

```
var a = [1, 2, 3, 4];
a.every(function(i){return i < 10}); //true
a.every(function(i){return i%2 == 0}); //false
```

```
var b = [1, 2, 3, 4];
b.some(function(i){ return i < -1 }); //false
b.some(function(i){ return i % 2 === 0 }); //true
```

- **reduce和reduceRight**
> 化简方法， 将数组进行组合，生成单个值并返回
###### reduce从左， reduceRight从右开始。这两个方法接收两个参数，第一个参数是执行的方法，第二个参数是可选的初始值。有初始值时，初始值作为执行方法的第一个参数注入，没有的话，则是数组的第一个元素或者最后一个作为初始值传入。

```
var a = [1,2,3];
var b = a.reduce(function(x, y){
    return x+y
}, 0); //--> 6

```


- **indexOf和lastIndexOf**
> 检索数组，并返回匹配的第一个元素的索引，indexOf从头开始，lastIndexOf从尾开始，如果没找到则返回-1。可以传入两个参数，第一个是需要匹配的元素，第二个是可选的，它是指定数组中的一个索引，从那开始搜索，第二个参数也可以是负数