Basic Coding Standard
=====================

這部分的標準制定了應該為哪些編程元素訂定標準，確保他們在共用的 PHP 程式碼間有高階的技術共通性

本文件中以 [RFC 2119][] 的關鍵字 “必須（ MUST ）”、”不可（ MUST NOT ）”、”必要（ REQUIRED ）”、”將要（ SHALL ）”、”將不（ SHALL NOT ）”
、”應該（ SHOULD ）”、“不應該（ SHOULD NOT ）”、”建議（ RECOMMENDED ）”、”可以/可能（ MAY ）” 和 “選用（ OPTIONAL ）” 來做解釋性的描述。

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md


1. 概觀
-----------

- 檔案 “必須” 只使用 `<?php` 和 `<?=` 標簽。

- 檔案 “必須” 在 PHP 程式碼裡使用沒有 BOM 的 UTF-8 。

- 一個檔案裡 “應該” *只能* 宣告 symbols（類別，函數，常數等）
  *或是* 產生 side-effects（如：產生輸出，變更 .ini 設定等），
  但是 "不應該" 兩個都做。

- 命名空間（ Namespaces ）和類別 “必須” 遵守 "autoloading" PSR: [[PSR-0] ， [PSR-4]]。

- 類別命名 “必須” 用首字大寫的駝峰式命名（ `StudlyCaps` ）。

- 類別常數命名 “必須” 全用大寫和底線.

- 類別方法命名 “必須” 用駝峰式命名（ `camelCase` ）.


2. 檔案
--------

### 2.1. PHP 標簽

PHP 程式碼 “必須” 使用長標籤 `<?php ?>` 或是 短標籤（ short-echo ）`<?= ?>` ；
“不可” 使用其他標籤。

### 2.2. 字元編碼

PHP 程式碼 “必須” 只使用沒有 BOM 的 UTF-8。

### 2.3. Side Effects

一個檔案裡 “應該” 宣告新的 symbols （類別，函數，常數等）而不導致 side effects, 或是 “應該” 執行有 side effects 的邏輯，但是 "不應該" 兩個都做。

"side effects" 指的是執行的邏輯不直接相關於宣告類別，函數，常數等，*只與引入檔案相關*。

"Side effects" 囊括但不限於以下：產生輸出，明確的使用 `require` 或 `include`，連結外部服務，修改 ini 設定，發送錯誤或是例外，修改全域或靜態變數，讀取或寫入檔案等等。

以下是同時包含了宣告和 side effects 的範例；也就是該避免如下範例：

```php
<?php
// side effect: 更改 ini 設定
ini_set('error_reporting', E_ALL);

// side effect: 引入檔案
include "file.php";

// side effect: 產生輸出
echo "<html>\n";

// 宣告
function foo()
{
    // function body
}
```

下面的範例是沒有 side effects的宣告；也就是可仿效的範例：

```php
<?php
// 宣告
function foo()
{
    // function body
}

// 條件陳述式 *不算是* side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```


3. 命名空間（ Namespace ）和類別名稱
----------------------------

命名空間和類別 “必須” 遵守 [PSR-0]。

意味著一個檔案只會有一個類別，而且至少有一層命名空間：最上層的 vendor 名稱

類別名稱 “必須” 以首字大寫的駝峰式命名（ `StudlyCaps` ）

PHP 5.3 以上的程式碼 “必須” 使用正式的命名空間。

例如：

```php
<?php
// PHP 5.3 以上版本:
namespace Vendor\Model;

class Foo
{
}
```

5.2.x 以及更之前版本的程式碼 “應該” 遵守偽命名空間（ pseudo-namespacing ）慣例，
類別名稱使用 `Vendor_` 前綴。

```php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
```

4. Class 常數，屬性，和方法
-------------------------------------------

這裡的 "class" 是指所有的類別（ classes ），介面（ interfaces ），和特性（ traits ）。

### 4.1. 常數

Class 常數命名 “必須” 全用大寫和底線。

例如：

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. 屬性

這份指南刻意避免了類似於首字大寫駝峰式命名（ `$StudlyCaps` ），駝峰式命名（ `$camelCase` ），或是底線分隔式命名 （ `$under_score` ）的屬性命名建議。

無論使用何種命名準則，在合理範圍內 “應該“ 要保持一致性。這個範圍可以是 vendor-level ， package-level ， class-level ， 或是 method-level 。

### 4.3. 方法

方法命名 “必須” 用駝峰式命名（ `camelCase()` ）。
