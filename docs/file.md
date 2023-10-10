---
sidebar_position: 4
slug: file-inclusion
---

# 包含文件

## 簡介

在 PHP 中，可以使用幾種 SPL 方法將一個檔案包含到另一個檔案中。

最常用的兩種方法是 <font color="green">`require`</font> 和 <font color="green">`require_once`</font>。 另外還有 `include` 和 `include_once `函數，它們具有相同的行為，但**錯誤處理方式不同**（後者沒有 `require` 嚴格）。

## `require` 與 `require_once`

要在另一個檔案 A 中包含檔案 B，很簡單：只需在檔案 A 中指定 `require_once 'path/to/B'`;。 一旦檔案 B 被包含，使用 `require_once` 就表示不能再使用` require_once 'path/to/B'`; ，檔案 B 不能被二次包含。
而使用`require`时，我們可以隨意多次包含文件。 關鍵在於使用`once`，這才是形成差異的關鍵。

:::info 提示
當 PHP 檔案 A 包含一個 PHP 檔案 B 時，會將 檔案 B 中定義的所有符號（常量、變量等...）匯入（導入）到檔案 A 中。 此外， 檔案 B 還可以使用在 A 中定義的任何符號。
:::

因此，我們可以將 PHP 腳本的不同部分分成幾個文件，以便更清晰地劃分我們的應用程序，例如：

-   數據定義
-   重複使用介面元素
-   顯示元素
-   ……

## 為什麼程式碼分割?

讓我們從兩個問題出發來探討使用文件包含的原因。

### 責任

讓我們看看以下腳本：

```bash
<?php
$myText = 'Hohoho Hello';
const SOFTWARE_VERSION = '1.0.0';

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

var_dump($users);
echo $myText;

```

項目的結構非常簡單：

```bash
| index.php
```

我們可以在該腳本中找到兩個主要部分：

-   變數和常數分配
-   變數的使用和顯示

因此每一行都有自己的**職責**。

例如，腳本的第 2 行負責為 `SOFTWARE_VERSION `常數賦值 `'1.0.0'`。`

我們可以將這些責任**分開**，以更明智的方式將它們隔離在不同文件中，並根據它們在應用程式中的角色為其命名。 然後，在應用程式的任何地方，我們都可以**重複使用**這些聲明。

因此，（根據上面的程式碼）我們可以列出以下可識別的角色：

-   定義 `$myText` 變量，該變量看起來是腳本的本地變量
-   定義一個 `SOFTWARE_VERSION` 常量，它似乎是應用程式的通用變量
-   定義數組 `$users`
-   顯示訊息

:::caution 注意
根據需求，我們也可以辨識其他職責和角色。 但唯一的條件是必須能夠合理地解釋這些選擇，而其他開發人員也能夠理解
:::

本地變量無需隔離到其他地方：因為它們僅限於本地腳本。

但是，對於常量 `SOFTWARE_VERSION` 似乎過於通用，不適合在一個負責顯示資料的文件中聲明。

用戶也是一樣：雖然在這裡使用了用戶，但很有可能以後會在另一個文件中重複使用。

因此，新的結構可能如下所示：

```bash
|📃constants.php
|📕data/
  |📃users.php
|📃index.php
```

> `constants.php`

```bash
<?php
const SOFTWARE_VERSION = '1.0.0';
```

> `data/users.php`

```bash
<?php
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

> `index.php`

```bash
<?php
require_once 'data/users.php';
$myText = 'Hohoho Hello';

var_dump($users);
echo $myText;
```

### 佈局

在這裡，我們將根據應用程式的外觀進行分隔。 我們不會將所有內容都寫在一個文件中，而是根據應用程式的不同部分將其分開。

舉例來說，你可能會發現這樣的結構：

```bash
<?php
|📃index.php
|📕layout/
  |📃header.php
  |📃footer.php
```

在`index.php`文件中：

```bash
<?php
require_once 'layout/header.php';

// 主頁內容

require_once 'layout/footer.php';
```

因此應用程式可以接收新的頁面，這些頁面將採用相同的包含系統。

:::caution 注意

再次強調，您可以確定您想要的部分（導航、側邊欄等），也可以為它們命名，等等......重要的是，要讓其他想要開發應用程式的開發人員能夠理解，以便他們可以在應用程式上進行工作。

:::

**在同一頁面重複使用同一段程式碼**

通常，在建立模型時，我們會建立頁面來顯示元素清單（列表）。

在這種情況下，我們可能會建立包含 50 或 100 個項目的頁面，這些項目會重複出現。

這時，我們將建立一個用於顯示單一項目的模板，然後使用 `require_once` 循環顯示集合中的每個項目。

需要注意的是，如果要根據集合中元素的數量多次包含模板，則需要使用 `require` 而不是 `require_once`。

## 絕對路徑和相對路徑

### 相對路徑

如果對什麼是絕對路徑和相對路徑仍有困惑，那麼最重要的一點就是：

> 如果我想到 "相對"，我馬上就會想到 "**相對於什麼？**"

此外，很容易識別一個路徑是相對路徑，因為它不會以 `/` 開頭，例如 ：

```bash
<?php
// 以下是相對於目前檔案的範例（「相對於什麼？」）
// 轉到包含“users.php”文件的“data”資料夾
require_once 'data/users.php';
// 上一層並包含 config.php 文件
require_once '../config.php';
// 在目前目錄中，包含 db.php 文件
require_once 'db.php';

```

我們也可以看到 `../` 這樣的表示方法，它可以讓你導航到**上一層目錄**。 當然，也可以用此表示方法向上導航任意級別， 例如：`../.../../test.php ` 將導航到三個目錄，以訪問更高層次的 `test.php` 檔案

### 絕對路徑

絕對路徑總是從根目錄開始：`C:\Users\...`, `/var/log/...`, `http://www.monsite.com/...`.

在 PHP 中，為了簡單起見，我們最初會使用相對路徑。 但隨後，我們會傾向於使用 **PHP 自動計算的絕對路徑**。 為此，我們可以使用 SPL 中提供的常數 `__DIR__` ：

```bash
// 例如在“data/users.php”中
require_once __DIR__ . '/../constants.php';
```

實際上，即使在絕對路徑中我們也可以使用 `../`。

:::info 為什麼要這樣做呢？

在上面的範例中，如果 `data/users.php` 包含在 `data` 資料夾（文件夾）之外的另一個檔案中，那麼它自己的 `require_once` 將相對於**正在執行的腳本目錄**，而不是相對於它自己的目錄。
因此，透過添加 `__DIR__` 常量可確保定位到正確的位置。 這就像一個自動化的絕對路徑，確保程式碼在不同環境中都能正常運作。

:::
