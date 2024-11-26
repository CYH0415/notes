- 静态类型
## 变量类型
- 变量名后用“冒号+类型”
- 首次赋值自动推断
### 顶层类型
- #### any
    关闭类型检查，可赋任何值，做任何运算，赋给别人
- #### unknown
    不能赋给 `any` 和 `unknown` 外，不能调用属性与方法，须先 `typeof` 缩小类型
- #### never
    底层类型
### 原始类型
即 `number` `string` `boolean` `bigint` `symbol`
原始类型没有方法，调用方法时会自动产生对应的包装类型，前三种也可以用构造函数：
```ts
const s = new String('hello');
```
大写类型同时包含字面量和包装类型，可以赋这两种值，小写类型只能赋前者
### object
`Object` 包含各种东西，没什么用，`object` 仅包含对象，数组和函数
- `{}` 是 `Object` 的简写
### 值类型
单个值是类型，如 `5` 是 `number` 的子类型，子类型可以赋值给父类型，除非使用类型断言 `as`
```ts
const x:5 = (4 + 1) as 5;
```
- `4 + 1` 是 `number`
### 联合类型
`|` 表示可以是多种值，通常要先 `typeof` 判断
### 交叉类型
`&` 表示交集，不相交集推断为 `never`（`string&number`）
主要用途是合成对象或为对象添加属性
### 数组
```ts
let a1:number[];
let a2:Array<number>; // 泛型写法
```
注意优先级 `|` < `[]`
#### 只读数组
`readonly number[]` 是 `number[]` 的父类型，传递时需要用 `as`，或：
- `ReadonlyArray<number>`
- `Readonly<number[]>`
- `as const`：生成只读值类型 `readonly [3, 4]`，可以作为数组与元组
### 元组
指定了类型的一些元素
```ts
let x:[string, string] = ['a', 'b'];
```
- 可选元素后加 `?`，但必须位于所有必选元素之后
- `...` + 数组或元组表示不限数量，可以位于任何位置
只读：
`readonly [string, number]` 或 `Readonly<[string, number]>` 或 `as const`
同样需要 `as` 断言传递给接受元组的函数
### 拓展运算符
数组传给函数时，`...arr` 是一个长度不确定的序列，可以传给 `console.log()` 等接受任意数量参数的函数
否则，应传递相应的元组或 `as const` 断言