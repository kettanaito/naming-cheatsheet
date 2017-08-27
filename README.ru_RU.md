<p align="center">
 <a href="https://github.com/kettanaito/naming-cheatsheet">
  <img src="./naming-cheatsheet.png" alt="Naming cheatsheet" />
 </a>
</p>

# Шпаргалка наименования
Давать имена вещам сложно. Давайте сделаем этот процес легче.

## Рекомендации
* Выберите **одну** конвенцию наименований и придерживайтесь ее. Будь-то `likeThis`, или `like_this`, или как-либо еще, это не столь важно. Что важно, так это консистентность в вашей работе.
```js
/* Плохо */
const pages_count = 5;
const shouldUpdate = true;

/* Хорошо */
const pagesCount = 5;
const shouldUpdate = true;

/* И это хорошо */
const pages_count = 5;
const should_update = true;
```
* Имя, будь-то переменная, метод, или что-то еще, должно быть *кратким*, *наглядным* и *интуитивно понятным*:
  * **Краткое**. Имя переменной должно быстро печататься, и, следовательно, запоминаться,
  * **Наглядное**. Имя переменной должно отражать суть данной переменной в наиболее эффективной и рациональной форме,
  * **Интуитивно понятное**. Имя переменной должно читаться естественно, как можно ближе к повседневной речи
```js
/* Плохо */
const a = 5; // "a" может означать что угодно
const isPaginatable = (postsCount > 10); // "Paginatable" читается ужасно неестественно
const shouldPaginatize = (postsCount > 10); // Выдуманые глаголы это так весело!

/* Хорошо */
const postsCount = 5;
const shouldDisplayPagination = (postsCount > 10);
```

* Имя не должно дублировать контекст переменной, в котором она была оглашена, а также если убрав контекст из имени переменной ее читабельность не ухудшается:
```js
class MenuItem {
  /* Название метода дублирует контекст в котором он объявлен - "...MenuItem..." */
  handleMenuItemClick = (event) => { ... }
  
  /* А таким образом метод читается как MenuItem.handleClick() */
  handleClick = (event) => { ... }
}
```
* Имя должно отражать *ожидаемый* результат:
```js
/* Плохо */
const isEnabled = (itemsCount > 3);
return (<Button disabled={!isEnabled} />);

/* Хорошо */
const isDisabled = (itemsCount <= 3);
return (<Button disabled={isDisabled} />);
```

## Схема
```
префикс? + действие (A) + высший контекст (HC) + низший контекст (LC)
```
Это вовсе не правило, а скорее закономерность, которую достаточно часто можно применить при наименовании переменных.

> Имейте в виду, что порядок контекстов влияет на коренное значение переменной. Например, `shouldUpdateComponent` скорее означает, что *Вы* собираетесь обновить компонент, в то время как `shouldComponentUpdate` говорит о том, что *компонент* обновится сам, а Вы лишь контролируете следует ли ему это сделать.
Другими словами, высший контекст акцентирует значение переменной.

### Пример
| Имя | Префикс | Действие | Выс. контекст | Низ. контекст |
| ---- | ---- | ------ | ------------ | ----------- |
| `getPost` | | `get` | `Post` |  |
| `getPostData` | | `get` | `Post` | `Data` |
| `handleClickOutside` | | `handle` | `Click` | `Outside` |
| `shouldDisplayMessage` | `should` | `Display` | `Message`| |

## Наименование методов

### Действие
Выбор правильного названия действия может придать дополнительной информативности Вашим методам. С выбора действия обычно хорошо начинать когда даете имена Вашим методам.

#### `get`
Немедленно получить доступ к данным (напр. упрощенная ссылка на внутренние данные).
```js
function getFruitsCount() {
  return this.fruits.length;
}
```
#### `fetch`
Запрос данных, который требует времени (напр. асинхронные операции).
```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', { ... });
}
```

#### `set`
Декларативно присвоить переменной `variableA` со значением `valueA` значение `valueB`.
```js
const fruits = 0;

function setFruits(nextFruits) {
  fruits = nextFruits;
}

setFruits(5); // fruits === 5
```

#### `reset`
Вернуть чему-то его первоначальное значение.
```js
const initialFruits = 5;
const fruits = initialFruits;
setFruits(10); // fruits === 10

function resetFruits() {
  fruits = initialFruits;
}

resetFruits(); // fruits === 5
```

#### `remove`
Убрать что-то *из* чего-то. Например, если у Вас есть список выбранных фильтров на странице поиска, убирание одного из них это `removeFilter`, а не `deleteFilter` (в английском языке естественно произносится именно первый вариант):
```js
const selectedFilters = ['price', 'availability', 'size'];

function removeFilter(filterName) {
  const filterIndex = selectedFilters.indexOf(filterName);
  if (filterIndex !== -1) selectedFilters.splice(filterIndex, 1);

  return selectedFilters;
}
```

#### `delete`
Полностью стереть что-то из существования. Представьте, что Вы редактор блога, и Вы решаете удалить одну из Ваших публикаций из CMS. Как только Вы нажмете на сияющую клавишу "Delete", Вы произведете операцию удаления (`deletePost`, а не `removePost`).

#### `compose`
Создание (композиция) новых данных из уже имеющихся. Скорее всего, применимо в большей степени к строкам.
```js
function composePageUrl(pageName, pageId) {
  return `${pageName.toLowerCase()}-${pageId}`;
}
```

#### `handle`
Обработчик определенного действия. Обычно, используется как callback метод.
```js
function handleLinkClick(event) {
  event.preventDefault();
  console.log('Clicked a link!');
}

link.addEventListener('click', handleLinkClick);
```

## Префиксы
Префиксы совершенствуют переменные или методы, придавая им дополнительного значения.

#### `is`
Описывает присущность определенной характеристики или состояния данного конекста.
```js
const color = 'blue';
const isBlue = (color === 'blue'); // характеристика
const isRemoved = false; // состояние

if (isBlue && !isRemoved) {
  console.log('Цвет - синий, и он существует!');
}
```

#### `min`/`max`
Обозначает минимальное или максимальное значение. Удобно при описании границ или допустиых лимитов.
```js
function PostsList() {
  this.minPosts = 3;
  this.maxPosts = 10;
}
```

#### `has`
Описывает принадлежность определенного значение или состояния текущему контексту.
```js
/* Плохо */
const isProductsExist = (productsCount > 0);
const areProductsPresent = (productsCount > 0);

/* Хорошо */
const hasProducts = (productsCount > 0);
```

#### `should`
Отражает условное утверждение (возвращает значение `Boolean`).
```js
const currentUrl = 'https://dev.com';

function shouldUpdateUrl(url) {
  return (url !== currentUrl);
}
```
