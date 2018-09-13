<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="./naming-cheatsheet.png" alt="Naming cheatsheet" />
  </a>
</p>

# Naming cheatsheet
Naming things is hard. This sheet attempts to make it easier.

Although these suggestions can be applied to any programming language, I will use JavaScript to illustrate them in practice.

## Naming convention
Pick **one** naming convention and follow it. It may be `likeThis`, or `like_this`, or anyhow else, it does not matter. What matters is for it to remain consistent.

```js
/* Bad */
const pages_count = 5
const shouldUpdate = true

/* Good */
const pagesCount = 5
const shouldUpdate = true

/* Good as well */
const pages_count = 5
const should_update = true
```

## S-I-D

A name must be *short*, *intuitive* and *descriptive*:
  * **Short**. A name must not take long to type and, therefore, to remember,
  * **Intuitive**. Name must read naturally, as close to the common speach as possible
  * **Descriptive**. Name must reflect what it does/possesses in the most efficient way,

```js
/* Bad */
const a = 5 // "a" could mean anything
const isPaginatable = (postsCount > 10) // "Paginatable" sounds extremely unnatural
const shouldPaginatize = (postsCount > 10) // Made up verbs are so much fun!

/* Good */
const postsCount = 5
const shouldDisplayPagination = (postsCount > 10)
```

## Avoid contractions

Do **not** use contractions. They contribute to nothing but decreased readability of your code. Finding a short, descriptive name may be hard, but contraction is not an excude to not doing so.

```js
/* Bad */
const onItmClk = () => {}

/* Good */
const onItemClick = () => {}
```

## Avoid context duplication

A name should not duplicate the context in which it is defined. Always remove the context from a name if that doesn't decrease its readability.

```js
class MenuItem {
  /* Method name duplicates the context (which is "MenuItem") */
  handleMenuItemClick = (event) => { ... }
  
  /* Reads nicely as `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## Reflect expected result

A name should reflect the expected result.

```jsx
/* Bad */
const isEnabled = (itemsCount > 3)
return <Button disabled={!isEnabled} />

/* Good */
const isDisabled = (itemsCount <= 3)
return <Button disabled={isDisabled} />)
```

---

# Naming functions

## A/HC/LC Pattern
There is a useful pattern to follow when naming functions:

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

To illustrate, take a look at how this pattern may be applied in the table below.

| Name | Prefix | Action | High context | Low context |
| ---- | ---- | ------ | ------------ | ----------- |
| `getPost` | | `get` | `Post` |  |
| `getPostData` | | `get` | `Post` | `Data` |
| `handleClickOutside` | | `handle` | `Click` | `Outside` |
| `shouldDisplayMessage` | `should` | `Display` | `Message`| |

> **Note:** Context order affects the meaning of a method. For example, `shouldUpdateComponent` means *you* are about to update a component, while `shouldComponentUpdate` tells you that *component* will update on itself, and you are but controlling whether it should do that right now.
In other words, **high context emphasizes the meaning of a variable**.

---

## Actions

The verb part of your function name. The most important part responsible for describing what the function *does*.

#### `get`
Accesses data immediately (i.e. shorthand getter of internal data).
```js
function getFruitsCount() {
  return this.fruits.length;
}
```

#### `set`
Declaratively sets a variable with value `A` to value `B`.

```js
const fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

#### `reset`
Sets a variable back to its initial value or state.

```js
const initialFruits = 5
const fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

#### `fetch`
Requests for a data, which takes time (i.e. async request).
```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', { ... })
}
```

#### `remove`
Removes something *from* somewhere.

For example, if you have a collection of selected filters on a search page, removing one of them from the collection is `removeFilter`, **not** `deleteFilter` (and this is how you would naturally say it in English as well):

```js
const selectedFilters = ['price', 'availability', 'size']

function removeFilter(filterName) {
  const filterIndex = selectedFilters.indexOf(filterName)

  if (filterIndex !== -1) {
    selectedFilters.splice(filterIndex, 1)
  }

  return selectedFilters
}
```

#### `delete`
Completely erazes something from the realms of existance.

Imagine you are a content editor, and there is that notorious post you wish to get rid of. Once you clicked a shiny "Delete post" button, the CMS performed a `deletePost` action, **not** `removePost`.

```js
function deletePost(id) {
 return database.find({ id }).delete()
}
```

#### `compose`
Creates a new data from the existing one. Mostly applicable to strings or objects.

```js
function composePageUrl(pageName, pageId) {
  return `${pageName.toLowerCase()}-${pageId}`
}
```

#### `handle`
Handles a dedicated action. Often used in naming the callback methods.

```js
function handleLinkClick(event) {
  event.preventDefault()
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

---

## Context

A domain that a function operates on.

---

## Prefixes

Prefix enhances the meaning of a variable.

### `is`
Describes certain characteristic or state of the current context (returns `boolean`).

```js
const color = 'blue'
const isBlue = (color === 'blue') // characteristic
const isPresent = true // state

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

### `has`
Describes whether the current context possesses a certain value or state (returns `boolean`).

```js
/* Bad */
const isProductsExist = (productsCount > 0)
const areProductsPresent = (productsCount > 0)

/* Good */
const hasProducts = (productsCount > 0)
```

### `should`
Reflects a positive conditional statement (returns `Boolean`) tightly coupled with a certain action.

```js
function shouldUpdateUrl(url, expectedUrl) {
  return (url !== expectedUrl)
}
```

### `min`/`max`
Represent minimum or maximum value. Useful for describing boundaries or limits.

```js
function PostsList() {
  this.minPosts = 3
  this.maxPosts = 10
}
```

### `prev`/`next`
Indicate the previous and the next state of a variable in the current context. Useful for describing state transitions.

```jsx
function fetchPosts() {
  const prevPosts = this.state.posts
 
  const fetchedPosts = fetch('...')
  const nextPosts = prevPosts.merge(fetchedPosts)

  return this.setState({ posts: nextPosts })
}
```
