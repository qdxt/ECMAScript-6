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

