# Map

ES6 提供了 Map 数据结构。它类似与对象，也是键值对的集合。但是'键'的范围不限于字符串，各种类型的值(包括对象)都可以当作'键'。Map 也实现了 iterator 接口，所以可以使用`扩展运算符`和`for...of...`进行遍历。

## Map 属性方法

1. size 返回 Map 的元素个数
2. set 增加一个新元素，返回当前 Map
3. get 返回键名对象的键值
4. has 检测 Map 中是否包含某个元素，返回 boolean 值
5. clear 清空集合，返回 undefined

## 使用

```javascript
const a = new Map();
console.log(a, typeof a); // Map(0) {size: 0}[[Entries]]No propertiessize: 0[[Prototype]]: Map 'object'
```

```javascript
// set 添加元素
a.set("name", "张三");
// 0: {"name" => "张三"}
//   key: "name";
//   value: "张三";
console.log("a", a);
// or
a.set("change", () => {
  console.log("你好");
});
console.log("a", a); // key: "change" value: () => { console.log("你好"); }
// or
let key = {
  shool: "985，211",
};
a.set(key, ["北大", "复旦"]);
console.log("a", a); // key: {shool: '985，211'} value: (2) ['北大', '复旦']

// size 获取个数
console.log(a.size); // 3

// delete 输入key
console.log(a.delete("name")); // true

// get 获取
console.log(a.get("change")); // () => {console.log("你好

// clear 清空
a.clear();
console.log(a); //Map(0) {size: 0}
```
