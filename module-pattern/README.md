# 模块化模式

```javascript
var person = (function(){
    var _name = 'qiqi'
    var _greeting = function(text){
        return text + ' ' + _name
    }
    var hello = function(){
        console.log(_greeting('hello'))
    }
    var hi = function(){
        console.log(_greeting('hi'))
    }
    var setName = function(newName){
        _name = newName
    }
    return {
        hello: hello,
        hi: hi,
        setName: setName
    }
})()

person.setName('supershy')
person.hello() // hello supershy
person.hi() // hello supershy
```

模块化模式可以提供私有和公共的变量及方法，可以减少命名冲突。

上面的写法其实是模块化模式的最佳实践，优点如下：

* 所有的方法/变量都外部不能直接访问，需要通过return对外提供公开的方法/变量
* 私有方法/变量通过下划线_来作为首字符，以便和公开的方法/变量做区分（上面的例子中\_name和\_greeting我们可以一目了然地知悉他们是私有的）
* 在return处对应的方法/变量都是一行，方便阅读

### 原理剖析：IIFE

使用了自执行函数（Immediately Invoked Function Expression）如下

```javascript
(function(){
    // some code
})()
```

###原理剖析：闭包

当函数A的内部定义了一个函数B，此时B可以访问A中的变量/方法，当函数A返回函数B的引用时，此时形成了闭包。

```javascript
function getCount(){
    var count = 0;
    return {
        addCount: function(){
            count++
            console.log(count)
        }
    }
}

getCount().addCount() // 1
getCount().addCount() // 1

var add = getCount()
add.addCount() // 1
add.addCount() // 2
```

这是因为将getCount()的返回值即addCount的引用给了add，而add作用域链和addCount的不同，此时变量count会在内存中，当不断地调用add后count因为在内存中没有被销毁，所以会一直递增。

### 原理结合：

利用闭包这个特性并结合自执行函数我们就得到了模块化模式的雏形

```javascript
var person = (function(){
    return {}
})()
```

### 总结

* 让方法/变量具有私有的功能
* 代码结构清晰，通过return来对外提供公开的方法/属性

### demo

使用模块化模式重构了[todo](https://jsfiddle.net/supershy/xh2egfvb/)





