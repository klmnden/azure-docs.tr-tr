---
title: Azure CDN kurallar altyapısı için HTTP değişkenleri | Microsoft Docs
description: HTTP değişkenleri HTTP istek ve yanıt meta verileri almanıza olanak sağlar.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus
ms.openlocfilehash: 53ad0c516547e17801bd57c2fd6b0d1704383797
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593811"
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı için HTTP değişkenleri
HTTP değişkenleri üzerinden HTTP istek ve yanıt meta verilerini almak bir yöntem sağlar. Bu meta veriler, ardından bir istek veya yanıt dinamik olarak değiştirmek için kullanılabilir. Aşağıdaki kural altyapısı özellikleri için HTTP değişkenlerini sınırlıdır:

- [Önbellek anahtarı yeniden yazma](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [İstemci istek üst bilgisini değiştirin](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [İstemci yanıt üst bilgisi değiştirme](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [URL yeniden yönlendirme](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [URL yeniden yazma](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Tanımlar
Aşağıdaki tabloda, desteklenen HTTP değişkenler açıklanmaktadır. Belirli bir istek için coğrafi meta verileri (örneğin, posta kodu) kullanılamadığında, boş bir değer döndürülür.


| Ad | Değişken | Açıklama | Örnek değer |
| ---- | -------- | ----------- | ------------ |
| ASN (istek sahibi) | % {geo_asnum} | İstek sahibinin sayı olarak gösterir. <br /><br />**Kullanım:** % {virt_dst_asnum}. <br />Bu değişken, % {geo_asnum}'ile değiştiriliyor kullanım dışı bırakıldı. Bu kullanım dışı değişkeni kullanan bir kural çalışmaya devam eder ancak yeni değişken kullanacak şekilde güncelleştirmeniz gerekir. | AS15133 |
| Şehir (istek sahibi) | %{geo_city} | İstek sahibinin Şehir gösterir. | Los Angeles |
| Kıta (istek sahibi) | %{geo_continent} | İstek sahibinin Kıta eşlememiz aracılığıyla gösterir. <br />Geçerli değerler şunlardır: <br />AF: Afrika<br />OLARAK: Asya<br />AV: Avrupa<br />NA: Kuzey Amerika<br />OC: Okyanusya<br />SA: Güney Amerika<br /><br />**Kullanım:** % {virt_dst_continent}. <br />Bu değişken, % {geo_continent}'ile değiştiriliyor kullanım dışı bırakıldı. <br />Bu kullanım dışı değişkeni kullanan bir kural çalışmaya devam eder ancak yeni değişken kullanacak şekilde güncelleştirmeniz gerekir.| Yok |
| Tanımlama bilgisi değeri | % {cookie_Cookie} | Tanımlama bilgisi terimi tarafından tanımlanan tanımlama bilgisi anahtarına karşılık gelen değeri döndürür. | Örnek Kullanım: <br />%{cookie__utma}<br /><br />Örnek değer:<br />111662281.2.10.1222100123 |
| Ülke (istek sahibi) | %{geo_country} | Kendi ülke kodu aracılığıyla sahibinin ülkeyi belirtir. <br />**Kullanım:** % {virt_dst_country}. <br /><br />Bu değişken, % {geo_country}'ile değiştiriliyor kullanım dışı bırakıldı. Bu kullanım dışı değişkeni kullanan bir kural çalışmaya devam eder ancak yeni değişken kullanacak şekilde güncelleştirmeniz gerekir. | ABD |
| Belirlenen Pazar alan (istek sahibi) | %{geo_dma_code} |Sahibinin medya market tarafından kendi bölge kodu gösterir. <br /><br />Bu alan yalnızca, Amerika Birleşik Devletleri'nden kaynaklanan istekler için geçerlidir.| 745 |
| HTTP istek yöntemi | %{request_method} | HTTP istek yöntemi gösterir. | GET |
| HTTP durum kodu | % {status} | Yanıt için HTTP durum kodu gösterir. | 200 |
| IP adresi (istek sahibi) | %{virt_dst_addr} | İstek sahibinin IP adresini gösterir. | 192.168.1.1 |
| Enlem (istek sahibi) | %{geo_latitude} | İstek sahibinin enlem gösterir. | 34.0995 |
| Boylam (istek sahibi) | %{geo_longitude} | İstek sahibinin boylam gösterir. | -118.4143 |
| İstatistiksel metropol alanı (istek sahibi) | %{geo_metro_code} | İstek sahibinin metropol alanı gösterir. <br /><br />Bu alan yalnızca, Amerika Birleşik Devletleri'nden kaynaklanan istekler için geçerlidir.<br />| 745 |
| Bağlantı noktası (istek sahibi) | %{virt_dst_port} | Kısa ömürlü bağlantı noktası sahibinin gösterir. | 55885 |
| Posta kodu (istek sahibi) | %{geo_postal_code} | İstek sahibinin posta kodu gösterir. | 90210 |
| Sorgu dizesi bulunamadı | %{is_args} | Bu değişken için değeri istek sorgu dizesini içeren göre değişir.<br /><br />-Sorgu dizesi bulunamadı:?<br />-Hiçbir sorgu dizesi: NULL | ? |
| Sorgu dizesi parametresi bulunamadı | %{is_amp} | Bu değişken için değeri istek en az bir sorgu dizesi parametresi içeriyor göre değişir.<br /><br />-Parametresi bulunamadı: &<br />-Parametreler: NULL | & |
| Sorgu dizesi parametresi değeri | % {arg_&lt;parametre&gt;} | Sorgu dizesi parametresi tarafından tanımlanan karşılık gelen bir değer döndürür &lt;parametre&gt; terim. | Örnek Kullanım: <br />%{arg_language}<br /><br />Örnek sorgu dizesi parametresi: <br />? Dil = tr<br /><br />Örnek değer: tr |
| Sorgu dizesi değeri | %{query_string} | İstek URL'SİNDE tanımlanan tüm sorgu dizesi değerini gösterir. |key1=val1&key2=val2&key3=val3 |
| Başvuran etki alanı | %{referring_domain} | Başvuran istek üstbilgisinde tanımlanan etki alanını gösterir. | <www.google.com> |
| Bölge (istek sahibi) | %{geo_region} | Alfasayısal eşlememiz aracılığıyla sahibinin bölge (örneğin, eyalet veya il) gösterir. | CA |
| İstek üstbilgisi değeri | %{http_RequestHeader} | RequestHeader terimi tarafından tanımlanan istek üst bilgisi için karşılık gelen değeri döndürür. <br /><br />İstek üstbilgisi adı bir tire (örneğin, User-Agent) içeriyorsa, bir alt çizgiyle (örneğin, User_Agent) değiştirin.| Örnek Kullanım: % {http_Connection}<br /><br />Örnek değer: Tutma | 
| İstek ana bilgisayarı | %{host} | İstek URL'SİNDE tanımlanan konak gösterir. | < www.mydomain.com> |
| İstek Protokolü | %{request_protocol} | İstek Protokolü belirtir. | HTTP/1.1 |
| İstek düzeni | % {scheme} | İstek düzenini gösterir. |http |
| İstek URI'si (göreli) | %{request_uri} | Tanımlanan istek URI'SİNDE sorgu dizesi dahil olmak üzere, göreli yol gösterir. | /Marketing/foo.js?loggedin=true |
| İstek URI'si (göreli sorgu dizesi olmadan) | %{uri} | Talep edilen içeriği göreli yolunu gösterir. <br /><br/>Anahtar bilgileri:<br />-Bu göreli yol, sorgu dizesi dahil değildir.<br />-Bu göreli URL yeniden yazma işlemleri yansıtır. Bir URL aşağıdaki koşullarda yazılacaktır:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-URL yeniden yazma özelliği: Bu özellik istek URI'SİNDE tanımlanan göreli yol yeniden yazar.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-Edge CNAME URL'si: Bu istek türü için karşılık gelen CDN URL'sine yeniden. |/800001/corigin/rewrittendir/foo.js |
| İstek URI'si | % {isteği} | İstek açıklar. <br />Sözdizimi: &lt;HTTP yöntemi&gt; &lt;göreli yol&gt; &lt;HTTP Protokolü&gt; | /Marketing/foo.js?loggedin=true HTTP/1.1 Al |
| Yanıt üstbilgi değeri | %{resp_&lt;ResponseHeader&gt;} | Yanıt üst bilgisi tarafından tanımlanan karşılık gelen bir değer döndürür &lt;ResponseHeader&gt; terim. <br /><br />Yanıt üst bilgisi adı bir tire (örneğin, User-Agent) içeriyorsa, bir alt çizgiyle (örneğin, User_Agent) değiştirin. | Örnek Kullanım: % {resp_Content_Length}<br /><br />Örnek değer: 100 |

## <a name="usage"></a>Kullanım
Aşağıdaki tablo, bir HTTP değişkeni belirtmek için doğru sözdizimi açıklar.


| Sözdizimi | Örnek | Açıklama |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | %{host} | Belirtilen karşılık gelen tüm değerini almak için şu söz dizimini kullanın &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | % {, konağı} | Servis talebi için belirtilen karşılık gelen tüm değer ayarlamak için bu sözdizimini kullanın &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{Host/=^www\.([^\.] +)\.([^\.:] +) /cdn.$2.$3:80} | İçin normal bir ifade kullanmanızı &lt;HTTPVariableDelimiterExpression&gt; değiştirmek için silmek veya bir HTTP değişken değerini düzenleme. |

HTTP değişken adı yalnızca alfabetik karakterler ve alt çizgi destekler. Desteklenmeyen karakterler, alt çizgileri dönüştürün.

## <a name="delimiter-reference"></a>Sınırlayıcı başvurusu
Bir sınırlayıcıdan aşağıdaki etkileri birini elde etmek için bir HTTP değişkeni sonra belirtilebilir:

- Değişkenle ilişkili değer dönüştürün.

     Örnek: Tüm değer küçük harfe Dönüştür.

- Değişkenle ilişkili değeri silin.

- Değişkenle ilişkili değer işleyin.

     Örnek: HTTP değişkenle ilişkili değeri değiştirmek için normal ifadeler kullanın.

Sınırlayıcılar aşağıdaki tabloda açıklanmıştır.

| Sınırlayıcı | Açıklama |
| --------- | ----------- |
| := | Varsayılan değer ya da olduğunda değişkenine atanacak gösterir: <br />-Eksik: <br />-NULL olarak ayarlayın. |
| :+ | Varsayılan değer için bir değer atandıktan sonra değişkenine atanacak gösterir. |
| : | Değişkene atanan değer bir alt dizesi genişletilecek gösterir. |
| # | Değişkenle ilişkili değerin başında bulunduğunda bu sınırlayıcıdan sonra belirtilen desenle silinmesi gerektiğini gösterir. |
| % | Değişkenle ilişkili değer sonunda bulunduğunda bu sınırlayıcıdan sonra belirtilen desenle silinmesi gerektiğini gösterir. <br />Bu tanım, yalnızca % sembol sınırlayıcı olarak kullanıldığında geçerlidir. |
| / | Bir HTTP değişkeni veya desen sınırlandırır. |
| // | Bulun ve tüm örneklerini belirtilen desenle değiştirin. |
| /= | Bulma, kopyalama ve belirtilen desenle tüm örneklerini yeniden yazın. |
| , | Küçük HTTP değişkenle ilişkili dönüştürün. |
| ^ | Büyük harfe HTTP değişkenle ilişkili dönüştürün. |
| ,, | Belirtilen karakter değeri küçük HTTP değişkenle ilişkili tüm örneklerini dönüştürün. |
| ^^ | Belirtilen karakter değeri büyük harfe HTTP değişkenle ilişkili tüm örneklerini dönüştürün. |

## <a name="exceptions"></a>Özel durumlar
Aşağıdaki tabloda, bir HTTP değişkeni olarak belirtilen metin kabul değil koşullar açıklanmaktadır.

| Koşul | Açıklama | Örnek |
| --------- | ----------- | --------|
| % Sembol kaçış | Yüzde simgesi ters eğik çizgi kullanılarak atlanması. <br />Örnek değer sağında bir HTTP değişkeni değil de, değişmez değer olarak kabul edilir.| \%{host} |
| Unknown variables | An empty string is always returned for unknown variables. | %{unknown_variable} |
| Invalid characters or syntax | Variables that contain invalid characters or syntax are treated as literal values. <br /><br />Example #1: The specified value contains an invalid character (for example, -). <br /><br />Example #2: The specified value contains a double set of curly braces. <br /><br />Example #3: The specified value is missing a closing curly brace.<br /> | Example #1: %{resp_user-agent} <br /><br />Example #2: %{{host}} <br /><br />Example #3: %{host |
| Missing variable name | A NULL value is always returned when a variable is not specified. | %{} |
| Trailing characters | Characters that trail a variable are treated as literal values. <br />The sample value to the right contains a trailing curly brace that will be treated as a literal value. | %{host}} |

## Setting default header values
A default value can be assigned to a header when it meets any of the following conditions:
- Missing/unset
- Set to NULL.

The following table describes how to define a default value.

| Condition | Syntax | Example | Description |
| --------- | ------ | --------| ----------- |
| Set a header to a default value when it meets any of the following conditions: <br /><br />- Missing Header <br /><br />- Header value is set to NULL.| %{Variable:=Value} | %{http_referrer:=unspecified} | The Referrer header will only be set to *unspecified* when it is either missing or set to NULL. No action will take place if it has been set. |
| Set a header to a default value when it is missing. | %{Variable=Value} | %{http_referrer=unspecified} | The Referrer header will only be set to *unspecified* when it is missing. No action will take place if it has been set. |
| Set the header to a default value when it does not meet any of the following conditions: <br /><br />- Missing<br /><br /> - Set to NULL. | %{Variable:+Value} | %{http_referrer:+unspecified} | The Referrer header will only be set to *unspecified* when a value has been assigned to it. No action will take place if it is either missing or set to NULL. |

## Manipulating variables
Variables can be manipulated in the following ways:

- Expanding substrings
- Removing patterns

### Substring expansion
By default, a variable will expand to its full value. Use the following syntax to only expand a substring of the variable's value.

`%<Variable>:<Offset>:<Length>}`

Key information:

- The value assigned to the Offset term determines the starting character of the substring:

     - Positive: The starting character of the substring is calculated from the first character in the string.
     - Zero: The starting character of the substring is the first character in the string.
     - Negative: The starting character of the substring is calculated from the last character in the string.

- The length of the substring is determined by the *Length* term:

     - Omitted: Omitting the Length term allows the substring to include all characters between the starting character and the end of the string.
     - Positive: Determines the length of the substring from the starting character to the right.
     - Negative: Determines the length of the substring from the starting character to the left.

#### Example:

The following example relies on the following sample request URL:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

The following string demonstrates various methods for manipulating variables:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Based on the sample request URL, the above variable manipulation will produce the following value:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### Pattern removal
Text that matches a specific pattern can be removed from either the beginning or the end of a variable's value.

| Syntax | Action |
| ------ | ------ |
| %{Variable#Pattern} | Remove text when the specified pattern is found at the beginning of a variable's value. |
| %{Variable%Pattern} | Remove text when the specified pattern is found at the end of a variable's value. |

#### Example:

In this sample scenario, the *request_uri* variable is set to:

`/800001/myorigin/marketing/product.html?language=en-US`

The following table demonstrates how this syntax works.

| Sample syntax | Results | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=en-US | Because the variable starts with the pattern, it was replaced. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Because the variable doesn't end with the pattern, there was no change.|

### Find and replace
The find and replace syntax is described in the following table.

| Syntax | Action |
| ------ | ------ |
| %{Variable/Find/Replace} | Find and replace first occurrence of the specified pattern. |
| %{Variable//Find/Replace} | Find and replace all occurrences of the specified pattern. |
| %{Variable^} |Convert the entire value to uppercase. |
| %{Variable^Find} | Convert the first occurrence of the specified pattern to uppercase. |
| %{Variable,} | Convert the entire value to lowercase. |
| %{Variable,Find} | Convert the first occurrence of the specified pattern to lowercase. |

### Find and rewrite
For a variation on find and replace, use the text that matches the specified pattern when rewriting it. The find and rewrite syntax is described in the following table.

| Syntax | Action |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Find, copy, and rewrite all occurrences of the specified pattern. |
| %{Variable/^Find/Rewrite} | Find, copy, and rewrite the specified pattern when it occurs at the start of the variable. |
| %{Variable/$Find/Rewrite} | Find, copy, and rewrite the specified pattern when it occurs at the end of the variable. |
| %{Variable/Find} | Find and delete all occurrences of the specified pattern. |

Key information:

- Expand text that matches the specified pattern by specifying a dollar sign followed by a whole integer (for example, $1).

- Multiple patterns can be specified. The order in which the pattern is specified determines the integer that will be assigned to it. In the following example, the first pattern matches "www.," the second pattern matches the second-level domain, and the third pattern matches the top-level domain:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- The rewritten value can consist of any combination of text and these placeholders.

    In the previous example, the hostname is rewritten to `cdn.$2.$3:80` (for example, cdn.mydomain.com:80).

- The case of a pattern placeholder (for example, $1) can be modified through the following flags:
     - U: Uppercase the expanded value.

         Sample syntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Lowercase the expanded value.

         Sample syntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- An operator must be specified before the pattern. The specified operator determines the pattern-capturing behavior:

     - `=`: Indicates that all occurrences of the specified pattern must be captured and rewritten.
     - `^`: Indicates that only text that starts with the specified pattern will be captured.
     - `$`: Indicates that only text that ends with the specified pattern will be capture.
 
- If you omit the */Rewrite* value, the text that matches the pattern is deleted.

\`{} konağıle: HTTP variables for Azure CDN rules engine | Microsoft Docs
description: HTTP variables allow you to retrieve HTTP request and response metadata.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''

ms.assetid: 
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus


---
# HTTP variables for Azure CDN rules engine
HTTP variables provide the means through which you can retrieve HTTP request and response metadata. This metadata can then be used to dynamically alter a request or a response. The use of HTTP variables is restricted to the following rules engine features:

- [Cache-Key Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [Modify Client Request Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [Modify Client Response Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [URL Redirect](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [URL Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## Definitions
The following table describes the supported HTTP variables. A blank value is returned when GEO metadata (for example, postal code) is unavailable for a particular request.


| Name | Variable | Description | Sample value |
| ---- | -------- | ----------- | ------------ |
| ASN (Requester) | %{geo_asnum} | Indicates the requester's AS number. <br /><br />**Deprecated:** %{virt_dst_asnum}. <br />This variable has been deprecated in favor of %{geo_asnum}. Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable. | AS15133 |
| City (Requester) | %{geo_city} | Indicates the requester's city. | Los Angeles |
| Continent (Requester) | %{geo_continent} | Indicates the requester's continent through its abbreviation. <br />Valid values are: <br />AF: Africa<br />AS: Asia<br />EU: Europe<br />NA: North America<br />OC: Oceania<br />SA: South America<br /><br />**Deprecated:** %{virt_dst_continent}. <br />This variable has been deprecated in favor of %{geo_continent}. <br />Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable.| N/A |
| Cookie Value | %{cookie_Cookie} | Returns the value corresponding to the cookie key identified by the Cookie term. | Sample Usage: <br />%{cookie__utma}<br /><br />Sample Value:<br />111662281.2.10.1222100123 |
| Country (Requester) | %{geo_country} | Indicates the requester's country of origin through its country code. <br />**Deprecated:** %{virt_dst_country}. <br /><br />This variable has been deprecated in favor of %{geo_country}. Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable. | US |
| Designated Market Area (Requester) | %{geo_dma_code} |Indicates the requester's media market by its region code. <br /><br />This field is only applicable to requests that originate from the United States.| 745 |
| HTTP Request Method | %{request_method} | Indicates the HTTP request method. | GET |
| HTTP Status Code | %{status} | Indicates the HTTP status code for the response. | 200 |
| IP Address (Requester) | %{virt_dst_addr} | Indicates the requester's IP address. | 192.168.1.1 |
| Latitude (Requester) | %{geo_latitude} | Indicates the requester's latitude. | 34.0995 |
| Longitude (Requester) | %{geo_longitude} | Indicates the requester's longitude. | -118.4143 |
| Metropolitan Statistical Area (Requester) | %{geo_metro_code} | Indicates the requester's metropolitan area. <br /><br />This field is only applicable to requests that originate from the United States.<br />| 745 |
| Port (Requester) | %{virt_dst_port} | Indicates the requester's ephemeral port. | 55885 |
| Postal Code (Requester) | %{geo_postal_code} | Indicates the requester's postal code. | 90210 |
| Query String Found | %{is_args} | The value for this variable varies according to whether the request contains a query string.<br /><br />- Query String Found: ?<br />- No Query String: NULL | ? |
| Query String Parameter Found | %{is_amp} | The value for this variable varies according to whether the request contains at least one query string parameter.<br /><br />- Parameter Found: &<br />- No Parameters: NULL | & |
| Query String Parameter Value | %{arg_&lt;parameter&gt;} | Returns the value corresponding to the query string parameter identified by the &lt;parameter&gt; term. | Sample Usage: <br />%{arg_language}<br /><br />Sample Query String Parameter: <br />?language=en<br /><br />Sample Value: en |
| Query String Value | %{query_string} | Indicates the entire query string value defined in the request URL. |key1=val1&key2=val2&key3=val3 |
| Referrer Domain | %{referring_domain} | Indicates the domain defined in the Referrer request header. | <www.google.com> |
| Region (Requester) | %{geo_region} | Indicates the requester's region (for example, state or province) through its alphanumeric abbreviation. | CA |
| Request Header Value | %{http_RequestHeader} | Returns the value corresponding to the request header identified by the RequestHeader term. <br /><br />If the name of the request header contains a dash (for example, User-Agent), replace it with an underscore (for example, User_Agent).| Sample Usage: %{http_Connection}<br /><br />Sample Value: Keep-Alive | 
| Request Host | %{host} | Indicates the host defined in the request URL. | <www.mydomain.com> |
| Request Protocol | %{request_protocol} | Indicates the request protocol. | HTTP/1.1 |
| Request Scheme | %{scheme} | Indicates the request scheme. |http |
| Request URI (Relative) | %{request_uri} | Indicates the relative path, including the query string, defined in the request URI. | /marketing/foo.js?loggedin=true |
| Request URI (Relative without query string) | %{uri} | Indicates the relative path to the requested content. <br /><br/>Key information:<br />- This relative path excludes the query string.<br />- This relative path reflects URL rewrites. A URL will be rewritten under the following conditions:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- URL Rewrite Feature: This feature rewrites the relative path defined in the request URI.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Edge CNAME URL: This type of request is rewritten to the corresponding CDN URL. |/800001/corigin/rewrittendir/foo.js |
| Request URI | %{request} | Describes the request. <br />Syntax: &lt;HTTP method&gt; &lt;relative path&gt; &lt;HTTP protocol&gt; | GET /marketing/foo.js?loggedin=true HTTP/1.1 |
| Response Header Value | %{resp_&lt;ResponseHeader&gt;} | Returns the value corresponding to the response header identified by the &lt;ResponseHeader&gt; term. <br /><br />If the name of the response header contains a dash (for example, User-Agent), replace it with an underscore (for example, User_Agent). | Sample Usage: %{resp_Content_Length}<br /><br />Sample Value: 100 |

## Usage
The following table describes the proper syntax for specifying an HTTP variable.


| Syntax | Example | Description |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | %{host} | Use this syntax to get the entire value corresponding to the specified &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | %{host,} | Use this syntax to set the case for the entire value corresponding to the specified  &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80} | Use a regular expression for &lt;HTTPVariableDelimiterExpression&gt; to replace, delete, or manipulate an HTTP variable's value. |

HTTP variable names only support alphabetic characters and underscores. Convert unsupported characters to underscores.

## Delimiter reference
A delimiter can be specified after an HTTP variable to achieve any of the following effects:

- Transform the value associated with the variable.

     Example: Convert the entire value to lowercase.

- Delete the value associated with the variable.

- Manipulate the value associated with the variable.

     Example: Use regular expressions to change the value associated with the HTTP variable.

The delimiters are described in the following table.

| Delimiter | Description |
| --------- | ----------- |
| := | Indicates that a default value will be assigned to the variable when it is either: <br />- Missing <br />- Set to NULL. |
| :+ | Indicates that a default value will be assigned to the variable when a value has been assigned to it. |
| : | Indicates that a substring of the value assigned to the variable will be expanded. |
| # | Indicates that the pattern specified after this delimiter should be deleted when it is found at the beginning of the value associated with the variable. |
| % | Indicates that the pattern specified after this delimiter should be deleted when it is found at the end of the value associated with the variable. <br />This definition is only applicable when the % symbol is used as a delimiter. |
| / | Delimits an HTTP variable or a pattern. |
| // | Find and replace all instances of the specified pattern. |
| /= | Find, copy, and rewrite all occurrences of the specified pattern. |
| , | Convert the value associated with the HTTP variable to lowercase. |
| ^ | Convert the value associated with the HTTP variable to uppercase. |
| ,, | Convert all instances of the specified character in the value associated with the HTTP variable to lowercase. |
| ^^ | Convert all instances of the specified character in the value associated with the HTTP variable to uppercase. |

## Exceptions
The following table describes circumstances under which the specified text isn't treated as an HTTP variable.

| Condition | Description | Example |
| --------- | ----------- | --------|
| Escaping % symbol | The percentage symbol can be escaped through the use of a backslash. <br />The sample value to the right will be treated as a literal value and not as an HTTP variable.| \%{host} |
| Bilinmeyen değişkenleri | Boş bir dize her zaman için bilinmeyen değişkenleri döndürülür. | % {unknown_variable} |
| Geçersiz karakterler veya söz dizimi | Geçersiz karakter veya sözdizimi içeriyor. değişken değişmez değerler kabul edilir. <br /><br />#1. örnek: Belirtilen değer geçersiz bir karakter içeriyor (örneğin,-). <br /><br />#2. örnek: Belirtilen değer küme ayracı çift kümesini içerir. <br /><br />#3. örnek: Belirtilen değer bir kapanış küme ayracı eksik.<br /> | #1. örnek: % {aracı resp_user} <br /><br />#2. örnek: % {{ana}} <br /><br />#3. örnek: % {konak |
| Değişken adı eksik | Bir değişken belirtilmediğinde NULL değer her zaman döndürülür. | %{} |
| Sondaki karakterler | Bir değişken izine karakter değişmez değerler kabul edilir. <br />Örnek sağındaki değer değişmez değer kabul edilir izleyen bir küme ayracı içerir. | %{host}} |

## <a name="setting-default-header-values"></a>Varsayılan üstbilgi değerleri ayarlama
Aşağıdaki koşullardan biri karşılaması durumunda varsayılan bir değer için bir başlık atanabilir:
- Eksik/ayarlama
- NULL olarak ayarlayın.

Aşağıdaki tabloda, varsayılan değer tanımlama açıklar.

| Koşul | Sözdizimi | Örnek | Açıklama |
| --------- | ------ | --------| ----------- |
| Aşağıdaki koşullardan biri karşılaması durumunda bir üst bilgi varsayılan bir değere ayarlayın: <br /><br />-Üst bilgisi eksik <br /><br />-Üst bilgi değeri NULL olarak ayarlanır.| % {Değişken: = Value} | % {http_referrer: = belirtilmeyen} | Başvuran üst bilgisi yalnızca ayarlanacak *belirtilmeyen* eksik ya da ayarlanmış olduğunda null. Bunu ayarlarsanız, hiçbir eylem gerçekleşir. |
| Eksik olduğunda, bir üst bilgi varsayılan bir değere ayarlayın. | % {Değişken = Value} | %{http_referrer=unspecified} | Başvuran üst bilgisi yalnızca ayarlanacak *belirtilmeyen* eksik olduğunda. Bunu ayarlarsanız, hiçbir eylem gerçekleşir. |
| Üst bilgi aşağıdaki koşullardan birini karşılamıyor varsayılan değerine ayarlanır: <br /><br />-Eksik:<br /><br /> -NULL olarak ayarlayın. | % {Değişken: + değeri} | % {http_referrer: + belirtilmeyen} | Başvuran üst bilgisi yalnızca ayarlanacak *belirtilmeyen* bir değer atandığında için. Eksik ya da ayarlanmış ise hiçbir eylem gerçekleşecek null. |

## <a name="manipulating-variables"></a>Değişkenleri düzenleme
Değişkenleri aşağıdaki yöntemlerle yönetilebilir:

- Alt dizeler genişletme
- Desenler kaldırılıyor

### <a name="substring-expansion"></a>Alt dize genişletme
Varsayılan olarak, bir değişkenin tam değerini genişletir. Yalnızca bir alt dizesi değişken değerini genişletmek için aşağıdaki sözdizimini kullanın.

`%<Variable>:<Offset>:<Length>}`

Anahtar bilgileri:

- Alt dizenin başlangıç karakteri uzaklığı terimi için atanan değer belirler:

     - Pozitif: Başlangıç karakteri alt dizenin ilk karakterin dizede hesaplanır.
     - Sıfır: Alt dizenin başlangıç karakteri dizedeki ilk karakteridir.
     - Negatif: Dizedeki son karakter, alt dizenin başlangıç karakteri hesaplanır.

- Alt dizenin uzunluğunu tarafından belirlenen *uzunluğu* dönemi:

     - Atlanmış: Uzunluğu terimi atlama başlangıç karakteri dize sonu arasındaki tüm karakterleri eklemek alt dize verir.
     - Pozitif: Başlangıç karakteri alt dizeyi sağa uzunluğunu belirler.
     - Negatif: Başlangıç karakteri alt dizeyi sol uzunluğunu belirler.

#### <a name="example"></a>Örnek:

Aşağıdaki örnek, aşağıdaki örnek istek URL'si hakkındaki kullanır:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

Aşağıdaki dizeyi değişkenleri düzenlemek için çeşitli yöntemler gösterilmektedir:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Yukarıdaki değişken işleme örnek istek URL'si hakkındaki bağlı olarak, aşağıdaki değerini üretir:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### <a name="pattern-removal"></a>Desen kaldırma
Belirli bir desenle eşleşen metnin başına veya sonuna bir değişkenin değeri kaldırılabilir.

| Sözdizimi | Action |
| ------ | ------ |
| % {Değişkeni #Pattern} | Bir değişkenin değeri başında belirtilen desenle bulunduğunda metni silin. |
| % {Değişken % deseni} | Bir değişkenin değerinin sonuna belirtilen desenle bulunduğunda metni silin. |

#### <a name="example"></a>Örnek:

Bu örnek senaryoda *request_urı* değişkeni ayarlanır:

`/800001/myorigin/marketing/product.html?language=en-US`

Aşağıdaki tabloda, bu sözdizimini nasıl çalıştığını gösterir.

| Örnek söz dizimi | Sonuçlar | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/Marketing/Product.HTML?Language=en-us | Değişken desen ile başladığından değiştirildi. |
| %{request_uri%html}htm | /800001/myorigin/Marketing/Product.HTML?Language=en-us | Değişken desen ile sonlanmıyor olduğundan, hiçbir değişiklik yoktu.|

### <a name="find-and-replace"></a>Bul ve Değiştir
Bul ve Değiştir söz dizimi aşağıdaki tabloda açıklanmıştır.

| Sözdizimi | Action |
| ------ | ------ |
| %{Variable/Find/Replace} | Bulma ve belirtilen desenle ilk geçtiği değiştirme. |
| %{Variable//Find/Replace} | Bul ve belirtilen desenle tüm oluşumlarını değiştirin. |
| % {Değişken ^} |Değerin tümünü büyük harfe Dönüştür. |
| % {Değişken ^ Bul} | Belirtilen desenin dizenin büyük harfe Dönüştür. |
| % {, Değişkeni} | Tüm değer küçük harfe Dönüştür. |
| % {Değişken Bul} | Belirtilen desenle ilk geçtiği küçük harfe Dönüştür. |

### <a name="find-and-rewrite"></a>Bul ve yeniden yazma
Bul ve Değiştir üzerinde bir değişim için yeniden yazma, belirtilen desenle eşleşen metin kullanın. Bul ve yeniden yazma sözdizimi aşağıdaki tabloda açıklanmıştır.

| Sözdizimi | Action |
| ------ | ------ |
| %{Variable/=Find/rewrite} | Bulma, kopyalama ve belirtilen desenle tüm örneklerini yeniden yazın. |
| %{Variable/^Find/rewrite} | Bul, kopyalama ve değişkenin başında gerçekleştiğinde belirtilen desenle yeniden yazın. |
| %{Variable/$Find/rewrite} | Bulma, kopyalama ve değişken sonunda olduğunda belirtilen deseni yeniden yazın. |
| %{Variable/Find} | Bul ve belirtilen desenle tüm örnekleri silin. |

Anahtar bilgileri:

- Tüm tamsayı (örneğin, $1) ve ardından bir dolar işareti belirterek belirtilen desenle eşleşen metin genişletin.

- Birden çok desen belirtilebilir. Kendisine atanmış bir tamsayı içinde desenini belirtilen sırasını belirler. Aşağıdaki örnekte, ilk desen eşleşmeleri "www" olabilir., ikinci desen ile eşleşir, ikinci düzey etki alanı ve üst düzey etki alanı üçüncü desenle eşleşen:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- Yeniden değeri metin ve bu yer tutucuları herhangi bir birleşimini içerebilir.

    Önceki örnekte, ana bilgisayar adı için yeniden `cdn.$2.$3:80` (örneğin, cdn.mydomain.com:80).

- Bir desen yer tutucu (örneğin, $1) büyük aşağıdaki bayrakları değiştirilebilir:
     - U: Genişletilmiş değeri büyük.

         Örnek sözdizimi:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Genişletilmiş değeri küçük.

         Örnek sözdizimi:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- Bir işleç deseni önce belirtilmelidir. Belirtilen işleç deseni Yakalama davranışını belirler:

     - `=`: Belirtilen desenle tüm oluşumlarını yakalanan ve yazılan olduğunu gösterir.
     - `^`: Yalnızca belirtilen desenle başlıyor metin yakalanması gösterir.
     - `$`: Belirtilen desen ile biten metin yakalama olacağını gösterir.
 
- Atlarsanız */yeniden yazma* deseniyle eşleşen metin değeri silinir.


