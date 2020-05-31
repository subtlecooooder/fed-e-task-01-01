# 模块1作业

## 1. 请说明执行结果并解释原因
- 题目
```javascript
var a = []
for(var i = 0; i < 10; i++) {
  a[i] = function() {
    console.log(i)
  }
}

a[6]()
```
- 答案
```javascript
10
```
- 解析
> i 定义的是一个全局变量，i++只是对同一个i进行不断地重新赋值，所以当循环结束时，i=10。而a[i]方法并不是立即执行，所以当在外部执行时，引用的i值就是内存中的10.


## 2. 请说明执行结果并解释原因
- 题目
  ```javascript
  var tmp = 123
  if(true) {
    console.log(tmp)
    let tmp
  }
  ```
- 答案
  ```javascript
  报错：Cannot access 'tmp' before initialization
  ```
- 解析
  > let声明的是块级作用域变量，且不会像var声明的变量会自动提升，这里的tmp被绑定在了块级作用域中，所以在打印语句执行时tmp变量还未被声明，导致报错

## 3. 结合ES6新语法，用最简单的方式找出数组中的最小值
- 题目
  ```javascript
  var arr = [12, 34, 32, 89, 4]
  ```
- 答案
  ```javascript
  Math.min(...arr)
  ```
- 解析
  > 通过扩展运算符将数组拆分成多个参数传给Math.min方法

## 4. 详细说明var，let，const三种声明变量的方式之间的具体差别
- 答案
  1. var声明变量为全局变量，且会变量提升，也就是说，变量可在声明之前使用。同时，var定义的变量允许重复声明，会导致变量被覆盖从而引发意料之外的问题。
  2. ES6中引入块级作用域。let声明的就是块级作用域变量，用let声明的变量只能在该作用域内引用，并且不允许被重复声明。
  3. const在let的基础上增加了[只读]属性，变量声明后不允许再被修改。这里的不能修改只是不能再被指向新的内存地址，const声明的对象变量中的属性可修改

## 5. 下列代码的输出结果并解释原因
- 题目
  ```javascript
  var a = 10
  var obj = {
    a: 20,
    fn() {
      setTimeout(() => {
        console.log(this.a)
      })
    }
  }

  obj.fn()
  ```
- 答案
  ```javascript
  20
  ```
- 解析
  > 箭头函数中的this总是指向外层调用者，也就是fn被调用时所在的作用域。这边是通过obj引用fn方法，所以打印结果也就是obj对象中a属性。

## 6. 简述Symbol类型的用途
- 答案
  > 通过Symbol创建的值都是唯一的，所以可用于创建对象的私有成员（外部无法访问），目前最主要的作用是为对象添加独一无二的属性名。
  ```javascript
  console.log(Symbol() === Symbol())// false
  console.log(Symbol('abc'))// 接受一个字符串作为值的描述文本
  console.log(Symbol('abc') === Symbol('abc'))// false

  const name = Symbol()
  const obj = {
    [name]: 'tom',
    age: 18
  }
  console.log(obj)// { age: 18, [Symbol()]: 'tom' }
  console.log(obj[name])// tom
  // 注意：该文件外部将无法再访问name属性
  ```

## 7. 说说什么是浅拷贝，什么是深拷贝
- 答案
  > 浅拷贝：只复制指向某个对象的指针，而不复制对象本身，新旧对象共享一块内存，也就是说，修改新对象后也会对旧对象产生影响；

  > 深拷贝：复制并创建一个一摸一样的对象，不共享内存，修改新对象，旧对象保持不变。

  > 目前的很多方法类似于数组的slice、concat方法和对象的assign方法都无法对引用对象的变量和属性进行深拷贝。

  >深拷贝的实现原理是定义一个新的对象，遍历源对象的属性并赋给新对象的属性。
  
  >之前传统的实现深拷贝的方式是使用JSON.stringify/parse方法。先通过stringify方法将一个js值转成json字符串，再通过parse方法将其转为js值。在es6之后，我们可以通过扩展运算符轻松实现。

## 8. 如何理解js异步编程的，Event Loop是做什么的，什么是宏任务，什么是微任务？
- 答案
  > 异步编程： Js是一门「单线程」编程语言。所谓「单线程】，就是指一次只能完成一件任务。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务，这样一来，当某个任务执行时间过长时，会拖延整个程序的运行。异步编程其实就是延迟处理，**比如当我路过一家书店想买一本书，老板告知我这本书明天才能到，那么我不可能在书店等到明天，就拜托老板明天书到了通知我，我再来购买。**

  > Event Loop就是一种主线程循环不断地从任务队列中读取事件的运行机制。
  `Call Stack => Web APIs => Queue => Call Stack ...`，主线程运行时，会产生**Call Stack（栈）**，stack中的代码会调用**Web APIs**,他们会在**Queue（任务队列）**中加入各种事件。只要**Call Stack（栈）**中的代码执行完毕，主线程就会去读取**Queue（任务队列）**，依次执行那些事件所对应的回调函数，循环往复。同时，**Call Stack（栈）**中的代码（同步任务），总是在读取**Queue（任务队列）**（异步任务）之前执行。

  > 宏任务可以理解为一个银行柜台办理业务，每个人都排着队等待解决业务，当来了一个人也要解决业务时，他不能插队，只能去队伍的末尾排队一起等待

  >微任务还是可以理解为一个银行柜台办理业务，比如我排队是为了存钱，存完钱之后我想再办张信用卡，柜员不可能让我重新排队，而是顺手就帮我一起办了。这边的办卡就是**微任务**，**在当前的微任务没有执行完成时，是不会执行下一个宏任务的。**

  ## 9. 将下面的异步代码使用Promise改进
  - 题目
    ```javascript
    setTimeout(function() {
    var a = 'hello'
    setTimeout(function() {
      var b = 'lagou'
        setTimeout(function() {
          var c = 'I ❤️ U'
          console.log(a+b+c)
        })
      })
    })
    ```
  - 答案
    ```javascript
    Promise.resolve()
      .then((resolve) => {
        setTimeout(() => {
          console.log('hello')
        },10)
      })
      .then((resolve) => {
        setTimeout(() => {
          console.log('lagou')
        },10)
      })
      .then((resolve) => {
        setTimeout(() => {
          console.log('I ❤️ U')
        },10)
      })
    ```

## 10. 请简述TypeScript和JavaScript的关系
- 答案
  > 是对js的扩展集，包含：JavaScript、类型系统、ES6+。最终编译为JavaScript执行

  > 功能更为强大，生态也更健全、完善

## 11. 谈谈你所认为的TypeScript优缺点
- 答案
  > 优点：
  1. TypeScript 增加了代码的可读性和可维护性；
  2. TypeScript是JavaScript的超集，对js的包容性非常好，将.js改为.ts即可使用，可以添加几乎所有类型注解。
  3. 兼容第三方库，即使第三方库不是使用TS编写。

  > 缺点：
  1. 语言本身多了很多概念，需要花费时间成本进行学习。例如接口（Interfaces）、泛型（Generics）、类（Classes）、枚举类型（Enums）等。
  2. 项目初期，TypeScript会增加一些成本，毕竟要多些一些类型的定义，不过从长期维护来看，利远远大于弊。
  3. 和有些第三方库的结合不太完美。在引用第三方模块时，如果当中不包含对应的类型声明文件，需要尝试安装对应的类型声明模块。如果没有对应的模块，只能自己使用declare语句进行声明。有可能增加时间成本。