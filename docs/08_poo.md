---
sidebar_position: 8
slug: poo
---

# 物件導向程式設計 (面向對象編程)

物件導向程式設計（POO）代表了一個重大的程式設計範式變化。 在本章節中，我們將討論需要了解的各種概念，以便能夠設計一個以物件為中心的架構，能夠表示比簡單變量更複雜的結構。

因此，我們不僅可以繼續處理 `int`、`string`、`bool` 等資料類型，還可以處理更豐富和更複雜的類型，如`User`、`Product` 等，這些類型具有更為明確和重要意義。

## 類別

為了表示更複雜的結構，我們首先會在應用程式中定義**類別**。

:::note 注意

一個類別代表了一個在我們的應用程式中可用的**新類型**。 你可以把它看作是一個骨架、一個結構或一個模具，它代表了我們希望在應用程式中表示的一個更複雜的概念。

:::

例如，如果我們想在應用程式中處理產品類，而不是使用關聯數組來儲存鍵值對，我們可以定義一個新類型``Product`, 以便於更嚴謹地**組織**應用程序，。

這個新類型可以包含與產品相關的信息，如名稱、價格、圖像等。

隨後，任何引入 Product 類別文件的程式碼都可以實例化 `Product` 類型的物件。 因此，我們將首先討論類別的**定義**，然後是物件的**實例化**。

## 類別的定義

通常，一個類別被定義在**單獨的文件中**。 然後，我們可以在需要實例化該類別的地方使用 `require` 來引入該檔案。

:::note 注意

這與函數類似：首先定義一個函數，在另一個文件中對其進行調用，在適當的地方使用 `require` 引入該文件。

:::

在不使用 **Composer** 工具的情況下，類別的組織並不是非常重要。 例如，您可以在專案根目錄下定義一個名為 `classes` 的資料夾，然後將所有的類別放在其中。

### 命名

關於命名，建議使用 **PascalCase** 命名我們的類別。

:::info PASCALCASE/CAMELCASE/SNAKECASE/KEBABCASE

在程式設計中，有不同的命方式（變量、常量、函數、類別等）：

| 名稱       | 描述                                                         | 舉例                          |
| ---------- | ------------------------------------------------------------ | ----------------------------- |
| PascalCase | 都以大寫字母開頭，包括第一個單詞                             | `Product`, `ProductCategory ` |
| CamelCase  | 對於名稱中的每個單詞，都以大寫字母開頭，但第一個單字可以小寫 | product, `productCategory`    |
| SnakeCase  | 使用底線 `_` 分隔單字                                        | `product`，`product_category` |
| KebabCase  | 使用連字號 `-` 分隔單字                                      | `product`, `product-category` |

:::

### 語法

我們使用關鍵字 `class` 來定義一個新的類型 :

```json
class Product
{}
```

:::note 注意

類別的花括號內部部分稱為類別的**主體**。

:::

### 屬性和方法

在類別的主體中，我們可以增加新屬性。 這些屬性屬於類別。

通常，當確定類別的各種屬性時，我們可以使用動詞**有**。 例如："一個產品**有**一個名稱和一個價格" ：

```bash
class Product
{
  public string $name;
  public float $priceVatFree;
}
```

透過這個定義，我們剛剛創建了一個名為 Product 的結構化類型，它本身包含兩個屬性：`$name` 和 `$price`。

在應用程式中建立新的結構化類型的另一個好處是我們可以賦予特定的功能的**能力**。

這些能力以類**方法**的形式具現化。

例如，我們可以說 `Product` 類別具有根據傳入的稅率，從其屬性 `$price` 返回產品的含稅價格的能力：

```json
class Product
{
    public string $name;
    public float $priceVatFree;

    /**
     * 給出產品價格，包含給定的稅費
     *
     * @param float $taxRate 在 0 和 1 之間
     * @返回浮動價格（包括給定稅費）
     */
    public function getFullPrice(float $taxRate): float
    {
        if ($taxRate >= 1) {
            // 拋出錯誤（異常）
        }

        return $this->priceVatFree * (1 + $taxRate);
    }

```

:::info 資訊
在一個方法中，可以使用關鍵字$this 存取相同類別的屬性（請參閱有關類別實例的部分）。
:::

每個屬性或方法都具有可見性：`public`、`protected` 或 `private`。

### 可見性

可見性是用來向將要實例化某個類型物件的程式碼指明它可以或不可以存取什麼。

在前面我們定義的 `Product` 類別中，這兩個屬性都是 public 的。

這意味著我們可以使用以下語法直接從物件實例中存取它們：

```bash
$productName = $product->name;
```

事實上，為了遵守**封裝**原則，我們將把 `Product` 的屬性定義為私有屬性 `private`。

### 封裝

封裝是將類別的屬性設為 `private`，然後定義**存取**和**修改**這些屬性的方法，也就是所謂的**getter**和**setter**方法。

此原則的主要優點是讓類別**保持對其屬性的控制**。 我們決定如何向外部程式碼提供類別的實例的屬性。

:::note 注意

另一個好處是可以將屬性設為唯讀(只讀)，因此不需要為該屬性聲明修改方法。 由於屬性是私有的，並且只有一個公共的存取方法，所以只能取得而無法修改它。

:::

以下是按照封裝原則重新編寫的 `Product` 類別：

```bash
class Product
{
    private string $name;
    private float $priceVatFree;
    private int $quantity;
    private bool $discount;

    // Getter
    public function getName(): string
    {
        // 這裡我們可以決定始終以大寫字母返回產品名稱
        return strtoupper($this->name);
    }

    // Setter
    public function setPriceVatFree(float $price): void
    {
        // 這裡我們只在價格大於 0 時更新價格
        if ($price >= 0) {
            $this->priceVatFree = $price;
        }
    }

    // Getter
    public function getQuantity(): int
    {
        return $this->quantity;
    }

    // Setter
    public function setQuantity(int $quantity): self
    {
        $this->quantity = $quantity;

        return $this;
    }

    // Getter
    public function getDiscount(): bool
    {
        return $this->discount;
    }

    // Setter
    public function setDiscount(bool $discount): self
    {
        $this->discount = $discount;

        return $this;
    }
}
```

:::note 流暢介面（Fluent Interface）

在上述的 `setQuantity` 和 `setDiscount` 中，我們注意到回傳類型是 `self`，並且在 `setter` 的最後執行了 `return $this`。

這種方法允許將類別方法的呼叫**連結**在一起，例如：

```bash
$product = new Product();
$product
  ->setQuantity(70)
  ->setDiscount(true);

// 而不是 :
$product->setQuantity(70);
$product->setDiscount(true);
```

這個原則被稱為**流暢介面**（**Fluent Interface**）。
:::

## 類別的實例化

:::caution 注意
當我們談到「類別的外部」時，我們指的是在類別的主體之外。
:::

一旦我們定義了產品的結構，在類別的外部，我們可以**實例化**和**操作**產品。 為此，我們只需聲明一個變量，並使用所需類型的 `new` 關鍵字：

```json
index.php

require_once "classes/Product.php";
$product = new Product();
```

一旦擁有了一個類別的實例，我們就可以存取它的**公共**方法：

```json
$product->setName("Téléviseur");
echo $product->getName(); // 將顯示 "Téléviseur"

$product->setPriceVatFree(800);
echo $product->getFullPrice(0.2); // 將顯示 960
```

### 建構函數 (構造函數)

在實例化一個類別時，我們可能希望在**實例化**時初始化某些值。 為此，可以定義一個類別的**建構函數**，這個方法會在類別實例化時自動執行：

```json
class Product
{
  // ...

  public function __construct(string $nom = "Téléviseur")
  {
    $this->nom = $nom;
  }
}
```

要使用建構函數，可以像呼叫函數一樣使用帶有參數的方式來實例化對象，就像這樣：

```json
class Product
// 我的產品將被稱為 "Téléphone"，但如果我在不傳遞任何參數的情況下實例化我的產品，它將自動被稱為 "Téléviseur"。
$produit = new Product("Téléphone");
```

:::info 資訊

在 PHP 中，一個類別的建構子被稱為一個**魔術方法**，這是因為它在特定的上下文中（這裡是創建類別的物件實例，使用 `new` 關鍵字）被自動呼叫。 魔術方法的名稱總是以兩個底線(下劃線)字元(字符) `__` 開頭。

在 PHP 中還有其他可在類別中定義的魔術方法。
:::

### 類別實例

必須記住類別的**定義**與類別的**實例**之間的區別。

正如我們前面所看到的，類別的定義是首要的。 這是**設計類別的結構**、聲明屬性和方法的階段。

完成類別的定義後，我就可以使用 `new` 關鍵字建立**物件**，其類型就是這個類別。

:::note 注意
物件是一個類別的**實例**。 你可以建立任意多類型的物件（當然是在機器記憶體(內存)允許的範圍內）。
:::

### 關鍵字 `$this`

讓我們回顧一下類別的定義。

在類別的主體中，尤其是在其方法中，可以使用關鍵字 `$this`。 這個關鍵字用來引用**呼叫方法的實例**。

舉個例子，如果我重新分析 Product 類，或至少其中的一部分：

```json

<?php
class Product
{
  private string $name;
  //...

  public function getName(): ?string //" ? "表示回傳值可能為 "空(null)"。
  {
    return strtoupper($this->name);
  }

  public function setName(string $name): void
  {
    $this->name = $name;
  }

  //...
}

// 這裡在類別的主體之外，因此可以建立 `Product` 的實例。
$tele = new Product();

// 當呼叫 setName 時，在 setName 的 $this 指的是我們的 $tele
// 因此，這是在呼叫方法的實例
$tele->setName("Super écran plat");

$macbook = new Product();

// 同樣，setName 不再在 $tele 上執行，而是在 $macbook 上執行。
// 我們有 2 個不同的實例，每個實例都有自己的屬性值
$macbook->setNom("Apple Macbook");

```

## 類別常量

**常量**可以在類別中定義。 這對於集中那些不想在類別中修改的數據非常有用，這樣就可以在類別的各種方法中使用這些數據，如果常數是公共的，也可以在類別外使用。

```json

<?php
class Email
{
  private string $email;

  //...

  public function getDomain(): string
  {
    $emailParts = explode('@', $this->email);
    return $emailParts[1];
  }
}

class SpamChecker
{
  private const SPAM_DOMAINS = ['youhou.com', 'mailinator.com', 'free.fr', 'hello.net'];

  public function isSpam(Email $email): bool
  {
    return array_search($email->getDomain(), self::SPAM_DOMAINS) !== false;
  }
}
```

:::caution 注意

存取類別常數的方法與存取屬性的方法不同。 若要存取屬性，我們將使用箭頭 `->`，前面加上 `$this` 關鍵字。

要存取常數，我們在類別內部使用 `self::MA_CONSTANT`，在類別外部使用 `ClassName::MA_CONSTANT`。這是存取類別的**靜態**成員的語法。

:::

## 靜態成員

正如我們剛剛提到的常數一樣，訪問靜態成員的語法是不同的。 原因是常數是類別中的**靜態**成員。

:::tip 靜態/非靜態

要記住**靜態成員**和**非靜態成員**之間的主要差異是，**靜態成員**與**類別的實例無關**，而是**與類別本身相關聯**。

這也意味著要訪問靜態成員，我們根本不需要建立類別的實例。

因此，在靜態方法中，我們只能訪問靜態的東西（因為我們與任何實例都沒有關聯）。

:::

靜態成員可以是屬性、方法或常數（常數預設為靜態的）。

因此，我們可以使用關鍵字 `static` 將屬性或方法變成靜態的：

```bash

class Utils
{
  public static function upper(string $input): string
  {
    return strtoupper($input);
  }
}

echo Utils::upper("test"); // TEST

```

:::note 注意

靜態成員並不常被使用。 實際上，在我們的類別中，我們最常使用的是聲明常數，無論是公共的還是私有的，它們預設（默認）是靜態的。

對於方法，偶爾我們會聲明靜態方法。 如上所示的 Utils 類，主要涉及到非常通用的方法，這些方法不能專門與應用程式的某個特定方面或概念相關聯。

否則，在"非通用"類別中，靜態方法可以用來初始化某些內容並傳回類別的實例，就像建構函數一樣，但不需要使用 new 關鍵字[範例](https://github.com/symfony/http-foundation/blob/6.3/Request.php#L300)。

:::

## 異常

異常用於在物件導向程式設計中進行**錯誤處理**。

事實上，當我們設計一個功能時，我們會考慮正常的執行流程。 但是，不希望發生的情況可能會出現。 因此，我們可以將它們視為與預期行為不符的**異常**情況。

因此，我們可以調用程式碼**拋出**異常並**捕獲**它。

錯誤處理的關鍵時刻：當我們捕捉異常時，可以採取適當的措施來處理異常的發生。

### 範例

在一個名為 `Email` 的類別中，我在建構時提供要儲存為屬性的電子郵件地址，如果傳遞的值不正確（如電子郵件格式錯誤），我希望阻止類別的實例化。

這個行為有雙重用途：首先，它讓我們在物件建構的早期階段儘早偵測到錯誤。 其次，透過禁止實例化，我們也防止了呼叫該建構函式的程式碼取得不穩定狀態的類別實例（避免了建立不正確電子郵件，不健康 `Email` 實例）。

因此：

```bash
class Email
{
  private string $email;

  /**
   * 建立一個新的電子郵件實例
   *
   * @param string $email 要儲存在實例中的值
   * @throws InvalidArgumentException 如果電子郵件格式無效，拋出InvalidArgumentException
   */
  public function __construct(string $email)
  {
    $this->email = $email;

    if (!$this->isValid()) {
      throw new InvalidArgumentException("電子郵件地址的格式無效");
    }
  }

  public function isValid(): bool
  {
    return filter_var($this->email, FILTER_VALIDATE_EMAIL) !== false;
  }

  //...
}
```

在需要 `Email` 類別的實例並且可能引發異常的檔案中，可以使用以下方式 `try...catch` 區塊來處理異常：

```bash
try {
  $email = new Email($_POST['email']);
} catch (InvalidArgumentException $ex) {
  echo $ex->getMessage();
  exit;
}
```

這樣可以避免我們做類似以下的事：

```bash
$email = new Email($_POST['email']);
// 此時，我們擁有一個無效狀態的實例。
// 因此，我們必須在呼叫isValid之前考慮其他操作
// quoi que ce soit d'autre.
// 這就是問題所在：我們很可能會忘記呼叫此方法來驗證對象的缺陷，
// 從而導致無效的對象，這將導致不可控制的錯誤。

if (!$email->isValid()) {
  echo "電子郵件地址的格式無效";
  exit;
}
```

:::tip 提示
異常允許更快速、更清晰地檢測到不想要的行為。
:::
