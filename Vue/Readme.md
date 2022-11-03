# Vue V2

## 安裝

### Vue Devtools

在使用 Vue 時，我們推薦在你的瀏覽器上安裝 [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools)。它允許你在一個更友好的介面中審查和偵錯 Vue 應用。

### 直接用 `<script>` 引入

直接下載並用 `<script>` 標籤引入，Vue 會被註冊為一個全域變數。

> 在開發環境下不要使用壓縮版本，不然你就失去了所有常見錯誤相關的警告!

* 開發版本： 包含完整的警告和偵錯模式
* 生產版本： 刪除了警告，37.36KB min+gzip

對於製作原型或學習，你可以這樣使用最新版本：

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.10/dist/vue.js"></script>

```

對於生產環境，我們推薦連結到一個明確的版本號和建構檔案，以避免新版本造成的不可預期的破壞：

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.10"></script>
```

## 介紹

### Hello World 例子

嘗試 Vue.js 最簡單的方法是使用 [Hello World 例子](https://codesandbox.io/s/github/vuejs/v2.vuejs.org/tree/master/src/v2/examples/vue-20-hello-world)。

```html
<!DOCTYPE html>
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <div id="app">
    {{ message }}
  </div>

  <script>
    var app = new Vue({
      el: '#app',
      data: {
        message: 'Hello Vue!'
      }
    })
  </script>
</body>
</html>
```

### 文字插值

```html
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

### 繫結元素 attribute

```html
<div id="app-2">
  <span v-bind:title="message">
    滑鼠懸停幾秒鐘查看此處動態繫結的提示資訊！
  </span>
</div>
```

```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '頁面載入於 ' + new Date().toLocaleString()
  }
})
```

### v-if

控制切換一個元素是否顯示也相當簡單：

```html
<div id="app-3">
  <p v-if="seen">現在你看到我了</p>
</div>
```

```js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

### v-for

`v-for` 指令可以繫結陣列的資料來渲染一個項目列表：

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '學習 JavaScript' },
      { text: '學習 Vue' },
      { text: '整個牛項目' }
    ]
  }
})
```

### v-on

為了讓使用者和你的應用進行互動，我們可以用 `v-on` 指令新增一個事件監聽器，通過它呼叫在 Vue 實例中定義的方法：

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">反轉消息</button>
</div>
```

```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

### v-model

Vue 還提供了 `v-model` 指令，它能輕鬆實現表單輸入和應用狀態之間的雙向繫結。

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```

### 元件

在 Vue 裡，一個元件本質上是一個擁有預定義選項的一個 Vue 實例。在 Vue 中註冊元件很簡單：

```js
// 定義名為 todo-item 的新元件
Vue.component('todo-item', {
  template: '<li>這是個待辦項</li>'
});

var app = new Vue(...)
```

現在你可以用它建構另一個元件範本：

```html
<ol>
  <!-- 建立一個 todo-item 元件的實例 -->
  <todo-item></todo-item>
</ol>
```

讓我們來修改一下元件的定義，使之能夠接受一個 prop：

```js
Vue.component('todo-item', {
  // todo-item 元件現在接受一個
  // "prop"，類似於一個自訂 attribute。
  // 這個 prop 名為 todo。
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
});

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: '蔬菜' },
      { id: 1, text: '奶酪' },
      { id: 2, text: '隨便其它什麼人吃的東西' }
    ]
  }
});
```

現在，我們可以使用 v-bind 指令將待辦項傳到循環輸出的每個元件中：

```html
<div id="app-7">
  <ol>
    <!--
      現在我們為每個 todo-item 提供 todo 對象
      todo 對像是變數，即其內容可以是動態的。
      我們也需要為每個元件提供一個“key”，稍後再
      作詳細解釋。
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>


