# 构造函数模式

```javascript
function Person(name){
    this.name = name

    this.hello = function(){
        console.log('hello ' + this.name)
    }
}
var person = new Person('qiqi')
```

用new关键字来调用的函数称之为构造函数，构造函数中的this指向new出来的实例

```javascript
// 构造函数中的this === Person的实例
// 在Person中打印
console.log(this instanceof Person) // true
```

可以看出构造函数定义了一种对象，它可以包含属性和方法。

下面我们稍微做一些优化，因为方法hello在每次new的时候都会重新创建，造成了资源的浪费。

### 优化1

```javascript
function Person(name){
    this.name = name
    this.hello = hello
}

function hello(){
    console.log('hello ' + this.name)
}

var personA = new Person('A') // 创建了hello方法，即为此开辟了新空间
var personB = new Person('B') // 同上
```

#####结论

personA和personB都会创建hello方法
personA和personB共享了hello这个函数，节省了资源

### 优化2

```javascript
function Person(name){
    this.name = name
    this.hello = hello
}

Person.prototype.hello = function(){
    console.log('hello ' + this.name)
}

var personA = new Person('A') // 没有创建hello
var personB = new Person('B') // 同上

console.log(personA.__proto__ === Person.prototype) // true
console.log(personA.__proto__ === Person.prototype) // true
```

##### 结论

personA和personB并没有创建hello函数

他俩的hello方法是通过原型链来得到的，因为实例后的对象的原型链(在chrome中是\_\_proto\_\_)指向了构造函数的prototype

### return陷阱

```javascript
function Person(name){
    var type
    this.name = name
    return type // 不写return会默认return this，当type为对象时，构造函数实例出来的对象会指向这个对象，当是其它类型的话还是指向this
}

var personA = new Person('A') // type为{}，p为{}
var personB = new Person('B') // type为123等基本类型时，p为{name: 'B'}
```

##### 陷阱利用

可以利用return 对象会返回这个对象的特性，我们可以使用另外种形式来写构造函数

```javascript
function Person(name){
    var tempObj = {}
    tempObj.name = name
    tempObj.hello = function(){
        console.log('hello ' + this.name)
    }

    return tempObj
}
```

此时new Person('qiqi')返回的实例和Person('qiqi')return出来的对象就没啥区别了，因为this压根没用到。

### 总结

通过new一个构造函数可以返回新的对象

可以通过prototype来共享方法

要知道在构造函数中return的陷阱