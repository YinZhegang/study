## let 和 const 命令
###  let 命令
* 只在let命令所在的代码有效
> let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块即{}内有效。
```javascript
{
  let a = 1;
  var b = 2;
}
console.log(a) // ReferenceError: a is not defined.
console.log(b) // 2
```
* 不存在变量提升
>var命令会发生“变量提升”现象，即变量可以在声明之前使用，值为undefined。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。
```javascript
// var 的情况
console.log(a); // 输出undefined
var a = 2;

// let 的情况
console.log(b); // 报错ReferenceError
let b = 2;
```
* 暂时性死区
>只要块级作用域内存在let命令，它所声明的变量就“绑定”这个区域，不再受外部的
```javascript
var a = 123;
if (true) {
  a = 'abc'; // ReferenceError
  let a;
}
```
* 不允许重复声明
> let不允许在相同作用域内，重复声明同一个变量。
***  
* const 命令

>const 声明一个只读的常量。一旦声明，常量的值就不能改变。const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

## 变量的解构赋值
* 数组的解构赋值
>数组中提取值，按照对应位置，对变量赋值
```javascript
let [a, b, c] = [1, 2, 3];
```
>嵌套数组进行解构的例子
```javascript
let [ , , x] = ["a", "b", "c"];
console.log(x) // c
let [head, ...end] = [1, 2, 3, 4];
console.log(head)  // 1
console.log(end) // [2,3,4]
```
>解构赋值允许指定默认值。
 ```javascript
let [x, y = 'b'] = ['a']; // x='a', y='b'
```
* 对象解构赋值
>对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
 ```javascript
let { a, b } = { a: 'aaa', b: 'bbb' };
console.log(a) // "aaa"
console.log(b) // "bbb"
```
实际上面的写法等于下面这个{ a, b }是 { a:a, b:b }的简写形式
 ```javascript
let { a:a, b:b } = { a: 'aaa', b: 'bbb' };
console.log(a) // "aaa"
console.log(b) // "bbb"
```
>对象的解构也可以指定默认值。
```javascript
 var {a = 3} = {};
 console.log(a) // 3
 
 var {a, b = 2} = {x: 1};
 console.log(a) // 1
 console.log(b) // 2
```
* 函数参数的解构赋值
>函数的参数使用解构赋值是平时最常见的用法。用法类似数组和对象结构赋值，包括默认值，非常好用。
```javascript
function test([x = 5, y]){
  return x + y;
}

test([1, 2]); // 3
test([, 2]); // 7

function test2({x = 0, y = 0} = {}) {
  return [x, y];
}

test2({x: 3, y: 8}); // [3, 8]
test2({x: 3}); // [3, 0]
test2({}); // [0, 0]
test2(); // [0, 0]
```
### 数组和对象扩展运算符
>扩展运算符（spread）是三个点（...），将一个数组或对象转为用逗号分隔的参数序列。
* 数组扩展运算符
```javascript
  console.log(...[1, 2, 3])
  // 1 2 3
  console.log(1, ...[2, 3, 4], 5)
  // 1 2 3 4 5
  //用于函数参数
  function push(array, ...items) {
    array.push(...items);
  }
  function add(x, y) {
    return x + y;
  }
  const numbers = [4, 38];
  add(...numbers) // 42
```
 * 对象扩展运算符

关于属性简写
 
 ```javascript
 //属性简写
const a = 'a';
const b = {a};
console.log(b) // {foo: "bar"}
// 等同于
const b = {a: a};

//方法简写。
let c = {
  method() {
    return "Hello!";
  }
};
// 等同于
let c = {
  method: function() {
    return "test!";
  }
};
```
>对象的扩展符用于从一个对象取值，相当于将目标对象自身的所有可遍历的
```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
console.log(x) // 1
console.log(y) // 2
console.log(z) // { a: 3, b: 4 }
```
### Symbol
***
>ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型
```javascript
let s = Symbol();
typeof s
// "symbol"
let a = Symbol('foo');
let a = Symbol('bar');
console.log(Symbol('test') == Symbol('test')) //fase
a // Symbol(foo)
b // Symbol(bar)

a.toString() // "Symbol(foo)"
b.toString() // "Symbol(bar)"
```
>作为属性名的 Symbol 
```javascript
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```
### Set 和 Map 数据结构
***
* Set
>ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
```javascript
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
//数组去重
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]
```
>Set 实例的属性和方法
+ Set.prototype.constructor：构造函数，默认就是Set函数。
+ Set.prototype.size：返回Set实例的成员总数。
+ Set.prototype.add(value)：添加某个值，返回 Set 结构本身。
+ Set.prototype.delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
+ Set.prototype.has(value)：返回一个布尔值，表示该值是否为Set的成员。
+ Set.prototype.clear()：清除所有成员，没有返回值。

```javascript
let a =new Set();
a.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false
```
>Set 遍历操作 
+ Set.prototype.keys()：返回键名的遍历器
+ Set.prototype.values()：返回键值的遍历器
+ Set.prototype.entries()：返回键值对的遍历器
+ Set.prototype.forEach()：使用回调函数遍历每个成员
```javascript
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```
* Map
>JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。
```javascript
const m = new Map([['name','yzg'],['age',25]]);
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
m.has('name')  // true
m.get('age') //25
```
>Map 遍历操作 
+ Map.prototype.keys()：返回键名的遍历器
+ Map.prototype.values()：返回键值的遍历器
+ Map.prototype.entries()：返回键值对的遍历器
+ Map.prototype.forEach()：使用回调函数遍历每个成员
