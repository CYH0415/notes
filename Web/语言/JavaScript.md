## 数据结构
- 弱类型
- 动态类型
## 函数
```js
function mine(argu1, argu2 = 10) {
}
```
- 在函数后加括号即立刻调用，在添加事件监听器等时注意
- 可选参数加 `=`
### 匿名函数
先看一个原型：`event` 见[[#^573862|事件对象]]
```js
function logKey(event) {
  console.log(`You pressed "${event.key}".`);
}

textBox.addEventListener("keydown", logKey);
```
**匿名函数**用于传递函数给另一函数，而不需先声明
```js
textBox.addEventListener("keydown", function (event) {
  console.log(`You pressed "${event.key}".`);
});
```
### 箭头函数
传递**匿名函数**的一种方法，用 `(event) => ` 代替了 `function(event)`，若有多个参数，只需添加在括号内
```js
textBox.addEventListener("keydown", (event) =>
  console.log(`You pressed "${event.key}".`),
);
```
对只有一行 `return` 语句的函数，可以：
```js
const originals = [1, 2, 3];
const doubled = originals.map(item => item * 2);
```
### 闭包
**闭包**（closure）是一个函数以及其捆绑的周边环境状态（**lexical environment**，**词法环境**）的引用的组合，随着函数的创建而创建，而不随函数作用域结束而消灭
```js
function makeAdder(x) {
  return function (y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2)); // 7
console.log(add10(2)); // 1
```
- 相当于将 5 和 10 保存在了 `add5` 和 `add10` 中

## 事件
一些常用事件：
- `click`
- `focus`：聚焦
- `blur`：失焦
- `dblclick`
- `mouseover`
- `mouseout`
最好不要使用内联事件处理器
### 添加与移除事件监听器
```js
btn.addEventListener("click", function, { signal: controller.signal });
btn.removeEventListener("click", function);
// or
controller.abort();
```
- 可添加多个
### 事件处理器
```js
btn.onclick = funciton;
```
- 会覆盖之前的
### 事件对象

^573862

事件对象是自动传递给函数的 `event` 对象
```js
function bgChange(event) {
  const rndCol = `rgb(${random(255)}, ${random(255)}, ${random(255)})`;
  e.target.style.backgroundColor = rndCol;
  console.log(event);
}

btn.addEventListener("click", bgChange);
```
#### 属性与额外属性
```js
textBox.addEventListener("keydown", (event) => {
  output.textContent = `You pressed "${event.key}".`;
});
```
事件 `keydown` 会创造 `KeyboardEvent` 对象，其有一个 `key` 属性，是触发事件时按下的键
类似的，`video` 有 `play()` 方法
#### 阻止默认行为
```css
  e.preventDefault();
```
如表单验证
#### 冒泡
最先触发点击的元素的事件，然后依次向外触发其相应父元素事件
例如，点击了 `div` 中的 `button`，会触发 `button` 的 `click` 事件后触发 `div` 的 `click`
- 阻止冒泡：
```js
event.stopPropagation();
```
#### 捕获
```js
document.body.addEventListener("click", handleClick, { capture: true });
container.addEventListener("click", handleClick, { capture: true });
button.addEventListener("click", handleClick);
```
这样则先在最小嵌套元素（最外层）触发
#### 委托
```js
container.addEventListener("click", (event) => {
  event.target.style.backgroundColor = bgChange();
});
```
这样，子元素的事件先冒泡到父元素，再由父元素通过 `event.target` 获取目标元素
- `event.currentTarget` 则访问处理该事件的元素，上面的例子即父元素
## 对象
```js
const objectName = {
  member1Name: member1Value,
  member2Name: member2Value,
  speak() {
    console.log("114514");
  },
};
```
包含**属性**与**方法**
- 对象的属性也可以是对象
- 点表示法：`object.member.smallerMember`，不能接受变量，只能直接写出
- 括号表示法：`object["member"]["smallerMember"]`，可以接受变量作为属性名
- 可以动态地添加成员
- 用 `this` 指定当前代码运行时的对象
### 构造函数
```js
function Person(name) {
    const obj = {};
    obj.name = name;
    obj.introduceSelf = function () {
        console.log(`你好！我是 ${this.name}。`);
    };
    return obj;
}
```
- 一般，构造函数以其创建的对象名称命名，且首字母大写
- 要使用 `new` 调用构造函数：
```js
const van = new Person("van");
```
### 对象原型
所有对象拥有一个内置属性，称为 **prototype**。这也是一个对象，故其拥有原型，构成**原型链**，终于以 `null` 为原型的对象。
访问对象属性时，如果找不到，则向其原型查找，直到末端返回 `undefined`
- `Object.getPrototypeOf()` 返回了原型
- `Object.prototype` 是所有原型的基础
**属性遮蔽**：同名属性存在，则不会向原型查找，造成了遮蔽
#### 设置原型
1. `Object.create()` 方法
```js
const personPrototype = {
  greet() {
    console.log("hello!");
  },
};

const carl = Object.create(personPrototype);
```
2. 构造函数
```js
const personPrototype = {
  greet() {
    console.log(`你好，我的名字是 ${this.name}！`);
  },
};

function Person(name) {
  this.name = name;
}

Object.assign(Person.prototype, personPrototype);
const reuben = new Person("Reuben");
reuben.greet(); // 你好，我的名字是 Reuben！
```
- 其中，`name` 为**自有属性**，即不从原型中继承
## 类
使用 `class` 关键字声明：
```js
class Person {
    name;
    
    constructor(name) {
        this.name = name;
    }
    
    introduceSelf() {
        console.log(`Hi! I'm ${this.name}`);
    }
}
```
- 使用 `constructor` 关键字声明构造函数，可以省略
- 调用构造函数时仍使用类名，如
```js
const van = new Person("van");
```
### 继承
```js
class Student extends Person {
    age;
    
    consturctor(name, age) {
        super(name);
        this.age = age;
    }
    
    introduceSelf() {
        console.log(`114514! I'm ${this.name} of ${this.age}`);
    }
}
```
- 使用 `super()` 调用父类的构造函数，并传递 `name`
- 覆盖 `introduceSelf()` 函数
### 封装
```js
class Student extends Person {
    #age;
    
    consturctor(name, age) {
        super(name);
        this.#age = age;
    }
    
    introduceSelf() {
        console.log(`114514! I'm ${this.name} of ${this.#age}`);
    }
    
    #speak() {
        console.log("1919810!");
    }
}
```
- 完成了 `age` 和 `speak()` 的封装
## JSON #待补充 
## 异步
### Promise
Promise 是一个由异步函数返回的对象，指示操作当前的状态
- 不能像操作一般对象一样操作 Promise 对象：
> [!failure]
> ```js
> const promise = fetchProducts();
> console.log(promise[0].name);
> ```

- 而应使用 `then()` 方法：
> [!success]
> ```js
> const promise = fetchProducts();
> promise.then((data) => console.log(data[0].name));
> ```

Promise 具有三种状态：
- `pending`：待定
- `fulfilled`：兑现，此时调用 `then()`
- `rejected`：拒绝，此时调用 `catch()`
后两种状态统称为 `settled`（敲定）
若 Promise 被敲定，或被锁定以跟随另一个 Promise，称 `resolved`（解决）
#### Promise 的链式使用
当有多个异步函数，且开始下个函数前要完成前一个函数，使用 Promise 链
注意 `then()` 方法仍然返回 Promise 对象
```js
const fetchPromise = fetch(
  "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);

fetchPromise
  .then((response) => response.json())
  .then((data) => {
    console.log(data[0].name);
  });
```
#### 合并 Promise
当所有 Promise 都兑现，且不相互依赖，使用 `promise.all()`，其接受 Promise 数组，返回单一 Promise：
- 当且仅当数组中*所有*的 Promise 都被兑现时，才会通知 `then()` 处理函数并提供一个包含所有响应的数组，数组中响应的顺序与被传入 `all()` 的 Promise 的顺序相同。
- 如果数组中有*任何一个* Promise 被拒绝。此时，`catch()` 处理函数被调用，并提供被拒绝的 Promise 所抛出的错误。
例子：
```js
const fetchPromise1 = fetch(
      "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
);
const fetchPromise2 = fetch(
    "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found",
);
const fetchPromise3 = fetch(
    "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json",
);
    
Promise.all([fetchPromise1, fetchPromise2, fetchPromise3])
    .then((responses) => {
        for (const response of responses) {
            console.log(`${response.url}：${response.status}`);
        }
    })
    .catch((error) => {
        console.error(`获取失败：${error}`);
    });
```
### 异步函数
使用 `async` 关键字:
```js
async function test() {
 // do some thing
}
```
在一个返回 Promise 的函数之前使用 `await` 就可以使代码在此等待，直到 Promise 完成:
```js
const response = await fetch(
      "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
    );
```
- 只能对 `aysnc` 函数使用 `await`
### `fetch()`
```js
const fetchPromise = fetch(
      "https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json",
      );

console.log(fetchPromise);

fetchPromise.then((response) => {
    console.log(`已收到响应：${response.status}`);
});

console.log("已发送请求……");
```
`fetch` 函数返回一个 `response` 对象，其有一些属性：
- `status`：状态码
- `statusText`：状态信息
- `ok`：请求是否成功
- `headers`：包含头信息的对象
还有一些方法：
- `json()`：转化为 JSON
- `text()`
- `blob()`

### 回调 #待补充 
古老的方式