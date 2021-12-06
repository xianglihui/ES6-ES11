# 迭代器

迭代器是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作。

1. ES6 有一种新的遍历命令 for...of 循环，Iterator 接口主要供 for...of 消费
2. 原生具备 Iterator 接口（这里的接口，指的是对象里的一个属性 Symbol.Iterator）的数据（可用 for...of 遍历）

## 什么是迭代（遍历器）

在 es5 中，最常使用表示“集合”的数据结构主要是数组（Array）和普通对象（Object），这些“集合”类元素都是由一系列的成员构成的，在日常工作中，都会有访问“集合”中每一个成员的需求。

### es5

数组（Array）成员主要通过 for 循环或原型方法 forEach,map 等方法来遍历，而对象（Object）则没有方法直接遍历（for … in、Object.keys()等都是在遍历 key，而不是 value）

### es6

新增的 Map 和 Set 提供了统一的遍历机制：遍历器（Iterator），并新增了 for … of 语法来使用遍历器。

- Array
- Arguments
- Set
- Map
- String
- TypedArray
- NodeList

## 工作原理

```javascript
// 使用for … of 语法来实现遍历器
const list = ["张三", "李四", "王五"];
for (let i of list) {
  console.log(i); // in proper order console 张三 李四 王五
}
// console
console.log("list", list);
// > [[Prototype]]
//  > Symbol(Symbol.iterator): ƒ values()
```

打印 list，你可以在原型对象`Prototype`下看见`Symbol(Symbol.iterator): ƒ values()`
,(`Symbol.iterator`定义一个对象的遍历器方法。凡是具有[Symbol.iterator]方法的对象都是可遍历的，可以使用 for … of 循环依次输出对象的每个属性)

### 为什么能够实现迭代（遍历）？

1. 创建一个指针对象，指向当前数据结构的起始位置

`Symbol(Symbol.iterator)`对应的函数创建一个对象

```javascript
const list = ["张三", "李四", "王五"];
const iterator = list[Symbol.iterator](); // Symbol(Symbol.iterator)对应的值是一个函数,这里属于函数调用
console.log(iterator);
// > [[Prototype]]: Array Iterator
//  > next: ƒ next()
// next方法，每次调用该方法可以依次枚举目标“集合”成员
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
  console.log(v); // 依次输出 Flandre Jiejie Scout Viper Meiko
}
```
