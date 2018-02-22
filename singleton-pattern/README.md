# 单例模式

单例顾名思义单个实例。当我们不断地产生各种实例后，希望产生过的实例就不再产生，这样就避免了浪费。比如网上买本js的书，过段时间我想再买书会check下是否买过，然后才继续购买。

在js中对象就是实例，而创建对象有好几种方式，我们分别来看一下

### 用Object()创建对象

```javascript
var o1 = new Object() // {}
var o2 = new Object({name: 'qiqi'}) // {name: 'qiqi'}
console.log(o1.__proto__ === Object.prototype) // true
console.log(o2.name) // qiqi
```

### 用Object.create()创建对象

```javascript
var o1 = Object.create(null) // {}
var o2 = Object.create({}) // {}
var o3 = Object.create({name: 'qiqi'})
console.log(o1.__ptoto__) // undefined
console.log(o2.__proto__.__proto__ === Object.prototype) // true
console.log(o3.__proto__.name) // qiqi
```

### 用对象字面量来创建对象

```javascript
var o1 = {} // {}
var o2 = {name: 'qiqi'}
console.log(o1.__proto__ === Object.prototype) // true
console.log(o2.name) // qiqi
```

### 用构造函数来创建对象

```javascript
function O1 (){
}
function O2 (name){
  this.name = name
}
var o1 = new O1()
var o2 = new O2('qiqi')
console.log(o1.__proto__.__proto__ === Object.prototype) // true
console.log(o2.__proto__.constructor === O2.prototype.constructor) // true
console.log(o2.name) // qiqi
```

由此可见用Object()和对象字面量来创建对象是类似的；用Object.create()创建的对象会多一层原型链，指向创建的对象；用构造函数创建的对象和Object()、对象字面量类似，虽然会多一层原型链，但这层是指向的它原型的构造函数。

### 单例实现

```javascript
var singleton1 = new Object({name: 'qiqi'})

var singleton2 = Object.create({name: 'qiqi'})

var singleton3 = {name: 'qiqi'}

var Singleton4 = function(name){
  if(Singleton4.instance){
     return Sington4.instance
  }
  this.name = name
  Singleton4.instance = this
}
var singleton4 = new Singleton4('qiqi')
```

使用以上1，2，3，4实例时都为单例模式，唯一的区别是第四种还有其他调用方式

```javascript
var singletonA = new Singleton4('qiqi')
var singletonB = new Singleton4('qiqi')
console.log(singletonA === singletonB) // true
```