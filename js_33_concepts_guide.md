# 33-js-concepts 深度学习指南

## 📚 项目概述

**仓库地址**: [https://github.com/leonardomso/33-js-concepts](https://github.com/leonardomso/33-js-concepts)

**项目简介**: 这是一个汇集了每个JavaScript开发者都应该知道的33个核心概念的学习资源库。由Leonardo Maldonado创建，包含大量优质文章、视频和代码示例。

**适合人群**: 
- 有1-2年JS经验，想系统提升的开发者
- 准备面试，需要巩固基础的工程师
- 希望从"会用"到"精通原理"的进阶开发者

---

## 🎯 33个核心概念清单

### 第一部分：基础核心（1-10）

#### 1. **调用栈 (Call Stack)** ⭐⭐⭐
**重要性**: 极高 - 理解JS执行机制的基础

**核心知识点**:
- 调用栈是什么：LIFO（后进先出）的数据结构
- 函数调用如何入栈和出栈
- 栈溢出（Stack Overflow）的原因
- 递归与调用栈的关系

**必读文章**:
- "Understanding the JavaScript Call Stack"
- "What is the Call Stack in JavaScript?"

**实践建议**:
```javascript
// 通过debugger观察调用栈
function first() {
  console.log('First');
  second();
}

function second() {
  console.log('Second');
  third();
}

function third() {
  console.log('Third');
  debugger; // 在这里观察调用栈
}

first();
```

---

#### 2. **原始类型 (Primitive Types)** ⭐⭐
**核心知识点**:
- 7种原始类型：string, number, bigint, boolean, undefined, symbol, null
- 原始类型的不可变性
- 值类型 vs 引用类型
- 包装对象（String, Number, Boolean）

**易错点**:
```javascript
// 原始类型的不可变性
let str = "hello";
str.toUpperCase(); // 不会改变原始字符串
console.log(str); // 仍然是 "hello"

// typeof null 的历史bug
typeof null === 'object' // true (这是JS的bug)
```

---

#### 3. **值类型和引用类型 (Value Types and Reference Types)** ⭐⭐⭐
**重要性**: 极高 - 影响数据传递和比较

**核心知识点**:
- 原始类型按值传递
- 对象类型按引用传递
- 深拷贝 vs 浅拷贝
- 内存分配：栈内存 vs 堆内存

**实战代码**:
```javascript
// 值传递
let a = 10;
let b = a;
b = 20;
console.log(a); // 10 - a不受影响

// 引用传递
let obj1 = { value: 10 };
let obj2 = obj1;
obj2.value = 20;
console.log(obj1.value); // 20 - obj1被改变了

// 深拷贝实现
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') return obj;
  
  const cloned = Array.isArray(obj) ? [] : {};
  
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloned[key] = deepClone(obj[key]);
    }
  }
  
  return cloned;
}
```

---

#### 4. **隐式类型转换 (Implicit Coercion)** ⭐⭐
**核心知识点**:
- ToPrimitive、ToNumber、ToString、ToBoolean规则
- == vs === 的本质区别
- 常见的隐式转换陷阱

**经典案例**:
```javascript
// 经典面试题
console.log([] + []); // ""
console.log([] + {}); // "[object Object]"
console.log({} + []); // "[object Object]" 或 0（取决于环境）
console.log({} + {}); // "[object Object][object Object]" 或 NaN

// 实用技巧
const num = +"42"; // 字符串转数字
const str = "" + 42; // 数字转字符串
const bool = !!value; // 转布尔值
```

---

#### 5. **== vs === (Equality)** ⭐⭐
**核心知识点**:
- == 会进行类型转换
- === 严格相等，不转换类型
- Object.is() 的特殊情况

**最佳实践**: 始终使用 === 除非你明确需要类型转换

---

#### 6. **函数作用域、块级作用域和词法作用域 (Scope)** ⭐⭐⭐
**核心知识点**:
- var vs let vs const
- 词法作用域（静态作用域）
- 作用域链的查找机制
- 暂时性死区（TDZ）

**实战示例**:
```javascript
// 词法作用域
function outer() {
  const x = 10;
  
  function inner() {
    console.log(x); // 可以访问外部作用域的x
  }
  
  return inner;
}

const fn = outer();
fn(); // 10

// 块级作用域
{
  let a = 1;
  const b = 2;
  var c = 3;
}
console.log(c); // 3
console.log(a); // ReferenceError
```

---

#### 7. **表达式与语句 (Expression vs Statement)** ⭐
**核心知识点**:
- 表达式产生值，可以赋值给变量
- 语句执行操作，不产生值
- 函数声明 vs 函数表达式

---

#### 8. **立即执行函数 IIFE (Immediately Invoked Function Expression)** ⭐⭐
**核心知识点**:
- IIFE的语法和用途
- 创建独立作用域
- 模块化模式的基础

**实战应用**:
```javascript
// 经典IIFE
(function() {
  var privateVar = 'secret';
  console.log('IIFE executed');
})();

// ES6+ 可以用块级作用域替代
{
  const privateVar = 'secret';
  console.log('Block scope');
}

// IIFE创建模块
const myModule = (function() {
  let privateCounter = 0;
  
  return {
    increment() { privateCounter++; },
    getCount() { return privateCounter; }
  };
})();
```

---

#### 9. **消息队列和事件循环 (Event Loop)** ⭐⭐⭐
**重要性**: 极高 - JS异步机制的核心

**核心知识点**:
- 事件循环的完整流程
- 宏任务（MacroTask）vs 微任务（MicroTask）
- 执行栈、任务队列、微任务队列的关系
- 浏览器渲染时机

**必须理解的执行顺序**:
```javascript
console.log('1');

setTimeout(() => {
  console.log('2');
}, 0);

Promise.resolve().then(() => {
  console.log('3');
});

console.log('4');

// 输出顺序: 1, 4, 3, 2
// 解释:
// 1. 同步代码: 1, 4
// 2. 微任务: 3 (Promise)
// 3. 宏任务: 2 (setTimeout)
```

**进阶示例**:
```javascript
console.log('start');

setTimeout(() => {
  console.log('setTimeout 1');
  Promise.resolve().then(() => {
    console.log('promise 1');
  });
}, 0);

Promise.resolve().then(() => {
  console.log('promise 2');
  setTimeout(() => {
    console.log('setTimeout 2');
  }, 0);
});

console.log('end');

// 输出: start, end, promise 2, setTimeout 1, promise 1, setTimeout 2
```

**学习资源**:
- Philip Roberts的演讲 "What the heck is the event loop anyway?"
- Jake Archibald的 "Tasks, microtasks, queues and schedules"

---

#### 10. **setTimeout, setInterval 和 requestAnimationFrame** ⭐⭐
**核心知识点**:
- setTimeout的最小延迟时间（4ms）
- setInterval的缺陷和替代方案
- requestAnimationFrame的优势
- 性能优化建议

**实战对比**:
```javascript
// setInterval的问题：可能堆积
let i = 0;
setInterval(() => {
  // 如果这个函数执行时间超过间隔时间，会导致堆积
  console.log(i++);
}, 100);

// 更好的方案：递归setTimeout
function recursiveTimeout() {
  console.log(i++);
  setTimeout(recursiveTimeout, 100);
}
recursiveTimeout();

// 动画用requestAnimationFrame
function animate() {
  // 动画逻辑
  requestAnimationFrame(animate);
}
animate();
```

---

### 第二部分：函数与闭包（11-15）

#### 11. **JavaScript引擎 (JavaScript Engines)** ⭐
**了解即可**:
- V8、SpiderMonkey、JavaScriptCore
- JIT编译
- 性能优化技巧

---

#### 12. **按位操作符 (Bitwise Operators)** ⭐
**实用技巧**:
```javascript
// 快速取整
~~3.14 // 3
3.14 | 0 // 3

// 判断奇偶
num & 1 // 1为奇数，0为偶数

// 交换变量
a ^= b; b ^= a; a ^= b;
```

---

#### 13. **DOM 和布局树 (DOM and Layout Trees)** ⭐
**前端必知**:
- DOM树的构建
- CSSOM树
- 渲染树（Render Tree）
- 重排（Reflow）vs 重绘（Repaint）

---

#### 14. **工厂函数和类 (Factories and Classes)** ⭐⭐
**核心知识点**:
- 工厂函数的优势
- ES6 class语法
- 原型继承 vs 类继承
- 组合优于继承

**实战对比**:
```javascript
// 工厂函数
function createUser(name, age) {
  return {
    name,
    age,
    greet() {
      console.log(`Hi, I'm ${this.name}`);
    }
  };
}

// ES6 Class
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    console.log(`Hi, I'm ${this.name}`);
  }
}

// 工厂函数的优势：更灵活的组合
function withLogger(obj) {
  return {
    ...obj,
    log(msg) {
      console.log(`[${this.name}]: ${msg}`);
    }
  };
}

const user = withLogger(createUser('Alice', 25));
user.log('Hello'); // [Alice]: Hello
```

---

#### 15. **this, call, apply 和 bind** ⭐⭐⭐
**重要性**: 极高 - 面试高频考点

**核心知识点**:
- this的4种绑定规则
- 箭头函数的this
- call、apply、bind的区别和实现

**this绑定规则优先级**:
1. **new 绑定** - 最高优先级
2. **显式绑定** - call/apply/bind
3. **隐式绑定** - 对象方法调用
4. **默认绑定** - 独立函数调用

**手写实现**:
```javascript
// 手写call
Function.prototype.myCall = function(context, ...args) {
  context = context || window;
  const fnSymbol = Symbol();
  context[fnSymbol] = this;
  const result = context[fnSymbol](...args);
  delete context[fnSymbol];
  return result;
};

// 手写apply
Function.prototype.myApply = function(context, args) {
  context = context || window;
  const fnSymbol = Symbol();
  context[fnSymbol] = this;
  const result = context[fnSymbol](...args);
  delete context[fnSymbol];
  return result;
};

// 手写bind
Function.prototype.myBind = function(context, ...args) {
  const fn = this;
  return function(...newArgs) {
    return fn.apply(context, [...args, ...newArgs]);
  };
};

// 实战示例
const person = {
  name: 'Alice',
  greet(greeting, punctuation) {
    console.log(`${greeting}, I'm ${this.name}${punctuation}`);
  }
};

const greet = person.greet;
greet.call(person, 'Hello', '!'); // Hello, I'm Alice!
greet.apply(person, ['Hi', '.']); // Hi, I'm Alice.

const boundGreet = greet.bind(person, 'Hey');
boundGreet('?'); // Hey, I'm Alice?
```

**箭头函数的特殊性**:
```javascript
const obj = {
  name: 'Alice',
  regularFunc: function() {
    console.log(this.name); // this指向obj
  },
  arrowFunc: () => {
    console.log(this.name); // this继承外部作用域
  }
};

obj.regularFunc(); // Alice
obj.arrowFunc(); // undefined (或全局的name)
```

---

### 第三部分：高级特性（16-25）

#### 16. **new、构造函数和instanceof** ⭐⭐⭐
**核心知识点**:
- new运算符的4个步骤
- 构造函数的返回值规则
- instanceof的原理

**new的实现原理**:
```javascript
function myNew(Constructor, ...args) {
  // 1. 创建新对象，原型指向构造函数的prototype
  const obj = Object.create(Constructor.prototype);
  
  // 2. 执行构造函数，绑定this
  const result = Constructor.apply(obj, args);
  
  // 3. 如果构造函数返回对象，则返回该对象；否则返回创建的新对象
  return result instanceof Object ? result : obj;
}

// 测试
function Person(name) {
  this.name = name;
}

const p = myNew(Person, 'Alice');
console.log(p.name); // Alice
console.log(p instanceof Person); // true
```

**instanceof的实现**:
```javascript
function myInstanceof(obj, Constructor) {
  let proto = Object.getPrototypeOf(obj);
  
  while (proto) {
    if (proto === Constructor.prototype) {
      return true;
    }
    proto = Object.getPrototypeOf(proto);
  }
  
  return false;
}
```

---

#### 17. **原型继承和原型链 (Prototype Inheritance)** ⭐⭐⭐
**重要性**: 极高 - JS面向对象的核心

**核心知识点**:
- prototype vs __proto__
- 原型链的查找机制
- Object.create()的作用
- 继承的多种实现方式

**原型链图示理解**:
```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  console.log(`Hi, I'm ${this.name}`);
};

const alice = new Person('Alice');

// 原型链关系
alice.__proto__ === Person.prototype // true
Person.prototype.__proto__ === Object.prototype // true
Object.prototype.__proto__ === null // true

// 查找顺序
alice.greet() // 1. alice自身找不到greet
              // 2. 去alice.__proto__(Person.prototype)找到greet
```

**继承的最佳实践 - ES6 Class**:
```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    console.log(`${this.name} makes a sound`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // 调用父类构造函数
    this.breed = breed;
  }
  
  speak() {
    super.speak(); // 调用父类方法
    console.log(`${this.name} barks`);
  }
}

const dog = new Dog('Buddy', 'Golden Retriever');
dog.speak();
// Buddy makes a sound
// Buddy barks
```

**经典继承方式（了解）**:
```javascript
// 1. 原型链继承（缺陷：共享引用类型）
function Parent() {
  this.colors = ['red', 'blue'];
}
Child.prototype = new Parent();

// 2. 构造函数继承（缺陷：方法无法复用）
function Child() {
  Parent.call(this);
}

// 3. 组合继承（最常用）
function Child(name) {
  Parent.call(this, name);
}
Child.prototype = new Parent();
Child.prototype.constructor = Child;

// 4. 寄生组合继承（最佳）
function Child(name) {
  Parent.call(this, name);
}
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;
```

---

#### 18. **Object.create 和 Object.assign** ⭐⭐
**核心知识点**:
- Object.create()创建指定原型的对象
- Object.assign()浅拷贝
- 深拷贝的实现

**实战应用**:
```javascript
// Object.create创建纯净对象
const pureObj = Object.create(null);
pureObj.toString // undefined（没有Object.prototype）

// Object.assign合并对象
const target = { a: 1 };
const source = { b: 2, c: { d: 3 } };
const result = Object.assign(target, source);

// 注意：浅拷贝
source.c.d = 4;
console.log(result.c.d); // 4 - 被修改了
```

---

#### 19. **map, reduce, filter** ⭐⭐⭐
**重要性**: 高 - 函数式编程基础

**核心知识点**:
- 三大数组方法的使用场景
- 链式调用的性能考虑
- 手写实现

**实战示例**:
```javascript
const numbers = [1, 2, 3, 4, 5, 6];

// map: 转换每个元素
const doubled = numbers.map(n => n * 2);
// [2, 4, 6, 8, 10, 12]

// filter: 筛选符合条件的元素
const evens = numbers.filter(n => n % 2 === 0);
// [2, 4, 6]

// reduce: 累积计算
const sum = numbers.reduce((acc, n) => acc + n, 0);
// 21

// 组合使用
const result = numbers
  .filter(n => n % 2 === 0)
  .map(n => n * 2)
  .reduce((acc, n) => acc + n, 0);
// 24
```

**手写实现**:
```javascript
// 手写map
Array.prototype.myMap = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));
  }
  return result;
};

// 手写filter
Array.prototype.myFilter = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};

// 手写reduce
Array.prototype.myReduce = function(callback, initialValue) {
  let acc = initialValue !== undefined ? initialValue : this[0];
  let startIndex = initialValue !== undefined ? 0 : 1;
  
  for (let i = startIndex; i < this.length; i++) {
    acc = callback(acc, this[i], i, this);
  }
  
  return acc;
};
```

**高级用法**:
```javascript
// reduce实现map
Array.prototype.mapByReduce = function(callback) {
  return this.reduce((acc, item, index) => {
    acc.push(callback(item, index, this));
    return acc;
  }, []);
};

// reduce实现filter
Array.prototype.filterByReduce = function(callback) {
  return this.reduce((acc, item, index) => {
    if (callback(item, index, this)) {
      acc.push(item);
    }
    return acc;
  }, []);
};
```

---

#### 20. **纯函数、副作用和状态变化 (Pure Functions)** ⭐⭐
**核心知识点**:
- 纯函数的定义：相同输入总是返回相同输出，无副作用
- 副作用的类型
- 函数式编程的优势

**对比示例**:
```javascript
// 非纯函数
let count = 0;
function increment() {
  count++; // 修改外部状态
  return count;
}

// 纯函数
function pureIncrement(num) {
  return num + 1; // 不修改外部状态
}

// 实际应用：Redux reducer必须是纯函数
function todoReducer(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, action.todo]; // 返回新数组
    case 'REMOVE_TODO':
      return state.filter(todo => todo.id !== action.id);
    default:
      return state;
  }
}
```

---

#### 21. **闭包 (Closures)** ⭐⭐⭐
**重要性**: 极高 - JS最核心特性之一

**核心知识点**:
- 闭包的定义：函数能访问其词法作用域外的变量
- 闭包的应用场景
- 内存泄漏的风险

**经典应用场景**:
```javascript
// 1. 数据私有化
function createCounter() {
  let count = 0; // 私有变量
  
  return {
    increment() { return ++count; },
    decrement() { return --count; },
    getCount() { return count; }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.getCount()); // 1
console.log(counter.count); // undefined - 无法直接访问

// 2. 函数柯里化
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return function(...nextArgs) {
        return curried.apply(this, [...args, ...nextArgs]);
      };
    }
  };
}

const add = (a, b, c) => a + b + c;
const curriedAdd = curry(add);
console.log(curriedAdd(1)(2)(3)); // 6
console.log(curriedAdd(1, 2)(3)); // 6

// 3. 延迟执行
function delayed(fn, delay) {
  return function(...args) {
    setTimeout(() => fn.apply(this, args), delay);
  };
}

const delayedLog = delayed(console.log, 1000);
delayedLog('Hello'); // 1秒后输出

// 4. 事件监听器
function attachListener(element) {
  let clickCount = 0;
  
  element.addEventListener('click', function() {
    clickCount++;
    console.log(`Clicked ${clickCount} times`);
  });
}
```

**常见陷阱**:
```javascript
// 经典错误：循环中的闭包
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i); // 全部输出5
  }, i * 1000);
}

// 解决方案1：使用let
for (let i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i); // 输出0, 1, 2, 3, 4
  }, i * 1000);
}

// 解决方案2：IIFE
for (var i = 0; i < 5; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j); // 输出0, 1, 2, 3, 4
    }, j * 1000);
  })(i);
}
```

---

#### 22. **高阶函数 (Higher Order Functions)** ⭐⭐⭐
**重要性**: 高 - 函数式编程核心

**核心知识点**:
- 接收函数作为参数
- 返回函数
- 常见的高阶函数模式

**实战示例**:
```javascript
// 1. 函数组合
const compose = (...fns) => x => fns.reduceRight((acc, fn) => fn(acc), x);

const add1 = x => x + 1;
const multiply2 = x => x * 2;
const subtract3 = x => x - 3;

const calculate = compose(subtract3, multiply2, add1);
console.log(calculate(5)); // (5 + 1) * 2 - 3 = 9

// 2. 管道函数
const pipe = (...fns) => x => fns.reduce((acc, fn) => fn(acc), x);

const calculate2 = pipe(add1, multiply2, subtract3);
console.log(calculate2(5)); // 9

// 3. 部分应用
function partial(fn, ...presetArgs) {
  return function(...laterArgs) {
    return fn(...presetArgs, ...laterArgs);
  };
}

const multiply = (a, b) => a * b;
const double = partial(multiply, 2);
console.log(double(5)); // 10

// 4. 记忆化（缓存）
function memoize(fn) {
  const cache = new Map();
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (cache.has(key)) {
      return cache.get(key);
    }
    
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const fibonacci = memoize(function(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

console.log(fibonacci(40)); // 快速计算

// 5. 防抖和节流
function debounce(fn, delay) {
  let timeoutId;
  
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn.apply(this, args), delay);
  };
}

function throttle(fn, limit) {
  let inThrottle;
  
  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

// 使用场景
const searchInput = document.querySelector('#search');
const debouncedSearch = debounce(searchAPI, 300);
searchInput.addEventListener('input', debouncedSearch);

const scrollHandler = throttle(handleScroll, 100);
window.addEventListener('scroll', scrollHandler);
```

---

#### 23. **递归 (Recursion)** ⭐⭐
**核心知识点**:
- 递归的基本要素：基线条件 + 递归条件
- 尾递归优化
- 递归 vs 迭代

**经典示例**:
```javascript
// 1. 阶乘
function factorial(n) {
  if (n <= 1) return 1; // 基线条件
  return n * factorial(n - 1); // 递归条件
}

// 尾递归优化版本
function factorialTail(n, acc = 1) {
  if (n <= 1) return acc;
  return factorialTail(n - 1, n * acc);
}

// 2. 斐波那契数列
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

// 3. 深度遍历树结构
function traverseTree(node) {
  console.log(node.value);
  
  if (node.children) {
    node.children.forEach(child => traverseTree(child));
  }
}

// 4. 深度优先搜索（DFS）
function dfs(graph, start, visited = new Set()) {
  visited.add(start);
  console.log(start);
  
  for (let neighbor of graph[start]) {
    if (!visited.has(neighbor)) {
      dfs(graph, neighbor, visited);
    }
  }
}

// 5. 扁平化数组
function flatten(arr) {
  return arr.reduce((acc, item) => {
    return acc.concat(Array.isArray(item) ? flatten(item) : item);
  }, []);
}

const nested = [1, [2, [3, [4]], 5]];
console.log(flatten(nested)); // [1, 2, 3, 4, 5]
```

---

#### 24. **集合类型 (Collections: Map, Set, WeakMap, WeakSet)** ⭐⭐⭐
**重要性**: 高 - ES6+必备数据结构

**核心知识点**:
- Map vs Object的区别
- Set的特性和应用
- WeakMap和WeakSet的使用场景
- 性能考虑

**Map vs Object**:
```javascript
// Object的局限性
const obj = {};
const key1 = { id: 1 };
const key2 = { id: 2 };

obj[key1] = 'value1';
obj[key2] = 'value2';

console.log(obj[key1]); // 'value2' - 键被转换成字符串，都是 "[object Object]"

// Map可以使用任何类型作为键
const map = new Map();
map.set(key1, 'value1');
map.set(key2, 'value2');

console.log(map.get(key1)); // 'value1' ✓
console.log(map.get(key2)); // 'value2' ✓

// Map的其他优势
map.set('name', 'Alice');
map.set(42, 'number key');
map.set(true, 'boolean key');

console.log(map.size); // 5 - 直接获取大小
map.has('name'); // true - 检查键是否存在
map.delete('name'); // 删除键值对
map.clear(); // 清空所有

// Map的遍历
const userMap = new Map([
  ['Alice', 25],
  ['Bob', 30],
  ['Charlie', 35]
]);

// for...of遍历
for (let [name, age] of userMap) {
  console.log(`${name}: ${age}`);
}

// forEach
userMap.forEach((age, name) => {
  console.log(`${name}: ${age}`);
});

// 获取所有键/值
console.log([...userMap.keys()]);   // ['Alice', 'Bob', 'Charlie']
console.log([...userMap.values()]); // [25, 30, 35]
console.log([...userMap.entries()]); // [['Alice', 25], ...]
```

**Set的应用**:
```javascript
// 1. 数组去重
const numbers = [1, 2, 2, 3, 3, 3, 4, 5, 5];
const unique = [...new Set(numbers)];
console.log(unique); // [1, 2, 3, 4, 5]

// 2. 判断元素是否存在
const set = new Set([1, 2, 3, 4, 5]);
console.log(set.has(3)); // true - O(1)时间复杂度

// 3. 集合运算
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);

// 并集
const union = new Set([...setA, ...setB]);
console.log([...union]); // [1, 2, 3, 4, 5, 6]

// 交集
const intersection = new Set([...setA].filter(x => setB.has(x)));
console.log([...intersection]); // [3, 4]

// 差集
const difference = new Set([...setA].filter(x => !setB.has(x)));
console.log([...difference]); // [1, 2]

// 4. 去除字符串中的重复字符
const str = "hello world";
const uniqueChars = [...new Set(str)].join('');
console.log(uniqueChars); // "helo wrd"
```

**WeakMap的使用场景**:
```javascript
// WeakMap的特点：
// 1. 键必须是对象
// 2. 键是弱引用，不影响垃圾回收
// 3. 不可遍历，无size属性

// 场景1：存储DOM节点的元数据
const metadata = new WeakMap();

function attachMetadata(element, data) {
  metadata.set(element, data);
}

const button = document.querySelector('#myButton');
attachMetadata(button, { clicks: 0, lastClicked: null });

// 当button被移除时，metadata中的数据也会被自动回收

// 场景2：私有属性实现
const privateData = new WeakMap();

class User {
  constructor(name, password) {
    this.name = name;
    privateData.set(this, { password }); // 私有数据
  }
  
  authenticate(password) {
    return privateData.get(this).password === password;
  }
}

const user = new User('Alice', 'secret123');
console.log(user.password); // undefined - 无法直接访问
console.log(user.authenticate('secret123')); // true

// 场景3：缓存计算结果
const cache = new WeakMap();

function expensiveOperation(obj) {
  if (cache.has(obj)) {
    return cache.get(obj);
  }
  
  const result = /* 复杂计算 */ obj.value * 2;
  cache.set(obj, result);
  return result;
}
```

**WeakSet的使用场景**:
```javascript
// WeakSet的特点：
// 1. 成员必须是对象
// 2. 弱引用，不影响垃圾回收
// 3. 不可遍历

// 场景1：标记对象
const processedNodes = new WeakSet();

function processNode(node) {
  if (processedNodes.has(node)) {
    return; // 已处理过
  }
  
  // 处理节点
  processedNodes.add(node);
}

// 场景2：防止循环引用
const seen = new WeakSet();

function traverse(obj) {
  if (seen.has(obj)) {
    return; // 已访问过，防止无限循环
  }
  
  seen.add(obj);
  // 遍历对象属性...
}
```

**性能对比**:
```javascript
// Map vs Object - 性能测试
const iterations = 1000000;

// Object
console.time('Object');
const obj = {};
for (let i = 0; i < iterations; i++) {
  obj[i] = i;
}
console.timeEnd('Object');

// Map
console.time('Map');
const map = new Map();
for (let i = 0; i < iterations; i++) {
  map.set(i, i);
}
console.timeEnd('Map');

// 结论：
// - 频繁增删：Map性能更好
// - 键为对象：只能用Map
// - 简单键值对：Object略快
// - 需要遍历：Map更方便
```

---

#### 25. **Promises** ⭐⭐⭐
**重要性**: 极高 - 异步编程核心

**核心知识点**:
- Promise的三种状态：pending、fulfilled、rejected
- Promise链式调用
- Promise静态方法：all、race、allSettled、any
- 错误处理
- 手写Promise

**Promise基础**:
```javascript
// 创建Promise
const promise = new Promise((resolve, reject) => {
  // 异步操作
  setTimeout(() => {
    const success = true;
    
    if (success) {
      resolve('操作成功'); // fulfilled状态
    } else {
      reject('操作失败'); // rejected状态
    }
  }, 1000);
});

// 使用Promise
promise
  .then(result => {
    console.log(result); // '操作成功'
    return result + ' 并继续';
  })
  .then(result => {
    console.log(result); // '操作成功 并继续'
  })
  .catch(error => {
    console.error(error);
  })
  .finally(() => {
    console.log('无论成功失败都执行');
  });
```

**链式调用与值传递**:
```javascript
// 返回值会传递给下一个then
Promise.resolve(1)
  .then(value => {
    console.log(value); // 1
    return value + 1;
  })
  .then(value => {
    console.log(value); // 2
    return value + 1;
  })
  .then(value => {
    console.log(value); // 3
  });

// 返回Promise
Promise.resolve(1)
  .then(value => {
    return new Promise(resolve => {
      setTimeout(() => resolve(value * 2), 1000);
    });
  })
  .then(value => {
    console.log(value); // 2 (1秒后)
  });

// then中抛出错误会被catch捕获
Promise.resolve(1)
  .then(value => {
    throw new Error('出错了');
  })
  .then(value => {
    console.log('不会执行');
  })
  .catch(error => {
    console.error(error.message); // '出错了'
    return 'recovered'; // 错误恢复
  })
  .then(value => {
    console.log(value); // 'recovered'
  });
```

**Promise静态方法**:
```javascript
// 1. Promise.all - 所有Promise都成功才成功
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve(2);
const promise3 = Promise.resolve(3);

Promise.all([promise1, promise2, promise3])
  .then(results => {
    console.log(results); // [1, 2, 3]
  });

// 只要有一个失败就失败
Promise.all([
  Promise.resolve(1),
  Promise.reject('error'),
  Promise.resolve(3)
])
  .then(results => {
    console.log('不会执行');
  })
  .catch(error => {
    console.log(error); // 'error'
  });

// 2. Promise.race - 第一个完成的结果
Promise.race([
  new Promise(resolve => setTimeout(() => resolve('slow'), 1000)),
  new Promise(resolve => setTimeout(() => resolve('fast'), 100))
])
  .then(result => {
    console.log(result); // 'fast'
  });

// 3. Promise.allSettled - 等待所有Promise完成（无论成功失败）
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject('error'),
  Promise.resolve(3)
])
  .then(results => {
    console.log(results);
    // [
    //   { status: 'fulfilled', value: 1 },
    //   { status: 'rejected', reason: 'error' },
    //   { status: 'fulfilled', value: 3 }
    // ]
  });

// 4. Promise.any - 第一个成功的结果
Promise.any([
  Promise.reject('error1'),
  Promise.resolve('success'),
  Promise.reject('error2')
])
  .then(result => {
    console.log(result); // 'success'
  });

// 所有都失败才失败
Promise.any([
  Promise.reject('error1'),
  Promise.reject('error2')
])
  .catch(error => {
    console.log(error); // AggregateError
  });
```

**实战应用场景**:
```javascript
// 1. 并行请求优化
async function fetchUserData(userId) {
  const [user, posts, comments] = await Promise.all([
    fetch(`/api/users/${userId}`).then(r => r.json()),
    fetch(`/api/users/${userId}/posts`).then(r => r.json()),
    fetch(`/api/users/${userId}/comments`).then(r => r.json())
  ]);
  
  return { user, posts, comments };
}

// 2. 超时控制
function timeout(promise, ms) {
  return Promise.race([
    promise,
    new Promise((_, reject) => 
      setTimeout(() => reject(new Error('Timeout')), ms)
    )
  ]);
}

// 使用
timeout(fetch('/api/data'), 5000)
  .then(response => response.json())
  .catch(error => console.error(error));

// 3. 重试机制
function retry(fn, maxAttempts = 3, delay = 1000) {
  return new Promise((resolve, reject) => {
    function attempt(attemptNumber) {
      fn()
        .then(resolve)
        .catch(error => {
          if (attemptNumber >= maxAttempts) {
            reject(error);
          } else {
            console.log(`Attempt ${attemptNumber} failed, retrying...`);
            setTimeout(() => attempt(attemptNumber + 1), delay);
          }
        });
    }
    
    attempt(1);
  });
}

// 使用
retry(() => fetch('/api/data'), 3, 1000)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('All attempts failed'));

// 4. 顺序执行Promise
function runInSequence(promises) {
  return promises.reduce((promiseChain, currentPromise) => {
    return promiseChain.then(results => {
      return currentPromise().then(result => [...results, result]);
    });
  }, Promise.resolve([]));
}

// 使用
const tasks = [
  () => Promise.resolve(1),
  () => Promise.resolve(2),
  () => Promise.resolve(3)
];

runInSequence(tasks).then(results => {
  console.log(results); // [1, 2, 3]
});
```

**手写Promise（简化版）**:
```javascript
class MyPromise {
  constructor(executor) {
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    this.onFulfilledCallbacks = [];
    this.onRejectedCallbacks = [];
    
    const resolve = (value) => {
      if (this.state === 'pending') {
        this.state = 'fulfilled';
        this.value = value;
        this.onFulfilledCallbacks.forEach(fn => fn());
      }
    };
    
    const reject = (reason) => {
      if (this.state === 'pending') {
        this.state = 'rejected';
        this.reason = reason;
        this.onRejectedCallbacks.forEach(fn => fn());
      }
    };
    
    try {
      executor(resolve, reject);
    } catch (error) {
      reject(error);
    }
  }
  
  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value;
    onRejected = typeof onRejected === 'function' ? onRejected : reason => { throw reason };
    
    const promise2 = new MyPromise((resolve, reject) => {
      if (this.state === 'fulfilled') {
        setTimeout(() => {
          try {
            const x = onFulfilled(this.value);
            resolve(x);
          } catch (error) {
            reject(error);
          }
        }, 0);
      }
      
      if (this.state === 'rejected') {
        setTimeout(() => {
          try {
            const x = onRejected(this.reason);
            resolve(x);
          } catch (error) {
            reject(error);
          }
        }, 0);
      }
      
      if (this.state === 'pending') {
        this.onFulfilledCallbacks.push(() => {
          setTimeout(() => {
            try {
              const x = onFulfilled(this.value);
              resolve(x);
            } catch (error) {
              reject(error);
            }
          }, 0);
        });
        
        this.onRejectedCallbacks.push(() => {
          setTimeout(() => {
            try {
              const x = onRejected(this.reason);
              resolve(x);
            } catch (error) {
              reject(error);
            }
          }, 0);
        });
      }
    });
    
    return promise2;
  }
  
  catch(onRejected) {
    return this.then(null, onRejected);
  }
  
  finally(callback) {
    return this.then(
      value => MyPromise.resolve(callback()).then(() => value),
      reason => MyPromise.resolve(callback()).then(() => { throw reason })
    );
  }
  
  static resolve(value) {
    return new MyPromise(resolve => resolve(value));
  }
  
  static reject(reason) {
    return new MyPromise((_, reject) => reject(reason));
  }
  
  static all(promises) {
    return new MyPromise((resolve, reject) => {
      const results = [];
      let completed = 0;
      
      promises.forEach((promise, index) => {
        MyPromise.resolve(promise).then(value => {
          results[index] = value;
          completed++;
          
          if (completed === promises.length) {
            resolve(results);
          }
        }).catch(reject);
      });
    });
  }
  
  static race(promises) {
    return new MyPromise((resolve, reject) => {
      promises.forEach(promise => {
        MyPromise.resolve(promise).then(resolve).catch(reject);
      });
    });
  }
}

// 测试
const p = new MyPromise((resolve, reject) => {
  setTimeout(() => resolve('success'), 1000);
});

p.then(value => {
  console.log(value); // 'success'
  return value + '!';
}).then(value => {
  console.log(value); // 'success!'
});
```

---

#### 26. **async/await** ⭐⭐⭐
**重要性**: 极高 - 现代异步编程标准

**核心知识点**:
- async函数返回Promise
- await等待Promise完成
- 错误处理
- 并行执行技巧
- async/await vs Promise的选择

**基础用法**:
```javascript
// async函数总是返回Promise
async function fetchUser() {
  return 'Alice'; // 自动包装成 Promise.resolve('Alice')
}

fetchUser().then(user => console.log(user)); // 'Alice'

// await等待Promise完成
async function getUser() {
  const response = await fetch('/api/user');
  const user = await response.json();
  return user;
}

// 等价于
function getUserPromise() {
  return fetch('/api/user')
    .then(response => response.json())
    .then(user => user);
}
```

**错误处理**:
```javascript
// 1. try-catch方式
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
    return null;
  }
}

// 2. catch方法
async function fetchData2() {
  const data = await fetch('/api/data')
    .then(r => r.json())
    .catch(error => {
      console.error('Error:', error);
      return null;
    });
  
  return data;
}

// 3. 统一错误处理
async function safeAsync(fn) {
  try {
    const result = await fn();
    return [null, result];
  } catch (error) {
    return [error, null];
  }
}

// 使用
async function main() {
  const [error, data] = await safeAsync(() => fetch('/api/data'));
  
  if (error) {
    console.error('Error:', error);
    return;
  }
  
  console.log('Data:', data);
}
```

**并行执行**:
```javascript
// ❌ 串行执行（慢）
async function fetchDataSerial() {
  const user = await fetch('/api/user').then(r => r.json());      // 1秒
  const posts = await fetch('/api/posts').then(r => r.json());    // 1秒
  const comments = await fetch('/api/comments').then(r => r.json()); // 1秒
  
  return { user, posts, comments }; // 总共3秒
}

// ✅ 并行执行（快）
async function fetchDataParallel() {
  const [user, posts, comments] = await Promise.all([
    fetch('/api/user').then(r => r.json()),
    fetch('/api/posts').then(r => r.json()),
    fetch('/api/comments').then(r => r.json())
  ]);
  
  return { user, posts, comments }; // 总共1秒
}

// ✅ 部分串行，部分并行
async function fetchDataMixed() {
  // 先获取用户信息
  const user = await fetch('/api/user').then(r => r.json());
  
  // 然后并行获取用户的帖子和评论
  const [posts, comments] = await Promise.all([
    fetch(`/api/users/${user.id}/posts`).then(r => r.json()),
    fetch(`/api/users/${user.id}/comments`).then(r => r.json())
  ]);
  
  return { user, posts, comments };
}
```

**循环中的async/await**:
```javascript
// ❌ 错误：forEach不支持async/await
async function processItems(items) {
  items.forEach(async (item) => {
    await processItem(item); // 不会等待
  });
  console.log('Done'); // 会立即执行
}

// ✅ 正确：使用for...of
async function processItemsSerial(items) {
  for (const item of items) {
    await processItem(item); // 顺序执行
  }
  console.log('Done');
}

// ✅ 正确：使用Promise.all并行
async function processItemsParallel(items) {
  await Promise.all(items.map(item => processItem(item)));
  console.log('Done');
}

// ✅ 正确：使用map + Promise.all
async function processItemsMap(items) {
  const results = await Promise.all(
    items.map(async (item) => {
      const result = await processItem(item);
      return result;
    })
  );
  
  return results;
}
```

**实战应用场景**:
```javascript
// 1. 数据预加载
async function preloadData() {
  const cacheKey = 'userData';
  
  // 先检查缓存
  const cached = localStorage.getItem(cacheKey);
  if (cached) {
    return JSON.parse(cached);
  }
  
  // 缓存未命中，请求数据
  const data = await fetch('/api/data').then(r => r.json());
  localStorage.setItem(cacheKey, JSON.stringify(data));
  
  return data;
}

// 2. 条件请求
async function fetchUserProfile(userId) {
  const user = await fetch(`/api/users/${userId}`).then(r => r.json());
  
  // 根据用户类型请求不同数据
  if (user.type === 'premium') {
    user.premiumFeatures = await fetch(`/api/premium/${userId}`).then(r => r.json());
  }
  
  return user;
}

// 3. 分页加载
async function loadAllPages(baseUrl) {
  const results = [];
  let page = 1;
  let hasMore = true;
  
  while (hasMore) {
    const response = await fetch(`${baseUrl}?page=${page}`).then(r => r.json());
    results.push(...response.data);
    
    hasMore = response.hasNextPage;
    page++;
  }
  
  return results;
}

// 4. 重试逻辑
async function fetchWithRetry(url, options = {}, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, options);
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      
      return await response.json();
    } catch (error) {
      if (i === maxRetries - 1) {
        throw error;
      }
      
      console.log(`Retry ${i + 1}/${maxRetries}...`);
      await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
    }
  }
}

// 5. 请求去重
const pendingRequests = new Map();

async function dedupeRequest(url) {
  // 如果已有相同请求，返回该Promise
  if (pendingRequests.has(url)) {
    return pendingRequests.get(url);
  }
  
  // 创建新请求
  const promise = fetch(url)
    .then(r => r.json())
    .finally(() => {
      pendingRequests.delete(url);
    });
  
  pendingRequests.set(url, promise);
  return promise;
}

// 6. 超时控制
async function fetchWithTimeout(url, timeout = 5000) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeout);
  
  try {
    const response = await fetch(url, { signal: controller.signal });
    clearTimeout(timeoutId);
    return await response.json();
  } catch (error) {
    clearTimeout(timeoutId);
    
    if (error.name === 'AbortError') {
      throw new Error('Request timeout');
    }
    
    throw error;
  }
}
```

**async/await vs Promise链式调用**:
```javascript
// Promise链式调用
function getUserDataPromise(userId) {
  return fetch(`/api/users/${userId}`)
    .then(response => response.json())
    .then(user => {
      return fetch(`/api/users/${user.id}/posts`);
    })
    .then(response => response.json())
    .then(posts => {
      return { user, posts };
    })
    .catch(error => {
      console.error(error);
      throw error;
    });
}

// async/await（更清晰）
async function getUserDataAsync(userId) {
  try {
    const userResponse = await fetch(`/api/users/${userId}`);
    const user = await userResponse.json();
    
    const postsResponse = await fetch(`/api/users/${user.id}/posts`);
    const posts = await postsResponse.json();
    
    return { user, posts };
  } catch (error) {
    console.error(error);
    throw error;
  }
}

// 选择建议：
// - 简单链式调用：Promise更简洁
// - 复杂逻辑、多条件分支：async/await更清晰
// - 需要并行：Promise.all更直观
// - 循环处理：async/await更方便
```

**顶层await（ES2022）**:
```javascript
// 在模块顶层使用await（无需async函数包裹）
const data = await fetch('/api/data').then(r => r.json());

export { data };

// 注意：只在ES模块中可用
// <script type="module">或.mjs文件
```

---

#### 27. **数据结构 (Data Structures)** ⭐⭐
**重要性**: 中高 - 算法面试必备

**核心知识点**:
- 常见数据结构：栈、队列、链表、树、图、堆
- JavaScript中的实现
- 时间复杂度分析
- 实际应用场景

**1. 栈（Stack）- LIFO**:
```javascript
class Stack {
  constructor() {
    this.items = [];
  }
  
  push(element) {
    this.items.push(element);
  }
  
  pop() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items.pop();
  }
  
  peek() {
    if (this.isEmpty()) {
      return null;
    }
    return this.items[this.items.length - 1];
  }
  
  isEmpty() {
    return this.items.length === 0;
  }
  
  size() {
    return this.items.length;
  }
  
  clear() {
    this.items = [];
  }
}

// 应用场景1：括号匹配
function isValidParentheses(str) {
  const stack = new Stack();
  const pairs = { '(': ')', '[': ']', '{': '}' };
  
  for (let char of str) {
    if (char in pairs) {
      stack.push(char);
    } else if (Object.values(pairs).includes(char)) {
      if (stack.isEmpty() || pairs[stack.pop()] !== char) {
        return false;
      }
    }
  }
  
  return stack.isEmpty();
}

console.log(isValidParentheses('()')); // true
console.log(isValidParentheses('([{}])')); // true
console.log(isValidParentheses('([)]')); // false

// 应用场景2：浏览器历史记录
class BrowserHistory {
  constructor() {
    this.backStack = new Stack();
    this.forwardStack = new Stack();
    this.current = null;
  }
  
  visit(url) {
    if