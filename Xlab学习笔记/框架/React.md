不知道从何谈起，先记录一些实践经验
## 安装
```bash
npm create-react-app app-name
```
会安装一些东西，初始化一些网页及样式，并自动提交 git
- 一个错误：`ENOENT: no such file or directory, lstat 'C:\Users\CYH\AppData\Roaming\npm'` 需要重新安装：`npm install -g npm@latest`
## 一些概念
在 React 中有两种“模型”数据：props 和 state。下面是它们的不同之处:
- **props** 至函数。它们使父组件可以传递数据给子组件，定制它们的展示。举个例子，`Form` 可以传递 `color` prop 至 `Button`。
 - **state** 像是组件的内存。它使组件可以对一些信息保持追踪，并根据交互来改变。举个例子，`Button` 可以保持对 `isHovered` state 的追踪。
 
props 和 state 是不同的，但它们可以共同工作。父组件将经常在 state 中放置一些信息（以便它可以改变），并且作为子组件的属性 **向下** 传递至它的子组件。
## 记录数据
使用 `usestate` 记录一些数据，参数是其初始值
```js
const [values, setValue] = useState(null)
```

## 传递带参数函数
有函数及按钮：
```js
function handleClick(i) {
    ...
 }
```
直接传递 `onClick = handleClick(0)` 会调用此函数，调用会导致重新渲染，重新渲染重复调用导致循环，此时可以：
`onClick = {() => handleClick(0)}`

# 例会
React：组件化
`strict mode`：渲染两遍，确保什么来着
`App.js` -> 导出 `App` -> `index.js` 使用此 `App` 并渲染
```jsx
function App() {
    return (
        <>
            <button>click</button>
            ...
        </>
    )
}

export default App
```
- 只能导出一个元素，需要用空标签包裹
此时可以封装重复的组件：
`/src/components/square.jsx` 中定义并导出此组件，然后在 `/src/App.js` 导入并使用 `<Square />`

### 父组件向子组件传递元素
```jsx
function Square({value}) {
    return <button>{value}</button>
}

export default Square
```
在外层使用：
```jsx
    ...
    <Square value="114514"/>
```
### 组件函数
小写于函数中
```jsx
function Square({value}) {
    function click() {
        const [values, setValue] = useState("")
        setValue("X")
    }
    return <button>{value}</button>
}
```
### 状态记录
见上 [[React#记录数据]]，也：
```js
import { useState } from "react"
...
const [values, setValue] = useState("")
```
若存储数组，则 `useState(Array(9).fill(""))`
### 传递函数

# 呃呃
浏览器->服务器
前端到浏览器：什么
框架启动服务：额 ![[Pasted image 20240705211020.png]]

**CSR**
*client side server*
所有的 react 代码打包到一个 js 文件发送到浏览器，客户端完成渲染
客户端过一会儿才显示出完整的界面
下载 js->渲染外壳->发送数据请求->渲染内容

**SSR**
*server side render*
服务端渲染
Pre-rendering：框架提前写一些东西在 html 文件，再发送给客户端
Data fetching：
预渲染的 html 与 js 水合
所有 js 加载完才能进行用户交互

**Suspense**
包裹一些组件，使其他组件先行水和，不影响用户交互
```html
<Suspense fallback={}>
    ...
</Suspense>
```

客户端组件可以交互，不能访问服务器数据
客户端组件是服务端组件的子组件 
# 钩子
## React 原生
- `useState`
```tsx
const [roomName, setRoomName] = useState<string | null>(localStorage.getItem('lastVisitedRoomName'));
```
- `useEffect`
将某个函数与某个值绑定，当值改变时触发
```tsx
useEffect(() => {
        if (messageAreaRef.current && messageListData) {
            messageAreaRef.current.scrollTop = messageAreaRef.current.scrollHeight;
        }
    }, [roomId]);
    
```
## SWR
十分清晰的 get 和 post 的 SWR 钩子使用
```tsx
const { // fetch room list data
        data: roomData
    } = useSWR<type.RoomListRes>('/api/room/list', getFetcher, {
        refreshInterval: 1000,
});

async function addNewRoom () {
        const newRoomName = prompt("Enter the name for the new room:") || "New Room";
        const newId = await addRoomTrigger({
            user: userName,
            roomName: newRoomName,
        })

        refreshData();
        console.log("Created room: " + newId.roomId);
    };

    const {
        trigger: addMessageTrigger
    } = useSWRMutation<
        null,
        Error,
        string,
        {
            roomId: number;
            content: string;
            sender: string;
        }
    >("/api/message/add", postFetcher);
```
## Ref
```tsx
const ref = useRef(null);
```
这一钩子返回一个对象，在 html 标签处使用 `ref={...}` 可以将这个元素绑定，此对象的 `current` 属性可以如 js 一般操作之，如：
```tsx
inputRef.current.focus();
```