---
sidebar_position: 11
slug: upload
---

# 使用 PHP 上傳文件

## 簡介

使用 PHP 上傳檔案時，我們將使用表單。

在表單中，可以包含一個檔案類型 `file` 的 `input` 輸入框。

```html
<form method="POST">
    <input type="file" name="myFile" />
    <input type="submit" value="Envoyer" />
</form>
```

關於這個表單，都包含什麼呢？

-   它使用 POST 方法。
-   它包含一個文件上傳字段。
-   表單的目標是文件本身。

## 除了$_GET和$\_POST，還有一個名為$\_FILES 的超級全域陣列。

當進行檔案上傳時，檔案相關的資訊會被儲存在 `$_FILES` 陣列中。

> 執行`var_dump($_FILES)`; 並提交一個圖片檔案到該表單

你應該什麼都得不到：因為 `$_FILES` 陣列是空的。

### 關於表單內容類型和資料編碼

為了確保檔案能夠正確上傳，需要指定表單的編碼類型。

為此，我們需要在表單中新增一個新的屬性：`enctype='multipart/form-data'`。

```json
- <form method="POST">
     + <form method="POST" enctype="multipart/form-data">
        <input type="file" name="myFile" />
        <input type="submit" value="Envoyer" />
    </form>
</form>
```

:::caution 注意
請注意語法：屬性名為 enctype，位於表單標記中，其值必須是 multipart ，緊接著是破折號 /，最後是 form-data。
:::
