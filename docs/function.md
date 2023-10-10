---
sidebar_position: 5
slug: function
---

# 函數

我們已經使用過 SPL 提供的函數。 可以得出的第一個結論是，函數具有**實用**目的：我們將建立一個函數，為另一段程式碼提供功能。 另一段程式碼就是**呼叫者（調用者）**。

因此，函數是一系列指令，並有名稱，可以在任何需要的地方進行呼叫（調用）。

當我們談論函數時，通常將其分為兩個步驟：

-   定義
-   呼叫

## 定義函數

當我們定義一個函數時，我們編寫它的程式碼，這段程式碼將在呼叫時執行。

```bash
<?php
// 定義函數
function maFonction(string $param1, string $param2 = 'defaultValue'): string
{
// 執行指令
}

```

### 函數簽名

函數定義行包括關鍵字 `function`、函數名稱、可選參數以及傳回類型。 這就是函數的**簽名**。

因此，我們要為執行函數的程式碼命名，並考慮以下幾點：

-   我的函數需要什麼才能運作？
-   我想給我的函數的名字是否是一個清晰的概念，而不是多個？
-   一旦我的函數執行完畢，是否需要向呼叫程式碼提供資訊？

### 參數

> 我的函數需要什麼才能運作？

函數的參數定義了由呼叫函數的程式碼傳遞的輸入值。 這是函數所需要的一切。

例如，如果我寫一個名為 `uppercase` 的函數，那麼我可以猜到這是為了將文本轉換為大寫。 我所需要的只是傳入初始文本，所以只需要一個參數。

我们也可以定義**可選**參數，並指定預設值（默認值）。

```bash
<?php
// 定義函數
function direBonjour(string $nom = "bel inconnu"): void // 函數簽名
{
  echo "BONJOUR $nom !!!";
}
```

這樣，我可以在呼叫函數時使用或不使用參數。 如果未傳遞參數，則將使用預設值：

```bash
direBonjour("Bob"); // 含參數
direBonjour(); // 不含參數：預設值 = "bel inconnu"
```

:::tip 可選參數
可選參數具有預設值。 因此，它們應該放在函數參數的**最後**，以保持參數傳遞順序的一致性。

自 PHP 8 開始，如果有許多可選參數，可以在呼叫時明確指定參數：

```json
function format(
  string $input,
  bool $uppercase = false,
  bool $color = '#000',
  bool $background = false,
  bool $bold = false, // <--
  bool $underline = false
): string {
  return $input; // 想像一下這裡的格式
}

format('Hello', bold: true);
```

:::

在函數的參數中，盡量對參數進行**類型聲明**，以便在查看函數簽名時更好地理解程式碼的行為，并達到想要實現的預期。

此外，盡量減少函數的參數數量，如果函數的參數太多，很可能表示它的功能過於複雜。 當然，也有例外情況，但始終記住這一點是有好處的。

### 命名

>     我想給我的函數的名字是否是一個清晰的概念，而不是多個？

在函數簽名中包含了函數的**名稱**。 函數名稱應該簡單而清晰地總結函數的功能。 不是解釋為什麼它這樣做，而是簡單地說明它在做什麼。

例如，如果我想在螢幕上顯示一些內容，我可以使用諸如 `display`、`print` 或 `echo` 等關鍵字，這是 SPL 中的一些常見詞彙。

但如果我想取得一些東西，我可能更願意使用 `get`、`fetch`、`retrieve` 等詞彙。

例如，如果我想建立一個函數，該函數將根據輸入的文字將其包裝在 `<p></p>` 標籤中，將輸入的文字轉換為段落。

这里有很多命名方式：`paragraph`、`transformIntoParagraph`、`displayParagraph`、`getAsParagraph` 等等...一切都取決於我們的目的。 在這種情況下，我們想要將文字**傳回（返回）**為 HTML 段落，所以我可能會使用 `getAsParagraph(string $text)`。 但這不是唯一的解決方案！ 我也可以將這個函數命名為 `surroundWithPTags(string $text)`，因為我認為這個命名表達的意圖更明確。

:::tip 小貼士
最重要的是，與我們寫的大多數程式碼一樣，它應該是清晰易懂的，在閱讀時需要最少的時間來理解。 沒有所謂的「最佳」解決方案，只要易於理解就可以。
:::

### 傳回值（返回值）

> 一旦我的函數執行完畢，是否需要向呼叫程式碼提供資訊？

函數可以使用 return 語句向呼叫它的程式碼**傳回一個值**。

:::caution 注意
使用 `return` 語句時，會導致函數的**退出**。
:::

```json
<?php
function getAsParagraph(string $text): string
{
  return "<p>$text</p>";
  // return後，不寫任何程式碼，因為「return」會導致函數退出
}

```

在這種情況下，可以在呼叫函數的程式碼中接收傳回的值：

```json
// on récupère et on met dans la variable $paragraph la valeur retournée depuis la fonction paragraphMajuscules
$paragraph = getAsParagraph("Hello world");

```

:::info 需要記住的是

總之，當我們想要使用或寫一個函數時，我們需要考慮以下三個面向：

-   參數
-   名稱
-   傳回類型和值

這三個元素組成了函數的簽名。
:::

## 函數的調用

要呼叫一個函數，它必須在我們可呼叫**範圍內**。

因此，我們經常會看到包含函數的文件的導入，以便可以呼叫或使用該文件中的函數。

```json
<?php
require_once 'functions.php'; // on inclut toutes les fonctions définies dans functions.php

format("texte", background: true); // Puis on appelle une fonction, une fois qu'elle se trouve à notre portée

```

無論是否使用 `require` ，都必須記住以下順序：先**定義**一個函數，然後**呼叫**它。

## 匿名函數

我們也可以使用**匿名函數**。 在這種情況下，我們將定義一個函數的簽名，但不指定函數名稱。

### 匿名函數有什麼用途呢？

我們以 `array_filter` 函數為例，看看它的簽名：

```json
// array_filter permet de filtrer les éléments d'un tableau en fonction d'une expression
array_filter(array $array, ?callable $callback = null, int $mode = 0): array

```

第二個參數的型別（類型）是 `callable`。 也就是說，我們可以傳遞給它一個"可呼叫"的類型，例如一個函數。 在這裡，我們可以在不需要宣告聲明和命名函數的情況下，在 `array_filter` 中聲明一個"即時"呼叫的函數。

### 用途

我們可以想像一個包含整數值的分數數組，我們希望應用一個過濾器，只保留平均分數（大於或等於 10）的值。
首先，我們創建一個陣列：

```json
//         O  X   O   O  X  X   O   O
$notes = [12, 7, 18, 15, 8, 9, 14, 20];

```

函數 `array_filter` 對陣列的**每個元素**執行給定的函數。 它將只保留函數傳回 ` ` 的元素。

我們定義一個傳統的函數來驗證平均分數：

```json
function validNote(int $note): bool
{
  return $note >= 10;
}

```

然後，我們可以使用 `array_filter` 將此函數應用於陣列的每個元素，將函數的名稱作為字串傳遞：

```json
$validNotes = array_filter($notes, "validNote");

```

`array_filter` 將為陣列`$notes`的每個元素呼叫` validNote`，並將每個元素作為 `validNote` 的參數傳遞。 例如，對於第一個元素，這就像呼叫 `validNote(12)`，然後查看結果（`true` 或 `false`），然後根據結果決定是否保留它。
但為了避免聲明和命名函數，我們也可以直接將匿名函數傳遞給 `array_filter`：

```json
$validNotes = array_filter($notes, function (int $note): bool {
  return $note >= 10;
});

```

### 箭頭函數（Arrow Function）

我們也可以使用箭頭函數（arrow function），類似於 JavaScript。 但語法略有不同：

```json
$validNotes = array_filter($notes, fn (int $note) => $note >= 10);
```

:::info 提示

在箭頭函數中，我們不會實際傳遞一個帶有花括號 `{}` 的函數體，而是將被傳回的表達式。 在這裡，我們直接傳回表達式 `$note >= 10` 的評估結果，要麼是 `true`，要麼是 `false`。

:::

### 在父級作用域中使用變量

讓我們來看另一個例子：我從 URL 接收一個 GET 參數，並希望在匿名函數中使用它。

```json
// Récupération du paramètre d'URL
// J'extraie du tableau $_GET la valeur qui se trouve derrière la clé 'q'
// et la place dans une variable $search
['q' => $search] = $_GET;

```

例如，我有一個關於用户的關聯數組，每個用户都包含一個名為` name` 的鍵。 在這個鍵後面是值，也就是用户的名字。
我想使用變量 `$search` 按名字找出指定用户。
如果使用語法`function（array $user）`，那麼我必須聲明正在使用變量 `$search`，而這個變量在上層作用域的上下文中已經聲明過了。
為此，我們將使用關鍵字` use`：

```json
['q' => $search] = $_GET;

// Toujours avec array_filter
// Je déclare avec use que je vais utiliser $search
// $search est issue du contexte parent, juste au-dessus
// Ma fonction anonyme n'a donc pas accès à cette variable
$results = array_filter($users, function ($user) use ($search) {
  return str_contains($user['name'], $search);
});
```

如果使用箭頭函數，那麼**自動可以存取父級上下文中的變量**，因此不需要使用` use`：

```json
['q' => $search] = $_GET;
$results = array_filter($users, fn ($u) => str_contains($u['name'], $search));
```

這是 PHP 語言中所包含和預設的功能，如文檔所示：

> 箭頭函數支持與匿名函數相同的功能，不同之處在於始它終自動使用父作用域中的變量。

:::info 提示

**portée parente** 在這裡意味著父級作用域，或在前面提到過被稱為**父級上下文**。

匿名函數，就像常規函數一樣，都有一個**主體**， 這個主體代表一個**作用域**。

因此，它不再是周圍的上下文，而是父級作用域。

:::

## 展開運算符

我們在陣列的章節中已經看過這個運算符。

我們也可以在**函數的參數**中使用它。

事實上，函數中可能會有可變數量的參數。

舉一個簡單的例子，一個名為 `sum` 的函數，對傳遞給它的元素求和。

我們無法預先知道要提供多少個元素，而這個數量本質上是可變的（我們不希望每次都添加相同數量的元素）。

因此，我們可以定義 `sum `函數接受可變數量的參數：

```json
function sum(int ...$elements): int {
  // ...
}
```

在 `sum` 函數內部，`$elements` 本身將會是一個陣列！ 因此，我們可以在其上進行迭代、應用函數等等... `**它包含了傳遞給函數的所有參數**`。

要呼叫 `sum` 函數，這裡可以使用可變數量的參數：

```json
$total1 = sum(4, 65, 78);
$total2 = sum(7, 987, 53, 45, 87 , 21);
// ...
```
