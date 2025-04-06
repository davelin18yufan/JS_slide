---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Modern Javascript
info: |
  ## JS 語法進化史及改變
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
fonts:
  # basically the text
  sans: Robot
  # use with `font-serif` css class from UnoCSS
  serif: Robot Slab
  # for code blocks, inline code, etc.
  mono: Fira Code
# open graph
# seoMeta:
#  ogImage: https://cover.sli.dev
---

# 🚀 Modern JavaScript

JS 的演變以及核心理念

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
layout: center
---

# 🧟‍♂️ 舊 JS 還能跑，那為什麼要改？

- 📝 **`var` 沒有限定範圍，全域污染常發生**
- 🎨 **callback 套 callback，讀不懂也改不動**
- 🧑‍💻 **函數放一堆，根本找不到是哪裡定義**
- 🤹 **明明是非同步卻不曉得哪時會執行**
- ...

<v-click>
<h3 class="text=center w-full mt-4">
👉 現代寫法不是「潮」，必須了解演進是為了解決某些問題
</h3>
</v-click>

<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<!--
Here is another comment.
-->

---
transition: slide-up
---

# 🧯 用 const 滅火，別讓 var 搞事

- `var` 有 hoisting 且 <span v-mark.red="0">function scope</span>，也可以重複宣告，容易誤用

- `let`、`const` 為 <span v-mark.red="0">block scope</span>，更合理

- `const` 限制 reassignment(重新賦值)，提升可預測性

````md magic-move {lines: true}
```javascript {*}
var count = 1
if (true) {
  var count = 2
  console.log(count) // 2
}
console.log(count) // ?
```

```javascript {*}
var count = 1
if (true) {
  var count = 2
  console.log(count) // 2
}
console.log(count) // 2 ❌
```

```javascript {*}
const count = 1
if (true) {
  const count = 2 // ❌ Uncaught SyntaxError: Identifier 'count' has already been declared
}
```
````

<div v-click="4"
  v-motion  
  :initial="{ scale: 0, opacity: 0.5 }" 
  :enter="{ scale: 1, opacity: 1, transition: {
      delay: 200, duration: 200
    }}"
>

```javascript
var memNo = sessionStorage.getItem("memno")
var Group_Course = {
  BindData: function () {
    var memNo = $("#student-container").value()
    // 使用的 ID 造成混淆及錯誤
  },
  // ...
}
```

</div>

---
transition: fade
---

# 🧠 JS 腦袋怎麼記住變數？

<div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-left my-6" 
v-motion
:initial="{ y: 40, opacity: 0 }"
:enter="{ y: 0, opacity: 1, transition: {delay: 400} }">
  <!-- Scope -->
  <div class="bg-white/10 p-4 rounded-xl shadow-md border border-white/20">
    <h3 class="text-xl font-bold text-yellow-300 mb-2">🔍 Scope（作用域）</h3>
    <p class="text-sm leading-relaxed">
      Block、Function、Global<br />
      決定變數在哪裡可以被存取。
    </p>
  </div>

  <!-- Hoisting -->
  <div class="bg-white/10 p-4 rounded-xl shadow-md border border-white/20">
    <h3 class="text-xl font-bold text-pink-400 mb-2">📤 Hoisting（提升）</h3>
    <p class="text-sm leading-relaxed">
      <code>var</code> 和函式宣告會提升。<br />
      容易造成預期外的行為。
    </p>
  </div>

  <!-- Closure -->
  <div class="bg-white/10 p-4 rounded-xl shadow-md border border-white/20">
    <h3 class="text-xl font-bold text-sky-400 mb-2">🧠 Closure（閉包）</h3>
    <p class="text-sm leading-relaxed">
      函式「記住」定義時的變數環境。<br />
      是 JS 很強大也常見的概念。
    </p>
  </div>
</div>

<v-click>

````md magic-move {lines:true}
```ts {*}
// 用 var 宣告在 block 裡
var count = 1
if (true) {
  var count = 2 // 但是因為 var 是函式作用域會被提升到最上面
  console.log(count) // 2
}
console.log(count)
```

```ts {*}
// 實際結果：變數提升
var count = 1
if (true) {
  var count = 2 // 🆙
  console.log(count)
}
console.log(count)
```

```ts {*}
// 用 const/let 避免覆蓋與污染
const count = 1
if (true) {
  const count = 2 // ❌ Uncaught SyntaxError: Identifier 'count' has already been declared
}
```
````

</v-click>

---
transition: fade-in
---

# Brain Storm Session

<div
  class=""
  v-click 
  v-motion
  :initial="{ y: 40, opacity: 0 }"
  :enter="{ y: 0, opacity: 1 }"
  :leave="{ y: -200, opacity: 0, transition: { duration: 300 } }"
>

  <!-- Closure 與 Counter 的範例 -->

```ts {monaco-run}
function makeCounter() {
  let count = 0
  return () => ++count
}

const counter = makeCounter()
// console.log(counter());
// console.log(counter());
```

```ts {monaco-run}{at:'+1'}
function makeGreeter(name: string) {
  const greeting = "Hello"
  return function () {
    console.log(`${greeting}, ${name}!`)
  }
}

const greetAmy = makeGreeter("Amy")
// greetAmy();
```

  <p v-click="2">
    ✅ 函式在外層已經執行完畢，但變數還「活著」！
  </p>
</div>

---

# Brain Storm Session

🧨 Hoisting 陷阱：var, let, const 宣告前使用

```ts {2,3|1|*}{at:'1'}
// var ➜ undefined（被提升但未賦值）
console.log(msg)
var msg = "hello"
```

```ts {2,3|1|*}{at:'1'}
// let ➜ ❌ ReferenceError（TDZ 區塊死區）
console.log(count)
let count = 10
```

```ts {2,3|1|*}{at:'1'}
// const ➜ ❌ ReferenceError（同樣有 TDZ）
console.log(apiKey)
const apiKey = "123456"
```

<div 
  class="absolute top-24 bg-slate-800 left-0 p-3"
  v-click="3" 
  v-motion
  :initial="{ y: 40, opacity: 0 }"
  :enter="{ y: 0, opacity: 1 }"
  :leave="{ y: -200, opacity: 0, transition: { duration: 300 } }"
>
  <table class="table-auto w-full text-white">
    <thead>
      <tr>
        <th class="px-4 py-2 text-green-400">項目</th>
        <th class="px-4 py-2">描述</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="px-4 py-2 text-green-400">作用域</td>
        <td class="px-4 py-2">
          <code>var</code> <span v-mark.circle.orange="4">可以是全域、也可以是以函式作為範圍</span>； 
          <code>let</code> 與 <code>const</code> 則是以 <span v-mark.circle.orange="4">區塊</span> 作為範圍。
        </td>
        </tr>
        <tr>
        <td class="px-4 py-2 text-green-400">宣告重複</td>
        <td class="px-4 py-2">
          <code>var</code> <span v-mark.circle.orange="5">可以被重複宣告</span>， 
          但是 <code>let</code> 與 <code>const</code> 則 <span v-mark.circle.orange="5">不行</span>。
        </td>
        </tr>
        <tr>
        <td class="px-4 py-2 text-green-400">提升（Hoisting）</td>
        <td class="px-4 py-2">
        <pre class="p-0 m-0">
<code>var</code> 宣告的變數會自動初始化為 <code>undefined</code>，因此在宣告前就使用變數，<span v-mark.circle.orange="6">不會出現錯誤</span>， 
而是 <code>undefined</code>；但是 <code>let</code> 與 <code>const</code> 則不會自動初始化，進入暫時死區 (TDZ)， 
因此在宣告前使用<span v-mark.circle.orange="6">會出現錯誤</span>。
        </pre>
        </td>
        </tr>
        <tr>
        <td class="px-4 py-2 text-green-400">重新賦值</td>
        <td class="px-4 py-2">
          <code>let</code> 與 <code>const</code> 在絕多數面向都相似，兩者的一大區別在於， 
          <span v-mark.circle.orange="7">用 <code>let</code> 宣告的變數可以重新賦值</span>，但用 <code>const</code> 的不行。
        </td>
        </tr>
        </tbody>
  </table>
</div>

---

# Brain Storm Session

<h5 class="!text-amber-500 mb-2">經典基礎 JS 考題，結合 Closure、Scope、Hoisting、非同步</h5>

```ts {monaco-run}
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    // console.log(i);
  }, 1000)
}

// setTimeout 可視為是模擬送出 API 請求
// 解法有兩種
```

---
transition: slide-left
---

# 🚑 Template String (模板字串)

Template string 讓你能夠在字串中直接嵌入變數，並且使字串拼接變得更加簡單易讀。

````md magic-move {lines:true}
```ts
const name = "リン"
const age = 25
const greeting = `こにちは ${name} です。 ${age + 5} さいです。`
console.log(greeting) // こにちは リン　です 30 さいです。
```

```ts
const items = ["🍎", "🍌", "🍇", "🍓"]
const list = items
  .map((item, index) => `<li>Item ${index + 1}: ${item}</li>`)
  .join("")
const html = `<ul>${list}</ul>`
console.log(html)
// Output:
// <ul>
//   <li>Item 1: 🍎</li>
//   <li>Item 2: 🍌</li>
//   <li>Item 3: 🍇</li>
//   <li>Item 4: 🍓</li>
// </ul>
```
````

> 使用模板字串可以簡化變數嵌入過程，提升可讀性。

---
transition: fade
---

# 🏹 箭頭函式 vs 傳統函式

````md magic-move {lines:true}
```ts {*}
function addOld(a: number, b: number) {
  return a + b
}
console.log(addOld(2, 3)) // 5
```

```ts {*}
const add = (a:number, b:number) => a + b
console.log(add(2, 3)) // 5
```
````


```js {monaco-run}
const person = {
  name: "Darren",
  sayHi: function () {
      console.log("一般函式:", this.name);
  },
  sayHiArrow: () => {
      console.log("箭頭函式:", this.name);
  }
};

// person.sayHi();       
// person.sayHiArrow();  

```

<div v-click="2" v-motion
  :initial="{ y: 40, opacity: 0 }"
  :enter="{ y: 0, opacity: 1 }"
  :leave="{ y: -200, opacity: 0, transition: { duration: 300 } }">

<ul class="text-amber-500">
      <li>不會創建自己的 <code>this</code>，讓你更容易管理作用域</li>
      <li>語法簡潔，提升可讀性</li>
      <li>適合用於簡單邏輯或 callback 函式</li>
    </ul>

</div>

---
transition: slide-up
---

# 🧩 解構賦值 Destructuring

拆解陣列與物件更快速、更清楚

````md magic-move {lines:true}
```ts
// 陣列解構
const [first, second] = [10, 20];
console.log(first); // 10

// 物件解構
const user = { name: 'Ada', age: 25 };
const { name, age } = user;
console.log(name); // Ada
```

```ts
// 解構參數
function show({ title, count = 2 }: {title: string; count: number}) {
  console.log(title, count);
}

const data1 = {title: "我是標題"}
const data2 = {title: "我是標題", count: 5}
show(data1) // 我是標題, 2
show(data2) // 我是標題, 5
```

```ts
const { ItemList: courseList, ErrorMessage: getCoursesError } = await $.ajax({
  method: "GET",
  url: "../api/My_tima_api",
  data: JSON.stringify({ crsNo: "123" }),
  async: true
})

if(getCoursesError) {
  alert(getCoursesError)
}

console.log(courseList) // api response
```
````

--- 
transition: fade-out
layout: two-cols-header
class: gap-1
---

# 📦 Spread vs Rest：一體兩面

**`...` 可以用來當作兩種語法，依上下文扮演不同角色！**
<div class="grid grid-cols-2 gap-4 mt-4">

<Card class="bg-gray-800 text-left p-4 rounded-2xl shadow-lg text-sm leading-relaxed">

#### 🌊 Spread 展開
將陣列、物件中的元素「展開」成個別項目
  <div class="text-base font-bold mb-2 text-white">🧪 場景 1：複製與合併（Spread）</div>
  <p class="text-slate-200 mb-2">避免資料被意外改動（Immutable）</p>
</Card>

<Card class="bg-gray-800 text-left p-4 rounded-2xl shadow-lg text-sm leading-relaxed">

#### 🪣 Rest 收集
將多個參數「收集」成一個陣列 
  <div class="text-base font-bold mb-2 text-white">🎯 場景 2：參數收集（Rest）</div>
  <p class="text-slate-200 mb-2">接收不定數量參數或過濾欄位</p>
</Card>

</div>


::left::


````md magic-move {lines:true}
```ts
// 展開陣列裡面的元素放入
const arr = [1, 2];
const more = [...arr, 3];
console.log(more); // [1, 2, 3]
```
```ts
const arr1 = [1, 2]
const arr2 = [3, 4]

const combinedArr = [...arr1, ...arr2]
console.log(combinedArr) // [1, 2, 3, 4]
```
```ts
// 展開物件裡面的屬性放入
const obj = { a: 1 };
const clone = { ...obj, b: 2 };
console.log(clone); // { a: 1, b: 2 }

```

```ts
const original = [1, 2, 3];
const clone = [...original]; // 淺拷貝
clone.push(4);

console.log(original); // [1, 2, 3]
console.log(clone);    // [1, 2, 3, 4]
```
````

::right::


````md magic-move {lines:true}
```ts
function logAll(...args) {
  args.forEach(arg => console.log(arg));
}

logAll("hello", true, 42);
// hello
// true
// 42
```
```ts
// 解構蒐集剩下的元素，並且賦值給變數 rest
const { password, ...userInfo } = userData;
// 只留下非敏感資訊

```
````

---
transition: slide-left
---

# 🧪 == 是陷阱，=== 才是真愛

> JavaScript 的彈性是雙面刃，弱比對雖方便，但也埋下許多 **隱性錯誤**。


````md magic-move {lines:true}

```ts {1-4|1-4,5|1-4,6|1-4,7|1-4,8|1-4,9|*}
❌ 弱比對的隱藏陷阱

`==` 會 「自動轉型」, 直覺錯了，還不報錯！

0 == ''         // true ❌
false == []     // true ❌
null == undefined // true ❌
'5' == 5        // true ❌
NaN == NaN      // false ❌

```

```ts {1-4,6|1-4,7|1-4,8|*}
✅ 使用 `===` or `Object.is` 更安全

- `===` 比較「值」也比較「型別」
- `Object.is` 處理 NaN / -0 等特殊情況

0 === ''        // false ✅
false === []    // false ✅
Object.is(NaN, NaN)  // true ✅
```

````

<div class="px-2 rounded-2xl border border-slate-600 bg-emerald-900 shadow-md text-sm "
    v-click="11"
    v-motion
    :initial="{ y: 20, opacity: 0 }"
    :enter="{ y: 0, opacity: 1 }">

<h3>🧠 小總結</h3>

<ul>
  <li><code>==</code>：太聰明，容易出事</li>
  <li><code>===</code>：更嚴謹，更安全</li>
  <li><code>Object.is()</code>：處理一些極端邊界情況</li>
</ul>

<p>✅ 預設使用 <code>===</code>，除非你<span v-mark.red="11">非常清楚</span>你要的行為！</p>

</div>

---
layout: image
class: place-content-center text-center brightness-80 sepia-10
image: https://images.unsplash.com/photo-1589254065878-42c9da997008?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
---

# 🧠 JavaScript 非同步與 Promise 演進之路

> 從 `callback` 到 `async/await`，揭開 JS 非同步處理的進化史

<style>
h1{
  background-image: linear-gradient(
    45deg,
    rgb(241, 185, 89) 15%,
    rgb(243, 125, 15) 20%
  );
  background-blend-mode: lighten;
}
</style>

---
transition: fade
---

# 🚦 同步 vs 非同步
> Javascript 是個單線呈語言


### 同步：一步一腳印

```ts
console.log('A');
console.log('B');
console.log('C');
```
⏱️ 全部照順序執行，會卡住等待


### 非同步：不等你！
```ts
console.log('A');
setTimeout(() => console.log('B'), 1000);
console.log('C');
```
📍 執行順序：A → C → B



---
layout: center
class: text-center
---

# Learn More

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/resources/showcases)

<PoweredBySlidev mt-10 />
