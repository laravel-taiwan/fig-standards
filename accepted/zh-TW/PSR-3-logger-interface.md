日誌介面
================

本文件描述日誌函式庫的通用介面。

目的在於讓函式庫能夠獲取 `Psr\Log\LoggerInterface` 物件且能夠透過一個簡單並且統一的方式來記錄日誌。有自訂需求的框架和內容管理系統 ”可以” 依據需求去擴展這個介面，但 “應該” 維持與此文件的相容性。這確保應用中使用的第三方函式庫可將日誌集中寫道應用程式日誌中。

本文件中以 [RFC 2119][] 的關鍵字 “必須（ MUST ）”、”不可（ MUST NOT ）”、”必要（ REQUIRED ）”、”將要（ SHALL ）”、”將不（ SHALL NOT ）”
、”應該（ SHOULD ）”、“不應該（ SHOULD NOT ）”、”建議（ RECOMMENDED ）”、”可以/可能（ MAY ）” 和 “選用（ OPTIONAL ）” 來做解釋性的描述。

本文件中的關鍵詞 “實作者 (implementor)” 意指在日誌相關函式庫或框架中實作 `LoggerInterface` 的開發人員。使用實現者開發出的函式庫的人皆被稱為 “用戶”。

[RFC 2119]: http://tools.ietf.org/html/rfc2119

1. 規範
-----------------

### 1.1 基礎

- `LoggerInterface` 擁有八個方法用來記錄 [RFC 5424][] 的八個層級 (debug, info, notice, warning, error, critical, alert, emergency)。

- 第 9 個方法 `log` 以日誌層級作為第一個參數。使用一個日誌層級常數來呼叫此方法 “必須” 和直接呼叫指定層級方法結果一致。以一個未定義於規範中的層級或並未被實作出來的日誌層級來呼叫此方法 “必須” 丟出 “Psr\Log\InvalidArgumentException”。用戶 “不應該” 使用自訂的日誌層級，除非你確定使用的函式庫有支援。

[RFC 5424]: http://tools.ietf.org/html/rfc5424

### 1.2 訊息

- 每個方法都接受一個字串或是帶有 `__toString()` 方法的物件作為訊息參數。實作者 ”可以” 對傳入的物件做特別的處理。如果沒有，實作者 “必須” 將其轉成字串。  

- 訊息 “可能” 含有 “可以” 被實作者從 context 陣列的數值所替換的佔位符。

	佔位符名稱 “必須” 與 context 陣列的鍵值對應。

	佔位符名稱 “必須” 使用大括號 ( `{` 和 `}` ) 包覆來作為分隔。前後大括號與佔位符之間 “不可” 有任何空格。

	佔位符名稱 ”應該” 只能由 `A-Z`, `a-z`, `0-9`, 下劃線 `_`, 和句點 `.` 組成。其他的字符作為之後佔位符規範的保留字。

	實作者 “可以” 使用佔位符來實作多種跳脫方法和日誌顯示時的轉換。因為不知道資料前後文會被怎樣呈現，所以用戶 “不應該” 預先跳脫佔位符的內容。

下面為一個佔位符替換的實作範例，僅供參考：

  ```php
  /**
   * 插入 content 數值到訊息佔位符
   */
  function interpolate($message, array $context = array())
  {
			// 用大括號包覆 context 鍵值來建立一個置換陣列
      $replace = array();
      foreach ($context as $key => $val) {
          $replace['{' . $key . '}'] = $val;
      }

			// 傳回置換後的數值內容訊息
      return strtr($message, $replace);
  }

	// 定義內含以大括弧包覆的佔位符訊息
  $message = "User {username} created";

	// 一個佔位符名稱 => 置換數值 的 context 陣列
  $context = array('username' => 'bolivar');

  // 回應 "User bolivar created"
  echo interpolate($message, $context);
  ```

### 1.3 Context

- 每個方法都接受陣列作為 context 資料。用來處理無法以字串處理的額外資訊。陣列可以包括任何東西。實作者 “必須” 確保盡其可能的對 context 資料做出各種處理。所以在 context 中的任何給定的數值 “不可” 造成任何的異常或是 php 錯誤，甚至是警告或提醒。

- 如果在 context 資料中傳入了一個 `Exception` 物件，他的鍵值 “必須” 
為 `’exception’`。記錄異常是常見的模式，當日誌後台支援時，使得實作者能從異常中取得堆棧記錄。實作者在使用 `’execption’` 鍵值之前，依然 “必須” 驗證其確為一個 `Exception`，因為它 ”可能” 包含任何東西。

### 1.4 輔助類別和介面

- `Psr\Log\AbstractLogger` 類別讓你更簡單的透過擴展它來實作 `LoggerInterface` 並且實作出通用的 `log` 方法。另外八個方法將會把訊息和 context 轉發給 `log` 方法。

- 相同地，使用 `Psr\Log\LoggerTrait` 讓你僅需實作通用的 `Log` 方法。但特性無法實作介面，所以你依然需要自行實作 `LoggerInterface`。

- `Psr\Log\NullLogger` 與介面一同提供。它在沒有可用的日誌記錄器時，”可以“ 為使用日誌介面的用戶提供一個備用的 “黑洞”。但當發現 context 資料的構建上非常費時時，直接判斷是否需要記錄日誌可能是更好的選擇。

- `Psr\Log\LoggerAwareInterfae` 只有一個方法 `setLogger(LoggerInterface $logger)`，他可以在框架中用來任意設置一個日誌記錄器。

- `Psr\Log\LoggerAwareTrait` 特性可以被用來在任何類別中輕鬆實作相同的介面。通過它可以存取 `$this->logger`。

- `Psr\Log\LogLevel` 類別內含八個日誌層級的常數。

2. 套件
----------

[psr/log](https://packagist.org/packages/psr/log) 套件中提供了上面介紹的介面和類別，以及相關的異常類別，還有一組用來驗證你的實作所使用的單元測試。

3. `Psr\Log\LoggerInterface`
----------------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger instance
 *
 * The message MUST be a string or object implementing __toString().
 *
 * The message MAY contain placeholders in the form: {foo} where foo
 * will be replaced by the context data in key "foo".
 *
 * The context array can contain arbitrary data, the only assumption that
 * can be made by implementors is that if an Exception instance is given
 * to produce a stack trace, it MUST be in a key named "exception".
 *
 * See https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md
 * for the full interface specification.
 */
interface LoggerInterface
{
    /**
     * System is unusable.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function emergency($message, array $context = array());

    /**
     * Action must be taken immediately.
     *
     * Example: Entire website down, database unavailable, etc. This should
     * trigger the SMS alerts and wake you up.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function alert($message, array $context = array());

    /**
     * Critical conditions.
     *
     * Example: Application component unavailable, unexpected exception.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function critical($message, array $context = array());

    /**
     * Runtime errors that do not require immediate action but should typically
     * be logged and monitored.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function error($message, array $context = array());

    /**
     * Exceptional occurrences that are not errors.
     *
     * Example: Use of deprecated APIs, poor use of an API, undesirable things
     * that are not necessarily wrong.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function warning($message, array $context = array());

    /**
     * Normal but significant events.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function notice($message, array $context = array());

    /**
     * Interesting events.
     *
     * Example: User logs in, SQL logs.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function info($message, array $context = array());

    /**
     * Detailed debug information.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function debug($message, array $context = array());

    /**
     * Logs with an arbitrary level.
     *
     * @param mixed $level
     * @param string $message
     * @param array $context
     * @return null
     */
    public function log($level, $message, array $context = array());
}
```

4. `Psr\Log\LoggerAwareInterface`
---------------------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger-aware instance
 */
interface LoggerAwareInterface
{
    /**
     * Sets a logger instance on the object
     *
     * @param LoggerInterface $logger
     * @return null
     */
    public function setLogger(LoggerInterface $logger);
}
```

5. `Psr\Log\LogLevel`
---------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes log levels
 */
class LogLevel
{
    const EMERGENCY = 'emergency';
    const ALERT     = 'alert';
    const CRITICAL  = 'critical';
    const ERROR     = 'error';
    const WARNING   = 'warning';
    const NOTICE    = 'notice';
    const INFO      = 'info';
    const DEBUG     = 'debug';
}
```
