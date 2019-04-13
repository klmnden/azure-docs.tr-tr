---
title: Azure CDN kurallar altyapısı için HTTP değişkenleri | Microsoft Docs
description: HTTP değişkenleri HTTP istek ve yanıt meta verileri almanıza olanak sağlar.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus
ms.openlocfilehash: 8d4fc5fbdc3185c46f00d94537b197ec03f66755
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59528178"
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı için HTTP değişkenleri
HTTP değişkenleri üzerinden HTTP istek ve yanıt meta verilerini almak bir yöntem sağlar. Bu meta veriler, ardından bir istek veya yanıt dinamik olarak değiştirmek için kullanılabilir. Aşağıdaki kural altyapısı özellikleri için HTTP değişkenlerini sınırlıdır:

- [Önbellek anahtarı yeniden yazma](cdn-rules-engine-reference-features.md#cache-key-rewrite)
- [İstemci istek üst bilgisini değiştirin](cdn-rules-engine-reference-features.md#modify-client-request-header)
- [İstemci yanıt üst bilgisi değiştirme](cdn-rules-engine-reference-features.md#modify-client-response-header)
- [URL yeniden yönlendirme](cdn-rules-engine-reference-features.md#url-redirect)
- [URL yeniden yazma](cdn-rules-engine-reference-features.md#url-rewrite)

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
| ,  | Küçük HTTP değişkenle ilişkili dönüştürün. |
| ^ | Büyük harfe HTTP değişkenle ilişkili dönüştürün. |
| ,, | Belirtilen karakter değeri küçük HTTP değişkenle ilişkili tüm örneklerini dönüştürün. |
| ^^ | Belirtilen karakter değeri büyük harfe HTTP değişkenle ilişkili tüm örneklerini dönüştürün. |

## <a name="exceptions"></a>Özel durumlar
Aşağıdaki tabloda, bir HTTP değişkeni olarak belirtilen metin kabul değil koşullar açıklanmaktadır.

| Koşul | Açıklama | Örnek |
| --------- | ----------- | --------|
| % Sembol kaçış | Yüzde simgesi ters eğik çizgi kullanılarak atlanması. <br />Örnek değer sağında bir HTTP değişkeni değil de, değişmez değer olarak kabul edilir.| \%{} konağı |
| Bilinmeyen değişkenleri | Boş bir dize her zaman için bilinmeyen değişkenleri döndürülür. | % {unknownvariable} |
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
| Aşağıdaki koşullardan biri karşılaması durumunda bir üst bilgi varsayılan bir değere ayarlayın: <br /><br />-Üst bilgisi eksik <br /><br />-Üst bilgi değeri NULL olarak ayarlanır.| % {Değişken: = Value} | % {http_referer: = belirtilmeyen} | Referer üst bilgisi yalnızca ayarlanacak *belirtilmeyen* eksik ya da ayarlanmış olduğunda null. Bunu ayarlarsanız, hiçbir eylem gerçekleşir. |
| Eksik olduğunda, bir üst bilgi varsayılan bir değere ayarlayın. | % {Değişken = Value} | %{http_referer=unspecified} | Referer üst bilgisi yalnızca ayarlanacak *belirtilmeyen* eksik olduğunda. Bunu ayarlarsanız, hiçbir eylem gerçekleşir. |
| Üst bilgi aşağıdaki koşullardan birini karşılamıyor varsayılan değerine ayarlanır: <br /><br />-Eksik:<br /><br /> -NULL olarak ayarlayın. | % {Değişken: + değeri} | % {http_referer: + belirtilmeyen} | Referer üst bilgisi yalnızca ayarlanacak *belirtilmeyen* bir değer atandığında için. Eksik ya da ayarlanmış ise hiçbir eylem gerçekleşecek null. |

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

| Sözdizimi | Eylem |
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

| Sözdizimi | Eylem |
| ------ | ------ |
| %{Variable/Find/Replace} | Bulma ve belirtilen desenle ilk geçtiği değiştirme. |
| %{Variable//Find/Replace} | Bul ve belirtilen desenle tüm oluşumlarını değiştirin. |
| % {Değişken ^} |Değerin tümünü büyük harfe Dönüştür. |
| % {Değişken ^ Bul} | Belirtilen desenin dizenin büyük harfe Dönüştür. |
| % {, Değişkeni} | Tüm değer küçük harfe Dönüştür. |
| % {Değişken Bul} | Belirtilen desenle ilk geçtiği küçük harfe Dönüştür. |

### <a name="find-and-rewrite"></a>Bul ve yeniden yazma
Bul ve Değiştir üzerinde bir değişim için yeniden yazma, belirtilen desenle eşleşen metin kullanın. Bul ve yeniden yazma sözdizimi aşağıdaki tabloda açıklanmıştır.

| Sözdizimi | Eylem |
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


