---
sidebar_position: 13
slug: psr
---

# PSR

PSR（**PHP Standard Recommendation**s）是由[PHP-FIG（PHP Framework Interop Group](https://www.php-fig.org/)發布的一系列 PHP 標準建議。

其目的是發佈公開的建議，以確保不同項目之間更高的兼容性，並對某些概念有共同的理解。

目前，PSR 的範圍從 `PSR-0` 到 `PSR-20` ，其中一些已經[被棄用、廢棄，或正在進行中製定](https://www.php-fig.org/psr/)。

## 程式碼風格

各種 PSR 包括關於應採用的**程式碼風格的建議**。

這樣做的目的是為了讓在多個專案中工作的開發人員更容易閱讀採用相同編寫規則的程式碼。 當然，沒有人有義務遵守這些規則，但對於開源專案來說，遵守公共標準可以吸引更多的貢獻者。

因此，我們可以找到 `PSR-1` 或 `PSR-12`，指明應遵循的規則，例如 ：

-   如果 PHP 檔案只包含 PHP，則不要用 `?>` 標籤關閉檔案。
-   在 PHP 文件末尾留一空行
-   抽象關鍵字必須出現在類別成員的可見性之後
-   類別成員的可見性是強制性的
-   等等...

要學習並記住編寫建議的所有規則是非常複雜的。 有一些工具可以自動檢查和格式化程式碼。 例如，[PHP-CS-Fixer（Coding Standards Fixer）](https://github.com/PHP-CS-Fixer/PHP-CS-Fixer)就可用於此類工作。

## 自動加載

另一個標準建議涉及類別的**自動加載**。

傳統的類別載入方法是簡單地使用 `require_once` 載入檔案。 但這很快就會變得非常繁瑣難以整理。

為了解決這個問題，我們可以使用 [`PSR-4`](https://www.php-fig.org/psr/psr-4/) 標準建議，該建議基於文件結構。

:::caution 文件結構
這意味著我們必須非常精確地組織文件，PSR-4 自動載入才能正常運作。
:::

### 命名空間

PSR-4 建議我們在不同的**命名空間（namespace）**下組織我們的類別。

例如，可以找到類似 `Symfony\Component\Mailer\Mailer` 或 `App\Shop\OrderService` 的類別。

在這裡，我們討論的類別分別是 `Mailer` 和 `OrderService` ，它們只是在不同的命名空間下。

因此，對於每個類，我們將聲明一個命名空間 ：

> OrderService

```json
<?php
namespace App\Shop;

//...

class OrderService {
  /* ... */
}
```

當在另一個文件中想要使用 OrderService 時 ：

```json
<?php
// 命名空間... (其他命名空間（如有必要）

use App\Shop\OrderService; // 包含類別

// ...

}
```

### FQCN

對於我們應用程式中的每個類，PSR-4 建議允許建立一個 **`FQCN`** 或**完全限定類別名稱(Fully Qualified Class Name)**。 這是從**根命名空間**到**類別名稱**的完整路徑。

:::note 區分類
這樣就可以區分 `Symfony\Component\Mailer` 和 `App\Service\Mailer` 這樣的類別。
:::

### 如何組織文件？

PSR-4 使用**檔案樹**。

事實上，對於每個根命名空間，PSR-4 都需要一個**對應的路徑**。 然後，對於其下的任何命名空間，PSR-4 只需遵循相應的樹形結構，直至類別名稱。

:::danger 命名和大小寫敏感性
在命名和匯入類別時，我們必須**特別**注意確保名稱區分**大小寫**（大寫和小寫）。

FQCN 必須與資料夾和最終 PHP 檔案的名稱**一一對應**。

:::

### 範例

**在專案中**

我們可以在自己的專案中聲明並使用 PSR-4 autoload。

為此，我們只需在 `composer.json` 檔案中以適當的結構聲明 `autoload` 部分即可。 例如，常用的結構（以 Symfony 為例）包括宣告一個與 `src/` 資料夾相關聯的 `App\` 根命名空間。 這樣，我們所有的類別和邏輯都將位於 `src/` 資料夾中。 我們可以自由建立新的目錄來組織子命名空間。

> composer.json

```json
{
    //...
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
    //...
}
```

然後，我們就可以在應用程式的 `src` 目錄中建立我們的類別。

:::note 應用程式中的類別
例如，我們可以建立一個 `src/Currency/Converter.php` 文件，對應的類別就是 `App\CurrencyConverter` 。
:::

**在 Symfony 元件中：Mailer**

在許多專案中，我們也會發現 PSR-4 自動載入規則。

例如，在 Symfony `Mailer` 元件中，如果我們查看 composer.json 文件 :

```json
{
    "autoload": {
        "psr-4": { "Symfony\\Component\\Mailer\\": "" }
        //...
    }
}
```

該聲明意味著 Mailer 專案根目錄下的所有類別或介面都將使用 Symfony\Component\Mailer 命名空間。

:::info 資訊
PSR-4 是 PSR-0 的後續版本，後者現已廢棄。
:::

## 介面

PSR 還可以定義通用**接口**，任何人都可以決定實作這些接口。

:::note 互通性(互操作性)
不同的開源工具都支援 PSR 定義的接口，這使得工具之間具有更強的互通性。 不僅可以使用已知的接口，因為這些接口是公開的、推薦的，而且只要實現的接口相同，就可以用一種實現方式替換另一種實現方式。

:::

讓我們以 [PSR-11](https://www.php-fig.org/psr/psr-11/) 為例：這是一個容器介面（`ContainerInterface`），如果要實作 PSR-11 的相容性，就必須實作該介面。 如推薦頁面所示，其目的是為庫和框架使用服務容器獲取應用程式中的物件和參數的方式建立一個標準。

例如，Symfony 的[依賴注入元件](https://symfony.com/doc/current/components/dependency_injection.html)在第一句中聲明：

> 依賴注入（DependencyInjection）元件實現了一個與 PSR-11 相容的服務容器，它允許你對應用程式中建構物件的方式進行標準化和集中化。

如果查看組件的程式碼，我們會發現想要的所有資訊。

## 範例：symfony/dependency-injection

讓我們來探索一下 [Symfony 的依賴注入元件](https://github.com/symfony/dependency-injection)。 它遵循 PSR-4 的自動加載，因此我們可以輕鬆瀏覽。

在 `composer.json` 檔案中，會發現對 `psr/container` 套件的依賴。 這是一個包含 PSR-11 介面的軟體包。

然後，如果我們轉到專案的根目錄，在 `ContainerInterface` 類別中，我們可以看到它**繼承**自 PSR 介面。 他們也為 PSR 介面指定了一個**別名**，因為他們想創建與 PSR 介面同名的自己的 `ContainerInterface`：

```json
namespace Symfony\Component\DependencyInjection;

use Psr\Container\ContainerInterface as PsrContainerInterface;
//...

interface ContainerInterface extends PsrContainerInterface
{
  //...
}
```

因此，我們可以找到 `get` 和 `has` 方法，以及其他方法，例如 `getParameter` 或 `set`。

最後，如果我們轉到 `Container` 類，會發現它聲明自己實作了 `ContainerInterface`。

:::caution 注意
Symfony 元件的 `Container` 類別實現的是同一元件的 `ContainerInterface` 接口，而不是 PSR 的 `ContainerInterface` 介面！ `ContainerInterface` 介面本身就繼承自 PSR 介面。
:::
PSR 中還有很多其他介面。 但基本上，我們現在知道了 "相容 PSR-XX "的庫意味著什麼。
