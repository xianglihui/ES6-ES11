# rest

es6 引入 rest 函数，用于获取函数实参，用来代替 argument

```javascript
// es5获取实参
function data() {
  console.log(arguments); // Arguments(3) ['张三', '李四', '王五', callee: ƒ, Symbol(Symbol.iterator): ƒ] constructor: ƒ Object()
}
data("张三", "李四", "王五");
```

**arguments 拿到的时 Object()**

```javascript
// es6获取实参,rest参数必须放到参数的最后
function data2(...args) {
  console.log(args);
}
data2("张三", "李四", "王五"); // ['张三', '李四', '王五'] constructor: ƒ Array()
```
es5`arguments`拿到的是`Object`，es6`rest`拿到的是数组。