---
sidebar_position: 15
slug: implementation
---

# 實現

有許多方法可以實現 REST API。 我們的目標是遵循 REST 的原則。 我們的文件組織或架構完全是自由的。

## 格式和編碼

如前所述，我們希望向客戶端傳回 JSON，使用 UTF-8 編碼。

因此，我們首要任務是將 `Content-Type` 和 `charset` 標頭加入我們的回應中：

```json
header('Content-type: application/json; charset=UTF-8');
```

## CORS

接下來，我們可以新增 `Access-Control-Allow-Origin` 標頭，以避免 CORS 問題：

```json
// 或您希望授權的任何其他網域
header('Access-Control-Allow-Origin: http://localhost:5500');
```

## 資源識別

我們要檢索的是客戶端請求的 URI。

使用 PHP 和超全域數組 `$_SERVER`，我們可以檢索 `REQUEST_URI` 關鍵字：

```json
$uri = $_SERVER['REQUEST_URI'];
```

## 辨識動詞

與用於識別資源的 URI 一樣，我們也要識別客戶端用於確定要執行的操作的動詞。

在同一個超全域 `$_SERVER` 陣列中，可以使用 `REQUEST_METHOD` ：

```json
$httpMethod = $_SERVER['REQUEST_METHOD'];
```

:::note 關於動詞的回顧
我們希望支援的 HTTP 動詞有 ：

-   GET
-   POST
-   PUT
-   PATCH
-   DELETE

:::

## 路由請求

現在我們有了 URI 和 HTTP 方法，需要執行適當的操作。

我們可以考慮使用 `PDO` 實例來執行資料庫查詢。 在此，我們將重點放在要執行的操作上。

例如，如果我們使用 `GET` 方法存取 URI `/api/products`，那麼我們將傳回 JSON 格式的產品清單。

然後，我們需要決定要包含哪些額外資訊（HATEOAS）：分頁？ 連結到其他頁面的連結？ 等等...

如果我們使用相同的 URI `/api/products`，但使用的是 `POS`T 方法，那麼我們就會期待不同的結果：在資料庫中插入一個新產品。

### 從請求正文中獲取數據

接收 API 呼叫的特別之處在於，它不是來自 "傳統 "表單。 請求不是由簡單的表單觸發的。

因此，我們無法在超全域 `$_POST` 陣列中找到任何內容。 相反，如果我們要檢索 JSON 格式的數據，我們將使用 PHP 的輸入流：`php://input`.

```json
$data = json_decode(file_get_contents('php://input'), true);
```

此方法適用於任何寫入作業（POST/PUT/PATCH），當然 DELETE 除外，因為 DELETE 只需要資源 URI，而不需要要求正文中的任何資料。

## PUT 和 PATCH 的問題

`PUT` 方法用於將資源的表示**替換**為**新的**表示。

而 `PATCH` 方法則是後來建立的，用於對資源進行**部分**更新。

因此，如果我們想遵循 HTTP 動詞，假設我們有一個 `/api/products` 資源，其中包含以下欄位：

```json
id: integer
name: string
basePrice: float
description: text
```

例如，如果我想檢索 ID 2 產品的表示，我可以使用產品 **URI** 向我的 API 發送以下 HTTP 請求：

```json
GET /api/products/2 HTTP/1.1
```

我可能會得到這樣的回答:

```json
HTTP/1.1 200 OK

{
  "uri": "/api/products/2",
  "id": 2,
  "name": "Product name",
  "basePrice": 65.50,
  "description": "might jack earlier main separate within determine safety idea"
}
```

但在客戶端，如果我想修改它，並且仍然尊重 HTTP 動詞，我可以使用 `PUT` 方法，但我必須提供一個**新的產品表示法**：

```json
PUT /api/products/2 HTTP/1.1

{
  "name": "MY NEW NAME",
  "basePrice": 65.50,
  "description": "might jack earlier main separate within determine safety idea"
}
```

在這裡，我只想更改名稱。 但我需要提供所有數據，否則 `PUT` 方法會將未提供的欄位替換為空值 `null`。

這就是 `PATCH` 方法的用處：它允許對資源進行**部分修改**：

```json
PATCH /api/products/2 HTTP/1.1

{
  "name": "MY NEW NAME"
}
```

:::caution 注意
您有責任嚴格遵守 HTTP PUT 和 PATCH 動詞。 實際上，許多系統都將 `PUT` 方法當作 `PATCH` 方法使用。 例如，在與 Symfony 一起使用的 [API Platform](https://api-platform.com/docs/distribution/) 中，在很長一段時間裡，你可以使用 `PUT` 方法，但實際上它對資源進行了部分修改，就像 `PATCH`（[原碼](https://github.com/api-platform/core/issues/4344#issuecomment-873418011)）一樣。

在第 3 版中，這種行為似乎得到了[糾正](https://github.com/api-platform/core/pull/4996)。

:::

## 錯誤處理

當客戶端呼叫我們的 API 時，可能會出現錯誤。

我們希望盡可能向客戶端傳回相關的回應，其中包含足夠的訊息，以使客戶端能夠與 API 進行有效的互動。 讓我們來看看一些錯誤範例以及相關的回應程式碼。

在 PHP 中，要設定(設置)回應代碼，我們將使用 `http_response_code` 方法。

### 400 Bad Request

如果客戶端在其請求的主體中發送了資源的格式錯誤表示，那麼我們可以傳回一個 `400 Bad Request` 錯誤。

### 401 Unauthorized

用戶端必須提供身份驗證資訊才能存取此終端。

### 403 Forbidden

客戶端已經提供了身份驗證訊息，但沒有權限存取資源。

### 404 Not Found

未找到指定的資源。

### 405 Method Not Allowed

客戶端使用的 HTTP 動詞在此資源上未被允許。

### 422 Unprocessable Content

如果在客戶端請求的主體中，表示看起來是正確的，但在內容驗證時發生錯誤，則可以傳回 `422 Unprocessable Content` 錯誤。

任何其他未預料到的錯誤
可能會發生其他錯誤。 在這種情況下，這更多是一個伺服器端錯誤（`5xx`）。

為了避免傳回意外的內容，並更多地控制我們向客戶端公開的內容，可以註冊一個全域異常處理程序，它將自動觸發。 我們將註冊一個匿名函數，該函數將接收一個``Throwable` 元素，並將回應代碼設為 500，同時包括我們選擇的資訊：

```json
set_exception_handler(function (Throwable $e) {
  http_response_code(500);
  echo json_encode([
    'error' => 'Une erreur est survenue',
    'code' => $e->getCode(),
    'message' => $e->getMessage(),
    'file' => $e->getFile(),
    'line' => $e->getLine()
  ]);
});
```

在這裡，我們決定返回有關錯誤的大量信息，但我們當然可以只返回代碼和錯誤信息，以避免文件路徑、行號等信息...
