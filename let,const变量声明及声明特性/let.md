# 声明变量

```javascript
//声明变量
let a;
// or
let b, c, d;
// or
let e = [];
```

# 声明变量的特性

1. 变量不能重复声明

```javascript
// Uncaught SyntaxError: Identifier 'name' has already been declared
let name = "张三";
let name = "李四";
```

2. 块级作用域

在 es5 中有三种作用域，分别是全局，函数，eval（严格模式开启）

3. 不存在变量提升

```javascript
// ReferenceError: Cannot access 'name' before initialization
console.log("name", name);
let name = "张三";
```

4. 不影响作用域链
