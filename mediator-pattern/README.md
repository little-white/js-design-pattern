# 中介者模式

在大型应用中数据的状态是非常多的，经常有多个组件共享或改变某个数据状态，一些优秀的框架如react搭配redux、vue搭配vuex来控制整个的整个状态树，这样组件之间就降低了耦合，而通过这个状态树来控制不同组件的数据状态，继而改变对应的view。

记得之前看过有人把mvc中的controller看成是中介者，我觉得非常形象，因为view的数据展示不关心是哪个model传来的，model也不关心是哪个view传来数据，统一交给controller就搞定了。当然生活中机场调度室、房屋中介、银行等等都可以看成是中介者。

用mvc来举例吧

### model

```javascript
var model = {
  getUsers: function(){
    return 'users data'
  },
  getBooks: function(){
    return 'books data'
  },
  addUser: function(data){
    console.log('set ' + data)
  }
}
```

### view

```javascript
var view = {
  showUsersPage: function(data){
    console.log('users page: ' + data)
  },
  showBooksPage: function(data){
    console.log('books page: ' + data)
  },
  show404Page: function(){
    console.log('page not found')
  },
  addUser: function(user){
    return user
  }
}
```

### controller(mediator)

```javascript
var mediator = {
  route: function(option){
    switch(option.url){
      case '/users':
        if(option.method === 'post'){
          model.addUser(view.addUser(option.data))
        } else {
          view.showUsersPage(model.getUsers())  
        }
        break;
      case '/books':
        view.showBooksPage(model.getBooks())
        break;
      default:
        console.log(view.show404Page())
    } 
  }
}

mediator.route({
  url: '/users'
}) // users page: users data
mediator.route({
  url: '/users',
  method: 'post',
  data: 'user: qiqi'
}) // set user: qiqi
mediator.route({
  url: '/books'
}) // books page: books data
mediator.route({
  url: '/abc'
}) // page not found
```

当然上面的例子写的简单又简陋，不过它展示了通过访问不同的url，mediator是如何控制model层的数据传递给对应的view层，view层的请求又如何让model层的数据发生改变。但也由于mediator是最核心的部分，它的职责越来越大，让代码体积变得越来越大，较难维护。