# ES7



## Array

### ES7数组如何判断元素是否存在

```JavaScript
const arr = [1, 2, 3, 4, 5, 6, 7]
arr.includes(4) // true
```



## Math

### ES7数学乘方如何简写

```javascript
// 之前的做法
console.log(Math.pow(2,5));
//32

console.log(2 ** 5); // Math.pow()的简写
// 32
```





# ES8



## Async\Await

### await 只能出现在saync函数中

```javascript
// 用一个小栗子演示一下async和Await的使用
// 使得下面两个函数顺序执行。先准备好，再出去玩。
function wait() {
    setTimeout(() => {
        console.log('朋友准备中....')
    },1000)
}
function go() {
    console.log('朋友准备完毕，一起出去玩')
}

async function test() {
    let promise = new Promise(resolve => {
        setTimeout(() => {
        	console.log('朋友准备中....')
        	resolve()
        })
    })
    await promise
    await go()
}
```



### async实现原理

> async函数的实现原理，就是将Generator函数和自动执行器，包装在一个函数里。

```

```



### 迭代器和生成器

	> 迭代器（就是遍历器）

> 迭代器是一种特殊对象，它具有一些专门为迭代过程设计的专有接口，所有的迭代器对象都有一个next()方法，每次调用都返回一个结果对象。结果对象有两个属性：一个是value，表示下一个将要返回的值；另一个是done，它是一个布尔类型的值。

```
> 生成器（Generator）
```

> 生成器是一种返回迭代器的函数，通过function关键字后的星号（*）来表示，函数中会用到新的关键字yield.



## String

### 对String补白

> padStart，起始位置补白

```javascript
for (let i = 1; i < 32; i++) {
  console.log(i.toString().padStart(2, '0'));
}
// 01
// 02
// ...
```



> padEnd，末尾位置补白

```javascript
for (let i = 1; i < 32; i++) {
  console.log(i.toString().padEnd(5, '*'));
}
```

## Object
### 描述符

> ES8如何获取Object数据的描述符？

```JavaScript
const data = {
    Lilei： '78/50',
    Lima: '58/40'
}

Object.defineProperty(data, 'Lima', {
    enumerable: false,
    writable: fasle
})

Object.keys(data) //["PortLand", "Dublin"]

Object.getOwnPropertyDescriptors(data)
//PortLand: {value: "78/50", writable: true, enumerable: true, configurable: true}
//Dublin: {value: "88/52", writable: true, enumerable: true, configurable: true}
//Lima: {value: "58/40", writable: false, enumerable: false, configurable: true}

Object.getOwnPropertyDescriptors(data, 'Lima')
//Lima: {value: "58/40", writable: false, enumerable: false, configurable: true}
```





# ES9

## For await of

> ES9中异步操作集合是如何遍历的？

```javascript
// for of 是用来遍历异步操作的（集合含有异步操作，拿不到正确结果）
function Gen(time) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(time)
        },time)
    })
}

async function test() {
    let arr = [Gen(2000), Gen(100), Gen(3000)]
    for await (let item of arr) {
        console.log(Date.now(), item)
    }
}

test()
//1591186908559 2000
//1591186908560 100
//1591186909560 3000
```

> 自定义数据结构异步遍历，该如何操作

```javascript
const obj = {
    count: 0,
    Gen (time) {
        return new Promise((resolve,reject) => {
            setTimeout(() => {
                resolve({done: false,value:time})
            },time)
        })
    },
    [Symbol.asyncIterator] () {
    let self = this
    return {
      next() {
        self.count++
        if(self.count < 4) {
          return self.Gen(Math.random() * 1000)
        } else {
          return Promise.resolve({
            done: true,
            value: ''
          })
        }
      }
    }
  }
}
async function test() {
    for await (let item of obj) {
        console.log(new Date(), item)
    }
}

test()
//Wed Jun 03 2020 20:26:08 GMT+0800 (中国标准时间) 534.6221733997855
//Wed Jun 03 2020 20:26:08 GMT+0800 (中国标准时间) 118.35034861243953
//Wed Jun 03 2020 20:26:09 GMT+0800 (中国标准时间) 575.9665993428888
```



## Promise

> finally

```javascript
const Gen = (time) => {
    return new Promise((resolve,reject) => {
        setTimeout(() => {
            if(time <500) {
                reject(time)
            } else {
                resolve(time)
            }
        })
    })
}
Gen(Math.random() * 1000)
.then(val => console.log('resolve', val))
.catch(err => console.log('reject',err))
.finally(() => { console.log('finsh')})
```

## Object(Rest & Spread)
