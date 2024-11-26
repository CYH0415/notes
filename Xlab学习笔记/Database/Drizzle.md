# 结构
## Schema 架构
数据库中表的定义与结构
- 要定义一个表，以及包含它的架构：
```ts
import { pgTable, serial, text, integer } from 'drizzle-orm/pg-core'; 

const users = pgTable('users', { 
    ...
}); 

const mySchema = { users, }; 

export default mySchema;
```
- 架构可以含多个表
- 可以抽象为更高层的创建器，以便添加前缀等：
```ts
export const createTable = pgTableCreator((name) => `chat-room-server_${name}`);
```
- 使用如 `yarn db:push` 将架构推入数据库
## 创建表
```ts
export const rooms = createTable(
  "room",
  {
    roomId: serial("room_id").primaryKey(),
    roomName: varchar("room_name", { length: 256 }).notNull(),
    creator: varchar("creator", { length: 256 }).notNull(),
    createdAt: timestamp("created_at", { withTimezone: true })
      .default(sql`CURRENT_TIMESTAMP`)
      .notNull(),
    lastMessageId: integer("last_message_id").notNull(),
  },
  (table) => ({
    roomNameIndex: index("room_name_idx").on(table.roomName),
  }),
);
```
- 如 `roomId` 为 TS 中标识符
- 如 `room_id` 为数据库内部列名
- `serial` 为自增整数
# ORM
一些模拟 ORM 操作
## SELECT
实际上与 SQL 语法极其相似。

```ts
const m = await db.
    select().from(messages).limit(1).orderBy(desc(messages.time))
    .where(eq(messages.roomId, roomId));
```
## QUERY
我们有 `.findMany()` 与 `.findFisrt()` 作为基于 ` SELECT ` 语法的查找，要添加条件，使用 `with`、`where` 等字段
## INSERT
