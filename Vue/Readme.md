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

### Hello World 範例

嘗試 Vue.js 最簡單的方法是使用 [Hello World 範例](https://codesandbox.io/s/github/vuejs/v2.vuejs.org/tree/master/src/v2/examples/vue-20-hello-world)。

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
      現在我們為每個 todo-item 提供 todo 物件
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
```

## Vue 實例

### 建立一個 Vue 實例

每個 Vue 應用程式都是透過用 Vue 函數建立一個新的 Vue 實例開始的：

```js
var vm = new Vue({
  // 選項
});
```

### 資料與方法

當一個 Vue 實例被建立時，它將 data 物件中的所有的 property 加入到 Vue 的響應式系統中。當這些 property 的值發生改變時，檢視將會產生“響應”，即匹配更新為新的值。

```js
// 我們的資料物件
var data = { a: 1 }

// 該物件被加入到一個 Vue 實例中
var vm = new Vue({
  data: data
})

// 獲得這個實例上的 property
// 返回源資料中對應的欄位
vm.a == data.a // => true

// 設定 property 也會影響到原始資料
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3
```

當這些資料改變時，檢視會進行重渲染。值得注意的是只有當實例被建立時就已經存在於 data 中的 property 才是響應式的。

這裡唯一的例外是使用 `Object.freeze()`，這會阻止修改現有的 property(唯讀)，也意味著響應系統無法再追蹤變化。

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```

除了資料 property，Vue 實例還暴露了一些有用的實例 property 與方法。它們都有前綴 `$`，以便與使用者定義的 property 區分開來。例如：

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一個實例方法
vm.$watch('a', function (newValue, oldValue) {
  // 這個回呼將在 `vm.a` 改變後呼叫
})
```

### 實例生命週期鉤子

每個 Vue 實例在被建立時都要經過一系列的初始化過程。在這個過程中會運行一些叫做生命週期鉤子的函數，這給了使用者在不同階段新增自己的程式碼的機會。

比如 created 鉤子可以用來在一個實例被建立之後執行程式碼：

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 實例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

[生命週期圖示](https://v2.vuejs.org/v2/guide/instance.html#Lifecycle-Diagram)

## 範本語法

### 插值

#### 文字

資料繫結最常見的形式就是使用“Mustache”語法 (雙大括號) 的文字插值：

```html
<span>Message: {{ msg }}</span>
```

通過使用 [v-once](https://v2.cn.vuejs.org/v2/api/#v-once) 指令，你也能執行一次性地插值，當資料改變時，插值處的內容不會更新。

```html
<span v-once>這個將不會改變: {{ msg }}</span>
```

#### 原始 HTML

雙大括號會將資料解釋為普通文字，而非 HTML 程式碼。為了輸出真正的 HTML，你需要使用 `v-html` 指令：

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

```js
var app = new Vue({
el: '#app',
data: {
rawHtml: '<span style="color: red">This should be red</span>'
}
})
```

這個 span 的內容將會被替換成為 property 值 `rawHtml`，直接作為 HTML——會忽略解析 property 值中的資料繫結。

#### Attribute

Mustache 語法不能作用在 HTML attribute 上，遇到這種情況應該使用 `v-bind` 指令：

```html
<div v-bind:id="dynamicId"></div>
```

`v-bind` 工作起來略有不同，在這個範例中：

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果 `isButtonDisabled` 的值是 `null`、`undefined` 或 `false`，則 `disabled` attribute 甚至不會被包含在渲染出來的 `<button>` 元素中。

#### 使用 JavaScript 運算式

對於所有的資料繫結，Vue.js 都提供了完全的 JavaScript 運算式支援。

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

這些運算式會在所屬 Vue 實例的資料範疇下作為 JavaScript 被解析。有個限制就是，每個繫結都只能包含單一運算式，所以下面的範例都不會生效。

```html
<!-- 這是敘述，不是運算式 -->
{{ var a = 1 }}

<!-- 流程控制也不會生效，請使用三元運算式 -->
{{ if (ok) { return message } }}
```

範本運算式都被放在沙盒中，只能訪問[全域變數的一個白名單](https://github.com/vuejs/vue/blob/v2.6.10/src/core/instance/proxy.js#L9)，如 `Math` 和 `Date` 。你不應該在範本運算式中試圖訪問使用者定義的全域變數。

### 指令

指令 (Directives) 是帶有 `v-` 前綴的特殊 attribute。指令 attribute 的值預期是單一 JavaScript 運算式 (`v-for` 是例外情況)。指令的職責是，當運算式的值改變時，將其產生的連帶影響，響應式地作用於 DOM。回顧我們在介紹中看到的例子：

```html
<p v-if="seen">現在你看到我了</p>
```

這裡，`v-if` 指令將根據運算式 `seen` 的值的真假來插入/移除 `<p>` 元素。

#### 參數

一些指令能夠接收一個 “參數”，在指令名稱之後以冒號表示。例如，`v-bind` 指令可以用於響應式地更新 HTML attribute：

```html
<a v-bind:href="url">...</a>
```

在這裡 `href` 是參數，告知 `v-bind` 指令將該元素的 `href` attribute 與運算式 `url` 的值繫結。

另一個例子是 `v-on` 指令，它用於監聽 DOM 事件：

```html
<a v-on:click="doSomething">...</a>
```

在這裡參數是監聽的事件名稱。

#### 動態參數

從 2.6.0 開始，可以用方括號括起來的 JavaScript 運算式作為一個指令的參數：

```html
<!--
注意，參數運算式的寫法存在一些約束，如之後的“對動態參數運算式的約束”章節所述。
-->
<a v-bind:[attributeName]="url"> ... </a>
```

這裡的 `attributeName` 會被作為一個 JavaScript 運算式進行動態求值，求得的值將會作為最終的參數來使用。例如，如果你的 Vue 實例有一個 data property `attributeName`，其值為 `"href"`，那麼這個繫結將等價於 `v-bind:href`。

同樣地，你可以使用動態參數為一個動態的事件名繫結處理函數：

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

在這個範例中，當 `eventName` 的值為 `"focus"` 時，`v-on:[eventName]` 將等價於 `v-on:focus`。

#### 對動態參數的值的約束

動態參數預期會求出一個字串，異常情況下值為 null。這個特殊的 null 值可以被顯性地用於移除繫結。任何其它非字串類型的值都將會觸發一個警告。

#### 對動態參數運算式的約束

動態參數運算式有一些語法約束，因為某些字元，如空格和引號，放在 HTML attribute 名裡是無效的。例如：

```html
<!-- 這會觸發一個編譯警告 -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

變通的辦法是使用沒有空格或引號的運算式，或用計算屬性替代這種複雜運算式。

在 DOM 中使用範本時 (直接在一個 HTML 檔案裡撰寫範本)，還需要避免使用大寫字符來命名鍵名，因為瀏覽器會把 attribute 名全部強制轉為小寫：

```html
<!--
在 DOM 中使用範本時這段程式碼會被轉換為 `v-bind:[someattr]`。
除非在實例中有一個名為“someattr”的 property，否則程式碼不會工作。
-->
<a v-bind:[someAttr]="value"> ... </a>
```

### 修飾符

修飾符 (modifier) 是以半形句號 . 指明的特殊後綴，用於指出一個指令應該以特殊方式繫結。例如，`.prevent` 修飾符告訴 `v-on` 指令對於觸發的事件呼叫 `event.preventDefault()`：

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

在接下來對 `v-on` 和 `v-for` 等功能的探索中，你會看到修飾符的其它例子。

### 縮寫

`v-` 前綴作為一種視覺提示，用來識別範本中 Vue 特定的 attribute。當你在使用 Vue.js 為現有標籤新增動態行為 (dynamic behavior) 時，`v-` 前綴很有幫助，然而，對於一些頻繁用到的指令來說，就會感到使用繁瑣。同時，在建構由 Vue 管理所有範本的單頁面應用程式 (SPA - single page application) 時，`v-` 前綴也變得沒那麼重要了。因此，Vue 為 `v-bind` 和 `v-on` 這兩個最常用的指令，提供了特定簡寫：

#### v-bind 縮寫

```html
<!-- 完整語法 -->
<a v-bind:href="url">...</a>

<!-- 縮寫 -->
<a :href="url">...</a>

<!-- 動態參數的縮寫 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

#### v-on 縮寫

```html
<!-- 完整語法 -->
<a v-on:click="doSomething">...</a>

<!-- 縮寫 -->
<a @click="doSomething">...</a>

<!-- 動態參數的縮寫 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

它們看起來可能與普通的 HTML 略有不同，但 `:` 與 `@` 對於 attribute 名稱來說都是合法字元，在所有支援 Vue 的瀏覽器都能被正確地解析。而且，它們不會出現在最終渲染的標記中。縮寫語法是完全選擇性的，但隨著你更深入地瞭解它們的作用，你會慶幸擁有它們。
