---
title: Azure CDN kurallar altyapısı için HTTP değişkenleri | Microsoft Docs
description: HTTP değişkenleri, HTTP istek ve yanıt meta verilerini almak izin verir.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: v-deasim
ms.openlocfilehash: ea7469b1d1c3d1c20beca9b1fb3bef0d4dac9492
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı için HTTP değişkenleri
HTTP değişkenleri HTTP istek ve yanıt meta verileri alabilir olanağı sağlar. Bu meta veriler daha sonra bir istek veya yanıt dinamik olarak değiştirmek için kullanılabilir. HTTP değişkenlerin kullanımını aşağıdaki kurallar altyapısı özellikleri sınırlıdır:

- [Önbellek anahtarı yeniden yazma](cdn-rules-engine-reference-features.md#cache-key-rewrite)
- [İstemci istek üstbilgisi değiştirme](cdn-rules-engine-reference-features.md#modify-client-request-header)
- [İstemci yanıt üstbilgisi değiştirme](cdn-rules-engine-reference-features.md#modify-client-response-header)
- [URL yeniden yönlendirme](cdn-rules-engine-reference-features.md#url-redirect)
- [URL yeniden yazma](cdn-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Tanımlar
Aşağıdaki tabloda, desteklenen HTTP değişkenler açıklanmaktadır. Belirli bir istek için coğrafi meta veriler (örneğin, posta kodu) kullanılamadığında boş bir değer döndürdü.


| Ad | Değişken | Açıklama | Örnek değer |
| ---- | -------- | ----------- | ------------ |
| ASN (istek) | % {geo_asnum} | Sahibinin sayı olarak gösterir. <br /><br />**Kullanım:** % {virt_dst_asnum}. <br />Bu değişken, % {geo_asnum} lehinde kullanım dışı bırakıldı. Bu kullanım dışı değişkeni kullanan bir kural çalışmaya devam eder ancak yeni değişken kullanacak şekilde güncelleştirmeniz gerekir. | AS15133 |
| Şehir (istek) | % {geo_city} | Sahibinin Şehir gösterir. | Los Angeles |
| Kıtada (istek) | % {geo_continent} | Kendi kısaltması aracılığıyla sahibinin kıtada gösterir. <br />Geçerli değerler şunlardır: <br />AF: Afrika<br />AS: Asya<br />AB: Avrupa<br />Ad: Kuzey Amerika<br />OC: Oceania<br />SA: Güney Amerika<br /><br />**Kullanım:** % {virt_dst_continent}. <ber />Bu değişken, % {geo_continent} lehinde kullanım dışı bırakıldı. <br />Bu kullanım dışı değişkeni kullanan bir kural çalışmaya devam eder ancak yeni değişken kullanacak şekilde güncelleştirmeniz gerekir.| Yok |
| Tanımlama bilgisi değeri | % {cookie_Cookie} | Tanımlama bilgisi terim tarafından tanımlanan tanımlama bilgileri anahtarı karşılık gelen değer döndürür. | Örnek Kullanım: <br />% {cookie__utma}<br /><br />Örnek değer:<br />111662281.2.10.1222100123 |
| Ülke (istek) | % {geo_country} | Ülke kodu aracılığıyla sahibinin ülkeyi belirtir. <br />**Kullanım:** % {virt_dst_country}. <br /><br />Bu değişken, % {geo_country} lehinde kullanım dışı bırakıldı. Bu kullanım dışı değişkeni kullanan bir kural çalışmaya devam eder ancak yeni değişken kullanacak şekilde güncelleştirmeniz gerekir. | ABD |
| Belirlenen Pazar alan (istek) | % {geo_dma_code} |Sahibinin medya Pazar kendi bölge koduna göre gösterir. <br /><br />Bu alan yalnızca ABD kaynaklanan istekleri için geçerlidir.| 745 |
| HTTP istek yöntemi | % {request_method} | HTTP istek yöntemini gösterir. | GET |
| HTTP durum kodu | % {status} | Yanıt için HTTP durum kodu gösterir. | 200 |
| IP adresi (istek) | % {virt_dst_addr} | Sahibinin IP adresini gösterir. | 192.168.1.1 |
| Enlem (istek) | % {geo_latitude} | Sahibinin enlem gösterir. | 34.0995 |
| Boylam (istek) | % {geo_longitude} | Sahibinin boylam gösterir. | -118.4143 |
| Metropol istatistiksel alanı (istek) | % {geo_metro_code} | Sahibinin metropol alanı gösterir. <br /><br />Bu alan yalnızca ABD kaynaklanan istekleri için geçerlidir.<br />| 745 |
| Bağlantı noktası (istek) | % {virt_dst_port} | Sahibinin kısa ömürlü bağlantı noktasını belirtir. | 55885 |
| Posta kodu (istek) | % {geo_postal_code} | Sahibinin posta kodu gösterir. | 90210 |
| Sorgu dizesi bulunamadı | % {is_args} | Bu değişkeni, istek bir sorgu dizesi içeriyor göre değişir.<br /><br />-Sorgu dizesi bulunamadı:?<br />-Hiçbir sorgu dizesi: NULL | ? |
| Sorgu dizesi parametresi bulunamadı | % {is_amp} | Bu değişkeni, istek en az bir sorgu dizesi parametresi içeriyor göre değişir.<br /><br />-Parametresi bulunamadı: &<br />-Herhangi bir parametre: NULL | & |
| Sorgu dizesi parametre değeri | % {arg_&lt;parametresi&gt;} | Sorgu dizesi parametresi tarafından tanımlanan karşılık gelen değer verir &lt;parametresi&gt; terim. | Örnek Kullanım: <br />% {arg_language}<br /><br />Örnek sorgu dizesi parametresi: <br />? Dil = tr<br /><br />Örnek değer: tr |
| Sorgu dizesi değeri | % {QUERY_STRING} | İstek URL'SİNDE tanımlanan tüm sorgu dizesi değerini gösterir. |key1 = değer1 & key2 = değer2 & anahtar3 val3 = |
| Başvuran etki alanı | % {referring_domain} | Başvuran istek üstbilgisinde tanımlanan etki alanını gösterir. | www.Google.com |
| Bölge (istek) | % {geo_region} | Alfasayısal kısaltması aracılığıyla sahibinin bölge (örneğin, durum veya il) gösterir. | CA |
| İstek üstbilgi değeri | % {http_RequestHeader} | RequestHeader terim tarafından tanımlanan istek üstbilgisi karşılık gelen değer döndürür. <br /><br />İstek üstbilgisi adı bir tire (örneğin, User-Agent) içeriyorsa, alt çizgi (örneğin, User_Agent) ile değiştirin.| Örnek Kullanım: % {http_Connection}<br /><br />Örnek değeri: Etkin tutma | 
| İstek ana bilgisayarı | % {konak} | İstek URL'SİNDE tanımlanan ana bilgisayarı gösterir. | www.mydomain.com |
| İstek Protokolü | % {request_protocol} | İstek protokolü gösterir. | HTTP/1.1 |
| İstek düzeni | % {şeması} | İstek düzenini gösterir. |http |
| İstek URI'si (göreli) | % {request_urı} | İstek URI'si tanımlanan sorgu dizesi dahil olmak üzere, göreli yol gösterir. | /Marketing/foo.js?loggedin=true |
| İstek URI'si (göreli sorgu dizesi olmadan) | % {URI} | İstenen içerik göreli yolunu gösterir. <br /><br/>Anahtar bilgileri:<br />-Bu göreli yolu sorgu dizesi dışlar.<br />-Bu göreli yol URL yeniden yazdırmaya yansıtır. Bir URL aşağıdaki koşullar altında yazılacaktır:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-URL yeniden yazma özelliği: Bu özellik istek URI'si tanımlanan göreli yolu yeniden yazar.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-Edge CNAME URL'si: Bu istek türü için karşılık gelen CDN URL'sine yeniden yazılmıştır. |/800001/corigin/rewrittendir/foo.js |
| İstek URI'si | % {isteği} | İstek açıklar. <br />Sözdizimi: `HTTPMethod RelativePath Protocol` | /Marketing/foo.js?loggedin=true HTTP/1.1 Al |
| Yanıt üstbilgi değeri | % {resp_&lt;ResponseHeader&gt;} | Tarafından tanımlanan yanıt üstbilgisi karşılık gelen değer verir &lt;ResponseHeader&rt; terim. <br /><br />Yanıt üstbilgisinin adı bir tire (örneğin, User-Agent) içeriyorsa, alt çizgi (örneğin, User_Agent) ile değiştirin. | Örnek Kullanım: % {resp_Content_Length}<br /><br />Örnek değer: 100 |

## <a name="usage"></a>Kullanım
Aşağıdaki tabloda bir HTTP değişkeni belirtmek için uygun söz dizimi açıklanmıştır.


| Sözdizimi | Örnek | Açıklama |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | % {konak} | Belirtilen karşılık gelen tüm değer almak için şu sözdizimini kullanın &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | % {konak} | Belirtilen karşılık gelen tüm değer söz konusu ayarlamak için şu sözdizimini kullanın &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{Host/=^www\.([^\.] +)\.([^\.:] +) /cdn.$2.$3:80} | Bir normal ifade kullanın &lt;HTTPVariableDelimiterExpression&gt; değiştirmek, silmek veya bir HTTP değişkenin değerini değiştirmek. |

HTTP değişken adları yalnızca alfasayısal karakterler ve alt çizgi destekler. Desteklenmeyen karakterler alt çizgileri dönüştürün.

## <a name="delimiter-reference"></a>Sınırlayıcı başvurusu
Aşağıdaki etkileri birini elde etmek için bir HTTP değişkeni sonra bir ayırıcısı belirtilebilir:

- Değişkenle ilişkili değer dönüştürün.

     Örnek: tüm değer küçük harfe Dönüştür.

- Değişkenle ilişkili değeri silin.

- Değişkenle ilişkili değeri yönetme.

     Örnek: normal ifadeler HTTP değişkeni ile ilişkilendirilmiş değerini değiştirmek için kullanın.

Sınırlayıcılar aşağıdaki tabloda açıklanmıştır.

| Sınırlayıcı | Açıklama |
| --------- | ----------- |
| := | Varsayılan değer ya da olduğunda değişkene atanır gösterir: <br />-Eksik <br />-NULL olarak ayarlayın. |
| :+ | Varsayılan bir değer için bir değer atandıktan sonra değişkenine atanan olduğunu gösterir. |
| : | Bir dizenin değişkenine atanan değeri genişletilecek gösterir. |
| # | Değişkenle ilişkili değerin başında bulunduğunda bu sınırlayıcı sonra belirtilen desenle silinmesi gerektiğini gösterir. |
| % | Değişkenle ilişkili değer sonunda bulunduğunda bu sınırlayıcı sonra belirtilen desenle silinmesi gerektiğini gösterir. <br />Bu tanım yalnızca % simgenin ayırıcı olarak kullanıldığında geçerlidir. |
| / | Bir HTTP değişkeni veya desen sınırlandırır. |
| // | Bulun ve belirtilen desenle tüm örneklerini değiştirin. |
| /= | Bulma, kopyalama ve belirtilen deseninin tüm yeniden yazın. |
| , | Küçük harfe HTTP değişkeni ile ilişkili değer dönüştürün. |
| ^ | Büyük harfe HTTP değişkeni ile ilişkili değer dönüştürün. |
| ,, | Belirtilen karakter küçük HTTP değişkeni ile ilişkilendirilmiş değeri tüm örneklerini dönüştürün. |
| ^^ | Belirtilen karakter büyük harfe HTTP değişkeni ile ilişkilendirilmiş değeri tüm örneklerini dönüştürün. |

## <a name="exceptions"></a>Özel durumlar
Aşağıdaki tabloda bir HTTP değişkeni olarak belirtilen metin kabul değil koşullar açıklanmaktadır.

| Koşul | Açıklama | Örnek |
| --------- | ----------- | --------|
| Kaçış % simgesi | Yüzde sembol kullanımı ile bir ters eğik çizgi kaçış. <br />Örnek değer sağındaki bir HTTP değişkeni değil de, bir hazır değer olarak kabul edilir.| \%{konak} |
| Bilinmeyen değişkenleri | Boş bir dize her zaman için bilinmeyen değişkenleri döndürülür. | % {unknownvariable} |
| Geçersiz karakterler veya sözdizimi | Geçersiz karakterler veya sözdizimi içeriyor değişkenleri değişmez değerler kabul edilir. <br /><br />Örnek #1: Belirtilen değer geçersiz bir karakter içeriyor (örneğin,-). <br /><br />Örnek #2: Belirtilen değer bir çift süslü ayraçlar içerir. <br /><br />Örnek #3: Kapanış kuşak belirtilen değeri eksik.<br /> | Örnek #1: % {resp_user-agent} <br /><br />Örnek #2: % {{ana}} <br /><br />#3. örnek: % {konak |
| Değişken adı eksik | Bir değişken belirtilmediyse, bir NULL değer her zaman döndürülür. | %{} |
| Sondaki karakterler | Bir değişken izi karakter değişmez değerleri olarak kabul edilir. <br />Sağa örnek değer değişmez değer kabul edilecek sonunda bir küme parantezi içerir. | % {konak}} |

## <a name="setting-default-header-values"></a>Varsayılan üstbilgi değerleri ayarlama
Aşağıdaki koşullardan herhangi biri karşılaması durumunda varsayılan bir değer için bir başlık atanabilir:
- Eksik/ayarlama
- NULL olarak ayarlayın.

Aşağıdaki tabloda, varsayılan değer tanımlama açıklar.

| Koşul | Sözdizimi | Örnek | Açıklama |
| --------- | ------ | --------| ----------- |
| Aşağıdaki koşullardan herhangi biri karşıladığında üstbilgi varsayılan bir değere ayarlayın: <br /><br />-Üstbilgi eksik <br /><br />-Üstbilgi değeri NULL olarak ayarlanır.| % {Değişken: = değer} | % {http_referer: = belirtilmeyen} | Başvuran üstbilgi yalnızca ayarlanacak *belirtilmeyen* olduğu zaman eksik ya da ayarlanmış null. Bunu ayarlarsanız, hiçbir eylem gerçekleşir. |
| Eksik olduğunda üstbilgi varsayılan bir değere ayarlayın. | % {Değişken = değer} | % {http_referer = belirtilmeyen} | Başvuran üstbilgi yalnızca ayarlanacak *belirtilmeyen* olduğu zaman eksik. Bunu ayarlarsanız, hiçbir eylem gerçekleşir. |
| Aşağıdaki koşullardan birini karşılamıyor zaman üstbilgisi varsayılan bir değere ayarlayın: <br /><br />-Eksik<br /><br /> -NULL olarak ayarlayın. | % {Değişken: + değeri} | % {http_referer: + belirtilmeyen} | Başvuran üstbilgi yalnızca ayarlanacak *belirtilmeyen* bir değer atandığında için. Eksik ya da ayarlanmış olması durumunda hiçbir eylem gerçekleşir null. |

## <a name="manipulating-variables"></a>Değişkenleri düzenleme
Değişkenleri aşağıdaki yöntemlerle yönetilebilir:

- Alt dizeler genişletme
- Desenler kaldırma

### <a name="substring-expansion"></a>Substring genişletme
Varsayılan olarak, bir değişken tam değerine genişletilir. Yalnızca değişken değerinin bir dizenin genişletmek için aşağıdaki sözdizimini kullanın.

`%<Variable>:<Offset>:<Length>}`

Anahtar bilgileri:

- Uzaklık terimine atanan değer alt dizeyi başlangıç karakteri belirler:

     - Pozitif: Başlangıç karakteri alt dizenin ilk karakter dizesi, hesaplanır.
     - Sıfır: Başlangıç alt dizenin ilk karakter dizesi, karakterdir.
     - Negatif: Alt dizeyi başlangıç karakteri dizedeki son karakter hesaplanır.

- Alt dizenin uzunluğunu tarafından belirlenen *uzunluğu* terim:

     - Atlanmış: uzunluk terim atlama başlangıç karakteri dize sonu arasındaki tüm karakterleri eklemek alt dizeyi sağlar.
     - Pozitif: Başlangıç karakteri alt dizeyi sağa uzunluğunu belirler.
     - Negatif: Başlangıç karakteri alt dizeyi sol uzunluğunu belirler.

#### <a name="examples"></a>Örnekler:

Aşağıdaki örnekte aşağıdaki örnek istek URL'SİNDE dayanır:

`https://cdn.mydomain.com/folder/marketing/myconsultant/proposal.html`

Aşağıdaki dizeyi değişkenleri düzenleme için çeşitli yöntemler gösterir:

`https://www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm`

Örnek istek URL temel alınarak, yukarıdaki değişken işleme aşağıdaki değeri oluşturur:

`https://www.mydomain.com/mobile/marketing/proposal.htm`


### <a name="pattern-removal"></a>Desen kaldırma
Belirli bir desenle eşleşen metnin başına veya sonuna kadar bir değişkenin değeri kaldırılabilir.

| Sözdizimi | Eylem |
| ------ | ------ |
| % {#Pattern değişkeni} | Bir değişkenin değeri başında belirtilen desenle bulunduğunda metin kaldırın. |
| % {Değişken % düzeni} | Belirtilen desen bir değişkenin değeri sonunda bulunduğunda metin kaldırın. |

#### <a name="example"></a>Örnek:

Bu örnek senaryoda request_urı değişkeni ayarlanır:

`/800001/myorigin/marketing/product.html?language=en-US`

Aşağıdaki tabloda, bu söz dizimini nasıl çalıştığını gösterir.

| Örnek sözdizimi | Sonuçlar |
| ------------- | ------- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/Marketing/Product.HTML?Language=en-us | Değişken deseni ile başladığından değiştirildi. |
| % {request_urı % html} htm | /800001/myorigin/Marketing/Product.HTML?Language=en-us | Değişken deseni ile bitmiyor olduğundan, hiçbir değişiklik vardı.|

### <a name="find-and-replace"></a>Bul ve Değiştir
Bul ve Değiştir sözdizimi aşağıdaki tabloda açıklanmıştır.

| Sözdizimi | Eylem |
| ------ | ------ |
| %{Variable/Find/Replace} | Bulun ve belirtilen desenle ilk örneğini değiştirin. |
| %{Variable//Find/Replace} | Bulun ve belirtilen deseninin tüm değiştirin. |
| % {Değişken ^} |Değerin tamamını büyük harfe Dönüştür. |
| % {Değişken ^ Bul} | Belirtilen desen ilk örneğini büyük harfe Dönüştür. |
| % {, Değişkeni} | Değerin tamamını küçük harfe Dönüştür. |
| % {Değişkeni, Bul} | Belirtilen desen ilk örneğini küçük harflere dönüştürür. |

### <a name="find-and-rewrite"></a>Bul ve yeniden yazma
Bul ve Değiştir üzerinde bir değişim için onu yeniden yazma işlemi, belirtilen desenle eşleşen metni kullanın. Bul ve yeniden yazma sözdizimi aşağıdaki tabloda açıklanmıştır.

| Sözdizimi | Eylem |
| ------ | ------ |
| %{Variable/=Find/rewrite} | Bulma, kopyalama ve belirtilen deseninin tüm yeniden yazın. |
| %{Variable/^Find/rewrite} | Bulma, kopyalama ve değişkeni başlangıcında olduğunda belirtilen deseni yeniden yazın. |
| %{Variable/$Find/rewrite} | Bulma, kopyalama ve değişken sonunda oluştuğunda belirtilen desenle yeniden yazın. |
| %{Variable/Find} | Bul ve belirtilen deseninin tüm silin. |

Anahtar bilgileri:

- Bir tamsayı (örneğin, $1) ve ardından dolar işareti belirterek belirtilen desenle eşleşen metni genişletin.

- Birden çok desenleri belirtilebilir. Kendisine atanmış tamsayı düzeni belirtilen sırasını belirler. Aşağıdaki örnekte, ilk desen eşleşmeleri "www.," ikinci deseni ile eşleşen ikinci düzey etki alanı ve üst düzey etki alanını üçüncü desenle eşleşen:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- Yeniden değeri metin ve bu yer tutucuları herhangi bir birleşimini içerebilir.

    Önceki örnekte, ana bilgisayar adı için yeniden yazılmıştır `cdn.$2.$3:80` (örneğin, cdn.mydomain.com:80).

- Bir desen yer tutucu (örneğin, $1) durumunda aşağıdaki bayraklar değiştirilebilir:
     - U: genişletilmiş değeri büyük.

         Örnek sözdizimi:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: genişletilmiş değeri küçük.

         Örnek sözdizimi:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- Bir işleç düzeni önce belirtilmiş olması gerekir. Belirtilen işleci düzeni Yakalama davranışını belirler:

     - `=`: Belirtilen deseninin tüm yakalanan yeniden yazılmıştır ve gereken gösterir.
     - `^`: Yalnızca belirtilen desenle başlıyor metin yakalanan olduğunu gösterir.
     - `$`: Belirli bir desen ile biten metin yakalama olacağını gösterir.
 
- Atlarsanız */yeniden* değeri, desenle eşleşen metin silinir.


