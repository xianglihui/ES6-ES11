1. 声明常量

```javascript
const name = "张三";
```

2. 一定要赋初始值

```javascript
const a;
// Uncaught SyntaxError: Missing initializer in const declaration
```

- 一般常量使用大写

```javascript
const AGE = 12;
```

3. 常量不能修改

```javascript
const b = 1;
const b = 2;
// Uncaught SyntaxError: Identifier 'b' has already been declared
```

4. 对于数组和对象的元素修改，不算做对常量的修改，不会报错

```javascript
const list = ["张三", "李四"];
list.push("王五");
// list (3) ['张三', '李四', '王五']
```

虽然数组元素发生改变，但是 list 常量指向的地址没有发生改变。
