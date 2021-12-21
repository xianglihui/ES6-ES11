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

```javascript
// es5
// 构造函数Phone本身也是个对象
function Phone() {}
// 函数对象上添加属性
Phone.name = "apple";
// 函数对象上添加方法
Phone.change = function () {
  console.log("13香");
};
const apple = new Phone();
console.log(apple.name); // undefined
// 实例上面是没有函数对象身上的属性和方法
// 函数对象是函数对象，实例是实例，实例只跟构造函数原型对象上属性方法有关系
// such as
Phone.prototype.price = "7999";
console.log(apple.price); // 7999
```

`name`和`change`是属于**函数对象**的,并不属于实例对象。`name`和`change`这样的属性，就被称为静态成员。在面向对象领域，它们是属于类的，不属于实例对象。

```javascript
// 静态属性
class Person {
  static name = "张三";
  static watch() {
    console.log("电视");
  }
}
const pesron = new Person();
console.log(pesron.name); // undefined
console.log(Person.name); // 张三
```

## 构造函数继承

es5

```javascript
// es5
// 父类
function Phone(brand, price) {
  this.brand = brand;
  this.price = price;
}
Phone.prototype.call = function () {
  console.log("打电话");
};
// 子类
function MobilePhone(brand, price, color, size) {
  // 通过Phone.call改变Phone的this指向,传入this(这个this指向MobilePhone中的this,也就是MobilePhone的实例对象)
  Phone.call(this, brand, price);
  // 特性
  this.color = color;
  this.size = size;
}
// 设置子类构造函数的原型
MobilePhone.prototype = new Phone();
// 声明子类的方法
MobilePhone.prototype.photo = function () {
  console.log("咔擦");
};
MobilePhone.prototype.playGame = function () {
  console.log("打豆豆");
};
const xiaomi = new MobilePhone("小米", 1999, "白色", "5");
console.log(xiaomi);
```
