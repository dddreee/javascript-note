##### 算数表达式
1. **+运算符**
   - 如果其中一个操作数是对象，则对象会遵循原始值的转化规则转换为原始值：日期对象通过toString()方法进行转换，其他方法通过valueOf()方法进行转换（如果valueOf返回一个原始值的话），大多数对象都不具备可用的valueof方法，因此它们会通过toString()方法来转换
   
   - 在对象进行原始值转换后，如果一个操作数是字符串，那么另一个也会转换成字符换进行字符串拼接
   
   - 否则，两个都转换成数字（或者NaN）然后进行加法操作
   
2. **一元运算符**

    递增++运算符的返回值依赖于它相对于操作数的位置。当++在操作数之前则为**前增量**，在操作数之后为**后增量**。前增量返回计算后的值，**后增量则对操作数进行增量计算，但是返回的是未增量的值**。
    
    ```javascript
    var a = 1,
        b = ++a,    //b=2
        c = a++;    //c=1
    ```
    **++a并不等于a = a + 1, ++a只进行数字的累加**
    
    ```javascript
    var a = '1',
        b = '1';
    ++a;        //2
    b = b + 1;  //'11'
    ```
    
    
#### **eval**
eval()只有一个参数。如果传入的参数不是字符串，它直接返回这个参数。如果参数是字符串，则会把字符串当做javascript代码进行编译，如果编译失败，则报出一个语法错误(SyntaxError)的异常。

如果编译成功，则开始执行这段代码，并返回字符串中最后一个表达式或者语句的值，如果没有值则最终返回undefined。如果字符串抛出一个异常，这个异常则会把调用传递给eval()

**关于eval()最重要的是，它调用了使用了eval的变量作用域环境**

传给eval()的字符串必须要在语法上讲得通，比如eval('return;')是没有意义的。

```javascript
var a = function(arg){
    eval(arg)
};
a('return; ')
//会抛出语法错误

```

在es3中，eval是无法定义别名的，而在es5中，又放开了这个限制

当直接调用eval()时，执行的代码在eval所处的上下文作用域中运行。
当使用别名调用eval()时，则是在全局作用域下执行。

```javascript
var geval = eval,
    a = 'global',
    b = 'global',
    
    x = function(){
        var a = 'local';
        eval('a += " changed"');
        return a
    },
    
    y = function(){
        var b = 'local';
        geval('b += " changed"');
        return b;
    };
    
console.log(x(), a);    //更改局部变量, "local changed", "global"
console.log(y(), b);    //更改全局变量, "local", "global changed"
```
**严格模式下的eval**

在严格模式下，eval执行的代码不能在局部作用域中定义新的函数或变量，此外严格模式将“eval”列为保留字！