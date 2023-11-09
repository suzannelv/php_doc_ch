---
sidebar_position: 14
slug: api_intro
---

# API - 簡介

開發 API 有助於簡化系統中不同參與者之間的資料交換。 因此，與其專門傳回 HTML 等內容，我們可以採用更中立的輸出格式，無論其性質或使用的程式語言如何， 任何客戶端都可以理解。

## 範例

假設您決定開發一個用於管理音樂庫的應用程式。 起初，您會開發一個網站，這很正常。

隨著用戶數量不斷增加，您的網站變得非常受歡迎。 現在用戶要求您提供移動版本。 您已經限制在一個響應式版本的網站, 而開發行動應用程式將迫使您從前後端都重新開始。

您也可以讓您的後端以 HTML 以外的方式公開數據，並將其變成**API**。 然後，該 API 可以與資料庫通訊並以任何類型的應用程式都能讀取的格式回應請求。

最常用的格式是**JSON（JavaScript Object Notation - JavaScript 物件表示法）**。 這是一種中立的格式，其唯一責任是表示資料。 JSON 回應中沒有 HTML、CSS 或業務邏輯，只有伺服器回傳的內容的表示。

這樣，您的網站變成了 API 的客戶端，當您開發行動應用程式時，可以讓它與相同的 API 進行通訊。 這樣，用戶可以在使用任何類型的應用程式時都能存取他們的資料。

隨後，您甚至可以繼續這個想法，開發嵌入式汽車應用程式或電視應用程式。

最後，如果某一天您決定更改後端，例如，切換到另一種程式語言，那也沒問題。 您唯一需要考慮的是為客戶（行動應用程式、Web 應用程式等）提供可以**呼叫以讀取系統資料或引發系統狀態變更**的**端點（或終端點）**。 從他們的角度來看，他們不需要知道 API 是用哪種程式語言寫的。

## API 的類型

有多種類型的 API，其中最常見的包括 REST、RPC、SOAP 和 GraphQL。

### SOAP

SOAP（Simple Object Access Protocol）是一種**協議**，用於向伺服器發出請求並在系統上執行操作。 它基於 XML。

### GraphQL

GraphQL 是一種**語言**，允許向伺服器發出請求並指定所需資料的結構。

### RPC

RPC 代表遠端過程呼叫。 這種類型的架構在多種程式語言中都有實作（包括 PHP），它允許客戶端程式碼從另一個伺服器呼叫方法，就好像這個方法是本機應用程式的一部分一樣。

### REST

REST（REpresentational State Transfer）是一種架構風格。 因此，它是我們在開發專案時可以遵循的一套原則。

總的來說，開發 REST API 將使我們能夠向任何客戶端（Web 瀏覽器、行動應用程式、嵌入式應用程式等）公開一系列在我們的系統內可執行的操作，以**改變其狀態**。 為此，我們將透過**終端點**或**端點**為客戶端提供**操作**。 可以說我們向客戶端公開**資源**。

## 基本應用程式介面 (API)

除了 SOAP、REST 等之外，我們可以自己建立 API 嗎？

當然可以。 在這種情況下，我們可以建立一個以**JSON 格式**傳回資料的 PHP 腳本：

> index.php

```bash
<?php
header('Content-type: application/json; charset=UTF-8');

// 一個包含使用者的 PHP 陣列的文件
// （關聯數組）
require_once 'data/users.php';

echo json_encode($users);
```

然後，可以使用以下命令啟動伺服器：`php -S localhost:8000`，並訪問 `URL http://localhost:8000`。

### 內容類型

由於 API 的目的是供多個不同的客戶端調用，我們不再必須返回 HTML。

默認情況下，當伺服器發送回應時，內容類型會自動設定為 HTML：`text/html`。

但考慮到我們不一定是在面向瀏覽器，我們將採用所有類型客戶端都能理解的格式：JSON。

因此，在回應中，我們將新增一個**回應標頭**，以告知客戶端我們將返回 JSON 資料：

```bash
header('Content-type: application/json; charset=UTF-8');
```

## 查看我們 API 的回應

我們的 API 位於**伺服器端**，是用 PHP 寫的。 為了測試它，我們可以使用不同格式的各種**客戶端**：

### Chrome, Firefox

我們完全可以在瀏覽器中直接測試我們的 API！ 如果我們使用命令列啟動伺服器，或透過 Apache 存取它，然後在瀏覽器中輸入 URL，我們將在螢幕上看到 JSON 格式的結果。

瀏覽器的問題在於很難精確地**配置**我們想要傳送給伺服器的請求。 我們只能輸入一個 URL 然後確認，因此無法指定我們要使用的 HTTP 方法，我們可能想要包含的請求標頭，要附加到請求的資料等等。

### Javascript

我們也可以建立一個小型客戶端應用程式：一個或多個載入 Javascript 程式碼的 HTML 文件，然後使用 Javascript 的 `fetch` 方法向我們的 API 發送請求。

> index.html

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>API Client</title>
    </head>
    <body>
        <h1>Bienvenue sur mon client !</h1>
        <p>youpi</p>
        <script type="text/javascript" src="app.js"></script>
    </body>
</html>
```

> app.js

```javascript
window.onload = async () => {
    try {
        const res = await fetch("http://127.0.0.1:8000"); // URL de l'API
        const users = await res.json();
        console.log(users);
    } catch (err) {
        console.log("Une erreur est survenue");
    }
};
```

了解更多關於 [Javascript Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 和 [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/fetch)方法的資訊。

:::caution PI FETCH？
請注意，必須明確區分正在開發的 API(可在 Web 上使用的**Web 服务**) 以及程式庫或模組可以提供給您的 API, 後者談論的是程式設計。

例如，JavaScript 的 Fetch API 提供了 `fetch()` 方法、`Response` 介面等等... 這並不表示 Fetch API 是一個 Web 服務。 這意味著在這個模組中，這些工具是可用的，我們可以使用它們來發出請求、接收回應等等...

這與開發一個類別時的想法是一樣的：如果你在類別中定義了公共方法，這就是類別的 API：類別中物件的消費者可以使用的方法。

:::

**CORS**

使用 JS 應用程序，如果嘗試使用 `fetch()` 請求我們的 API，瀏覽器將引發**CORS**錯誤。

CORS 是跨域資源共享（**C**ross **O**rigin **R**esource **S**haring）的縮寫。 為了防止兩個不同參與者之間交換資源，將設定一個限制：如果伺服器沒有明確批准不同網域的客戶端，那麼瀏覽器將阻止交換。

在這種情況下，如果我們在本機伺服器上執行我們的客戶端（HTML/JS），例如地址是 `http://localhost:5500`，而伺服器在 `http://localhost:8000` 上，那麼瀏覽器會阻止請求。

為了解決這個問題，可以在伺服器端新增一個回應頭 `Access-Control-Allow-Origin`，用於授權給客戶端（包括瀏覽器）。

因此，為了授權 `http://localhost:5500`：

```bash
header("Access-Control-Allow-Origin: http://localhost:5500");
```

如果我們想授權所有客戶 :

```bash
header("Access-Control-Allow-Origin: *");
```

### Postman

Postman 是一個用來請求 API 的應用程式。與瀏覽器類似，我們在其中輸入 URL，然後將請求傳送到伺服器。

與瀏覽器的區別在於設定方面：在像 Postman 這樣的應用程式中，我們可以完全配置要發送的請求：HTTP 方法、標頭、請求正文、請求正文格式等等...

### cURL、HTTPie..

如果無法使用圖形應用程序，也可以使用命令列工具。

cURL 適用於所有平台：

```bash
# URL de l'API
curl http://localhost:8000
```

當然，您可以使用各種選項來指定標頭、HTTP 方法等... :

```bash
curl -X POST -H "[header]" -d "[data]" [options] [URL]
```

要在控制台中格式化伺服器傳回的 JSON，可以使用 jq ：

```bash
curl http://localhost:8000 | jq
```

您也可以安裝 HTTPie，它可以註冊此類 http 命令：

```bash
# Requête POST sur http://localhost:8000
http POST :8000
```
