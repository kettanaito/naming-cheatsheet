# Naming cheatsheet
Naming things is hard. Is it?

## Guidelines
* Pick **one** naming convention and follow it. Whether it is `likeThis`, or `like_this`, or anyhow else, it does not matter. What matters is consistency in your work.
* Name, whether of a variable, method, or something else, should be *short*, *descriptive* and *intuitive*:
  * **Short**. Variable should not take long to type, and therefore to remember,
  * **Descriptive**. Name of the variable should reflect what this variable possesses/does in the most efficient way,
  * **Intuitive**. Name of the variable should read naturally, as close to common speach as possible
```js
/* Bad */
const a = 5; // "a" could mean anything
const isPaginatable = (a > 10); // "Paginatable" sounds extremely unnatural

/* Good */
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
* Name should reflect expected result:
```js
/* Bad */
const isEnabled = this.props.enabled;
return (<Button disabled={!isEnabled} />);

/* Good */
const isDisabled = this.props.disabled;
return (<Button disabled={isDisabled} />);
```

## Pattern
```
prefix? + action (A) + high context (HC) + low context (LC)
```
This is not a rule, but rather a pattern which can be applied quite often when naming variables.

> Keep in mind, that order of the contexts affects the core meaning of a variable. For example, `shouldUpdateComponent` means *you* are about to update a component, while `shouldComponentUpdate` tells you that *component* will update on itself, and you are but controlling whether it should.
In other words, high context emphasizes the meaning of the variable.

### Example
| Name | Prefix | Action | High context | Low context |
| ---- | ---- | ------ | ------------ | ----------- |
| `getPost` | | `get` | `Post` |  |
| `getPostData` | | `get` | `Post` | `Data` |
| `handleClickOutside` | | `handle` | `Click` | `Outside` |
| `shouldDisplayMessage` | `should` | `Display` | `Message`| |

## Naming methods

### Action
#### `get`
Access data immediately (i.e. shorthand getter of internal data).
```js
function getFruitsCount() {
  return this.fruits;
}
```
#### `fetch`
Request for data, which takes time (i.e. async request).
```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', { ... });
}
```
#### `set`
Declaratively set `variableA` with `valueA` to `valueB`.
```js
function Component() {
  this.state = { fruits: 0 };

  function setFruits(nextFruits) {
    this.state.fruits = nextFruits;
  }
}
```
#### `reset`
Set something back to its initial value.
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
Create new data from the existing one. Probably, applicable mostly to strings.
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

## Prefixes
Prefixes enhance variables or methods, indicating additional meaning behind them.

#### `is`
Describes certain characteristic or state of the context.
```js
const color = 'blue';
const isBlue = (color === 'blue'); // characteristic
const isRemoved = false; // state

if (isBlue && !isRemoved) {
  console.log('The color is blue and it is present!');
}
```

#### `min`/`max`
Represent minimum or maximum value. Usually describe allowed limits.
```js
function PostsList() {
  this.minPosts = 3;
  this.maxPosts = 10;
}
```

#### `has`
Describes whether current context possesses a certain value or state.
```js
/* Bad */
const isProductsExist = (productsCount > 0);
const areProductsPresent = (productsCount > 0);

/* Good */
const hasProducts = (productsCount > 0);
```

#### `should`
Reflects conditional statement (returns `Boolean` value).
```js
const currentUrl = 'https://dev.com';

function shouldUpdateUrl(url) {
  return (url !== currentUrl);
}
```
