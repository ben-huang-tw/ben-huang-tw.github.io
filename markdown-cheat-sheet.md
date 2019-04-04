# Markdown Cheat Sheet

段落
----

連續行會被串接成一個段落，若要換另一個段落，必須插入一行或多行的空白行。

範例：

```
Here is text for one paragraph.
The text continue in same paragraph.

We continue with more text in another paragraph.
```

結果：

Here is text for one paragraph.
The text continue in same paragraph.

We continue with more text in another paragraph.

標題
-------

Markdown 支援兩種型式的標題文字。第一種在標題文字的下一行使用等號表示標題1；使用減號表示標題2。等號或減號的數量不重要。

範例：

```
This is a level 1 header
=========================

This is a level 2 header
-------------------------
```

結果：

This is a level 1 header
=========================

This is a level 2 header
-------------------------

另外，你也可以使用 # 開頭表示標題文字，# 的數量表示標題文字的等級。標題文字可以不必使用 # 結尾，但是結尾的 # 不會出現在輸出文件中，而且數量也沒有限制。

範例：

```
# This is a level 1 header

## This is a level 2 header #######
```

結果：

# This is a level 1 header

## This is a level 2 header #######

引言區塊
------

Email 引言, 輸出時每一行會用 | 開頭。引言區塊中的文字會自動斷行。

要建立引言區塊，請在每一行開頭加上一個或多個的 >，類似純文字 Email 中的引言文字。

清單項目(Lists)和程式碼區塊(Code Blocks) 可以出現在引言區塊中。引言區塊也可以被巢狀。

每一行 > 符號後至少要有一個空白符號(space)。

範例：

```
> This is a block quote
> spanning multiple lines
>
> Code Blocks:
>
>     int integerVar = 0;
>
> Lists:
>
> 1. item 1
> 2. item 2
>
>> This is a nested block quote
```

結果：

> This is a block quote
> spanning multiple lines
>
> Code Blocks:
>
>     int integerVar = 0;
>
> Lists:
>
> 1. item 1
> 2. item 2
>
>> This is a nested block quote

清單
----

以 -、+、或 * 開頭可以建立簡單的項目清單。項目清單可以包含多個段落，每個項目下的段落都必須縮排4個空白或是一個 tab。依照這樣的縮排規則，您也可以在清單中放入程式碼區塊：

範例：

```
-   Item 1

    More text for this item.

        int x = 1;
        int y = 2;

-   Item 2
    +   nested list item.
    +   another nested item.
-   Item 3
```

結果：

-   Item 1

    More text for this item.

        int x = 1;
        int y = 2;

-   Item 2
    +   nested list item.
    +   another nested item.
-   Item 3

每個清單項目可以插入多個段落(要做適當的縮排，每個段落之後要插入空白行)。而且可以巢狀清單項目。

你也可以建立編號清單：

```
1.  First item.

    Code Block:

        int x = 5;
        int y = 10;

2.  Second item.
```

結果：

1.  First item.

    Code Block:

        int x = 5;
        int y = 10;

2.  Second item.

數字順序並不重要，但數字必須大於或等於 1。

當然，項目清單很可能會不小心產生，像是下面這樣的寫法：

```
1986. What a great season.
```

會產生如下的結果：

1986. What a great season.

換句話說，也就是在行首出現數字－句點－空白，要避免這樣的狀況，你可以在句點前面加上反斜線。

```
1986\. What a great season.
```

結果：

1986\. What a great season.

程式碼區塊
---------

格式化文字區塊可以使用至少 4 個空白字元的縮排來建立。每行的第一階的縮排（最前面的4個空白或是tab），都會被移除：

```
This a normal paragraph

    This is a code block

We continue with a normal paragraph again.
```

結果：

This a normal paragraph

    This is a code block

We continue with a normal paragraph again.

程式碼區塊中，一般的 Markdown 語法不會被轉換。

無法在段落中間插入格式化文字區塊，格式化文字區塊之前必須空一行。

水平線
------

使用至少三個 -、*、_ 來建立水平線，而且可包含任意數量的空白。

範例：

```
- - -

______
```

結果：

- - -

______

強調
----

一個 * 號或底線字元括住文字表示斜體字，兩個 * 號或底線字元括住文字表示粗體字。

範例：

```
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

*單一星號*

_單一底線_

**雙星號**

__雙底線__
```

結果：

*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

*單一星號*

_單一底線_

**雙星號**

__雙底線__

如果要在文字前後直接插入普通的星號或底線，你可以用反斜線：

    \*this text is surrounded by literal asterisks\*

結果：

\*this text is surrounded by literal asterisks\*

行內程式碼
----------

行內程式碼會以 html `<code>` 標記來輸出。

要建立行內程式碼，請用倒引號(`)括住文字。行內程式碼可放在段落中。

範例：

```
Use the `printf()` function.
```

結果：

Use the `printf()` function.

如果行內程式碼文字中有倒引號，請用多個倒引號括住行內程式碼文字。行內程式碼的起始和結束端都可以放入一個空白，起始端後面一個，結束端前面一個，這樣你就可以在開始或結束時插入倒引號：

例如：

```
To assign the output of command `ls` to `var` use ``var=`ls` ``.
```

結果：

To assign the output of command `ls` to `var` use ``var=`ls` ``.

行內連結
-------

連結是以方括弧括住連結文字開始，接著括號，括號內放入 URL 以及選擇性的連結標題，連結標題必須用雙引號括住。

範例：

```
[The link text](http://example.net/)

[The link text](http://example.net/ "Link title")

[The link text](/relative/path/to/index.html "Link title")

[The link text](somefile.html)
```

結果：

[The link text](http://example.net/)

[The link text](http://example.net/ "Link title")

[The link text](/relative/path/to/index.html "Link title")

[The link text](somefile.html)

參考連結
-------

除了 inline 連結外，你也可以定義參考連結，然後在其他地方重複使用。

參考連結(Reference Links)，就是為連結定義參考名稱。如果同一個連結使用多次時，使用參考連結，就不用每次都加入這麼長的 Inline 連結。

參考連結定義如下：

```
[link name]: http://www.example.com "Optional title"
```

`Option title` 可以用單引號或括號括住。也可以把 `Option title` 屬性放到下一行，也可以加一些縮排，網址太長的話，這樣會比較好看。

使用方式如下：

```
[link text][link name]
```

結果：

[link text][link name]

[link name]: http://www.example.com "Optional title"

如果連結文字和連結名稱相同，可以這麼用：

```
[link name][]
```

[link name][]

甚至是這樣：

```
[link name]
```

[link name]


連結名稱可以包含字母、數字、空白和標點符號，但是並不區分大小寫。

例如：

```
I get 10 times more traffic from [Google] than from [Yahoo] or [MSN].

[google]: http://google.com/        "Google"
[yahoo]:  http://search.yahoo.com/  "Yahoo Search"
[msn]:    http://search.msn.com/    "MSN Search"
```

結果：

I get 10 times more traffic from [Google] than from [Yahoo] or [MSN].

[google]: http://google.com/        "Google"
[yahoo]:  http://search.yahoo.com/  "Yahoo Search"
[msn]:    http://search.msn.com/    "MSN Search"

註：參考連結的定義只有在產生連結的時候用到，並不會直接出現在文件之中。

圖片
----

Markdown 的圖片連結語法類似於 inline 連結。唯一的不同是圖片連結語法用 `!` 做開頭。

範例：

```
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Title text")
![Alt text][img def1]
![img def1]

[img def1]: /path/to/img1.jpg "Title text"
```

結果：

![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Title text")
![Alt text][img def1]
![img def1]

[img def1]: /path/to/img1.jpg "Title text"

`Alt text` 及 `Title text` 是選擇性的。

若使用 image inline link 時，Alt text 雖可省略，但要保留其中括號。

使用 image define link 時，Alt text 及其中括號都可省略。

自動連結
-------

若要建立 URL 或 E-Mail 連結，Markdown 支援下列的語法：

```
<http://www.example.com>

<https://www.example.com>

<ftp://www.example.com>

<mailto:address@example.com>

<address@example.com>
```

結果：

<http://www.example.com>

<https://www.example.com>

<ftp://www.example.com>

<mailto:address@example.com>

<address@example.com>

跳脫字元
--------

Markdown 可以利用反斜線來插入一些在語法中有其它意義的符號，例如：如果你想要用星號加在文字旁邊的方式來做出強調效果（但不用 `<em>` 標籤），你可以在星號的前面加上反斜線：

    \*literal asterisks\*

Markdown 支援在下面這些符號前面加上反斜線來幫助插入普通的符號：

    \   反斜線
    `   反引號
    *   星號
    _   底線
    {}  大括號
    []  方括號
    ()  括號
    #   井字號
    +    加號
	-	減號
    .   英文句點
    !   驚嘆號

表格
----

表格由標題列、分隔列以及至少一個資料列。表格欄位以管線符號(|)分隔。

範例：

```
First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell
```

結果：

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

欄位對齊可以透過分隔列上的一個或兩個冒號來控制：

```
| Right | Center | Left  |
| ----: | :----: | :---- |
| 10    | 10     | 10    |
| 1000  | 1000   | 1000  |
```

結果：

| Right | Center | Left  |
| ----: | :----: | :---- |
| 10    | 10     | 10    |
| 1000  | 1000   | 1000  |

Fenced Code Blocks
------------------

Fenced Code Blocks 是另一種型式的程式碼區塊，它不需要縮排，而是在程式碼前後使用一對的 "fence lines" 包圍著，"fence line" 由至少3個 ~ 符號所組成。開始的 fence line 應該和結束的 fence line 有相同數量 ~ 符號。

範例：

```
This is a paragraph introducing:

~~~~~~~~~~~~~~~~~~~~~
a one-line code block
~~~~~~~~~~~~~~~~~~~~~
```

結果：

This is a paragraph introducing:

~~~~~~~~~~~~~~~~~~~~~
a one-line code block
~~~~~~~~~~~~~~~~~~~~~

行內 HTML
-----------

Markdown 不是要來取代 HTML，所以沒有直接對應到的 HTML 標記，都可以直接使用 HTML 標籤，而它大部分可以運作的很好。只有區塊元素 ── 例如 `<div>`、`<table>`、`<pre>`、`<p>` 等標籤，必須在前後加上空行，以利與內容區隔。

範例：

```
<dl>
  <dt>Definition list</dt>
  <dd>Is something people use sometimes.</dd>

  <dt>Markdown in HTML</dt>
  <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
</dl>
```

結果：

<dl>
  <dt>Definition list</dt>
  <dd>Is something people use sometimes.</dd>

  <dt>Markdown in HTML</dt>
  <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
</dl>

請注意，Markdown 語法在 HTML 區塊標籤中將不會被進行處理。

HTML 的區段標籤如 `<span>`、`<cite>`、`<del>`、`<a>` 則不受限制，可以在 Markdown 的段落、清單或是標題裡任意使用。

HTML 區段標籤和區塊標籤不同，在區段標籤的範圍內， Markdown 的語法是有效的。

特殊字元自動轉換
---------------

在 HTML 文件中，有兩個字元需要特殊處理： `<` 和 `&` 。Markdown 允許你直接使用這些符號，但是你要小心跳脫字元的使用，如果你是在 HTML 實體中使用 `&` 符號的話，它不會被轉換，而在其它情形下，它則會被轉換成 `&amp;`。所以你如果要在文件中插入一個著作權的符號，你可以這樣寫：

    &copy;

輸出結果：&copy;

Markdown 將不會對這段文字做修改，但是如果你這樣寫：

    AT&T

Markdown 就會將它轉為：

    AT&amp;T

輸出結果：AT&T

類似的狀況也會發生在 `<` 符號上，因為 Markdown 支援行內 HTML，如果你是使用 `<` 符號作為 HTML 標籤使用，那 Markdown 也不會對它做任何轉換，但是如果你是寫：

    4 < 5

Markdown 將會把它轉換為：

    4 &lt; 5

輸出結果：4 < 5

不過需要注意的是，code 範圍內，不論是行內還是區塊， `<` 和 `&` 兩個符號都*一定*會被轉換成 HTML 實體。

Footnote
--------

範例：

```
Footnotes[^1] have a label[^label] and a definition[^!DEF].

[^1]: This is a footnote
[^label]: A footnote on "label"
[^!DEF]: The definition of a footnote.
```

結果：

Footnotes[^1] have a label[^label] and a definition[^!DEF].

[^1]: This is a footnote
[^label]: A footnote on "label"
[^!DEF]: The definition of a footnote.

Code Fencing
------------

在程式碼的前後一行，獨立使用連續的 3 個倒引號。開頭時還可標示程式碼的種類。

範例：

<pre>
```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

```python
s = "Python syntax highlighting"
print s
```

```
No language indicated, so no syntax highlighting.
But let's throw in a <b>tag</b>.
```
</pre>

結果：

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

```python
s = "Python syntax highlighting"
print s
```

```
No language indicated, so no syntax highlighting.
But let's throw in a <b>tag</b>.
```

C# 範例：

<pre>
```csharp
using System;

#pragma warning disable 414, 3021

public class Program
{
    /// <summary>
    /// The entry point to the program.
    /// </summary>
    public static int Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
        string s = @"This
""string""
spans
multiple
lines!";
        return 0;
    }
}

async Task<int> AccessTheWebAsync()
{
    // ...
    string urlContents = await getStringTask;
    return urlContents.Length;
}
```
</pre>

結果：

```csharp
using System;

#pragma warning disable 414, 3021

public class Program
{
    /// <summary>
    /// The entry point to the program.
    /// </summary>
    public static int Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
        string s = @"This
""string""
spans
multiple
lines!";
        return 0;
    }
}

async Task<int> AccessTheWebAsync()
{
    // ...
    string urlContents = await getStringTask;
    return urlContents.Length;
}
```
