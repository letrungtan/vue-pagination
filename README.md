# vue-pagination

A simple pagination component for VueJS.

<img src="https://raw.githubusercontent.com/letrungtan/vue-pagination/main/img/pagination.png" width="400" style="height:auto;" />

## Installation
### NPM
```js
$ npm install @letrungtan/vue-pagination
```
Use the component.
```js
import Pagination from '@letrungtan/vue-pagination'
Vue.component('pagination', Pagination)
```
## Usage

**Basic Usage**
```html
<pagination
  :currentPage="1"
  :perPage="20"
  :totalItems="123"  
  @pageSelected="callbackFunction"
>
</pagination>
```
**Example**
```html
<template>
  <pagination
    :currentPage="1"
    :perPage="20"
    :totalItems="123"  
    @pageSelected="changePage"
  </pagination>
</template>

<script>
export default {
  methods: {
    changePage (page) => {
      console.log(page);
    }
  }
}
</script>
```

## API

| Name | Type | Description |
| :---------------- | :-------- | :-------------------------------- |
| `currentPage`     | `Number`  | Current page number. **required**  |
| `perPage`         | `Number`  | Number of item limit per page. **required**  |
| `totalItems  `    | `Number`  | Number of all item available. **required**  |
| `@pageSelected`   | `Function`| The callback function when page selected. Return page number in parameter.  |
| `prevText`        | `String`  | Text for the previous button. **default: Prev**  |
| `nextText`        | `String`  | Text for the next button. **default: Next**  |
| `maxSize`         | `Number`  | Max length of pagination. **default: 5** |
