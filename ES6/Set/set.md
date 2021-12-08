# Set

ES6 提供新的数据结构 Set(集合)。它类似于数组，但成员的值都是唯一的，集合实现了`iterator`接口,所以可以使用`扩展运算符`和`for...of...`进行遍历。

## 集合的属性和方法

1. size 返回集合的元素个数
2. add 增加一个新元素，返回当前集合
3. delete 删除元素，返回 boolean 值
4. has 检验集合中是否包含某个元素，返回 boolean 值

```javascript
const s = new Set();
console.log(s); // Set(0)
console.log(typeof s); // object
```

## 使用

```javascript
// 使用 带有去重
const p = new Set(["张三", "李四", "王五", "张三"]);
console.log(p); // Set(3) {'张三', '李四', '王五'}
// 元素个数
console.log(p.size); // 3
// 添加新的元素
console.log(p.add("赵六")); // Set(4) {'张三', '李四', '王五', '赵六'}
// 删除元素
console.log(p.delete("张三")); // true
// 检测
console.log(p.has("李四")); // true
// 清空
p.clear();
console.log("p", p); // Set(0) {size: 0}
// for...of..
for (let i of p) {
  console.log(i); // 依次打印 张三 李四 王五 赵六
}
```

## 实践

```javascript
// 数组去重
const arr = [1, 2, 3, 4, 5, 6, 1, 1, 2, 3];
const newArr = [...new Set(arr)];
console.log("newArr ", newArr); // newArr  (6) [1, 2, 3, 4, 5, 6]

// 交集
const arr2 = [2, 7, 9, 3];
const result = [...new Set(arr)].filter((item) => {
  const test = new Set(arr2);
  if (test.has(item)) {
    return true;
  } else {
    return false;
  }
});
console.log("result", result); // result (2) [2, 3]

// 简化
const result = [...new Set(arr)].filter((item) => new Set(arr2).has(item));

// 并集
const union = [...new Set([...arr, ...arr2])];
console.log("union", union); // union (8) [1, 2, 3, 4, 5, 6, 7, 9]
// 差集
const unUnion = [...new Set(arr)].filter((item) => !new Set(arr2).has(item));
console.log("unUnion", unUnion); // unUnion (4) [1, 4, 5, 6]
```
