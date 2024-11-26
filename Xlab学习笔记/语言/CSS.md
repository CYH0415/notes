```css
selector {
    attribute : value;
}
```
---
## Selector 选择器
### 类型
```css
h1 {
}
```
### 类
```css
.highlight.class {
}
```
- 匹配含所有指定类的元素，不加空格地写入类名
### ID
```css
#unique {
}
```
### 属性
| 选择器               | 描述                              | 注                          |
| ----------------- | ------------------------------- | -------------------------- |
| `a[attr]`         | 含有某属性                           |                            |
| `a[attr=value]`   | 含有某属性，其值特定                      |                            |
| `a[attr~=value]`  | 含有某属性，且属性中有此值                   | `class="A B"`即有两个值         |
| `a[attr\|=value]` | 含有某属性，其值以`value-`为开头，或正为`value` | 注意连字符                      |
| `li[attr^=value]` | 开头                              |                            |
| `li[attr$=value]` | 结尾                              |                            |
| `li[attr*=value]` | 属性出现字段                          | `class="AB"`可以这样匹配到`A`或`B` |
| `a[attr=value i]` | 忽略大小写                           |                            |

### 伪类，伪元素
伪类以冒号开头：
```css
a:first-child{
  font-size: 150%;
}
```
一些常用伪类：
- `:first-child`
- `:last-child`
- `:only-child`
- `:hover`：指针悬浮
- `:focus`：键盘选定
伪元素以双冒号开头：
```css
article p::first-line {
  font-size: 120%;
  font-weight: bold;
}
```
一些伪元素：
- `::after`：原有元素的实际内容之后的第一个可样式化元素
- `::before`
- `::first-line`
- `::first-letter`
- `::selection`：被选中部分
### 关系
- 所有后代：空格
```css
article p
```
- 直接子代：`>`
```css
body > h1
```
- 邻接：`+`，同级的元素旁边
```css
h1 + p
```
匹配紧邻标题的段落
- 通用兄弟：`~`，此元素之后的所有同级元素
```css
h1 ~ p
```
此标题下的所有段落
## 层叠、优先级与继承
### 层叠
同级规则应用于同一元素，总是后者生效
### 继承
子代继承父亲的一部分样式，其中 `width`、`margin`、`padding` 和 `border` 不继承
设置样式时，可用以下属性值：
- `inherit`：继承父类
- `initial`：初始值
- `revert`：重置为默认值
- `revert-layer`
- `unset`：自然值，属性是自然继承则取`inherit`，否则取`initial`
### 优先级
样式冲突时参考：`!important` > 内联 > ID > 类 > 元素
- 此优先级具有累加性，累加对应匹配次数
- 具有`!important`声明的规则会覆盖其他规则
- 内联，即在元素标签内的`style`
### 层叠层
层叠层的优先级 #待补充
#### 创建层叠层，以及添加样式
```css
@layer site;
@layer site {
    font-size: 120%;
}
@layer {
    background-color: yellow;
}
@layer page {
    color: green;
}
body {
    color: green
}
```
- 创建了具名层、匿名层与未分层的样式
#### 导入层叠层
```css
@import url("test.css") layer(page);
@import url("yum.css") layer(page);
```
- 须在任何样式之前
#### 嵌套层
```css
@layer site.wide {
    color: black;
}
```
#### 优先级
#待补充
## Box Model
![[Pasted image 20240519132615.png]]
把HTML元素看作盒子
盒子有**区块盒子**与**行内盒子**，**内部显示**与**外部显示**
```css
.block {
    display: block;
}
```
