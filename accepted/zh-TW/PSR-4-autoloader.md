# Autoloader

本文件中以 [RFC 2119][] 的關鍵字 “必須（ MUST ）”、”不可（ MUST NOT ）”、”必要（ REQUIRED ）”、”將要（ SHALL ）”、”將不（ SHALL NOT ）”
、”應該（ SHOULD ）”、“不應該（ SHOULD NOT ）”、”建議（ RECOMMENDED ）”、”可以/可能（ MAY ）” 和 “選用（ OPTIONAL ）” 來做解釋性的描述。

[RFC 2119]: http://tools.ietf.org/html/rfc2119

## 1. 總覽

這份 PSR 敘述關於「從檔案路徑[自動載入][]類別」的規格。規格具有互操作性，而且可以與其他自動載入規格一起運作，包含 [PSR-0][]。這份 PSR 也敘述了根據規格，要在哪裡放置會被自動載入的檔案。


## 2. 規格

1. 這裡的 "class" 是指所有的類別（ classes ），介面（ interfaces ），特性（ traits ），和其他相似的結構

2. 一個完整 class 名稱有以下格式：

        \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>

    1. 完整 class 名稱 “必須” 包含頂層命名空間名稱，也被稱為供應者（ vendor ）命名空間

    2. 完整 class 名稱 “可能” 包含一個或更多次級命名空間名稱。

    3. 完整 class 名稱 “必須” 包含一個尾端類別名稱

    4. 底線在完整 class 名稱的任何位置都沒有特殊意義。

    5. 完整 class 名稱裡的英文字母 “可以” 是大小寫混合。

    6. 所有 class 名稱 “必須” 以區分大小寫的方式被參考。

3. 當載入對應完整 class 名稱的檔案...

    1. 接在完整 class 名稱（的命名空間前綴）之後，一個或多個前導命名空間和接續的命名空間（不包含前導命名空間前的分隔符號），至少會對應到一個「基底資料夾」。

    2. 在「命名空間前綴」之後，接續的命名空間名稱會對應到「基底資料夾」下的子資料夾，命名空間分隔符號代表了資料夾的分層。子資料夾 “必須” 符合命名空間名稱的大小寫。

    3. 尾端 class 名稱對應到以 `.php` 結尾的檔案，檔案 “必須” 符合 class 名稱的大小寫。

4. 自動載入實作 “不可” 拋出例外， “不可” 發出任何等級的錯誤，並且 “不應該” 回傳任何值。


## 3. 範例

下面的表格顯示了完整 class 名稱對應的檔案路徑，命名空間前綴，以及基底資料夾。

| 完整 class 名稱    | 命名空間前綴   | 基底資料夾           | 對應檔案路徑
| ----------------------------- |--------------------|--------------------------|-------------------------------------------
| \Acme\Log\Writer\File_Writer  | Acme\Log\Writer    | ./acme-log-writer/lib/   | ./acme-log-writer/lib/File_Writer.php
| \Aura\Web\Response\Status     | Aura\Web           | /path/to/aura-web/src/   | /path/to/aura-web/src/Response/Status.php
| \Symfony\Core\Request         | Symfony\Core       | ./vendor/Symfony/Core/   | ./vendor/Symfony/Core/Request.php
| \Zend\Acl                     | Zend               | /usr/includes/Zend/      | /usr/includes/Zend/Acl.php

符合規格的自動載入實作的範例檔，請看[範例檔][]。實作範例 “不可” 視為規則的一部份，並且 “可能” 隨時改變。

[自動載入]: http://php.net/autoload
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[範例檔]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader-examples.md
