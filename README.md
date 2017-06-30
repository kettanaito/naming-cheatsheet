# Naming cheatsheet
Naming things is hard. Is it?

## Guidelines
* Pick **one** naming convention and follow it. Whether it is `likeThis`, or `like_this`, or anyhow else, it does not matter. What matters is consistency in your work.
* Name, whether of a variable, method, or something else, should be *short*, *descriptive* and *intuitive*:
  * **Short**. Variable should not take long to type, and therefore to remember,
  * **Descriptive**. Name of the variable should reflect what this variable possesses/does in the most efficient way,
  * **Intuitive**. Name of the variable should read naturally, as close to common speach as possible
```js
/* Bad namings */
const a = 5; // "a" could mean anything
const isPaginatable = (a > 10); // "Paginatable" sounds extremely unnatural

/* Good namings */
const postsCount = 5;
const shouldDisplayPagination = (postsCount > 10);
```
* Name should not duplicate the context when the latter is known, and when removing the context from the name does not decrease its readability:
```js
class MenuItem {
  /* Method name duplicates the context it is in "...MenuItem..." */
  handleMenuItemClick = (event) => { ... }
  
  /* This way it reads as MenuItem.handleClick() */
  handleClick = (event) => { ... }
}
```

## Methods
**Pattern:**
```
prefix? + action + high context (HC) + low context (LC)
```

### Actions
#### `get`
Access data immediately (i.e. shorthand getter of internal data).
```js
function getFruitsCount() {
  return this.fruits;
}
```
#### `fetch`
Request for variable/data, which takes time (i.e. async request).
```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', { ... });
}
```
#### `set`
Declaratively set `variableA` to next value `valueB`.
```js
function Component() {
  this.state = { fruits: 0 };

  function setFruits(nextFruits) {
    this.state.fruits = nextFruits;
  }
}
```
#### `reset`
Set something to its initial state/value.
```js
const initialFruits = 5;
const fruits = initialFruits;
setFruits(10); // fruits = 10

function resetFruits() {
  fruits = initialFruits;
}

resetFruits(); // fruits = 5
```
#### `compose`
Create new data from existing one.
```js
function composePageUrl(pageName, pageId) {
  return `${pageName.toLowerCase()}-${pageId}`;
}
```

#### `handle`
Handler for a dedicated action. Usually, used as a callback method.
```js
function handleLinkClick(event) {
  event.preventDefault();
  console.log('Clicked a link!');
}

link.addEventListener('click', handleLinkClick);
```

### Prefixes
#### `should`
Prompting computation usually returninng `Boolean` value.
```js
const currentUrl = 'https://dev.com';

function shouldUpdateUrl(url) {
  return (url !== currentUrl);
}
```
