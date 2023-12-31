---
sidebar_position: 3
slug: /array
---

# 陣列（數組）

## 定義

數組(array)是數值的**集合**。 它是程式語言中一種非常常見的數據結構。

## 聲明

```json
<?php
// 聲明包含 3 個元素的陣列
$monTableau = [1, 2, 3];
// 聲明空數組
$emptyArray = [];
```

## 索引/鍵

陣列的每個元素都有索引。 在上面的範例中，陣列的索引（或鍵）會自動從 0 開始編號，因此每個鍵對應一個值。

上面的程式碼只指定了值就創建了一個陣列。 PHP 將自動分配從 0 開始編號的鍵，從而建立一個結構如下的陣列：

| 索引/鍵 | 值  |
| ------- | --- |
| 0       | 1   |
| 1       | 2   |
| 2       | 3   |

### 訪問數值（讀取）

要訪問一個值，我們使用陣列的名稱（此處為 $myArray），然後在方括號 [] 中寫入要尋找的鍵值。

如果要訪問第一個元素，可以寫成 ：

```bash
<?php
// 使用方括號訪問數組中的元素
// 0 是數組中的第一個鍵
// 將顯示值 "1"。
echo $monTableau[0];

// 0 可以用變量或表達式代替
// 例如，如果想訪問最後一個元素
// count 是一個 SPL 函數，用來計算數組中元素的個數
$nbElements = count($monTableau);
$derniereValeur = $monTableau[$nbElements - 1];
```

### 添加

在數組中添加元素非常簡單：

```json
$monTableau[] = 8; // 任何值
```

PHP 還原了自動編號，並將下一個索引（此處為 "3"）賦值給 "8"：

| 索引/鍵 | 值  |
| ------- | --- |
| 0       | 1   |
| 1       | 2   |
| 2       | 3   |
| 3       | 8   |

### 修改

要修改表中某處的已存在的值，我們也要使用方括號來指明要對哪個索引進行操作：

```bash
$monTableau[2] = 4;
```

| 索引/鍵 | 值  |
| ------- | --- |
| 0       | 1   |
| 1       | 2   |
| 2       | 4   |
| 3       | 8   |

### 插入

在表格中插入一個或多個元素並非易事。

在位置和保留現有元素等方面會出現問題。

為了解決這些問題，我們可能需要定義一個可重複使用的函數來完成這項工作。 請參閱 "函數"一章。

### 自定義鍵值

我们也可以自定義數組中的鍵。 這會建立一個**關聯數組**，或**鍵/值數組**。

```bash

// 關聯數組
// 鍵/值數組
// 我們自己指定鍵
$arrayIndexes = [2 => 1, "autre test" => 4];

```

數組結構如下：

| 索引/鍵    | 值  |
| ---------- | --- |
| 2          | 1   |
| "其他測試" | 2   |

在這種情況下，可以透過指定元素的鍵，甚至是自定義鍵或**字串**鍵來訪問該元素：

```bash

$value = $arrayIndexes["autre test"]; // 变量$value将包含值4

```

:::info 自动键/索引 0 呢？
对于我们刚刚创建的关联数组，我们可能会想知道 PHP 自动分配的键在哪里。
如果我们不确定索引"0"是否存在，那么可以简单地进行测试：

```bash
echo $arrayIndexes[0];
```

这时屏幕上会出现一个错误，提示 `Undefined array key`（未定义数组键）或 `Undefined index/offset `（未定义索引/偏移量）。如果在创建数组时没有指定键，或者 PHP 认为没有必要自动为其赋值，那么它就不存在。
:::

## 操作符在數組中的使用

:::info 提示
在 PHP 中，在陣列中使用操作符並不常見。
:::

可以使用加法運算符（+）將一個陣列新增到另一個陣列中，但必須非常謹慎，因為此操作將檢查陣列的鍵，並只添加不同的鍵！

```bash
$tab1 = [1, 2, 3];
$tab2 = [4, 5, 6];
var_dump($tab1 + $tab2); // [1, 2, 3]

```

在這種情況下，不要混淆加法運算 `+` 和 `array_merge` 函數。

## 數組重構(array destructuring)

透過自動提取數組中的一個或多個值並將其賦值給變量，可以對現有數組進行重構：

```bash
$tab1 = [1, 2, 3];
// 將陣列解構到變量 $one、$two、$three 中
[$one, $two, $three] = $tab1;
var_dump($one, $two, $three); // 1, 2, 3

```

如果在某個位置未指定變量，則該位置的提取將被忽略。 例如，如果你想提取第二個元素，或只提取最後一個元素，就會很有用。

```bash
[,, $last] = $tab1;
var_dump($last); // 3

```

我們也可以直接針對索引提取其值：

```bash
// 將陣列的第二個元素放入變量 $secondElement 中
[1 => $secondElement] = $tab1;
var_dump($secondElement); // 2

// 等效於
$secondElement = $tab1[1];
// 也等效於
[, $secondElement] = $tab1;

```

:::info 提示
最後一個範例也適用於自定義鍵（字符串）.
:::

## 展開運算符

在處理陣列時，可以使用`展開運算符（...）`來 "擴充 "陣列元素。 只需在數組前面添加展開運算符即可。
舉個例子，如果我想將兩個數字組合併為一個數組，可以嘗試使用下面的語法：

```bash
$tab1 = [1, 2, 3];
$tab2 = [4, 5, 6];

$tab3 = [$tab1, $tab2];
```

但是，如果我使用 `var_dump` 來檢查這個陣列的結構時，就會發現它**只包含 2 個元素**，而這 2 個元素本身就是陣列！

```json
array (size=2)
  0 =>
    array (size=3)
      0 => int 1
      1 => int 2
      2 => int 3
  1 =>
    array (size=3)
      0 => int 4
      1 => int 5
      2 => int 6
```

這完全合理。 我建立了一個數組，其中包含 2 個數組。

另一方面，如果我想把兩個數組中的元素寫入第三個數組，我可以使用展開運算符 ：

```bash
$tab3 = [...$tab1, ...$tab2];
```

```json
array (size=6)
  0 => int 1
  1 => int 2
  2 => int 3
  3 => int 4
  4 => int 5
  5 => int 6
```

現在我的陣列 `$tab3` 不再包含 2 個元素，而是 6 個，就像上面的例子一樣。

## 數組函數

SPL 中有許多關於數組的操作函數，所有這些函數都在<font color="green">數組文檔</font>中列出。 當然，你並不需要熟記這些函數。 在尋找執行操作的最佳方法時，可以使用文檔作為參考。

特別是當我們對元素進行排序、提取數組的一部分、對數組的**鍵**進行排序、為數組填充一定大小的值，等等...都可以使用數組相關的操作函數。

### 命令式/聲明式

在使用 PHP 標準函式庫中的陣列處理函數時，需要注意如何使用這些函數。

事實上，其中一些函數會**修改**現有的陣列（**命令式風格**），例如`sort`函數，而其他函數則會**傳回一個新的陣列副本**，不會修改原始陣列（**聲明式風格**），例如`array_reverse`函數。

:::info 命令式/聲明式編程

這裡我們討論的**命令式編程**，也稱為**過程式編程**，與我們的算法中存在一個可以**改變狀態**有關（這裡指的是數組）。

這與**聲明式**或**函數式**風格形成鮮明對比，後者通常處理**不可改變**（或不可修改）的數據：我們不是修改現有狀態，而是創建一個新的狀態，並將其傳回給呼叫者（調用者）（例如我們的數組）。

我們稍後會學習另一種命令式程式設計：物件導向程式設計（面向對象編程）。

:::

## 多維數組

數組是一種將鍵與值相關聯的資料結構。

對於給定的鍵，你可以想像值是......一個陣列。

因此，透過嵌套數組和指定自訂鍵，我們可以開始建立有意義的數據結構，例如：

```bash
$users = [
  [
    'name' => 'Lennon',
    'firstname' => 'John'
  ],
  [
    'name' => 'Dylan',
    'firstname' => 'Bob'
  ]
];
```

在上面的例子中，第一個陣列是一個**值**。 正如我們前面所看到的，PHP 會自動將 `0` 鍵與該值關聯，其他值也是如此。

## 循環

有幾種控制結構可用於對陣列執行操作（顯示、操作）。

**循環**通常用於顯示數組的每個元素。

> 請參閱原程式碼中的 `arrays/loops.php` 檔案：
>
> -   `while`
> -   `for`
> -   `do...while`
> -   `foreach`

### 循環的一般結構

在我們給出的範例中，循環（與陣列一起）的使用總是圍繞著索引進行：要遍歷數組的元素，就必須遍歷數組的**鍵**。
因此，`while`、`for` 和 `do...while` 循環的使用一般有三個要素：

-   初始化指令：在進入循環之前，我們先**初始化**所需的內容。
-   保持條件：每次循環**迭代**時都要檢查該條件。 只要該條件為真（`true`），循環就會繼續。
-   步驟指令：該指令在每次迭代後執行。 例如，我們用它來**遞增**索引變量。

### PHP `foreach` 循環

對於`foreach`循環，工作方式有所不同：我們可以直接使用**值**，不需要使用前面提到的三個元素：

```bash
foreach ($array as $element) {
  echo $element . "<br />";
}
```

不過，如果我們仍想訪問元素的鍵，可以使用另一種語法 ：

```bash
foreach ($array as $key => $element) {
  echo $key . " : " . $element . "<br />";
}
```
