# Symbol

ES6 引入一种新的原始数据类型 Symbol，表示独一无二的值，它是 javascript 语言的第七种数据类型，是一种类似于**字符串**的数据类型

## Symbol 的特点

1. Symbol 的值是唯一的，用来解决命名冲突的问题
2. Symbol 值不能与其他数据进行运算
3. Symbol 定义的对象属性不能使用 for...in 循环遍历，但是可以使用 Reflect.ownKeys 来获取对象的所有键名。

#### 创建 Symbol

```javascript
let t = Symbol();
console.log(t, typeof t); // Symbol() 'symbol'
// or
const t1 = Symbol("张三");
console.log(t1, typeof t1); // Symbol(张三) 'symbol'
```

### Symbol.for 创建

```javascript
const t2 = Symbol.for("李四");
console.log(t2, typeof t2); // Symbol(李四) 'symbol'
```

### 不能与其他数据进行运算

```javascript
let result = s + 100; // x
let result = s > 100; // x
let result = s + s; // x
```

## Symbol 的使用

### 给对象添加 Symbol 类型的属性

```javascript
let tools = {
  name: "工具箱",
  // up
  [Symbol("up")]: function () {
    console.log("++");
  },
  // down
  [Symbol("down")]: function () {
    console.log("--");
  },
};
console.log(tools);// {name: '工具箱', Symbol(up): ƒ, Symbol(down): ƒ}
```
