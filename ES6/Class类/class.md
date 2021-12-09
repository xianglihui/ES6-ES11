# class 类

ES6 提供更加接近面向对象的写法，引入 Class（类）这个概念，作为对象的模板。通过 class 关键字，可以定义类，基本上，ES6 的 class 可以看作只是一个语法糖，它的绝大部分功能，ES5 都能做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

## 知识点

1. class 声明类
2. constructor 定义构造函数初始化
3. extends 继承父类
4. super 调用父级构造方法
5. static 定义静态方法和属性
6. 父类方法可以重写

## 基本使用

```javascript
// es5
function Phone(name, price) {
  this.name = name;
  this.price = price;
}
//添加方法
Phone.prototype.call = function () {
  console.log("执行call方法");
};
let phone = new Phone("小米", 1999);
console.log(phone); // name: "小米" price: 1999 call: ƒ ()

// es6
class XiaoMi {
  // constructor命名固定，调用new XiaoMi时会调用
  constructor(type, price) {
    this.type = type;
    this.price = price;
  }
  // 方法
  call() {
    console.log("小米手机就是牛");
  }
}
let xiaomi = new XiaoMi("小米11", 1999); // XiaoMi {type: '小米11', price: 1999}
console.log(xiaomi);
```

## 类的静态成员
