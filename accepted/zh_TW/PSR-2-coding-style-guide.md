Coding Style Guide
==================

這份指南繼承於 [PSR-1]（ basic coding standard ）之上並且擴展。

這份指南試圖在檢視不同作者撰寫的程式碼時減少認知分歧。本文藉由列舉共同的規則，以及撰寫 PHP 程式碼的共識來達成目的。

這裡的指南風格歸納了各種多人專案的共通之處。當很多作者共同合作多個專案，有一份專案間共通的指南是相當有幫助的。因此，這份指南的好處不在於制定的規則本身，而是在共同遵守這些規則。

文件裡的這些關鍵字 "MUST" ， "MUST NOT" ， "REQUIRED" ， "SHALL" ， "SHALL NOT" ， "SHOULD" ，
 "SHOULD NOT" ， "RECOMMENDED" ， "MAY" ， 和 "OPTIONAL" 的解釋在 [RFC 2119]

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-1]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md


1. 概觀
-----------

- 程式碼 MUST 遵守 "coding style guide" PSR [[PSR-1]].

- 程式碼 MUST 以 4 個空格，而不是用 tabs 當作縮排。

- MUST NOT 硬性規定單行長度；單行字元數 MUST 限制在最多 120 字元左右；每行字元數 SHOULD 在 80 字元或更少。

- 在 `namespace` 宣告後 MUST 空一行，在全部 `use` 宣告完後 MUST 空一行。

- 類別宣告的左大括號 MUST 放在類別名稱的下一行，右大括號 MUST 放在類別內容最後單獨一行。

- 方法宣告的左大括號 MUST 放在方法名稱的下一行，右大括號 MUST 放在方法內容最後單獨一行。

- 所有屬性和方法宣告 MUST 加上可見性（ Visibility ）； `abstract` 和 `final` MUST 寫在可見性之前； `static` MUST 寫在可見性之後。
  
- 控制結構的關鍵字之後 MUST 空一格；但方法和函數呼叫後 MUST NOT 空格。

- 控制結構的左大括號 MUST 放在同一行，右大括號 MUST 放在內容最後單獨一行。

- 控制結構的左小括號之後 MUST NOT 空格，右小括號之前也 MUST NOT 空格。

### 1.1. 範例

下面的範例含括了一些規則，可作為概覽：

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
```

2. 一般準則
----------

### 2.1 基本程式碼標準

程式碼 MUST 遵守 [PSR-1] 裡的規則。

### 2.2 PHP 文件擋案

所有的 PHP 文件擋案 MUST 使用 Unix LF (linefeed) 做每行結尾。

所有的 PHP 文件擋案 MUST 在檔案結尾空一行。

在文件內容只有 PHP 的檔案 MUST 省略 PHP 結尾標簽 `?>`。

### 2.3. 行

MUST NOT 硬性規定每行長度。

單行字元數 MUST 最多 120 字元左右；自動格式檢查 MUST 提出警告，但是 MUST NOT 發出錯誤。

單行字元數 SHOULD NOT 超過 80 字元；超過的時候 SHOULD 拆分成不超過 80 字元的數行。

非空白的單行 MUST NOT 在最後留有空白。

空行 MAY 可以用來增加可讀性，以及區分出相關程式碼邏輯。

MUST NOT 在一行內寫超過一個陳述式。

### 2.4. 縮排

程式碼 MUST 使用 4 個空格做縮排， MUST NOT 使用 tabs 做縮排。

> 提醒：只使用空格，而不和 tabs 混用，可以避免在作版本比較（ diff ）、補丁（ patched ）、
> 歷史版本（ history ）和注釋（ annotations ）時產生問題。只使用空格也可以讓行間對齊時，
> 插入子縮排的表現更好。

### 2.5. 關鍵字和 True / False / Null

PHP [關鍵字] MUST 使用小寫。

PHP 常數 `true` ， `false` ，和 `null` MUST 小寫。

[關鍵字]: http://php.net/manual/en/reserved.keywords.php



3. 命名空間（ Namespace ）和 Use 宣告
---------------------------------

如果有命名空間，在 `namespace` 宣告下面 MUST 空一行。

如果有使用 Use，所有的 `use` 宣告 MUST 在 `namespace` 在宣告之後。

每個 Use 宣告 MUST 用一個 `use` 關鍵字。

在所有的 `use` 宣告完後，下面 MUST 空一行。

範例：

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... 其餘的 PHP 程式碼 ...

```


4. "Class" ，屬性和方法
-----------------------------------

這裡的 "class" 是指所有的類別（ classes ），介面（ interfaces ），和特性（ traits ）。

### 4.1. 繼承和實作

`extends` 和 `implements` 關鍵字 MUST 跟 class 名稱宣告在同一行。

class 的 左大括號 MUST 單獨成行； class 的右大括號 MUST 在宣告內容之後單獨成行。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // 常數，屬性，方法
}
```

`implements` 後的介面名稱列表 MAY 分成很多行，每行縮排一次。當使用分行的時候，第一個介面名稱 MUST 寫在下一行，每一行 MUST 只有一個介面名稱。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // 常數，屬性，方法
}
```

### 4.2. 屬性

所有屬性宣告 MUST 加上可見性（ Visibility ）。

MUST NOT 使用 `var` 關鍵字宣告屬性。

每個宣告 MUST NOT 多於一個屬性。

屬性名稱前 SHOULD NOT 使用單一底線前綴來表示 protected 或 private 可見性。

一個屬性宣告看起來如下：

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 4.3. 方法

所有方法都 MUST 宣告可視性。

方法名稱前 SHOULD NOT 使用單一底線前綴來表示 protected 或 private 可見性。

在方法名稱之後 MUST NOT 空格。左大括號 MUST 單獨成行，右大括號 MUST 在方法內容之後單獨成行。在左小括號之後 MUST NOT 空格，在右小括號之前也 MUST NOT 空格。

一個方法宣告看起來如下，注意小括號，逗號，空格和大括號的用法：

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // 方法內容
    }
}
```    

### 4.4. 方法參數

每個方法參數的逗號前 MUST NOT 空格，並且 MUST 在逗號後面空一格。

有預設值的參數 MUST 排在參數列表的後面。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // 方法內容
    }
}
```

參數列表 MAY 分成多行，每一行前面要縮排一次。如果要分行的話，第一個參數 MUST 寫在方法名稱的下一行，並且每一行 MUST 只能有一個參數。

當參數列表分成多行，右小括號和左大括號 MUST 一同放在單獨一行，並且用一個空格分開。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // 方法內容
    }
}
```

### 4.5. `abstract` ， `final` ， 和 `static`

如果有的話， `abstract` 和 `final` 宣告 MUST 寫在可視性前面。

如果有的話，`static` 宣告 MUST 寫在可視性後面。

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // 方法內容
    }
}
```

### 4.6. 方法和函式呼叫

呼叫方法或函示時，方法或函示名稱和左小括號之間 MUST NOT 空一格，左小括號之後 MUST NOT 空格，右小括號之前 MUST NOT 空格。參數列表的每個逗號前 MUST NOT 空格，但逗號後 MUST 空一格。

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

參數列表 MAY 分列成數行，每一行前面要縮排一次。要如此表示的時候，第一個參數 MUST 放到下一行，並且每一行 MUST 只能有一個參數。

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

5. 控制結構
---------------------

下面是通用的控制結構寫作風格：

- 在控制結構關鍵字後 MUST 空一格
- 在左小括號後 MUST NOT 空格
- 在右小括號前 MUST NOT 空格
- 右小括號 MUST 和左大括號之間空一格
- 控制結構內容 MUST 縮排一次
- 右大括號 MUST 放在結構內容後下一行

每個控制結構內容 MUST 以大括號包圍住。這會讓結構外觀標準化，以及減少在新增一行時發生錯誤的機率。


### 5.1. `if` ， `elseif` ， `else`

一個 `if` 陳述語法看起來結構如下。注意小括號，空格和大括號的位置； `else` 和 `elseif` 要和上一個的右大括號放在同一行。

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

SHOULD 使用關鍵字 `elseif` 而不是 `else if` ，讓所有控制結構關鍵字看起來是一個單字。


### 5.2. `switch` ， `case`

一個 `switch` 陳述語法看起來結構如下。注意小括號，空格和大括號的位置； `case` 陳述句 和 `switch` 之間 MUST 縮排一次， `break` 關鍵字（或其他結束陳述語法的關鍵字） MUST 和 `case` 的內容縮排一致。當刻意想要非空白的 `case` 內容陳述語法執行後繼續向下時， MUST 加上像是 `// no break` 的註解。 

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```


### 5.3. `while` ， `do while`

一個 `while` 陳述語法看起來結構如下。注意小括號，空格和大括號的位置。

```php
<?php
while ($expr) {
    // structure body
}
```

很相似的， `do while` 陳述語法看起來結構如下。注意小括號，空格和大括號的位置。

```php
<?php
do {
    // structure body;
} while ($expr);
```

### 5.4. `for`

`for` 陳述語法看起來結構如下。注意小括號，空格和大括號的位置。

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 5.5. `foreach`
    
`foreach` 陳述語法看起來結構如下。注意小括號，空格和大括號的位置。

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6. `try` ， `catch`

`try catch` 陳述語法看起來結構如下。注意小括號，空格和大括號的位置。

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

6. 閉包
-----------

閉包宣告在 `function` 關鍵字後 MUST 空一格，並在 `use` 關鍵字前後各空一格。

左大括號 MUST 放在同一行，右大括號 MUST 放在結構內容後下一行。

在參數列表或變數列表的左小括號後 MUST NOT 空格，並且在參數列表或變數列表的右小括號之前也 MUST NOT 空格。

參數列表和變數列表中，每個逗號前 MUST NOT 空格，但每個逗號後 MUST 空一格。

有預設值的參數 MUST 作為參數列表的最後一個。

閉合宣告會如下所示。注意小括號，逗號，空格和大括號的位置：

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

參數列表和變數列表 MAY 分成很多行，每行縮排一次。當使用分行的時候，列表第一項 MUST 放在方法名稱的下一行，每行 MUST 只有一個參數或變數。

分行參數結構的最後一行，右小括號和左大括號 MUST 一起單獨放在一行，兩者中間空一行。

下面是函式結構的範例，有參數和無參樹以及分行參數結構。

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

注意格式規則也適用在函數或方法呼叫時，作為參數傳入的閉合函數。

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```


7. 結論
--------------

這份指南有意的略去了很多寫作風格元素和實務。包含但不僅限於以下：

- 全域變數和全域常數宣告

- 函數宣告

- 運算子和設定值

- 行間對齊

- 註解和文件區塊

- Class 名稱前綴和後綴

- 最佳實務

未來 MAY 推薦之後修改並且擴展這份指南，加入這些或是其他的寫作風格元素和實務。


Appendix A. Survey
------------------

In writing this style guide, the group took a survey of member projects to
determine common practices.  The survey is retained herein for posterity.

### A.1. Survey Data

    url,http://www.horde.org/apps/horde/docs/CODING_STANDARDS,http://pear.php.net/manual/en/standards.php,http://solarphp.com/manual/appendix-standards.style,http://framework.zend.com/manual/en/coding-standard.html,http://symfony.com/doc/2.0/contributing/code/standards.html,http://www.ppi.io/docs/coding-standards.html,https://github.com/ezsystems/ezp-next/wiki/codingstandards,http://book.cakephp.org/2.0/en/contributing/cakephp-coding-conventions.html,https://github.com/UnionOfRAD/lithium/wiki/Spec%3A-Coding,http://drupal.org/coding-standards,http://code.google.com/p/sabredav/,http://area51.phpbb.com/docs/31x/coding-guidelines.html,https://docs.google.com/a/zikula.org/document/edit?authkey=CPCU0Us&hgd=1&id=1fcqb93Sn-hR9c0mkN6m_tyWnmEvoswKBtSc0tKkZmJA,http://www.chisimba.com,n/a,https://github.com/Respect/project-info/blob/master/coding-standards-sample.php,n/a,Object Calisthenics for PHP,http://doc.nette.org/en/coding-standard,http://flow3.typo3.org,https://github.com/propelorm/Propel2/wiki/Coding-Standards,http://developer.joomla.org/coding-standards.html
    voting,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,no,no,no,?,yes,no,yes
    indent_type,4,4,4,4,4,tab,4,tab,tab,2,4,tab,4,4,4,4,4,4,tab,tab,4,tab
    line_length_limit_soft,75,75,75,75,no,85,120,120,80,80,80,no,100,80,80,?,?,120,80,120,no,150
    line_length_limit_hard,85,85,85,85,no,no,no,no,100,?,no,no,no,100,100,?,120,120,no,no,no,no
    class_names,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,lower_under,studly,lower,studly,studly,studly,studly,?,studly,studly,studly
    class_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,next,next,next,next,next,next,same,next,next
    constant_names,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper
    true_false_null,lower,lower,lower,lower,lower,lower,lower,lower,lower,upper,lower,lower,lower,upper,lower,lower,lower,lower,lower,upper,lower,lower
    method_names,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,lower_under,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel
    method_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,same,next,next,next,next,next,same,next,next
    control_brace_line,same,same,same,same,same,same,next,same,same,same,same,next,same,same,next,same,same,same,same,same,same,next
    control_space_after,yes,yes,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes
    always_use_control_braces,yes,yes,yes,yes,yes,yes,no,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes
    else_elseif_line,same,same,same,same,same,same,next,same,same,next,same,next,same,next,next,same,same,same,same,same,same,next
    case_break_indent_from_switch,0/1,0/1,0/1,1/2,1/2,1/2,1/2,1/1,1/1,1/2,1/2,1/1,1/2,1/2,1/2,1/2,1/2,1/2,0/1,1/1,1/2,1/2
    function_space_after,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no
    closing_php_tag_required,no,no,no,no,no,no,no,no,yes,no,no,no,no,yes,no,no,no,no,no,yes,no,no
    line_endings,LF,LF,LF,LF,LF,LF,LF,LF,?,LF,?,LF,LF,LF,LF,?,,LF,?,LF,LF,LF
    static_or_visibility_first,static,?,static,either,either,either,visibility,visibility,visibility,either,static,either,?,visibility,?,?,either,either,visibility,visibility,static,?
    control_space_parens,no,no,no,no,no,no,yes,no,no,no,no,no,no,yes,?,no,no,no,no,no,no,no
    blank_line_after_php,no,no,no,no,yes,no,no,no,no,yes,yes,no,no,yes,?,yes,yes,no,yes,no,yes,no
    class_method_control_brace,next/next/same,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/next,same/same/same,same/same/same,same/same/same,same/same/same,next/next/next,next/next/same,next/same/same,next/next/next,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/same,next/next/next

### A.2. Survey Legend

`indent_type`:
The type of indenting. `tab` = "Use a tab", `2` or `4` = "number of spaces"

`line_length_limit_soft`:
The "soft" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`line_length_limit_hard`:
The "hard" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`class_names`:
How classes are named. `lower` = lowercase only, `lower_under` = lowercase with underscore separators, `studly` = StudlyCase.

`class_brace_line`:
Does the opening brace for a class go on the `same` line as the class keyword, or on the `next` line after it?

`constant_names`:
How are class constants named? `upper` = Uppercase with underscore separators.

`true_false_null`:
Are the `true`, `false`, and `null` keywords spelled as all `lower` case, or all `upper` case?

`method_names`:
How are methods named? `camel` = `camelCase`, `lower_under` = lowercase with underscore separators.

`method_brace_line`:
Does the opening brace for a method go on the `same` line as the method name, or on the `next` line?

`control_brace_line`:
Does the opening brace for a control structure go on the `same` line, or on the `next` line?

`control_space_after`:
Is there a space after the control structure keyword?

`always_use_control_braces`:
Do control structures always use braces?

`else_elseif_line`:
When using `else` or `elseif`, does it go on the `same` line as the previous closing brace, or does it go on the `next` line?

`case_break_indent_from_switch`:
How many times are `case` and `break` indented from an opening `switch` statement?

`function_space_after`:
Do function calls have a space after the function name and before the opening parenthesis?

`closing_php_tag_required`:
In files containing only PHP, is the closing `?>` tag required?

`line_endings`:
What type of line ending is used?

`static_or_visibility_first`:
When declaring a method, does `static` come first, or does the visibility come first?

`control_space_parens`:
In a control structure expression, is there a space after the opening parenthesis and a space before the closing parenthesis? `yes` = `if ( $expr )`, `no` = `if ($expr)`.

`blank_line_after_php`:
Is there a blank line after the opening PHP tag?

`class_method_control_brace`:
A summary of what line the opening braces go on for classes, methods, and control structures.

### A.3. Survey Results

    indent_type:
        tab: 7
        2: 1
        4: 14
    line_length_limit_soft:
        ?: 2
        no: 3
        75: 4
        80: 6
        85: 1
        100: 1
        120: 4
        150: 1
    line_length_limit_hard:
        ?: 2
        no: 11
        85: 4
        100: 3
        120: 2
    class_names:
        ?: 1
        lower: 1
        lower_under: 1
        studly: 19
    class_brace_line:
        next: 16
        same: 6
    constant_names:
        upper: 22
    true_false_null:
        lower: 19
        upper: 3
    method_names:
        camel: 21
        lower_under: 1
    method_brace_line:
        next: 15
        same: 7
    control_brace_line:
        next: 4
        same: 18
    control_space_after:
        no: 2
        yes: 20
    always_use_control_braces:
        no: 3
        yes: 19
    else_elseif_line:
        next: 6
        same: 16
    case_break_indent_from_switch:
        0/1: 4
        1/1: 4
        1/2: 14
    function_space_after:
        no: 22
    closing_php_tag_required:
        no: 19
        yes: 3
    line_endings:
        ?: 5
        LF: 17
    static_or_visibility_first:
        ?: 5
        either: 7
        static: 4
        visibility: 6
    control_space_parens:
        ?: 1
        no: 19
        yes: 2
    blank_line_after_php:
        ?: 1
        no: 13
        yes: 8
    class_method_control_brace:
        next/next/next: 4
        next/next/same: 11
        next/same/same: 1
        same/same/same: 6
