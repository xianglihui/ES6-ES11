# Promise

promise 是 ES6 引入的异步编程的新解决方案，语法上 Promise 是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果。

```javascript
// if true
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    let data = "服务端数据";
    // 调用resolve表示成功
    resolve(data);
  }, 1000);
});

// 成功并可调用.then方法，then接收两个参数，都是函数
p.then(
  // 成功的形参一般叫value,调用resolve后会被执行
  (value) => {
    console.log("value", value); // 服务端数据
  },
  // 失败的应参一般叫reson
  (reson) => {}
);
```

```javascript
// if false
const b = new Promise((resilve, reject) => {
  setTimeout(() => {
    let error = "服务端报错";
    reject(error);
  }, 1000);
});

b.then(
  // 成功的形参一般叫value,调用resolve后会被执行
  (value) => {},
  // 失败的应参一般叫reson,调用reject后会被执行
  (reson) => {
    console.log("reson", reson); // 服务端数据
  }
);
```
