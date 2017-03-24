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
    
```


