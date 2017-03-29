# 1.let 命令
### 基本用法
ES6新增加了<code>let</code>命令，用来声明变量。他的用法类似于<code>var</code>，但是所声明的变量，只在<code>let</code>命令所在的代码块（作用域）内有效。

```javascript
{
    let a = 10;
    var b = 1;
}
console.log(a);// a is not defined
console.log(b);//1
```
[总结]: 上面代码在一个花括号（作用域）中，分别用<code>let</code>和<code>var</code>声明了两个变量。然后在花括号（作用域）之外调用这两个变量，结果<code>let</code>声明的变量报错，<code>var</code>声明的变量返回了正确的值。这表明 在ES6中花括号是一个块级作用域，块级作用域之外无法直接访问块级作用域之内的变量。

<code>for</code>循环的计数器，就很适合用<code>let</code>命令
```javascript
for(let i = 0; i<10; i++){//code}
console.log(i)// i  is not defined
```
[总结]上边代码中，计数器<code>i</code>只在<code>for</code>循环体内有效，在循环体外引用就会抱错，这样就避免了污染全局变量。

```javascript
    var a = [];
    for(var i = 0;i<10;i++){
        a[i] = function(){
            console.log(i);
        };
    }
    a[6]();//10
```
[总结]上边代码中，变量<code>i</code>是<code>var</code> 声明的，在全局范围内都有效，所以全局只有一个变量<code>i</code>。
每一次循环，变量<code>i</code>的值都会发生改变，而循环内被赋给数组<code><a>的<code>function</code>在运行时，会通过闭包读到这同一个变量<code>i</code>，导致最后输出的是最后一轮的<code>i</code>值，也就是10。

```javascript
    var a = [];
    for(let i = 0; i<10; i++){
        a[i] = function(){
            console.log(i)
        }
    }
    a[6]()//6
```
[总结]上边代码中，变量<code>i</code>是<code>let</code>声明的，当前的<code>i</code>只在本轮循环有效，所以每一次循环的<code>i</code>其实都是新的变量，所以最后输出的是6.你可能会问，如果每一轮循环的变量i都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为JavaScript引擎内部会记住上一轮循环的值，初始化本轮的变量<code>i</code>时，就在上一轮循环的基础上进行计算。
另外，<code>for</code>循环还有一个特别之处，就是循环语句部分是一个父作用域，而循环体内部是一个单独的子作用域举个例子：
```javascript
    for(let i = 0; i<3; i++){
        let i = 'abc';
        console.log(i)
    }
    //abc
    //abc
    //abc
```
[总结]：上面代码输出了三次<code>abc</code>,表明函数内部的变量<code>i</code>和外部的变量<code>i</code>是分离的。

###不存在变量提升
<code>var</code>命令会发生“变量提升”现象（Hoisting），即变量可以在声明之前使用，值为<code>undefined</code>。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。
为了纠正这种现象， <code>let</code>命令改变了语法行为，它所声明的变量一定要在声明之后才能使用，否则就会报错。
```javascript
    //var的情况
    console.log(foo);//输出undefined
    var foo = 2;

    //let 的情况
    console.log(bar);// error
    let bar = 2;
```
上边代码中，变量<code>foo</code>用<code>var</code>命令声明，会发生变量提升，即脚本开始运行时，变量<code>foo</code>已经存在了，但是没有值，所以会输出<code>undefined</code>。变量<code>bar</code>用<code>let</code>命令声明，不会发生变量提升。这表示在声明它之前，变量<code>bar</code>是不存在的，这时候如果用到它，就会抛出来一个错误。
 ###暂时性死区
 只要块级作用域内存在<code>let</code>命令，它所声明的变量就"绑定"这个区域，不再受外部的影响。
 ```javascript 
    var tmp = 123;
    if(true){
        tmp = 'abc';// error
        let tmp;
    }
 ```
 上边代码中，存在全局变量<code>tmp</code>但是块级作用域内<code>let</code>又声明了一个局部变量<code>tmp</code>,导致后者绑定这个块级作用域，所以在<code>let</code>声明变量前，对<code>tmp</code>赋值会报错
 ES6明确规定，如果区块中存在<code>let</code>和<code>const</code>命令，这个区块对这些命令声明的变量，从一开始就形成了封闭的作用域。凡是在声明之前就使用这些变量，就会报错。
 总之，在代码块内，使用<code>let</code>命令声明变量之前，该变量都是不可用的。这在语法上，称为"暂时性死区"（temporal dead zone，简称 TDZ）
```javascript
if(true){
    //TDZ开始 
    tmp = 'abc';//error
    console.log(tmp);//error
    
    let tmp ;//TDZ 结束
    console.log(tmp);//undefined
    tmp = 123;
    console.log(tmp)//123
}
```
上边代码中，在<code>let</code>命令声明变量<code>tmp</code>之前，都属于变量<code>tmp</code>的死区。

"暂时性死区"也意味着<code>typeof</code>不再是一个百分之百安全的操作
```javascript
 typeof x; //error
 let x;
```
[总结]上面代码中，变量<code>x</code>使用<code>let</code>命令声明，所以在声明之前都属于<code>x</code>的“死区”，只要用到该变量酒会报错。因此<code>typeof</code>运行时就会抛出一个<code>ReferenceError</code>
作为比较，如果一个变量根本没有被声明，使用<code>typeof</code>反而不会报错。
```javascript
    typeof undefind_value;//undefined
```


