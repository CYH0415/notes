在 JavaScript 中，`null` 通常是显式赋值的，表示有意将某个变量设置为空值。然而，除了显式赋值外，某些内置 API 或框架有时会返回 `null`，具体场景包括但不限于以下情况：

1. **DOM 操作中获取元素**：
   当使用 DOM API 获取不存在的元素时，方法如 `document.getElementById()` 会返回 `null`。
   ```javascript
   let element = document.getElementById('nonExistentId');
   console.log(element);  // 输出: null
   ```

2. **正则表达式匹配**：
   如果正则表达式未匹配到任何结果，`match` 方法会返回 `null`。
   ```javascript
   let str = "Hello, World!";
   let result = str.match(/notfound/);
   console.log(result);  // 输出: null
   ```

3. **JSON 解析**：
   当解析 `JSON` 字符串时，如果遇到 `null`，解析后的值也是 `null`。
   ```javascript
   let jsonString = '{"key": null}';
   let obj = JSON.parse(jsonString);
   console.log(obj.key);  // 输出: null
   ```

4. **函数中的默认返回值**：
   虽然函数默认返回 `undefined`，但在某些编程约定或特定情况下，函数可能显式返回 `null` 来表示特殊的空值情况。
### 总结

虽然在大多数情况下 `null` 是显式赋值的，但在与一些内置 API 交互时，有可能会得到 `null`。理解这些情况有助于在编写代码时正确处理 `null` 值，避免潜在的错误。