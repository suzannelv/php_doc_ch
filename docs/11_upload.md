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
請注意語法：屬性名為 `enctype`，位於表單標記中，其值必須是 `multipart` ，緊接著是破折號 `/`，最後是 `form-data`。
:::

[W3C 網站](https://www.w3.org/TR/html401/interact/forms.html#form-content-type)關於表單內容類型方面提供更多資訊。

重新進行測試，只需新增編碼屬性。 例如使用 `var_dump($_FILES)` 您應該能夠看到更多資訊：

```php
array (size=1)
  'myFile' =>
    array (size=5)
      'name' => string 'depardieu.png' (length=13)
      'full_path' => string 'depardieu.png' (length=13)
      'type' => string 'image/png' (length=9)
      'tmp_name' => string 'C:\wamp64\tmp\php4D27.tmp' (length=25)
      'error' => int 0
      'size' => int 106078
```

:::note 注意
如果您的 `$_FILES` 陣列為空，請前往「檔案屬性和 PHP 配置」部分。
:::

可以看到，`$_FILES` 數組是一個關聯數組，就像 `$_GET` 和 `$_POST` 一樣。 它為每個上傳欄位（字段）都有一個鍵，該鍵的名稱就是欄位的名稱（`input` 標記的 `name` 屬性）。

該值是另一個關聯數組，包含有關文件的屬性！

## 文件屬性和 PHP 配置

### 文件屬性

您要上傳的檔案會對應到$\_FILES 陣列中，並具有一些屬性：

| 名稱      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| name      | 機器上的檔案名                                               |
| full_path | 瀏覽器發送的完整路徑（自 PHP 8.1.0 起）                      |
| type      | 檔案的 MIME 類型                                             |
| tmp_name  | 建立的暫存檔案的路徑（稍後我們將查看該路徑，以確認上傳內容） |
| error     | 文件大小                                                     |

### PHP 配置

您可以在您的 PHP 設定檔案 `php.ini` 或 `phpForApache.ini` 中設定**檔案上傳的最大值**。 這是關於 `upload_max_filesize` 參數：

> 文件 : php.ini

```ini
; Maximum allowed size for uploaded files.
; http://php.net/upload-max-filesize
upload_max_filesize = 9M
```

除了這個參數，您還可以設定您的**伺服器能夠接受的 POST 資料的最大值**。 這是 `post_max_size` 參數：

> 文件 : php.ini

```ini
; Maximum size of POST data that PHP will accept.
; Its value may be 0 to disable the limit. It is ignored if POST data reading
; is disabled through enable_post_data_reading.
; http://php.net/post-max-size
post_max_size = 9M
```

### 錯誤處理和配置

在這裡，故意將這兩個參數都設置為 `9M`，即 9 兆字節，然後嘗試上傳 300 兆位元組的視訊文件。

:::caution 注意
如果上傳非常大的文件，伺服器會花一些時間來接收它們，因此如果您的腳本執行需要一些時間，這是正常的。 在 Google Chrome 中，您應該在視窗左下角看到一個帶有「Transfert en cours」標籤的百分比。
:::

如果再次查看我們的 `$_FILES` 數組，您應該使用 `var_dump` 得到以下結果：

```php
array (size=0)
  empty
```

您可能還會收到類似這樣的警告訊息：

```ini
Warning: POST Content-Length of 33400131 bytes exceeds the limit of 9437184 bytes in Unknown on line 0
```

> POST 資料的大小超過了我們配置的大小，因此 PHP 甚至沒有映射文件，它拒絕了表單提交請求！

請將 post_max_size 參數的值調整為 980M，重新啟動伺服器，然後嘗試再次上傳：

```php
array (size=1)
  'myFile' =>
    array (size=5)
      'name' => string 'WP_20160810_10_53_30_Pro.mp4' (length=28)
      'type' => string '' (length=0)
      'tmp_name' => string '' (length=0)
      'error' => int 1
      'size' => int 0
```

我們的檔案已經成功映射到了 `$_FILES` 數組中，但 `error` 鍵包含值 1。此外，它沒有 `tmp_name` 或 `type`。

錯誤代碼對應於文件上傳的最大限制已被此文件超過。 實際上，PHP 中有一個與此值對應的常量：`UPLOAD_ERR_INI_SIZE`。

您可以在[本頁](https://www.php.net/manual/en/features.file-upload.errors.php)找到錯誤代碼和相關常數的清單。

最後，將 `upload_max_filesize` 參數的值調整為 `980M` ，然後重新嘗試。 正常情況下，一切都會正常運行，錯誤代碼應為 0。

## 伺服器上的檔案保存

回顧整個過程：客戶端發送一個檔案（可以是圖像檔案），我們希望將其儲存在我們的伺服器上。

### 過渡區域

當檔案在 `$_FILES` 數組中正確映射，即沒有錯誤時，在我們希望將它保存到我們所需的位置時，它會自動儲存在臨時區域。 我們可以透過 `tmp_name` 鍵的值知道它儲存在哪裡。

:::tip 提示

想法如下：如果我們要驗證上傳的文件，我們將把已儲存在臨時資料夾中的檔案複製到我們的目標位置。
:::

### 將檔案保存在伺服器上

要取得我們的文件，我們可以檢查 `$_FILES` 數組中是否存在與我們的 `input` 名稱對應的鍵。

然後，我們可以執行我們需要的操作，如果一切正常，可以使用 `move_uploaded_file` 方法將其儲存：

```json
if (isset($_FILES['myFile'])) {
  // 將檔案放入變數中，以便讀取
  $file = $_FILES['myFile'];

  // 取得檔案名稱
  $filename = $file['name'];

  // 建立目標路徑
  $destination = __DIR__ . "/img/" . $filename;

  // 將暫存檔案移至目的地
  if (move_uploaded_file($file['tmp_name'], $destination)) {
    echo $filename . " correctement enregistré<br />";
  }
}
```

請注意，我們在使用 `move_uploaded_file` 函數時才將臨時檔案移到目標位置。 此外，在目標位置中，我們需要指定完整的檔案路徑，包括檔案名稱！

:::note 注意

如果您只想驗證檔案是否已透過 HTTP POST 方法成功上傳，但不想像 `move_uploaded_file` 那樣確認上傳和移動文件，您也可以使用 `is_uploaded_file` 函數。 在這種情況下，請記住將臨時檔案的路徑傳遞給 `is_uploaded_file`。

:::

:::note 注意
在我們的範例中，我們將檔案保存在名為'/img/'的資料夾中。 但是，此資料夾在操作之前必須存在！ `move_uploaded_file` 函數不會為我們建立資料夾。
:::

## 與資料庫關聯

在學習如何將檔案儲存在伺服器上之後，接下來是例如要儲存使用者的個人資料照片，並在需要時進行顯示。

因此，我們需要持久保存數據，以便以後能夠檢索和使用它。

至於文件本身，問題似乎已經解決：我們已經知道如何將文件儲存在伺服器上。

但我們想要檢索的資訊呢？

例如，個人資料照片。 它肯定會儲存在使用者 `users` 表中，具體欄位可能是 `profilePic` 嗎？

### 在資料庫欄位中儲存檔案名稱

需要記住的是，文件在應用程式中會有一個生命週期：

-   用戶嘗試上傳文件。 在通過大小檢查和 MIME 類型檢查（如果您想在程式碼中檢查它是否真的是圖片等）後，我們將把它實際保存到 /A/B/photo.png 資料夾中。

:::tip 提示

為了提高程式碼的條理性，我們將把初始路徑 (/A/B) 和檔案名稱 (photo.png) 分開，因為初始路徑將來可能會改變。

:::

-   因此，我們將在資料庫中保存 `photo.png`，而不是檔案的完整路徑。 如果儲存使用者照片的位置發生變化，我們可以在程式碼中進行更改，而無需更新整個資料庫。

-   之後，使用者希望顯示一個頁面，在該頁面中再次顯示照片。 因此，透過從資料庫中取得的記錄，我們可以重建照片的完整路徑。 我們將把完整路徑注入到 `img` 標籤的 `src` 屬性中！

-   最後，如果需要更改或刪除圖片，我們仍然可以重建圖片的路徑。
