---
sidebar_position: 9
slug: pdo
---

# 資料庫(數據庫) - PDO(資料對象)

目前，我們的應用程式中涉及了兩個角色：客戶端（瀏覽器）和伺服器（我們的 PHP 應用程式）。

如果我們希望創建一個可以儲存資料並允許資料隨時間發展的應用程序，那麼我們需要引入第三個角色：資料庫伺服器。

> 資料庫伺服器代表資料的**持久化**層，也可以理解為資料的儲存和保留層。

因此，從我們的 PHP 應用程式中，我們將使用 `PDO` 類型的物件(對象)與資料庫進行通訊。

## PDO

PDO 代表"PHP Data Objects" (PHP 資料物件)。 這是一個抽象層，透過使用各種物件和方法，允許我們與資料庫進行通訊。

:::info 資訊
在[PHP 文檔](https://www.php.net/manual/en/book.pdo.php)中尋找 PDO 類別的文檔。
:::

## 配置

安裝 PHP 時，預設會包含 PDO。

接下來要檢查的是要使用的**驅動程式**是否已啟用。

作為資料庫存取的抽象層，PDO 可以與各種資料庫管理系統（MySQL、PostgreSQL、SQLite 等）協同工作。 如果我們使用的是 MySQL，則需要檢查驅動程式是否已安裝並啟動。

:::info 激活 `pdo_mysql`
要使用 MySQL，請進入 PHP 配置檔案（`php.ini`）並啟動 `pdo_mysql`擴充：

-   在 `php.ini `中搜尋 `pdo_mysql`
-   如果找到的行有註解（以 分號 `;` 開頭），取消該行的註解（刪除 分號 `;`）
-   如果使用的是 Apache，不要忘記重新啟動伺服器。

:::

## 訪問

在訪問資料庫時，首先需要定義連接資料庫的屬性。 為此，我們需要向我們的應用程式提供以下資訊：

-   主機（資料庫伺服器所在的位置），可能還包括連接埠號碼(端口號)。
-   資料庫名稱（包含我們的資料表的"目錄"）。
-   使用者(用戶名)名稱.
-   密碼。
-   字符集。

### DSN（資料來源名稱）

`PDO` 類別的建構子需要一個**DSN**（Data Source Name）作為第一個參數。

我們可以按照以下方式定義 DSN：

```json
/*
mysql:  => 用於連接的驅動程式
dbname  => 資料庫名稱
host    => 要連線的主機
charset => 要使用的字元集
*/
$dsn = 'mysql:dbname=my-db-php;host=127.0.0.1;charset=utf8mb4';
```

### 連接

接下來，在第二和第三個參數中，提供使用者名稱和密碼，然後我們可以實例化一個新的 PDO 物件：

```json
$dsn = 'mysql:dbname=my-db-php;host=127.0.0.1;charset=utf8mb4';
$user = 'mon_user';
$password = 'mon_mot_de_passe';

try {
  $pdo = new PDO($dsn, $user, $password);
} catch (PDOException $e) {
  echo 'Connexion échouée : ' . $e->getMessage();
}
```

:::caution 注意

文件告訴我們，如果連接失敗，該類別的建構函數可以引發異常。 因此，最好將物件的建立放在一個 `try/catch` 區塊中，以實現最小的錯誤處理。

:::

## 查詢

一旦我們的 `PDO` 物件被實例化，我們就可以使用它向資料庫伺服器發送 SQL 查詢。

執行查詢的最快方法是使用 `query` 方法：

```json
$query = "SELECT * FROM users";
$stmt = $pdo->query($query);
```

這個方法將會傳回一個 `PDOStatement` 物件的實例。

隨後，我們需要遍歷這個語句的記錄。 讓我們開始取得第一條記錄：

```json
// row = 行（包含一條記錄的資料）
// fetch 意味著"讀取"/"獲取"
$row = $stmt->fetch();
var_dump($row);
```

如果我們想逐一取得所有記錄，使用 `fetch` 方法並對其使用循環：

```json
// while循環將在結果的最後一行後自動停止，因為fetch方法將傳回false。。
while ($row = $stmt->fetch()) {
  var_dump($row);
}
```

最後，如果我們想要將所有記錄直接儲存在一個變數中，我們也可以使用 `fetchAll` 方法：

```json
$results = $stmt->fetchAll();
```
