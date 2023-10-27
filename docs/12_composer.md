---
sidebar_position: 12
slug: composer
---

# Composer

Composer 是用於 PHP 的**依賴關係管理**工具。

## 依賴關係管理

在編寫應用程式時，我們經常需要一些輔助功能來實現最終產品。

例如，我可能需要發送電子郵件，但不想花費 25 天來完全實現一個電子郵件發送模組，或者我想為用戶上傳圖像，但不想花費數十天來設計和實現圖像處理模組。 類似的例子還有很多。

在這種情況下，我們可以依賴社群開發的模組（模塊），並將它們整合到我們的應用程式中以供使用。

類似的原則也適用於許多其他程式語言，例如[Java](https://central.sonatype.com/?smo=true)、[C#](https://www.nuget.org/packages)、[Rust](https://crates.io/)、[JavaScript](https://www.npmjs.com/)等。

這些模組中的絕大多數，就像應用**程式套件（程序包）**一樣，是由開源社群的開發人員開發和維護的，它們通常託管在 GitHub、Gitlab 或 Bitbucket 等平台上。

然後，它們會發佈到**儲存庫**，這些儲存庫列出了給定語言的所有現有套件。

對於 Composer，您可以在[Packagist](https://packagist.org/)網站上找到這些套件。 這是 PHP 的主要套件儲存庫。

## 安裝

### 適用於所有系統

要下載和安裝 Composer，您可以造訪[專門的網頁](https://getcomposer.org/download/)。 透過命令列進行安裝非常快速且易於使用。

:::tip 提示

為了更快速地安裝並且能夠從機器的任何位置（使用命令列）使用 Composer，您可以使用 `--install-dir` 選項來直接指定一個包含在 `PATH` 中的目錄，以及 `--filename` 選項來直接命名可執行文件 `composer` ，而不是 `composer.phar` ：

```bash
php composer-setup.php --install-dir /usr/bin --filename composer
```

:::

### Windows 系統

對於 Windows，[網站](https://getcomposer.org/doc/00-intro.md#installation-windows)上甚至提供了一個可執行的安裝程式。

## 啟動

安裝完成後，我們可以使用命令列來執行 `Composer`，只需輸入 `composer [選項] [參數]`。

例如，若要驗證安裝是否成功，您可以使用下列命令來顯示版本：

```bash
composer --version
```

我們也可以執行 `composer list` 或簡單地執行 `composer` 以顯示所有可用的命令。

### 升級

要升級 Composer 工具本身，命令也非常簡單：

```bash
composer self-update
```

## 使用 Composer 的項目

### 不僅僅是依賴管理器

現在，我們將使用 Composer 初始化一個項目，並看到它不僅僅是一個依賴管理工具。

如果我們進入一個空資料夾並執行 `composer init` 命令，然後助手會開始向我們提問：

### 包的名稱

包名稱的格式應為 `<vendor>/<name>`。

-   `vendor` 部分對應包的發布者或組織。例如，我們會發現以 `symfony` 為 vendor 的軟體包多得數不清，因為在成為框架之前，Symfony 首先是一套可重複使用的 PHP 元件（組件）...

-   `name` 部分要容易理解得多：這是我們的套件的名稱。

例如，我將使用 `ld-web/php`。

:::note 注意
在其他管理器中，例如 NPM，也存在類似的操作，只不過 vendor 會用 "@" 符號作為前綴（例如，@angular/cli）。
:::
