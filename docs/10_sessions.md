---
sidebar_position: 10
slug: sessions
---

# 會話

會話允許 PHP 伺服器識別特定的瀏覽會話，從而識別潛在的登入使用者的機制。例如，透過會話，使用者一旦登錄，就不需要在每個查看的頁面上重新登入。

伺服器透過讀取**cookie**來識別會話。

預設情況下，該 cookie 的名稱為 `PHPSESSID` ，它包含一個隨機值，即會話識別碼。

伺服器成功識別了 `PHPSESSID` 後，它將能夠還原先前定義的上下文（鍵/值對的陣列），並且可以在瀏覽的各個頁面上更新此上下文。

因此，會話可以在用戶在應用程式上瀏覽期間追蹤用戶。

會話提供了一些功能，例如身份驗證或購物車功能。

在 PHP 中，我們將使用名為 `$_SESSION` 的超級全域數組來處理會話資料。

:::note 注意：
`$_SESSION` 是唯一可以**讀取**和寫入資料的全域（全局）數組。
:::

## 启动会话

要在應用程式中使用會話，必須先使用 SPL ` session_start()` 函數。

如果沒有啟動會話，伺服器會建立新的會話識別碼和相關 cookie，並將其傳送回瀏覽器。 伺服器也會初始化超全域變量 `$_SESSION`，該變數預設為空。

如果伺服器上已經儲存了一個會話，伺服器就會傳回 `$_SESSION` 數組，其中包含為該使用者定義的鍵/值對（使用者是否已登入？有購物籃嗎？有哪些產品？等等）。

```bash

// 第一次執行：建立會話標識符
// 後續執行：讀取 PHPSESSID cookie 並恢復會話n
// 警告：如果未使用 session_start()，則未定義 $_SESSION 陣列！
session_start();

// 此處，$_SESSION 為空
var_dump($_SESSION);

$_SESSION['connected'] = false;

// 現在，$_SESSION 包含一個 "connected "鍵，其值為 false。
// 如果我們重新載入頁面，那麼之前顯示空會話的 var_dump，現在將顯示已包含 "connected "鍵的數組
var_dump($_SESSION);

```

:::caution 注意

如果多次呼叫 `session_start()` ，PHP 將產生錯誤。 請確保在腳本執行的開始只呼叫一次 session_start()。 您也可以查閱文檔，找到一種方法來檢查會話是否已啟動，如果沒有啟動，則啟動它。

:::

## 銷毀一個會話

要結束一個會話（例如，使用者登出），首先需要清空超級全域數組 `$_SESSION` ，然後使用 SPL 的方法銷毀會話。

```bash
session_start(); // 請記得使用 session_start，否則 $_SESSION 將未定義！
$_SESSION = [];
session_destroy();

```
