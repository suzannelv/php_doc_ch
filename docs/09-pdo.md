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

文件告訴我們，如果連接失敗，該類別的建構函數可以引發異常。 因此，最好將物件的建立放在一個 `try/catch` 區塊中，以實現錯誤處理最小化。

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

```bash
// row = 行（包含一條記錄的資料）
// fetch 意味著"讀取"/"獲取"
$row = $stmt->fetch();
var_dump($row);
```

如果我們想逐一取得所有記錄，使用 `fetch` 方法並對其使用循環：

```bash
// while循環將在結果的最後一行後自動停止，因為fetch方法將傳回false。。
while ($row = $stmt->fetch()) {
  var_dump($row);
}
```

最後，如果我們想要將所有記錄直接儲存在一個變數中，我們也可以使用 `fetchAll` 方法：

```bash
$results = $stmt->fetchAll();
```

## 錯誤/配置

在建構中，我們已經看到 PDO 的建構函數可能引發 `PDOException`。

除了在建構時的錯誤外，我們可以配置 PDO 將其錯誤處理模式設為"異常"模式。

即在實例化物件後完成：

```bash
try {
  $pdo = new PDO($dsn, $user, $password);
+  $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
  echo $e->getMessage();
  exit;
}
```

或在物件實例化時，直接以選項的形式出現：

```bash
+$options = [
+  PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
+];

try {
+  $pdo = new PDO($dsn, $user, $password, $options);
} catch (PDOException $e) {
  echo $e->getMessage();
  exit;
}
```

## 讀取模式

### 陣列

預設情況下，`fetch` 或 `fetchAll` 方法會傳回一個混合了數值索引和結果列名稱的鍵的陣列。

如果我們想要只取得一個關聯數組，我們可以透過 PDO 的方法執行：

```bash
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
  var_dump($row);
}
```

這樣，我們可以訪問每個 `$row` 的列，透過列名來進行訪問，就像操作關聯數組一樣（例如 `$row['nom']` ）。

### 對象

我們还可以使用 `fetchObject` 方法要求 PDO 将结果转换为对象：

```bash
while ($row = $stmt->fetchObject('People')) {
  require 'people-card.php';
}
```

在這種情況下，PDO 會自動根據列名將對應的屬性賦值給具有相同名稱的類別。

然後，它會呼叫類別的建構函數。

:::note 注意

與異常格式中的錯誤模式一樣，預設讀取模式可在建立 PDO 實例時進行設定：

```bash
$options = [
  PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
+  PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC
];

//...new PDO(..., $options); ...
```

:::

## 預處理查詢

使用 PDO 實例的 `query` 方法對於簡單或靜態查詢可能很有用。 但對於動態查詢，特別是那些需要使用**來自使用者的數據**（主要是 GET/POST 參數）的查詢，我們應該使用**預處理查詢**，而不是手動建立表示查詢的字串。

當我們在 PDO 實例上使用 `query` 方法時，我們直接傳遞查詢來執行。 所有這些都只需一步即可完成。

但對於預處理查詢，流程分為兩步驟：

-   我們使用一個或多個**查詢參數**來準備一個 `Statement`（語句）。
-   我們執行該 `Statement` 並提供參數的值，以取代查詢中的參數。

:::danger SQL 注入

預處理查詢可用於防止 SQL 注入，而這種類型的攻擊在 URL 或表單中非常容易實施。

如果我們看一下下面沒有使用預處理查詢的程式碼 ：

```bash
$stmt = $pdo->query("SELECT * FROM users WHERE id=" . $_GET['id']);
```

所以問題是：我們要如何控制 `$_GET` 陣列中的內容？ 而預處理查詢不會有這個問題，因為提供給參數的值會經過過濾。

:::

### 命名參數

我們可以使用**命名參數**來準備一個查詢。 在這種情況下，我們需要在參數名稱前加上冒號 `：`

```bash
// 1 - 使用命名參數「id」來準備查詢
$stmt = $pdo->prepare("SELECT * FROM users WHERE id=:id");
// 2 - 提供參數值來執行查詢
$stmt->execute([
  'id' => $_GET['id']
]);
```

### 匿名參數

我們也可以使用匿名參數來準備查詢，使用問號（`?`）作為**佔位符**。 在這種情況下，參數沒有明確的名稱，因此在執行查詢時必須確保按照參數聲明的順序提供值。

```bash
// 1 - 準備查詢
$stmt = $pdo->prepare("SELECT * FROM users WHERE id >= ? AND username LIKE ?");
// 2 - 提供參數值來執行查詢
$stmt->execute([
  $_GET['id'],
  '%' . $_GET['username'] . '%'
]);
```

:::note 注意
在最後一個例子中，對於 `username` ，我們沒有為字串提供引號（無論是在準備階段還是在執行階段）。 PDO 會自動處理，因此不需要為字串加上引號。
:::

### 參數綁定和類型

我們也可以使用 `PDOStatement` 的專用方法來為預處理查詢的不同參數指派值，並在執行之前更精確地指定參數的類型。 `bindValue`方法：

```bash
// 1 - 準備查詢
$stmt = $pdo->prepare("SELECT * FROM users WHERE id >= ? AND username LIKE ?");
// 1 bis - 我將不同的參數綁定到不同的值。
$stmt->bindValue(1, $_GET['id'], PDO::PARAM_INT);
$stmt->bindValue(2, '%' . $_GET['username'] . '%', PDO::PARAM_STR);
// 2 - 執行查詢
$stmt->execute();
```

注意，在使用佔位符（即問號，也就是匿名參數）的情況下，我們指定參數的**位置**而不是名稱。

如果使用命名參數，我們可以使用參數的名稱而不是位置。

:::note 注意

還有另一種 `PDOStatement` 的方法叫做 `bindParam` 。

它的行為有所不同，因為它**將 PHP 變量綁定到查詢參數**。 在這種情況下，PHP 變量與查詢參數透過引用綁定在一起，甚至可以被修改。

因此，在使用這個方法時要小心潛在的副作用。 通常情況下，為了避免過度耦合程式碼和查詢，我們會更常使用 `bindValue` 方法。

:::

## 外部化配置

我們的程式碼（代碼）成功連接到資料庫，可以執行查詢、插入記錄等等...

但是連接憑證儲存在程式碼中。

-   第一個問題是，當我們將專案發佈到 Github 時，連接憑證會以明文形式儲存在程式碼中，任何查看檔案的人都可以看到它們。
-   第二個問題是，如果我想克隆項目，甚至是派生(fork)項目以參與開發並發起拉取請求，那麼要么我**必須**擁有與數據庫相同的連接配置，也就是說我必須修改包含憑證的文件，這將導致 Git 將該檔案標記為已修改。 因此，需要小心不要將其包含在提交中！

為了解決這兩個問題，我們將連接參數外部化到一個設定檔中。

然後，從 PHP 程式碼中，我們將載入這個設定檔的內容，並使用載入的值建立 PDO 實例。

### INI 文件

在 PHP 配置中，我們已經看到了 `ini` 檔案是如何運作的。

因此，我們可以以 `ini` 格式建立自己的設定(配置)文件，然後使用 `parse_ini_file` 函數將其內容載入到 PHP 陣列中。

> config/db.ini

```ini
; Mettre les valeurs réelles ci-dessous
dbname=xxxxx
host=xxxxx
port=xxxx
charset=utf8mb4
user=xxxx
password=xxxx
```

> index.php

我們使用數組重構來直接分配變量：

```bash
[
  'dbname' => $dbname,
  'host' => $host,
  'port' => $port,
  'charset' => $charset,
  'user' => $user,
  'password' => $password
] = parse_ini_file(__DIR__ . "/config/db.ini");

$dsn = "mysql:dbname=$dbname;host=$host;port=$port;charset=$charset";

$options = [
  PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
  PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC
];

try {
  $pdo = new PDO($dsn, $user, $password, $options);
} catch (PDOException $e) {
  echo $e->getMessage();
  exit;
}
```

因此，連接參數是在程式碼之外載入的。

### 從版本控制中移除 INI 文件

之後的問題是，"我們如何將參數排除版本控制？" 因為現在看來，我們似乎只是把問題轉移了！

為此，有不同的解決方案：

-   可以將 `db.ini` 檔案新增至 `.gitignore` 檔案中，這樣它就不在版本控制範圍內。但如果沒有該文件，為了避免其他開發人員在查看程式碼時確定需要建立該文件，可以在專案根目錄下的 `README.md` 文件中新增說明。

-   可以將 `db.ini` 檔案新增至 `.gitignore` 檔案中，並建立第二個文件，例如 `db.ini.template` ，其中只包含像 "xxxxx" 這樣的佔位符。 因此，這個檔案保持不變，因為唯一需要做的就是將其內容複製到 db.ini 檔案中。

-   可以將前兩種方法結合起來，提供一個模板並提供說明。

:::caution
無論哪種情況，都必須將 `db.ini` 檔案排除在版本控制之外（對於所有提到的方法都是如此）。
:::
