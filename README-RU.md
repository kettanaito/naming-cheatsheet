<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="./naming-cheatsheet.png" alt="Шпаргалка по именованию" />
  </a>
</p>

[English](./README.md) | Русский

# Шпаргалка по именованию

- [Английский язык](#english-language)
- [Соглашение об именовании](#naming-convention)
- [S-I-D](#s-i-d)
- [Избегайте сокращений](#avoid-contractions)
- [Избегайте дублирования контекста](#avoid-context-duplication)
- [Отражайте ожидаемый результат](#reflect-the-expected-result)
- [Именование функций](#naming-functions)
  - [A/HC/LC pattern](#ahclc-pattern)
    - [Действия](#actions)
    - [Контекст](#context)
    - [Префиксы](#prefixes)
- [Единственное и множественное число](#singular-and-plurals)

---

Называть вещи по именам очень трудно. Этa шпаргалка пытается сделать это проще.

Хотя эти предложения можно применить к любому языку программирования, я буду использовать JavaScript, чтобы проиллюстрировать их на практике.

## Английский язык

Используйте английский язык, когда именуете ваши переменные и функции.

```js
/* Плохо */
const primerNombre = 'Gustavo'
const amigos = ['Kate', 'John']

/* Хорошо */
const firstName = 'Gustavo'
const friends = ['Kate', 'John']
```

> Нравится вам это или нет, но английский является доминирующим языком в программировании: синтаксис почти всех языков программирования написан на английском, а также как бесчисленное число документации и обучающих материалов. Написав свой код на английском языке, вы значительно повысите его связность.

## Соглашение об именовании

Выберите **один** способ именования и следуйте ему. Это может быть `camelCase`, `PascalCase`, `snake_case`, или что-нибудь еще, главное, чтобы оно оставалось последовательным. Многие языки программирования имеют свои собственные традиции соглашения имен; ознакомьтесь с документацией для вашего языка или изучите некоторые популярные репозитории на Github!

```js
/* Плохо */
const page_count = 5
const shouldUpdate = true

/* Хорошо */
const pageCount = 5
const shouldUpdate = true

/* Ещё лучше */
const page_count = 5
const should_update = true
```

## S-I-D

Имя должно быть _коротким_ (S), _интуитивно понятным_ (I) и _ёмким_ (D)

- **Коротким**. Набор имени не должен занимать много времени и, следовательно, запоминаться;
- **Интуитивно понятным**. Имя должно читаться естественно, как можно ближе к обычной речи;
- **Ёмким**. Имя должно отражать то, что оно делает/обладает наиболее эффективным способом.

```js
/* Плохо */
const a = 5 // "a" может означать всё что угодно
const isPaginatable = a > 10 // "Paginatable" звучит абсолютно "неестественно"
const shouldPaginatize = a > 10 // Придуманные глаголы - это так весело!

/* Хорошо */
const postCount = 5
const hasPagination = postCount > 10
const shouldPaginate = postCount > 10 // альтернативно
```

## Избегайте сокращений

**Не** используйте сокращения. Они не приводят ни к чему, кроме снижения удобочитаемости кода. Найти короткое, ёмкое имя может быть тяжело, но сокращение не является оправданием для того, чтобы не делать этого.

```js
/* Плохо */
const onItmClk = () => {}

/* Хорошо */
const onItemClick = () => {}
```

## Избегайте дублирования контекста

Имя не должно дублировать контекст, в котором оно определено. Всегда удаляйте контекст из имени, если это не ухудшает его удобочитаемость.

```js
class MenuItem {
  /* Имя метода дублирует контекст (здесь он определён как "MenuItem") */
  handleMenuItemClick = (event) => { ... }

  /* Читается лучше `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## Отражайте ожидаемый результат

Имя должно отражать ожидаемый результат.

```jsx
/* Плохо */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* Хорошо */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

---

# Именование функций

## A/HC/LC Паттерн

Существует полезный шаблон, которому следует следовать при присвоении имен функциям:

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

Взгляните на то, как этот шаблон может быть применен в таблице ниже.

| Name                   | Prefix   | Action (A) | High context (HC) | Low context (LC) |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getUser`              |          | `get`      | `User`            |                  |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **Примечание:** Порядок контекста влияет на значение переменной. Например, `shouldUpdateComponent` означает, что _вы_ собираетесь обновить компонент, в то время как `shouldComponentUpdate` сообщает вам, что  _component_ обновиться сам по себе, и вы всего лишь контролируете, когда он должен быть обновлен.
> Другими словами, **контекст выше (HC) подчеркивает значение переменной**.

---

## Действия

Глагольная часть имени вашей функции. Самая важная часть, ответственная за описание того, что _делает_ функция.

### `get`

Немедленно получает доступ к данным (т.е. сокращенный способ получения внутренних данных).

```js
function getFruitCount() {
  return this.fruits.length
}
```

> См. также [compose](#compose).

Вы также можете использовать `get` при выполнении асинхронных операций:

```js
async function getUser(id) {
  const user = await fetch(`/api/user/${id}`)
  return user
}
```

### `set`

Устанавливает переменную декларативным способом со значением `A` на значение `B`.

```js
let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

### `reset`

Возвращает переменную к ее исходному значению или состоянию.

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

### `remove`

Удаляет что-то _от_ куда-то.

Например, если у вас есть коллекция выбранных фильтров на странице поиска, удаление одного из них из коллекции - это `removeFilter`, а **не** `deleteFilter` (и это то, как вы, естественно, сказали бы это и на английском языке):

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

> См. также [delete](#delete).

### `delete`

Полностью стирает что-то из областей присутствия.

Представьте, что вы редактор контента, и есть тот пресловутый пост, от которого вы хотите избавиться. Как только вы нажали блестящую кнопку "Удалить сообщение", CMS выполнила действие `deletePost`, а **не** `removePost`.

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

> См. также [remove](#remove).

> **`remove` или `delete`?**
>
> Если разница между `remove` и `delete` для вас не так очевидна, я бы посоветовал посмотреть на их противоположные действия - `add` and `create`.
> Ключевое различие между `add` и `create` заключается в том, что `add` требует назначения, в то время как `create` **не требует назначения**. Вы `add` элемент _куда-то_, но вы не "`create` его _ куда-то_".
> Просто соедините `remove` с `add` и `delete` с `create`.
>
> Подробно объяснено [here](https://github.com/kettanaito/naming-cheatsheet/issues/74#issue-1174942962).

### `compose`

Creates new data from the existing one. Mostly applicable to strings, objects, or functions.

```js
function composePageUrl(pageName, pageId) {
  return pageName.toLowerCase() + '-' + pageId
}
```

> См. также [get](#get).

### `handle`

Обрабатывает действие. Часто используется при присвоении имени колбэка.

```js
function handleLinkClick() {
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

---

## Контекст

Домен, с которым работает функция.

Функция часто является действием на _что-то_. Важно указать, каков рабочий домен или, по крайней мере, ожидаемый тип данных.

```js
/* Чистая функция, работающая с примитивами */
function filter(list, predicate) {
  return list.filter(predicate)
}

/* Функция, работающая точно c блогпостами */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

> Некоторые языковые допущения могут позволить опустить контекст. Например, в JavaScript обычно `фильтр` работает с массивом. Добавление явного `массива фильтров` было бы излишним.

---

## Префиксы

Префикс усиливает значение переменной. Он редко используется в именах функций.

### `is`

Описывает характеристику или состояние текущего контекста (обычно `boolean`).

```js
const color = 'blue'
const isBlue = color === 'blue' // характеристика
const isPresent = true // состояние

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

### `has`

Описывает, обладает ли текущий контекст определенным значением или состоянием (обычно `boolean`).

```js
/* Плохо */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Хорошо */
const hasProducts = productsCount > 0
```

### `should`

Отражает положительное условное выражение (обычно `boolean`) в сочетании с определенным действием.

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

### `min`/`max`

Представляет минимальное или максимальное значение. Используется при описании границ или пределов.

```js
/**
 * Отображает случайное количество сообщений в пределах
 * заданных минимальных /максимальных границ.
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

### `prev`/`next`

Указывает предыдущее или следующее состояние переменной в текущем контексте. Используется при описании переходов состояний.

```jsx
async function getPosts() {
  const prevPosts = this.state.posts

  const latestPosts = await fetch('...')
  const nextPosts = concat(prevPosts, latestPosts)

  this.setState({ posts: nextPosts })
}
```

## Единственное и множественное число

Как и префикс, имена переменных могут быть единственными или множественными в зависимости от того, содержат ли они одно значение или несколько значений.

```js
/* Плохо */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Хорошо */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```
