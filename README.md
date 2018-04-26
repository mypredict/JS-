# JS-clone
原生JS的深浅拷贝问题
```
// 深浅拷贝的基本概念就是对于栈和堆的复制过程, 而基本数据类型都是存在于栈中的,
// 也就不存在浅拷贝一说了, 就是在栈中重新开辟了一块内存空间存放新的数据, 更改
// 也不会影响原数据的改变. 所以深浅拷贝主要是用来对对象的操作.

// 对于对象, 栈中的变量存储的是对象在堆中存放数据的地址.
// 浅拷贝实际上就是又在栈中生成了一个新的变量, 但是这个变量保存的数据也是指
// 向堆中对象的地址, 并没有再开辟一个新的堆空间, 所以当操作堆中数据时, 两个变
// 量的引用都会发生更改, 所以叫浅拷贝
// 深拷贝生成的变量指向的是一个在堆中重新开辟出来空间, 这个空间里存放着同被拷贝
// 对象相同的数据, 但是当更改这两个堆中的数据时确是互不影响了

// 这是源对象 obj1, 没有函数
let obj1 = {
  name: '小明',
  age: 20,
  skill: {
    frondEnd: 'js',
    backEnd: 'node'
  }
}
// 这是原对象 obj2, 有函数
let obj2 = {
  name: '小花',
  age: 20,
  skill: {
    frondEnd: 'js',
    backEnd: 'node',
    web: {
      css: 'good'
    }
  },
  messages () {
    return `${this.name} ${this.age} ${this.skill.frondEnd} ${this.skill.backEnd}`
  }
}


// 首先先来看下基本的浅拷贝 有直接赋值, 数组的 Array.prototype.concat, 
// Array.prototype.slice 等. 在这我就说一个Object.create()
let obj0 = Object.create(obj2)
console.log(obj2)
console.log(obj0)
// 打开chrome控制台可以看到 obj0为一个空对象, 而他的隐式原型指向了 obj2, 也就是共享了
// 堆对象


// 接下来看看伪深拷贝
// JSON方法
let obj3 = JSON.parse(JSON.stringify(obj1))
obj3.name = '小明二号'
obj3.skill.frondEnd = 'html'
console.log(obj1)
console.log(obj3)
// 此时会发现已经完成了深拷贝, 但是接下来再看
let obj4 = JSON.parse(JSON.stringify(obj2))
obj4.name = '小明三号'
obj4.skill.frondEnd = 'html'
console.log(obj2)
console.log(obj4)
// 此时你会发现虽然实现了深拷贝但是函数方法没有被拷贝进来
// 原因就是 JSON对象不支持函数的解析

// Object.assign() 方法
let obj5 = Object.assign({}, obj2)
obj5.name = '小明三号'
obj5.skill.frondEnd = 'html'
console.log(obj2)
console.log(obj5)
// 此时会发现嵌套的对象还是指向同一个堆地址, 所以也不是深拷贝


// 那最后来实现一个深拷贝
function deepClone (sourceObj) {
  // 首先判断源数据是否是对象, 如果不是对象就不存在深拷贝一说
  if (typeof sourceObj !== 'object') {
    return sourceObj
  } else {
    // 通过隐式原型的构造器判断源数据是数组还是对象然后创建相应的新对象
    var newObj = sourceObj.constructor === Array ? [] : {}
    // 通过 for in 遍历源数据
    for (let i in sourceObj) {
      // 如果属性不再是对象就直接赋值给相应新对象的属性, 否则就递归继续循环赋值
      newObj[i] = typeof sourceObj[i] === 'object' ? deepClone(sourceObj[i]) : sourceObj[i]
    }
  }
  return newObj
}
let obj6 = clone1(obj2)
obj6.name = '小明'
obj6.age = 30
obj6.skill = {
  web: 'html'
}
obj6.messages = function () {
  return `${this.name} ${this.age} ${this.skill.web}`
}
console.log(obj2.messages())
console.log(obj6.messages())
// 此时会发现所有的数据都被拷贝出来了
```
## 欢迎指出不对的地方共同进步
