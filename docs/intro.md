---
sidebar_position: 1
slug: /
---

# 介紹

PHP 是一種用於建立動態網站的程式（编程）語言。

PHP 是一個遞歸縮寫詞，它的全名為：`PHP：Hypertext Preprocessor`。

## 安裝

根據我們使用的系統，我們會選擇不同的 PHP 安裝方式。

:::note 注意
以下提到的應用程式例如 MAMP、XAMPP， （WAMP 除外）都是跨平台的。 我們只是根據不同的作業系統來介紹最常見的用法。 當然，安裝適合您的應用，只要它能運作就可以…
:::

### Windows 作業（操作）系統

Windows 作業系統下最常用的開發環境應用程式是 Wamp。
Wamp 是一個縮寫詞，代表將安裝的所有應用程式以及相關作業系統：

-   **W**indows（作業系統）
-   **A**pache（網頁伺服器）
-   **M**ySQL（資料庫管理系統，或稱 DBMS）
-   **P**HP（使用的程式語言）

### MAC 作業（操作）系統

对于 MAC，我們將遵循與 Windows 使用<font color="green"> **MAMP** </font>相同的原則。

### Linux 作業（操作）系統

在 Linux 上，有各种不同的选择，但能够很容易地安装 <font color="green"> **XAMPP** </font> 应用程序。

### Docker

為了避免使用 Wamp、MAMP 或 XAMPP 等應用程式的混淆，我們也可以使用 Docker 將開發環境**容器化**（無論作業系統是什麼，只需在電腦上安裝 Docker 就可以）。

可以使用的基礎鏡像是 `php:apache`，但在這種情況下，我們可能需要為資料庫建立另一個容器。

然而，容器的優勢不僅在於能夠選擇版本（例如，7 或 8 版本），還在於可以將其轉換 VSCode 中的真正的**開發容器**或 <font color="green"> **Dev container** </font>。甚至可以建立自己的<font color="green"> 客製化（定制）鏡像</font>，其中包含需要的相關配置。

## 操作步骤

一旦在系统上安装了 PHP，我們可以使用以下命令轻松检查其版本：

```bash
php -v
```

将产生类似于如下输出：

```bash
PHP 8.2.1 (cli) (built: Jan 11 2023 07:25:22) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.2.1, Copyright (c) Zend Technologies
    with Xdebug v3.2.0, Copyright (c) 2002-2022, by Derick Rethans
```

:::caution 注意
當然，上述輸出可能會根據 PHP 的版本和其他因素而有所不同，所以有略微不同的細節是正常的。
:::

### CLI

上述命令列輸出是否意味著我可以使用它來做網站？ 答案是可以的，但不一定是我們想要的方式。

這裡使用的命令在命令列介面 **CLI**（CLI，Command-Line Interface）下執行 PHP，因此適用於以**腳本(script)**方式使用 PHP 的情境。

例如，在開發 Symfony 應用程式或使用 Composer 時，通常會使用此方式。 在這種情況下，PHP 用於執行腳本，甚至可以從命令列啟動本機伺服器。 但這並不意味著您在此時使用了像 Apache 這樣的 Web 伺服器。

:::info 提示
如果我們正在開發 Symfony 應用程序，我們將專門使用命令列來進行開發。 不再需要使用 WAMP 等應用。

然而，這僅適用於開發階段。 如果我們要將應用程式部署到生產環境中，需要使用像 Apache 或 Nginx 這樣的伺服器來接收和處理請求。
:::

### 使用 Web 伺服器

如果我們使用 PHP 外部 Web 伺服器（例如 Wamp 或 MAMP），那麼將存在一個連接 Web 伺服器和 PHP 之間的**橋樑**。
以 Docker 為例，當我們使用 `php:apache` 映像時，PHP 透過 Apache 的 `mod_php` **模組**運作。 因此，Apache Web 伺服器**接收**傳入的請求並將其傳遞給 PHP 來執行腳本。
使用 Wamp 伺服器時，我們將有兩個不同的環境：命令列環境和與 Apache 運行環境。

## 配置

PHP 的配置通过 `ini` 格式的配置文件来进行。这种格式允许以`键值对(clé=value)`的方式声明参数，同时还允许对某些参数进行**分组**：

```bash
[curl] <-- 關於cURL的內容：
; CURLOPT_CAINFO選項的預設值。. This is required to be an
; absolute path.
;curl.cainfo =
```

### CLI 模式

要查找 PHP 載入的設定文件，可以使用以下命令列：

```bash
php –ini
```

輸出結果：

```bash
Configuration File (php.ini) Path: /usr/local/etc/php
Loaded Configuration File:         /usr/local/etc/php/php.ini
Scan for additional .ini files in: /usr/local/etc/php/conf.d
Additional .ini files parsed:      /usr/local/etc/php/conf.d/docker-php-ext-intl.ini,
/usr/local/etc/php/conf.d/docker-php-ext-pdo_mysql.ini,
/usr/local/etc/php/conf.d/docker-php-ext-sodium.ini,
/usr/local/etc/php/conf.d/docker-php-ext-zip.ini,
/usr/local/etc/php/conf.d/xdebug.ini
```

首先，我們可以看到 PHP 的主要**設定檔（配置文件）**是 `php.ini`，它包含了大多數設定參數。`

此外，我們可能會找到其他與擴充功能相關的文件，例如最後提到的 xdebug，用於協助偵錯應用程式（调试应用程序）。

### Web 伺服器模式

正如我們之前所了解的，PHP 可以在兩種不同的環境中運作：CLI（命令列介面）和 Web 伺服器。

在 Web 伺服器環境中，PHP 載入的設定檔可能與命令列模式下的設定檔不同。

例如，在 Wamp 伺服器的安裝中，我們可以在 PHP 資料夾`（C:\wamp64\bin\php\php[version]\）`中找到一個`php.ini`檔案（CLI）以及一個`phpForApache.ini`檔案。

要驗證在顯示 Web 頁面時載入了哪個文件，我們可以使用 SPL（Standard PHP Library）的 `phpinfo()` 方法。
這個操作的結果會在頁面上顯示與 Web 伺服器一起運行的 PHP 配置的所有資訊。

對於 Wamp 伺服器來說，我們會看到像 `C:\wamp64\bin\apache\conf\php.ini` 之類的內容在"Loaded Configuration File"行中。 但是，如果我們到指定的資料夾並查看 `php.ini` 文件，會發現它實際上是一個鏈接，指向了位於 PHP 安裝資料夾中的 `phpForApache.ini` 文件。

### 修改配置

我們可以在 `php.ini` 檔案中修改許多設置，例如上傳的檔案的最大值、PHP 擴展的存放目錄、擴充的啟用/停用等等。

:::info Web 伺服器
如果我們修改了 PHP 的設定並且正在使用 Apache，那麼需要重新啟動 Web 伺服器以重新載入設定。
:::
