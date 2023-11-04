---
sidebar_position: 16
slug: Requêter API
---

# 請求 API

在 PHP 應用程式中，我們可能需要自己請求 API。

在這種情況下，可以使用 [cURL](https://www.php.net/manual/en/book.curl.php) 函式庫或 Symfony 的 [HTTP Client](https://symfony.com/doc/current/http_client.html)等。

## cURL

使用 cURL 時，分為 3 個步驟：

-   定義處理程序
-   定義[請求選項](https://www.php.net/manual/en/function.curl-setopt.php)（有很多選項）
-   執行請求

完成這 3 個步驟後，我們會關閉一開始建立的處理程序，以釋放資源。

### GET 請求

```json
$client = curl_init("https://api.myapp.com/endpoint");
// 將查詢結果設定為傳回值，預設不會顯示在螢幕上
curl_setopt($client, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($client);

var_dump(json_decode($response, true));

curl_close($client);
```

### POST 請求

```json

$credentials = ['username' => 'cronin.josiah@herman.biz', 'password' => "test1234"];

$client = curl_init("https://api.myapp.com/login");

// 執行 POST 請求
curl_setopt($client, CURLOPT_POST, true);

// 設定請求標頭
curl_setopt($client, CURLOPT_HTTPHEADER, ['Content-Type: application/json']);

// 使用標識符設定請求正文中的字段
curl_setopt($client, CURLOPT_POSTFIELDS, json_encode($credentials));

// 將結果放入回傳值
curl_setopt($client, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($client);

var_dump(json_decode($response, true));

```

## Symfony HTTP Client

為了避免處理繁瑣的 cURL 的所有選項，我們可以使用專門用於發出請求的軟體包。

因此，我們擁有一個現成的物件接口，比預設的 cURL 庫更加舒適。

文件寫得非常出色 🤓，例如下面類型的範例：

```php

use Symfony\Component\HttpClient\HttpClient;

$client = HttpClient::create();
$response = $client->request(
    'GET',
    'https://api.github.com/repos/symfony/symfony-docs'
);

$statusCode = $response->getStatusCode();
// $statusCode = 200
$contentType = $response->getHeaders()['content-type'][0];
// $contentType = 'application/json'
$content = $response->getContent();
// $content = '{"id":521583, "name":"symfony-docs", ...}'
$content = $response->toArray();
// $content = ['id' => 521583, 'name' => 'symfony-docs', ...]

```
