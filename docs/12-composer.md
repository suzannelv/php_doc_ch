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
在其他管理器中，例如 NPM，也存在類似的操作，只不過 vendor 會用 "@" 符號作為前綴（例如@angular/cli）。
:::

### 其他資訊

其他資訊相對不太重要，特別是如果我們不打算創建和發布一個軟體包。

相反，對於問題“是否要以交互方式定義依賴關係”，以及下一個問題（“dev 依賴關係”），我們都要回答“不”。

然後，我們將透過**選擇“n”來跳過下一部分**，因為我們打算自己編寫我們的 PSR-4 自動載入配置。

最後，我們將得到一個基本的 `json` 格式文件，描述了我們專案的配置。

我們可以確認生成。

### composer.json 文件

最後，這個工具幫助我們產生了一個描述專案配置的檔案。

除了依賴關係之外，我們將在其中找到項目的名稱、描述、作者、許可證等其他資訊。

## 新增依賴

假設我們暫時想要在我們的應用程式中發送電子郵件。

我們可以在 packagist.org 網站上搜尋關鍵字 `mail`，這將傳回與我們搜尋相符的潛在軟體包清單。

我們可以看到第一個結果是 `swiftmailer/swiftmailer`，已被棄用，而推薦使用 `symfony/mailer` 軟體包。 這可以為我們提供一個很好的指示，安裝一個仍然受到維護和定期更新的軟體包。

為了安裝這個依賴，我們將使用以下命令：

```bash
composer require symfony/mailer
```

Composer 將在套件儲存庫中尋找對應的元件（組件）並將其安裝。

它還將對我們的項目進行各種修改。

### `require` 部分

在 `composer.json` 檔案中的 `require` 部分，一開始是空的，而現在包含了一個條目：

```json
{
    "require": {
        "symfony/mailer": "^6.2"
    }
}
```

Composer 將 `symfony/mailer` 元件註冊下來作為我們專案的依賴項，並指定了一個對應的版本。 實際上，這是可安裝版本的可接受範圍，我們稍後將詳細討論這一點。 目前，我們正在分析專案文件結構的變更（變化）。

:::note 注意
版本可能會根據套件的安裝日期而有所不同。
:::

### `vendor` 資料夾

此外，Composer 在我們專案的根目錄下建立了一個新的名為 `vendor` 的資料夾。

如果我們打開它，可以看到許多資料夾已經建立。 我們可以輕鬆地在 `symfony` 資料夾中找到 `mailer` 資料夾，實際上這裡包含了 `symfony/mailer` 元件的程式碼。

:::tip 但是，`vendor` 資料夾中的所有其他資料夾都有什麼作用？

如果我們查看 `mailer` 資料夾，我們會發現其中還有一個名為 `composer.json` 的檔案。 這個檔案包含了 Mailer 元件運作所需的**依賴項**。 這就是為什麼 Composer 也下載並安裝了這些其他元件的原因。

:::

舉個例子，讓我們看看 `psr/container` 元件，它位於 `vendor/psr/container` 資料夾中。

如果我們想知道它被安裝的原因，我們可以透過下面命令向 Composer 詢問。

```bash
composer depends psr/container
```

Composer 告訴我們，`symfony/service-contracts` 元件需要 `psr/container` 元件的存在。

但是，`symfony/service-contracts` 從何而來？

```bash
composer depends symfony/service-contracts
```

Composer 告訴我們 `symfony/mailer` 需要 `symfony/service-contracts`。

:::info 依賴關係樹
因此，只有在安裝軟體套件時，Composer 才會安裝一整棵**依賴關係樹**。

因此，專案的所有依賴項都可以在 `vendor` 資料夾中找到

:::

### composer.lock 文件

最後，Composer 也在專案的根目錄中建立了一個 `composer.lock` 檔案。

如果我們打開它，我們會看到一個包含數百行的 `json` 格式檔案。 它明確指出這個文件是自動產生的。

我們可以看到一個 `packages` 部分。 這個部分包含了**安裝在 `vendor` 資料夾中的套件的精確版本**。

如果我們使用 `Ctrl+F` 來尋找 `symfony/mailer` 元件，我們可以輕鬆找到其版本（6.2.5）。

但在 `composer.json` 檔案中，指定的版本是 `^6.2`。 現在讓我們來談談元件的版本。

## 版本語意化（Semver）

版本語意化，也稱為 Semver，分為三個部分 **X.Y.Z**：

-   **X**：主要版本（該版本與先前版本相比引入了重大更改，可能會刪除某些先前存在的功能）
-   **Y**：次要版本（可在次要版本中引入新功能，同時確保現有的一切繼續正常運作）
-   **Z**：補丁版本（修復錯誤和安全問題）

這使得維護軟體包的團隊可以逐步升級版本並組織發布。 通常情況下，安全補丁可能會使版本從 6.2.5 升級到 6.2.6。 而引入全新類別和功能、或重寫現有程式碼以更改其使用方式可能會導致從 6.2.5 升級到 7.0.0。

## Composer 中的版本約束

在語意化版本控制中，**如何管理**所有這些版本就成了一個問題。 這正是依賴管理器的作用。

### composer.json

在 `composer.json` 檔案中，Composer 會為專案所依賴的每個軟體包指定**版本約束**，或可接受版本的範圍。

為此，Composer 使用了一種特定的語法，其中包含具有特定含義的運算符。 再以我們的 `symfony/mailer` 為例：

| 版本約束  | 間隔           | 說明                                                                                                                                                                             |
| --------- | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `6.2.1`   | =6.2.1         | 我**明確**要 `6.2.1` 版，而不是其他版本                                                                                                                                          |
| `^6.2`    | >=6.2.0 <7.0.0 | 我**對軟體包的更改持開放態度**，因為間隔意味著我可以容納多個不同的版本。 所以要小心使用 `>=` 和 `<` 符號，這裡我們表示**排除**主要版本 `7.0.0`，因為它是**嚴格意義上的次版本**。 |
| `6.2.\* ` | >=6.2.0 <6.3.0 | 在這種情況下，我願意接受更新，但僅限於 `6.2` 以下的**小版本**。                                                                                                                  |
| `~6.2`    | >=6.2.0 <7.0.0 | 使用波浪號 `~` 時，我授權增加**最後指定的部分**（這裡是次要版本）。 因此，無論次版本是什麼，我都授權更新                                                                         |
| `~6.2.0`  | >=6.2.0 <6.3.0 | 同樣，使用波浪號 `~` 時，我允許**指定的最後一部分**增加（此處為補丁版本）                                                                                                        |
| `\* `     | \*             | 所有版本均可接受                                                                                                                                                                 |

### `composer.lock`

鑑於``composer.json` 檔案包含版本**區間**訊息，Composer 需要一種方法來記錄專案中安裝的元件的確切版本。

有人可能認為，組件的版本可以在組件的 `composer.json` 文件中找到，但實際上並不是這樣的（檢查 `symfony/mailer` 組件的 `composer.json` 文件，你會發現該組件的版本資訊並不在其中）。

因此，如果我想要與團隊合作，確保不同同事之間使用相同的套件版本可能會變得複雜。 同樣，如果新的貢獻者加入團隊，將整個 `vendor` 資料夾發送給他可能會繁瑣耗時。

相反，Composer 會自動產生並維護一個名為 `composer.lock` 的文件，其中包含了**專案中安裝的確切版本資訊**。 進一步講，實際上該版本與 GitHub 上的一個**提交**相關聯，以便**透過 Git 的角度精確找到該版本**。

:::danger 版本控制

因此，無需將 `vendor` 資料夾包含在版本控制中，因為它可以僅透過更輕量的 `composer.lock` 檔案由 Composer 重新建構。 即使它龐大且複雜，我們也不必擔心，因為 Composer 會負責管理它。

因此，`composer.json` 和 `composer.lock` 檔案將被納入版本控制，而不是 `vendor` 資料夾。

:::

## 安裝和更新依賴項

Composer 提供了一些指令來安裝、更新和刪除依賴項。

### `composer install`

當你取得一個現有項目時，`vendor` 資料夾通常不存在，這是正常的。 你只會找到 `composer.json` 和 `composer.lock` 檔案。

你可以執行以下命令來安裝依賴項：

```bash
composer install
```

:::tip 小貼士
使用 `composer install` 指令時，Composer 會直接查看是否存在 `composer.lock` 檔案。 這將使它能夠精確還原由提交的開發人員安裝的版本。
:::

### `composer require`

如前面所示，這個命令用於向專案新增新的依賴項。

:::tip 版本
當然，你可以告訴 Composer 你想要的版本約束：

```bash
composer require "symfony/mailer:6.1.*"
```

:::

:::caution `require` 和 `install`

`require` 和 `install` 之間的差異要注意，require 用來新增新的依賴項，而 install 則用於使用 composer 檔案下載和安裝依賴項。
:::

### `composer update`

如其名稱所示，該命令用於更新依賴項，同時遵守 `composer.json` 檔案中為每個依賴項指定的版本區間。

當然，你可以透過加入套件的名稱來限制要更新的套件：

```bash
composer update symfony/mailer
```

:::tip 別名
存在一個名為 `composer upgrade` 的別名，它執行完全相同的操作。
:::

### `composer remove`

刪除軟體包（當然是在 `remove` 後面填寫其名稱）。

## 开发依赖项

在 `composer.json` 檔案中，您也可以找到一個名為 `require-dev` 的部分。

這個部分列出了**開發依賴項**，也就是在開發階段所需的一切，但不需要在生產環境中使用（例如，當網站部署到線上時）。

一個典型的例子是[PHPUnit](https://packagist.org/packages/phpunit/phpunit)，它是 PHP 生態系統中最常用的自動化**測試**框架。

通常情況下，在開發或測試階段需要 PHPUnit，但一旦程式碼部署到生產環境，就不再需要執行測試。 例如，在生產環境中，要安裝依賴項但不包括開發依賴項，可以執行下列命令：

```bash
composer install --no-dev
```

安裝開發依賴項 :

```bash
composer require --dev phpunit/phpunit # 或任何其他依賴項
```

## 腳本

腳本用於透過 Composer 在專案中執行命令。

例如，避免每次都重新編寫命令，可以定義一個腳本來執行 PHPUnit 測試。 您可以為命令命名，並透過執行 `composer [script-name]` 來執行它。

例如，如果您想要在 Docker 容器中執行測試：

> composer.json

```json
{
    "scripts": {
        "test-in-docker": "docker-compose exec php ./vendor/bin/simple-phpunit --testdox"
    }
}
```

然後，在專案的根目錄：

```bash
composer test-in-docker
```

## 其他

在 `composer.json` 文件中，我們可能會看到以下部分，包括：

-   `suggest`（建議）：此部分建議使用者安裝與目前套件非常相容的其他套件。 這些建議不是必需的依賴項，而是推薦。

-   `config`（配置）：此部分包含了一些 Composer 的配置元素，例如按字母順序對套件進行排序、允許第三方插件等。

-   `extra`（額外資訊）：可供插件使用的客製化訊息。

-   `conflict`（衝突）：顧名思義，此部分列出了與軟體包不相容的軟體包清單，安裝將被拒絕。

-   `autoload`（自動載入）：專案內自動載入類別的規則。

-   `autoload-dev`（開發自動載入）：在測試環境中套用的附加自動載入規則。
