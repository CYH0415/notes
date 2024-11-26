## 路由
自动根据 `/app` 下的文件夹建立路由，每个文件夹必须有：
- `layout`：存放字体，CSS，Meta data 等
- `page`：核心文件，没有此文件则这里没有路由，存放 UI
一般在 `/app/api` 下：
- `route`：使其成为后端路由
## 导航
页面跳转
1. `<Link>`：即 `<a>`
2. `useRouter`：可以控制跳转的逻辑
```ts
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
    const router = useRouter()
    return (
        <button onClick={() => router.push('/another_page')}>
            another page
        </button>
    )
}
```
3. `redirect(url)`，用于服务端
## 后端迁移？
将以下文件置于`app/api/route.ts`
```ts
export async function GET(request: Request) {
      const res = await fetch('/...', {
        headers: {
          'Content-Type': 'application/json',
          'API-Key': process.env.DATA_API_KEY,
        },
      })
      const data = await res.json()
     
      return Response.json({ data })
    }
```

```ts
export async function POST(request: Request) {
      const res = await fetch('/...', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'API-Key': process.env.DATA_API_KEY!,
        },
        body: JSON.stringify({ time: new Date().toISOString() }),
      })
     
      const data = await res.json()
     
      return Response.json(data)
    }
```
## 鉴权
一般在中间件完成
```ts
import { NextResponse } from 'next/server'
import type { NextResponse } from 'next/server'

export function middleware(req: Request) {
    let cookie = req.cookies.get('nextjs')
    ...
    const res = NextResponse.next()
    ...
    return res
}
```
## 客户端组件
在文件开头添加 `'use client'`
不能访问服务端数据
- `state`，`effect`，`localStorage`
## 服务端组件
在文件开头添加 `'use server'`，将里面的函数声明为 server actions，服务端正常调用，客户端调用时表现为 rpc
代码编译后，每个 server action 函数会对应一个 ID，客户端调用时在请求（且是 POST）头包含此 ID
- 服务端组件不能使用 `hook`，`useState`，事件监听器等，这些东西必须用在客户端组件中，然后导入到服务端

| 你需要做什么？                                                           | 服务器组件 | 客户端组件 |
| ----------------------------------------------------------------- | ----- | ----- |
| 获取数据                                                              | √     | ×     |
| 访问后端资源（直接）                                                        | √     |       |
| 在服务器上保留敏感信息（访问令牌、API 密钥等）                                         | √     |       |
| 保持对服务器的大量依赖/减少客户端 JavaScript                                      | √     |       |
| 添加交互性和事件监听器（`onClick()`、`onChange()` 等）                           |       | √     |
| 使用状态和生命周期效果（`useState()`、`useReducer()`、`useEffect()` 等）          |       | √     |
| 使用仅限浏览器的 API                                                      |       | √     |
| 使用依赖于状态、效果或仅限浏览器的 API 的自定义钩子                                      |       | √     |
| 使用 [React 类组件](https://react.nodejs.cn/reference/react/Component) |       | √     |
### 使用方法
在一个页面中，不可避免的存在客户端组件与服务端组件混合使用的情况。在使用中，尽量把服务端组件定义在外层去获取数据，把客户端组件定义在最里层去处理用户交互

注意：`在服务端组件中可以使用import导入客户端组件，但在客户端组件中不能使用import导入服务端组件，只能把服务端组件作为props传递给客户端组件`

在客户端组件中直接使用import导入服务端组件（错误使用示例）：

```tsx
'use client'
 
// You cannot import a Server Component into a Client Component.
import ServerComponent from './Server-Component'
 
export default function ClientComponent({
  children,
}: {
  children: React.ReactNode
}) {
  const [count, setCount] = useState(0)
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
 
      <ServerComponent />
    </>
  )
}
```

如果服务端组件仅包含静态文件，通常不会引发报错。然而，一旦服务端组件中涉及到接口请求，就会导致接口被多次不必要地调用，并且如果接口数据频繁变动。这种情况下，服务端请求得到的接口数据可能与客户端请求的数据不一致，进而引发渲染错误。

正确做法，把服务端组件作为props传递给客户端组件

```tsx
'use client'
 
import { useState } from 'react'
 
export default function ClientComponent({
  children,
}: {
  children: React.ReactNode
}) {
  const [count, setCount] = useState(0)
 
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
      {children}
    </>
  )
}
```

```tsx
import ClientComponent from './client-component'
import ServerComponent from './server-component'
 
// Pages in Next.js are Server Components by default
export default function Page() {
  return (
    <ClientComponent>
      <ServerComponent />
    </ClientComponent>
  )
}
```