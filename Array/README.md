##### 声明一个数组的方式有：
```
var a = []; 
var b = new Array(); // --> []
var c = new Array(6); // --> length为6的数组，每个元素都是undefined
var d = new Array(1,2,3); // --> [1,2,3]
```
##### 还有不常见的稀疏数组， 数组中不连续，会有undefined存在



##### 可以通过Object.defineProperty()让数组的length属性变成只读的，如：
```
var a= [1, 2, 3];
Object.defineProperty(a, 'length', {writable: false});
a.length = 1; //--> length属性不会改变
```


##### 数组的元素的增加和减少 posh pop shift unshift

```
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
