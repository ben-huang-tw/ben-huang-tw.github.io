# 基礎類型

## 介紹

為了讓程序有價值，我們需要能夠處理最簡單的資料單元：數字，字串，結構體，布爾值等。TypeScript 支援與 JavaScript 幾乎相同的資料類型，此外還提供了實用的列舉類型方便我們使用。

## Boolean

最基本的資料類型就是簡單的 `true` / `false` 值，在 JavaScript 和 TypeScript 裏叫做 `boolean`（其它語言中也一樣）。

```typescript
let isDone: boolean = false;
```

## Number

和 JavaScript 一樣，TypeScript 裏的所有數字都是浮點數或者大整數 。 這些浮點數的類型是 `number`， 而大整數的類型則是 `bigint`。 除了支援十進制和十六進制字面量，TypeScript 還支援 ECMAScript 2015 中引入的二進制和八進制字面量。

```typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
let bigLiteral: bigint = 100n;
```

## String

JavaScript 程序的另一項基本操作是處理網頁或服務器端的文字資料。 像其它語言裏一樣，我們使用 `string` 表示文字資料類型。和 JavaScript 一樣，可以使用雙引號（`"`）或單引號（`'`）表示字串。

```typescript
let name: string = "bob";
name = "smith";
```

你還可以使用 _模版字串_ ，它可以定義多行文字和內嵌表達式。 這種字串是被反引號包圍（\` \`），並且以`${ expr }`這種形式嵌入表達式

```typescript
let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.

I'll be ${ age + 1 } years old next month.`;
```

這與下面定義 `sentence` 的方式效果相同：

```typescript
let sentence: string = "Hello, my name is " + name + ".\n\n" +
    "I'll be " + (age + 1) + " years old next month.";
```

## Array

TypeScript 像 JavaScript 一樣可以操作陣列元素。 有兩種方式可以定義陣列。 第一種，可以在元素類型後面接上 `[]`，表示由此類型元素組成的一個陣列：

```typescript
let list: number[] = [1, 2, 3];
```

第二種方式是使用陣列泛型，`Array<元素類型>`：

```typescript
let list: Array<number> = [1, 2, 3];
```

## Tuple

元組類型允許表示一個已知元素數量和類型的陣列，各元素的類型不必相同。比如，你可以定義一對值分別為 `string` 和 `number` 類型的元組。

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

當訪問一個已知索引的元素，會得到正確的類型：

```typescript
console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'
```

當訪問一個越界的元素會報錯。

```typescript
x[3] = "world"; // Error, Property '3' does not exist on type '[string, number]'.

console.log(x[5].toString()); // Error, Property '5' does not exist on type '[string, number]'.
```

## Enum

`enum` 類型是對 JavaScript 標準資料類型的一個補充。 像 C# 等其它語言一樣，使用列舉類型可以為一組數值賦予友好的名字。

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

預設情況下，從 `0` 開始為元素編號。 你也可以手動的指定成員的數值。 例如，我們將上面的例子改成從 `1` 開始編號：

```typescript
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
```

或者，全部都採用手動賦值：

```typescript
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
```

列舉類型提供的一個便利是你可以由列舉的值得到它的名字。 例如，我們知道數值為 `2`，但是不確定它映射到 `Color` 裏的哪個名字，我們可以尋找相應的名字：

```typescript
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName);  // 顯示'Green'因為上面代碼裏它的值是2
```

## Unknown

當我們在寫應用的時候可能會需要描述一個我們還不知道其類型的變數。這些值可以來自動態內容，例如從用户獲得，或者我們想在我們的 API 中接收所有可能類型的值。在這些情況下，我們想要讓編譯器以及未來的用户知道這個變數可以是任意類型。這個時候我們會對它使用 `unknown` 類型。

```typescript
let notSure: unknown = 4;
notSure = "maybe a string instead";

// OK, definitely a boolean
notSure = false;
```

如果你有一個 `unknwon` 類型的變數，你可以透過進行 `typeof` 、比較或者更高級的類型檢查來將其的類型範圍縮小，這些方法會在後續章節中進一步討論：

```typescript
// @errors: 2322 2322 2322
declare const maybe: unknown;
// 'maybe' could be a string, object, boolean, undefined, or other types
const aNumber: number = maybe;

if (maybe === true) {
  // TypeScript knows that maybe is a boolean now
  const aBoolean: boolean = maybe;
  // So, it cannot be a string
  const aString: string = maybe;
}

if (typeof maybe === "string") {
  // TypeScript knows that maybe is a string
  const aString: string = maybe;
  // So, it cannot be a boolean
  const aBoolean: boolean = maybe;
}
```

## Any

有時候，我們會想要為那些在編程階段還不清楚類型的變數指定一個類型。 這些值可能來自於動態的內容，比如來自用户輸入或第三方代碼庫。 這種情況下，我們不希望類型檢查器對這些值進行檢查而是直接讓它們透過編譯階段的檢查。 那麼我們可以使用`any`類型來標記這些變數：

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

在對現有代碼進行改寫的時候，`any`類型是十分有用的，它允許你在編譯時可選擇地包含或移除類型檢查。 你可能認為`Object`有相似的作用，就像它在其它語言中那樣。 但是`Object`類型的變數只是允許你給它賦任意值 - 但是卻不能夠在它上面呼叫任意的方法，即便它真的有這些方法：

```typescript
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

> 注意：應避免使用`Object`，而是使用非原始`object`類型，正如[Do's and Don'ts](http://www.patrickzhong.com/TypeScript/zh/declaration-files/do-s-and-don-ts.html)裏所講的那樣。

當你只知道一部分資料的類型時，`any`類型也是有用的。 比如，你有一個陣列，它包含了不同的類型的資料：

```typescript
let list: any[] = [1, true, "free"];

list[1] = 100;
```

## Void

某種程度上來説，`void`類型像是與`any`類型相反，它表示沒有任何類型。 當一個函數沒有返回值時，你通常會見到其返回值類型是`void`：

```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
```

宣告一個`void`類型的變數沒有什麼大用，因為你只能為它賦予`null`（只在`--strictNullChecks`未指定時）和`undefined`：

```typescript
let unusable: void = undefined;
```

## Null 和 Undefined

TypeScript 裏，`undefined`和`null`兩者各自有自己的類型分別叫做`undefined`和`null`。 和`void`相似，它們的本身的類型用處不是很大：

```typescript
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

預設情況下`null`和`undefined`是所有類型的子類型。 就是説你可以把`null`和`undefined`賦值給`number`類型的變數。

然而，當你指定了`--strictNullChecks`標記，`null`和`undefined`只能賦值給`any`和它們各自的類型（有一個例外是`undefined`還可以賦值給`void`類型）。 這能避免 _很多_ 常見的問題。 也許在某處你想傳入一個`string`或`null`或`undefined`，你可以使用聯合類型`string | null | undefined`。

聯合類型是高級主題，我們會在以後的章節裏討論它。

> 注意：我們鼓勵儘可能地使用`--strictNullChecks`，但在本手冊裏我們假設這個標記是關閉的。

## Never

`never`類型表示的是那些永不存在的值的類型。 例如，`never`類型是那些總是會拋出異常或根本就不會有返回值的函數表達式或箭頭函數表達式的返回值類型； 變數也可能是`never`類型，當它們被永不為真的類型保護所約束時。

`never`類型是任何類型的子類型，也可以賦值給任何類型；然而，_沒有_ 類型是`never`的子類型或可以賦值給`never`類型（除了`never`本身之外）。 即使`any`也不可以賦值給`never`。

下面是一些返回`never`類型的函數：

```typescript
// 返回never的函數必須存在無法達到的終點
function error(message: string): never {
    throw new Error(message);
}

// 推斷的返回值類型為never
function fail() {
    return error("Something failed");
}

// 返回never的函數必須存在無法達到的終點
function infiniteLoop(): never {
    while (true) {
    }
}
```

## Object

`object`表示非原始類型，也就是除`number`，`string`，`boolean`，`bigint`，`symbol`，`null`或`undefined`之外的類型。

使用`object`類型，就可以更好的表示像`Object.create`這樣的API。例如：

```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

## 類型斷言

有時候你會遇到這樣的情況，你會比 TypeScript 更瞭解某個值的詳多資訊。 通常這會發生在你清楚地知道一個實體具有比它現有類型更確切的類型。

透過 _類型斷言_ 這種方式可以告訴編譯器，“相信我，我知道自己在幹什麼”。 類型斷言好比其它語言裏的類型轉換，但是不進行特殊的資料檢查和解構。 它沒有運行時的影響，只是在編譯階段起作用。 TypeScript 會假設你，程序員，已經進行了必須的檢查。

類型斷言有兩種形式。 其一是“尖括號”語法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一個為`as`語法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

兩種形式是等價的。 至於使用哪個大多數情況下是憑個人喜好；然而，當你在 TypeScript 裏使用JSX時，只有`as`語法斷言是被允許的。

## 關於`let`

你可能已經注意到了，我們使用`let`關鍵字來代替大家所熟悉的 JavaScript 關鍵字`var`。 `let`是ES2015引入的關鍵字，它比`var`更加安全，因此被看做是宣告變數的標準方式。 我們會在以後詳細介紹它，很多常見的問題都可以透過使用`let`來解決，所以儘可能地使用`let`來代替`var`吧。

## 關於 Number, String, Boolean, Symbol 和 Object

我們很容易會認為 `Number`、 `String`、 `Boolean`、`Symbol` 以及 `Object` 這些類型和我們以上推薦的小寫版本的類型是一樣的。但這些類型不屬於語言的基本類型，並且幾乎在任何時候都不應該被用作一個類型：

```typescript
// @errors: 2339
function reverse(s: String): String {
  return s.split("").reverse().join("");
}

reverse("hello world");
```

相對地，我們應該使用 `number`、`string`、`boolean`、`object` 和 `symbol`

```typescript
function reverse(s: string): string {
  return s.split("").reverse().join("");
}

reverse("hello world");
```

