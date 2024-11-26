一些抽象玩意儿的集合
### 文件结构
- `\src`
    - `\app`：在文件夹下建立 `page.ts`，也就完成了路由
        - `\_components`
        - `\api`：trpc 不使用这里的东西
    - `\server`
        - `\api`
            - `\routers`：在此写后端 api，注意导出到 `root.ts`
            - `root.ts`：在此定义对应的路由，将 api 传给前端
            - `trpc.ts`：包含中间件，关于写中间件，见-> [[中间件]]
        - `\db`
            - `index.ts`
            - `schema.ts`：关于数据库架构，见[[Drizzle]]
    - `\trpc`
        - `query-client.ts`：创建 react query 客户端
        - `react.tsx`：将 `unstable_httpBatchStreamLink` 改为 `httpBatchLink`，以便设置 cookie；此外，包含 `RouterInputs` 与 `RouterOutputs`，可以方便地取用类型
### 概念
`publicProcedure`：都可以用

对于需要经过中间件的 procedure，如此写：

```ts
export const authProcedure = publicProcedure.use( loginMiddleWare );
```

写 `procedure`：

```ts
hello: publicProcedure
    .input( z.string() )
    .query( async ({input}) => {
        return `hello, ${input}`;
    }),
```

-  `query` 是 get，`mutation` 是 post
而后，可以在前端找到对应的 api，以下是一个前端调用示例：
```ts
export function userHello() {
    const router = useRouter();
    const { data, isLoading, error } = api.{backend-path}.hello.useQuery("111");
    return (
        ...
    );
}
```
Api 返回许多数据，一般取上述三种数据（react query 特性，类似 SWR）

一个完整的案例，可见，开发者*十分不聪慧*地加入了重复的讯息：
```json
{
"status":"success",
"fetchStatus":"idle",
"isPending":false,
"isSuccess":true,
"isError":false,
"isInitialLoading":false,
"isLoading":false,
"data":{"message":"success","code":0,"data":{"rooms":[{"roomId":1,"roomName":"testing","lastMessage":null},{"roomId":2,"roomName":"testing","lastMessage":null},{"roomId":3,"roomName":"testing","lastMessage":null}]}},
"dataUpdatedAt":1723973806855,"error":null,"errorUpdatedAt":0,"failureCount":0,"failureReason":null,"errorUpdateCount":0,"isFetched":true,"isFetchedAfterMount":true,"isFetching":false,"isRefetching":false,"isLoadingError":false,"isPaused":false,"isPlaceholderData":false,"isRefetchError":false,"isStale":false,"trpc":{"path":"room.list"}}
```