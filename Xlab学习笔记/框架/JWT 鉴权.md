一种奇异搞笑的导入方式：
```ts
jwt = require("jsonwebtoken");
const secret = "secret";
```

要用 jwt 签名：
```ts
    const token = jwt.sign(u, secret, { expiresIn: "114514 days" });
```

要解密 jwt 签名：
```ts
    const u = jwt.verify(opts.token, "secret");
```

# 修改浏览器 cookies
要使 trpc 的 api context 具有修改、读取 cookies 的功能
## 1. 修改前后端通信参数
### 1. 创建接口与函数
```ts
export type JWTUserType = {
    uid?: number,
    userName?: string,
}
```

```ts
export function createContextInner(opts: { token: string }): JWTUserType {
    if(!opts.token) {
        return {};
    }
    try {
        const u = jwt.verify(opts.token, "secret") as JWTUserType;
        if (u.uid)
            return u;
        else
            return {};
    } catch (e) {
        console.log(e);
        console.log("Verift jwt error.")
        return {};
    }
}
```
使得路由能够将 cookies 解密为用户信息
### 2. 于 `~/server/api/trpc.ts
作如下修改：
```ts
export const createTRPCContext = async (
  cookies: ReadonlyRequestCookies | RequestCookies,
  setCookie: (name: string, value: string) => void,
) => {
  return {
    ...createContextInner({ token: cookies?.get("token")?.value ?? "" }),
    db,
    setCookie,
  };
};
```

使得后端拿到的 context 具有：
1. `createContextInner` 返回值，即用户信息或空
2. `db`
3. `setCookie` 函数
并删去 trpc 自带的 `headers` 字段
### 3. 于 `~/trpc/server.ts`
作如下修改：
```ts
const createContext = cache(() => {
  const heads = new Headers(headers());
  heads.set("x-trpc-source", "rsc");
  return createTRPCContext(
    cookies(),
    (name, value) => {
      console.log(`Setting cookie: ${name}=${value}}`);
      cookies().set(name, value, {
        maxAge: 114514,
      })
    }
  );
});
```
使得后端接收 cookies，并定义了 `setCookie()` 的逻辑
### 4. 于 `~/app/api/trpc/[trpc]/route.ts`
作如下修改：
```ts
const createContext = async (req: NextRequest, setCookie: (name: string, value: string) => void) => {
  return createTRPCContext(req.cookies, setCookie);
};

const handler = (req: NextRequest) =>
  fetchRequestHandler({
    endpoint: "/api/trpc",
    req,
    router: appRouter,
    createContext: () => createContext(req, (name, value) => {
      console.log(`Setting cookie: ${name}=${value}}`);
      cookies().set(name, value, {
        maxAge: 114514,
      })
    }),
    onError:
      env.NODE_ENV === "development"
        ? ({ path, error }) => {
            console.error(
              `❌ tRPC failed on ${path ?? "<no-path>"}: ${error.message}`
            );
          }
        : undefined,
  });
```
使得前端将请求的 cookies 与 `setCookie()` 函数传给服务器
## 2. 修改 api 相应逻辑
现在在定义 api 时，`ctx` 具有 `uid`、`userName`、`setCookie()` 等字段，通过如下方式，可以将用户信息加密并写入 cookie
```ts
login: publicProcedure
    .input(z.object({
        userName: z.string(),
        password: z.string(),
    }))
    .mutation(async ({ ctx, input }) => {
        ... // 验证用户与密码
        const token = jwt.sign(u, secret, { expiresIn: "114514 days" });
        ctx.setCookie("token", token);
        return {
            user: u,
            get_uid: ctx.uid || "null",
            get_name: ctx.userName || "null",
            cookie: cookies(),
        };
    }),
```
