# 原型模式

原型模式就是创建了一个新的对象，这个新对象指向原对象的原型。

### 方法一

```javascript
function createObject(obj){
    function F(){}
    F.prototype = obj
    
    return new F()
}

var person = {
  name: 'qiqi',
  hi: function(){
    console.log('hi ' + this.name)
  }
}
var personClone = createObject(person)
console.log(personClone.name) // qiqi
personClone.hi() // hi qiqi
```

如果此时我们希望修改personClone的值会怎样呢？

```javascript
personClone.name = 'supershy'

console.log(personClone.name) // supershy
personClone.hi() // hi supershy
console.log(personClone.__proto__.name) // qiqi
console.log(person.name) // qiqi
```

这是因为我们新建的personClone对象是它的原型链指向person，而本身是没有name属性和hi方法的，当给personClone的name属性赋值其实是给它创建了个属性name并赋值，而它原型链上的name并没有发生变化。

### 方法二

```javascript
// person的创建同上
var personClone = Object.create(person)

console.log(personClone.name) // qiqi
personClone.hi() // hi qiqi
```

效果和方法一一致

### 参考资料

[The Prototype Pattern](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#prototypepatternjavascript)