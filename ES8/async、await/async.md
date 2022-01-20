# es8

## async 和 await

async 和 await 两种语法结合可以让异步代码像同步代码一样

### async

1. async 函数的返回值为 promise 对象。
2. promise 对象大的结果由 async 函数执行的返回值决定

### await 表达式

1. await 必须写在 async 函数中
2. await 右侧的表达式一般为 promise 对象
3. await 返回的是 promise 成功的值
4. await 的 promise 失败了，就会抛出异常，需要通过 try...catch 捕获。

```javascript
// 普通函数
function fn() {}
console.log(fn); // ƒ fn() {}
// asnyc 函数
async function asyncFn() {}
console.log(asyncFn()); // Promise {<fulfilled>: undefined}
```

返回的结果不是一个 promise 类型的对象，则这个函数的返回结果就是一个成功的 promise

```javascript
// if undefined
async function asFn() {
  return;
}
//[[Prototype]]: Promise
//[[PromiseState]]: "fulfilled"
//[[PromiseResult]]: undefined
console.log(asFn());

// if 抛出错误
async function errFn() {
  // throw new Error("报错");
}
// [[Prototype]]: Promise
// [[PromiseState]]: "rejected"
// [[PromiseResult]]: Error: 报错 at errFn
console.log(errFn());

// if return promise对象
async function PormiseFn() {
  return new Promise((resolve, rejected) =>
    resolve("成功");
  });
}
// [[Prototype]]: Promise
// [[PromiseState]]: "fulfilled"
// [[PromiseResult]]: "成功"
console.log(PormiseFn());
```

当我们调用 then 方法

```javascript
async function PormiseFn() {
  return new Promise((resolve, rejected) => {
    // resolve("成功");// 走success
    rejected("失败"); // 走fail
  });
}
PormiseFn().then(
  (success) => {
    console.log(success);
  },
  (fail) => {
    console.warn(fail);
  }
);
```
