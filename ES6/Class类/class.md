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

函数对象与实例对象属性是不相通的

```javascript
function phone() {}
phone.change = function () {
  console.log("我可以打电话");
};
phone.name = "小米";
// 构造函数
const Phone = new phone();
console.log("Phone", Phone.name); // undefined
// 普通函数
console.log("Phone", phone.name); // phone

// prototype
phone.prototype.size = 5;
// class是个语法糖，本质是基于原型链
console.log(Phone.size); // 5
```

`change`与`name`是属于函数对象`phone`的,不属于实例对象`Phone`,`change`与`name`属于静态**静态资源**，换到面向对象时，`change`与`name`属于类

```javascript
// class
class newPhone {
  // 静态资源
  static name = "华为";
  static change() {
    console.log("5G");
  }
}
const Huawei = new newPhone();
console.log(Huawei.name); // undefined
console.log(newPhone.name); // 华为
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

es6

```javascript
// 父类
class Phone {
  // 构造方法
  constructor(brand, price) {
    this.brand = brand;
    this.price = price;
  }
  // 父类方法
  call() {
    console.log("call");
  }
}
// 子类 继承父类 关键词 extends
class MobilePhone extends Phone {
  constructor(brand, price, color, size) {
    // 相当于父类构造器
    super(brand, price); // Phone.call(this,brand,price)
    this.color = color;
    this.size = size;
  }
  photo() {
    console.log("photo");
  }
}
// 实例化
const xiaomi = new MobilePhone("小米", 1999, "白色", 5);
console.log(xiaomi);
```

es6 重写父类方法

```javascript
class MobilePhone extends Phone {
  constructor(brand, price, color, size) {
    // 相当于父类构造器
    super(brand, price); // Phone.call(this,brand,price)
    this.color = color;
    this.size = size;
  }
  photo() {
    console.log("photo");
  }
  call() {
    console.log("call2");
  }
}
const xiaomi = new MobilePhone("小米", 1999, "白色", 5);
xiaomi.call(); // call2
```

## set 与 get

```javascript
class Phone {
  get price() {
    console.log("价格被读取");
    return "123";
  }
  set price(newVal) {
    console.log("价格被修改了");
  }
}
const xiaomi = new Phone();
// 打印了两个值，一个是价格被读取，一个是undefined
// 对price这个属性的读取，会执行price函数里面的代码，若调用retrun，返回的就是这个属性的值
console.log(xiaomi.price); // 123
xiaomi.price = "free"; // 价格被修改了
```
