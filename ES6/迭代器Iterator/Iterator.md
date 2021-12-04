# 迭代器

迭代器是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作。

1. ES6 有一种新的遍历命令 for...of 循环，Iterator 接口主要供 for...of 消费
2. 原生具备 Iterator 接口（这里的接口，指的是对象里的一个属性 Symbol.Iterator）的数据（可用 for...of 遍历）

- Array
- Arguments
- Set
- Map
- String
- TypedArray
- NodeList

## 工作原理

```javascript
const list = ["张三", "李四", "王五"];
for (let i of list) {
  console.log(i); // 张三 李四 王五
}
console.log("list", list); // Symbol(Symbol.iterator): ƒ values()
```

这里 Symbol(Symbol.iterator)对应的值是一个函数。

为什么能够实现迭代（遍历）？

1. 创建一个指针对象，指向当前数据结构的起始位置

`Symbol(Symbol.iterator)`对应的函数创建一个对象

```javascript
const iterator = list[Symbol.iterator]();
console.log(iterator); // next: ƒ next()
```

2. 第一次调用对下的 next 方法，指针自动指向数据结构的第一个成员

```javascript
// next 调用对象next方法
console.log(iterator.next()); // {value: '张三', done: false}
```

3. 接下来不断调用 next 方法，指针一直往后移动，直到指向最后一个成员
4. 每调用 next 方法返回一个包含 value 和 done 属性的对象

```javascript
// if
console.log(iterator.next()); // {value: '张三', done: false}
console.log(iterator.next()); // {value: '李四', done: false}
console.log(iterator.next()); // {value: '王五', done: false}
console.log(iterator.next()); // {value: undefined, done: true} done代表是否完成时
```

### 案例

**需要自定义遍历数据的时候，要想到迭代器**

```javascript
const s11 = {
  name: "EDG",
  stus: ["Flandre", "Jiejie", "Scout", "Viper", "Meiko"],
};
// 需求：使用for...of来遍历，每次返回的是stus数组的成员
// 遍历对象
for (let v of s11) {
  console.log(v); // Uncaught TypeError: s11 is not iterable
}
```

错误

错误原因对照第一条：创建一个指针**对象**，指向当前数据结构的起始位置

```javascript
const s11 = {
  name: "EDG",
  stus: ["Flandre", "Jiejie", "Scout", "Viper", "Meiko"],
  [Symbol.iterator]() {},
};
for (let v of s11) {
  // Uncaught TypeError: Result of the Symbol.iterator method is not an object
  console.log(v);
}
```

错误

错误原因对照第二条：第一次调用对下的 **next** 方法，指针自动指向数据结构的第一个成员

```javascript
const s11 = {
  name: "EDG",
  stus: ["Flandre", "Jiejie", "Scout", "Viper", "Meiko"],
  [Symbol.iterator]() {
    return {};
  },
};
for (let v of s11) {
  // index.html:56 Uncaught TypeError: undefined is not a function
  console.log(v);
}
```

错误

错误原因对照第四条：每调用 next 方法返回一个包含 **value** 和 **done** 属性的对象

```javascript
const s11 = {
  name: "EDG",
  stus: ["Flandre", "Jiejie", "Scout", "Viper", "Meiko"],
  [Symbol.iterator]() {
    return {
      next: function () {},
    };
  },
};
for (let v of s11) {
  // index.html:72 Uncaught TypeError: Iterator result undefined is not an object
  //意思是next返回接口是undefined
  console.log(v);
}
```

错误

错误原因 每次调用 next 方法，返回 false

```javascript
const s11 = {
  name: "EDG",
  stus: ["Flandre", "Jiejie", "Scout", "Viper", "Meiko"],
  [Symbol.iterator]() {
    return {
      next: function () {
          value:'123',
          done:false
      },
    };
  },
};
for (let v of s11) {
    // 死循环
  console.log(v);
}
```

正确

```javascript
const s11 = {
  name: "EDG",
  stus: ["Flandre", "Jiejie", "Scout", "Viper", "Meiko"],
  [Symbol.iterator]() {
    // 索引变量
    let index = 0;
    return {
      next: () => {
        if (index < this.stus.length) {
          const result = { value: this.stus[index], done: false };
          // 下标自增
          index++;
          // 返回结果
          return result;
        } else {
          return { value: undefined, done: true };
        }
      },
    };
  },
};
for (let v of s11) {
  console.log(v);  // 依次输出 Flandre Jiejie Scout Viper Meiko
}
```
