<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="./naming-cheatsheet.png" alt="Naming cheatsheet" />
  </a>
</p>

[English](./README.md) | 简体中文

# 命名小抄

- [英语语言](#英语语言)
- [命名惯例](#命名惯例)
- [S-I-D](#s-i-d)
- [避免缩略语](#避免缩略语)
- [避免上下文重复](#避免上下文重复)
- [反映预期结果](#反映预期结果)
- [给函数命名](#给函数命名)
  - [A/HC/LC模式](#ahclc模式)
    - [动作](#动作)
    - [上下文](#上下文)
    - [前缀](#前缀)
- [单数和复数](#单数和复数)

---

为事物命名是很困难的。这张表试图让它变得更容易。

虽然这些建议可以适用于任何编程语言，但我将用JavaScript来说明它们的实际情况。

## 英语语言

在给你的变量和函数命名时，使用英语语言。

```js
/* Bad */
const primerNombre = 'Gustavo'
const amigos = ['Kate', 'John']

/* Good */
const firstName = 'Gustavo'
const friends = ['Kate', 'John']
```

> 不管你喜不喜欢，英语是编程中的主流语言：所有编程语言的语法都是用英语写的，还有无数的文档和教育材料也是如此。通过用英语写代码，你可以极大地提高代码的凝聚力。

## 命名惯例

选择**一个**命名惯例并遵循它。它可以是 `camelCase`，`PascalCase`，`snake_case`，或其他任何东西，只要它保持一致。许多编程语言在命名规则方面有自己的传统；查看你的语言的文档或研究Github上一些流行的存储库

```js
/* Bad */
const page_count = 5
const shouldUpdate = true

/* Good */
const pageCount = 5
const shouldUpdate = true

/* Good as well */
const page_count = 5
const should_update = true
```

## S-I-D

一个名字必须是 _简短的_ ，_直观的_ 和 _描述性的_ 。

- **短**。一个名字必须不需要花很长时间来输入，因此，要记住。
- **直观**。名字必须读起来自然，尽可能地接近普通话。
- **描述性**。一个名字必须以最有效的方式反映它所做的/所拥有的东西。

```js
/* Bad */
const a = 5 // "a" could mean anything
const isPaginatable = a > 10 // "Paginatable" sounds extremely unnatural
const shouldPaginatize = a > 10 // Made up verbs are so much fun!

/* Good */
const postCount = 5
const hasPagination = postCount > 10
const shouldPaginate = postCount > 10 // alternatively
```

## 避免缩略语

**不要**使用缩略语。它们只会降低代码的可读性。找到一个简短的、描述性的名字可能很难，但缩略语并不是不这样做的借口。

```js
/* Bad */
const onItmClk = () => {}

/* Good */
const onItemClick = () => {}
```

## 避免上下文重复

一个名字不应该与它所定义的上下文重复。如果不会降低名字的可读性的话，一定要把上下文从名字中删除。

```js
class MenuItem {
  /* Method name duplicates the context (which is "MenuItem") */
  handleMenuItemClick = (event) => { ... }

  /* Reads nicely as `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## 反映预期结果

一个名字应该反映出预期的结果。

```jsx
/* Bad */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* Good */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

---

# 给函数命名

## A/HC/LC模式

在命名函数时，有一种有用的模式可以遵循。

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

看看这个模式如何在下面的表格中应用。

| 名称                    | 前缀     | 动作(A)     | 高背景(HC)         | 低背景(LC)          |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getUser`              |          | `get`      | `User`            |                  |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **注意：** 上下文的顺序会影响变量的含义。例如，`shouldUpdateComponent`意味着**你**要更新一个组件，而`shouldComponentUpdate`告诉你**组件**会自己更新，而你只是控制它何时更新。
> 换句话说，**高的上下文强调了一个变量的意义**。

---

## 动作

你的函数名称中的动词部分。最重要的部分，负责描述该函数 _做什么_ 。

### `get`

立即访问数据（即内部数据的速记获取器）。

```js
function getFruitCount() {
  return this.fruits.length
}
```

> 参见[compose](#compose)。

### `set`

以声明的方式设置一个变量，将值`A`改为值`B`。

```js
let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

### `reset`

将一个变量设置为其初始值或状态。

```js
const initialFruits = 5
let fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

### `fetch`

请求一些数据，这需要一些不确定的时间（即异步请求）。

```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', {...})
}
```

### `remove`

从某处移除某物。

例如，如果你在搜索页面上有一个选定过滤器的集合，从该集合中移除其中一个过滤器就是`removeFilter`，**不是**`deleteFilter`（在英语中你也会自然地这样说）。

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

> 参见[delete](#delete).

### `delete`

将某样东西从存在的领域中彻底删除。

想象一下，你是一个内容编辑，有那个臭名昭著的帖子你想摆脱它。一旦你点击了一个闪亮的 "删除帖子"按钮，CMS就会执行`deletePost`动作，**不是**`removePost`。

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

> 参见[remove](#remove)。

### `compose`

从现有的数据中创建新的数据。主要适用于字符串, 对象, 或函数.

```js
function composePageUrl(pageName, pageId) {
  return (pageName.toLowerCase() + '-' + pageId)
}
```

> 参见[get](#get)。

### `handle`

处理一个动作。通常在命名回调方法时使用。

```js
function handleLinkClick() {
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

---

## 上下文

一个函数所操作的领域。

一个函数通常是对 _某物_ 的操作。说明它的可操作域是什么是很重要的, 或者至少是一个预期的数据类型。

```js
/* A pure function operating with primitives */
function filter(list, predicate) {
  return list.filter(predicate)
}

/* Function operating exactly on posts */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

> 一些特定语言的假设可能允许省略上下文。例如，在JavaScript中，`filter`通常是对数组进行操作。添加明确的`filterArray`就没有必要了。

--

## 前缀

前缀可以增强变量的含义。它很少在函数名中使用。

### `is`

描述当前上下文的一个特征或状态(通常是`boolean`)。

```js
const color = 'blue'
const isBlue = color === 'blue' // characteristic
const isPresent = true // state

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

### `has`

描述当前上下文是否拥有某个值或状态（通常是`boolean`）。

```js
/* Bad */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Good */
const hasProducts = productsCount > 0
```

### `should`

反映了一个积极的条件语句（通常是 "布尔"）与某一行动相联系。

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

### `min`/`max`

代表一个最小或最大的值。在描述边界或限制时使用。

```js
/**
 * Renders a random amount of posts within
 * the given min/max boundaries.
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

### `prev`/`next`

表示变量在当前环境中的上一个或下一个状态。在描述状态转换时使用。

```jsx
function fetchPosts() {
  const prevPosts = this.state.posts

  const fetchedPosts = fetch('...')
  const nextPosts = concat(prevPosts, fetchedPosts)

  this.setState({ posts: nextPosts })
}
```

## 单数和复数

和前缀一样，变量名也可以做成单数或复数，这取决于它们是持有一个值还是多个值。

```js
/* Bad */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Good */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```
