# Autoloader

文件裡的這些關鍵字 "MUST" ， "MUST NOT" ， "REQUIRED" ， "SHALL" ， "SHALL NOT" ， "SHOULD" ，
 "SHOULD NOT" ， "RECOMMENDED" ， "MAY" ， 和 "OPTIONAL" 的解釋在 [RFC 2119]


## 1. 總覽

這份 PSR 敘述關於「從檔案路徑 [自動載入][] 類別」的規格。規格具有互操作性，而且可以與其他自動載入規格一起運作，包含 [PSR-0][]。這份 PSR 也敘述了根據規格，要在哪裡放置會被自動載入的檔案。


## 2. 規格

1. 這裡的 "class" 是指所有的類別（ classes ），介面（ interfaces ），特性（ traits ），和其他相似的結構

2. 一個完整 class 名稱有以下格式：

        \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>

    1. 完整 class 名稱 MUST  包含頂層命名空間名稱，也被稱為供應者（ vendor ）命名空間

    2. 完整 class 名稱 MAY 包含一個或更多次級命名空間名稱。

    3. 完整 class 名稱 MUST 包含一個尾端類別名稱

    4. 底線在完整 class 名稱的任何位置都沒有特殊意義。

    5. 完整 class 名稱裡的英文字母 MAY 是大小寫混合。

    6. 所有 class 名稱 MUST 以區分大小寫的方式被參考。

3. 當載入對應完整 class 名稱的檔案...

    1. 接在完整 class 名稱（的命名空間前綴）之後，一個或多個首個命名空間和接續的命名空間，不包含首個命名空間前的分隔符號，至少會對應到一個「基底資料夾」。

    2. 在「命名空間前綴」之後，接續的命名空間名稱會對應到「基底資料夾」下的子資料夾，命名空間分隔符號代表了資料夾的分層。子資料夾 MUST 符合命名空間名稱的大小寫。

    3. 尾端 class 名稱對應到以 `.php` 結尾的檔案，檔案 MUST 符合 class 名稱的大小寫。

4. 自動載入實作 MUST NOT 拋出例外，MUST NOT 發出任何等級的錯誤，並且 SHOULD NOT 回傳任何值。


## 3. 範例

下面的表格顯示了完整 class 名稱對應的檔案路徑，命名空間前綴，以及基底資料夾。

| 完整 class 名稱    | 命名空間前綴   | 基底資料夾           | 對應檔案路徑
| ----------------------------- |--------------------|--------------------------|-------------------------------------------
| \Acme\Log\Writer\File_Writer  | Acme\Log\Writer    | ./acme-log-writer/lib/   | ./acme-log-writer/lib/File_Writer.php
| \Aura\Web\Response\Status     | Aura\Web           | /path/to/aura-web/src/   | /path/to/aura-web/src/Response/Status.php
| \Symfony\Core\Request         | Symfony\Core       | ./vendor/Symfony/Core/   | ./vendor/Symfony/Core/Request.php
| \Zend\Acl                     | Zend               | /usr/includes/Zend/      | /usr/includes/Zend/Acl.php

符合規格的自動載入實作的範例檔，請看[範例檔][]。實作範例 MUST NOT 視為規則的一部份，並且 MAY 隨時改變。

[自動載入]: http://php.net/autoload
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[範例檔]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader-examples.md
