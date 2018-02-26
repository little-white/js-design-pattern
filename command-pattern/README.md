# 命令模式

命令模式是典型的动词，将函数的请求、调用封装成对象，然后再进行处理。

```javascript
var shop = {
    orderMap: {},
    receiveOrderOfUser: function(user){
        this.orderMap[user.name] = {
            order: user.order
        }
        console.log(user.name + '的' + user.order.name + '已发货')
    },
    cancelOrderOfUser: function(user){
        delete this.orderMap[user.name]
        console.log(user.name + '的' + user.order.name + '已退货')
    }
}

shop.excute = function(user){
  	this[user.request](user)
}

var user = {
    init: function(request, name, order){
        this.request = request
        this.name = name
        this.order = order
    }
}

user.init('receiveOrderOfUser', 'qiqi', {name: '衣服'})
shop.excute(user) // qiqi的衣服已发货
user.init('cancelOrderOfUser', 'qiqi', {name: '衣服'})
shop.excute(user) // qiqi的衣服已退货
```

目测除了根据动作来执行，其它没看出它的作用。

先这样。。。