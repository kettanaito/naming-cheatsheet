<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="./naming-cheatsheet.png" alt="Naming cheatsheet" />
  </a>
</p>

# Naming cheatsheet
Naming things is hard. Let's make it easier.

The purpose of this document is to break down and systematize the concepts and patterns commonly used for variable naming. Beware that *variable* in this document refers to variables, methods, and generally anything created during your programming work.

## Summary
* [Guidelines](#guidelines)
* [HC/LC Pattern](#hclc-pattern)
* **[Actions](#actions)**
  * [get](#get)
  * [fetch](#fetch)
  * [set](#set)
  * [reset](#reset)
  * [remove](#remove)
  * [delete](#delete)
  * [compose](#compose)
  * [handle](#handle)
* **[Prefixes](#prefixes)**
  * [is](#is)
  * [has](#has)
  * [should](#should)
  * [min/max](#minmax)
  * [prev/next](#prevnext)

## Guidelines
* Pick **one** naming convention and follow it. Whether it is `likeThis`, or `like_this`, or anyhow else, it does not matter. What matters is consistency in your work.

```js
/* Bad */
const pages_count = 5;
const shouldUpdate = true;

/* Good */
const pagesCount = 5;
const shouldUpdate = true;

/* Good as well */
const pages_count = 5;
const should_update = true;
```

* Name, whether of a variable, method, or something else, should be *short*, *descriptive* and *intuitive*:
  * **Short**. Variable should not take long to type and, therefore, to remember,
  * **Descriptive**. Name of the variable should reflect what it does/possesses in the most efficient way,
  * **Intuitive**. Name of the variable should read naturally, as close to the common speach as possible

```js
/* Bad */
const a = 5; // "a" could mean anything
const isPaginatable = (postsCount > 10); // "Paginatable" sounds extremely unnatural
const shouldPaginatize = (postsCount > 10); // Made up verbs are so much fun!

/* Good */
const postsCount = 5;
const shouldDisplayPagination = (postsCount > 10);
```

* Name should not duplicate the context when the latter is known, and when removing the context from the name does not decrease its readability:

```js
class MenuItem {
  /* Method name duplicates the context it is in (which is "MenuItem") */
  handleMenuItemClick = (event) => { ... }
  
  /* This way it reads as MenuItem.handleClick() */
  handleClick = (event) => { ... }
}
```
* Name should reflect the expected result:

```js
/* Bad */
const isEnabled = (itemsCount > 3);
return (<Button disabled={!isEnabled} />);

/* Good */
const isDisabled = (itemsCount <= 3);
return (<Button disabled={isDisabled} />);
```

## HC/LC Pattern
There is a useful pattern you may follow when naming your methods:

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

> **Note:** The order of the contexts affects the core meaning of a method. For example, `shouldUpdateComponent` means *you* are about to update a component, while `shouldComponentUpdate` tells you that *component* will update on itself, and you are but controlling whether it should do that right now.
In other words, **high context emphasizes the meaning of the variable**.

## Actions
Chosing a proper action name may grant explicit descriptiveness to your methods. This is a good place to start when naming your methods.

#### `get`
Accesses data immediately (i.e. shorthand getter of internal data).
```js
function getFruitsCount() {
  return this.fruits.length;
}
```
#### `fetch`
Requests for a data, which takes time (i.e. async request).
```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', { ... });
}
```

#### `set`
Declaratively sets a variable with `valueA` to `valueB`.

```js
const fruits = 0;

function setFruits(nextFruits) {
  fruits = nextFruits;
}

setFruits(5);
console.log(fruits) // 5
```

#### `reset`
Sets something back to its initial value or state.

```js
const initialFruits = 5;
const fruits = initialFruits;
setFruits(10);
console.log(fruits); // 10

function resetFruits() {
  fruits = initialFruits;
}

resetFruits();
console.log(fruits); // 5
```

#### `remove`
Removes something *from* somewhere. For example, if you have a collection of selected filters on a search page, removing one of them from the collection is `removeFilter`, **not** `deleteFilter` (and this is how you would naturally say it in English as well):

```js
const selectedFilters = ['price', 'availability', 'size'];

function removeFilter(filterName) {
  const filterIndex = selectedFilters.indexOf(filterName);

  if (filterIndex !== -1) {
    selectedFilters.splice(filterIndex, 1);
  }

  return selectedFilters;
}
```

#### `delete`
Completely erazes something from the realms of existance.

Imagine you are a content editor, and there is that notorious post you wish to get rid of. Once you clicked a shiny "Delete post" button, the CMS performed a `deletePost` action, **not** `removePost`.

```js
function deletePost(id) {
 return database.find({ id }).delete();
}
```

#### `compose`
Creates a new data from the existing one. Applicable mostly to strings or objects.

```js
function composePageUrl(pageName, pageId) {
  return `${pageName.toLowerCase()}-${pageId}`;
}
```

#### `handle`
Handles a dedicated action. Often used in naming the callback methods.

```js
function handleLinkClick(event) {
  event.preventDefault();
  console.log('Clicked a link!');
}

link.addEventListener('click', handleLinkClick);
```

## Prefixes
Prefixes enhance variables and methods, indicating an additional meaning behind them.

#### `is`
Describes certain characteristic or state of the current context (returns `Boolean`).

```js
const color = 'blue';
const isBlue = (color === 'blue'); // characteristic
const isPresent = true; // state

if (isBlue && isPresent) {
  console.log('The color is blue and it is present!');
}
```

#### `has`
Describes whether the current context possesses a certain value or state.

```js
/* Bad */
const isProductsExist = (productsCount > 0);
const areProductsPresent = (productsCount > 0);

/* Good */
const hasProducts = (productsCount > 0);
```

#### `should`
Reflects a positive conditional statement (returns `Boolean`) tightly coupled with a certain action.

```js
const currentUrl = 'https://dev.com';

function shouldUpdateUrl(url) {
  return (url !== currentUrl);
}
```

#### `min`/`max`
Represent minimum or maximum value. Handy when describing boundaries or allowed limits.

```js
function PostsList() {
  this.minPosts = 3;
  this.maxPosts = 10;
}
```

#### `prev`/`next`
Indicate the previous and the next state of the variable in the current context. Useful for describing a state mutation.

```jsx
function fetchPosts() {
  const prevPosts = this.state.posts;
 
  const fetchedPosts = fetch('...');
  const nextPosts = prevPosts.merge(fetchedPosts);

  return this.setState({ posts: nextPosts });
}
```
