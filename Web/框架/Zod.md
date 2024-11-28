自动推断静态数据类型

```ts
import { z } from 'zod'
```
## 模式
我们称任何广义上的数据类型为“模式”
如，创建一个对象模式，包含一个字符串模式：

```ts
const User = z.object({ 
    username: z.string(), 
});
```

### 验证
`.parse()` 方法验证一个数据是否符合先前创建的模式，是则返回数据，否则抛出错误
```ts
const numberSchema = z.string();
try { 
    numberSchema.parse("Not a number");
} catch (e) { 
    console.error(e.errors); 
}
```