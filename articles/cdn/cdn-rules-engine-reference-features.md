---
title: Azure CDN kuralları altyapısı özellikleri | Microsoft Docs
description: Azure CDN başvuru belgelerine altyapısı özellikleri kuralları.
services: cdn
documentationcenter: ''
author: Lichard
manager: akucer
editor: ''
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: rli
ms.openlocfilehash: fd670e3b01812b7fa8fc708a02d02210b598ac6a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-cdn-rules-engine-features"></a>Azure CDN kuralları özellikleri altyapısı
Kullanılabilir özelliklerin ayrıntılı açıklamaları Azure içerik teslim ağı (CDN) için bu makalede listelenmektedir [kurallar altyapısı](cdn-rules-engine.md).

Üçüncü bir kuralın parçası özelliğidir. Bir özellik eşleşme koşullar kümesi tarafından tanımlanan istek türü için uygulanan eylem türünü tanımlar.

## <a name="access-features"></a>Erişim özellikleri

Bu özellikler, içeriğe erişimi denetlemek için tasarlanmıştır.

Ad | Amaç
-----|--------
[(403) erişimini engelle](#deny-access-403) | 403 Yasak yanıtta reddedilen tüm isteği olup olmadığını belirler.
[Belirteç kimlik doğrulama](#token-auth) | Belirteç tabanlı kimlik doğrulaması için bir istek uygulandığını belirler.
[Belirteç kimlik doğrulama reddi kodu](#token-auth-denial-code) | Belirteç tabanlı kimlik doğrulaması nedeniyle bir istek reddedildiğinde kullanıcıya dönen yanıtının türünü belirler.
[Belirteç kimlik doğrulama URL çalışması yoksay](#token-auth-ignore-url-case) | Belirteç tabanlı kimlik doğrulaması ile yapılan URL karşılaştırmaları büyük küçük harfe duyarlı olup olmadığını belirler.
[Belirteç kimlik doğrulama parametresi](#token-auth-parameter) | Belirteç tabanlı kimlik doğrulaması sorgu dizesi parametresi yeniden adlandırılmış olup olmadığını belirler.


## <a name="caching-features"></a>Önbelleğe alma özellikleri

Bu özellikler, ne zaman ve nasıl içeriğin önbellekte özelleştirmek için tasarlanmıştır.

Ad | Amaç
-----|--------
[Bant genişliği parametreleri](#bandwidth-parameters) | Bant genişliği azaltma parametreler (örneğin, ec_rate ve ec_prebuf) etkin olup olmadığını belirler.
[Bant genişliği azaltma](#bandwidth-throttling) | Noktası bulunma tarafından (POP) sağlanan yanıt için bant genişliği kısıtlar.
[Önbelleği atlama](#bypass-cache) | İstek önbelleğe almayı Atla gerekmediğini belirler.
[Cache-Control üstbilgisi işleme](#cache-control-header-treatment) | Nesil denetimleri `Cache-Control` dış Max-Age özelliği etkin olduğunda POP tarafından üstbilgileri.
[Önbellek anahtarı sorgu dizesi](#cache-key-query-string) | Önbellek anahtarını içerir veya dışlar bir istekle ilişkili sorgu dizesi parametreleri belirler.
[Önbellek anahtarı yeniden yazma](#cache-key-rewrite) | Bir istekle ilişkili önbellek anahtarını yeniden yazar.
[Önbellek dolgu tamamlayın](#complete-cache-fill) | POP üzerinde istek sonuçları yokken Kısmi önbellek isabetsizliği ne olacağını belirler.
[Sıkıştırma dosya türleri](#compress-file-types) | Sıkıştırılmış dosya için dosya biçimlerini sunucuda tanımlar.
[Varsayılan iç Max-Age](#default-internal-max-age) | Varsayılan, max-age aralığı POP için kaynak sunucusu önbellek COLLECTION belirler.
[Üstbilgi işleme süresi](#expires-header-treatment) | Nesil denetimleri `Expires` dış Max-Age özelliği etkin olduğunda POP tarafından üstbilgileri.
[Dış Maksimum yaş](#external-max-age) | POP önbellek COLLECTION tarayıcıya max-age aralığını belirler.
[İç Max-Age zorla](#force-internal-max-age) | Max-age aralığı POP için kaynak sunucusu önbellek COLLECTION belirler.
[H.264 desteği (HTTP aşamalı indirme)](#h264-support-http-progressive-download) | İçerik akışını sağlamak için kullanılabilir H.264 dosya biçimleri türlerini belirler.
[Uy No Cache isteği](#honor-no-cache-request) | Bir HTTP istemcinin no-cache istekleri kaynak sunucuya iletilip iletilmeyeceğini belirler.
[Kaynak No-Cache yoksay](#ignore-origin-no-cache) | CDN bir kaynak sunucudan sunulan belirli yönergeleri yoksayar olup olmadığını belirler.
[Unsatisfiable aralıkları yoksay](#ignore-unsatisfiable-ranges) | Bir istek 416 İstenen aralık değil sağlanabilir durum kodu oluşturduğunda istemcilere döndürülen yanıt belirler.
[İç Max-eski](#internal-max-stale) | POP kaynak sunucu ile önbelleğe alınmış varlık düzeltin erişemediğinde POP denetimleri normal sona erme süresini geçen ne kadar süreyle önbelleğe alınan varlık hizmet edilebilir.
[Kısmi önbellek paylaşımı](#partial-cache-sharing) | İstek kısmen önbelleğe alınmış içeriği oluşturmak olup olmadığını belirler.
[Önbelleğe alınmış içeriği prevalidate](#prevalidate-cached-content) | TTL'si süresi dolmadan önce önbelleğe alınmış içeriği erken yeniden doğrulanması için uygun olup olmadığını belirler.
[Sıfır bayt önbellek dosyaları Yenile](#refresh-zero-byte-cache-files) | 0-bayt önbellek varlık için bir HTTP istemcinin isteğini POP tarafından nasıl işleneceğini belirler.
[Önbelleğe alınabilir durum kodları](#set-cacheable-status-codes) | Önbelleğe alınmış içeriği sonuçlanabilir durum kodları kümesini tanımlar.
[Eski içerik teslim hata](#stale-content-delivery-on-error) | İstenen içerik müşteri kaynak sunucudan alınırken bir hata önbellek yeniden doğrulanması sırasında veya ortaya çıktığında önbelleğe alınan içerik teslim süresi olup olmadığını belirler.
[Revalidate sırasında eski](#stale-while-revalidate) | POP COLLECTION gerçekleştirilirken eski istemci istemciye hizmet sağlayarak performansı geliştirir.

## <a name="comment-feature"></a>Açıklama özelliği

Bu özellik, bir kural içindeki ek bilgi sağlamak için tasarlanmıştır.

Ad | Amaç
-----|--------
[Açıklama](#comment) | İçindeki bir kural eklemek Not sağlar.
 
## <a name="header-features"></a>Üstbilgi Özellikleri

Bu özellikler, eklemek, değiştirmek veya istek veya yanıt üstbilgileri silmek için tasarlanmıştır.

Ad | Amaç
-----|--------
[Age yanıtı üstbilgisi](#age-response-header) | Age yanıtı üstbilgisi istemciye gönderilen yanıta dahil edilip edilmeyeceğini belirler.
[Önbellek yanıt üstbilgilerini hata ayıklama](#debug-cache-response-headers) | Yanıt için önbellek ilkesini için istenen varlık bilgileri sağlayan X EC Debug yanıt üst bilgisi içerebilir olup olmadığını belirler.
[İstemci istek üstbilgisi değiştirme](#modify-client-request-header) | Geçersiz kılar, ekler veya bir istekten bir üstbilgi siler.
[İstemci yanıt üstbilgisi değiştirme](#modify-client-response-header) | Geçersiz kılar, ekler veya bir yanıt üstbilgi siler.
[İstemci IP özel üstbilgi ayarlayın](#set-client-ip-custom-header) | İsteği özel istek üstbilgisi olarak eklenecek istemcinin IP adresi sağlar.


## <a name="logging-features"></a>Günlüğe kaydetme özellikleri

Bu özellikler, ham günlük dosyalarında depolanan verileri özelleştirmek için tasarlanmıştır.

Ad | Amaç
-----|--------
[Özel günlük alanı 1](#custom-log-field-1) | Biçim ve ham bir günlük dosyası özel günlük alanına atanan içeriği belirler.
[Günlük sorgu dizesi](#log-query-string) | Bir sorgu dizesi erişim günlükleri URL'de birlikte depolanır olup olmadığını belirler.


<!---
## Optimize

These features determine whether a request will undergo the optimizations provided by Edge Optimizer.

Name | Purpose
-----|--------
Edge Optimizer | Determines whether Edge Optimizer can be applied to a request.
Edge Optimizer – Instantiate Configuration | Instantiates or activates the Edge Optimizer configuration associated with a site.

### Edge Optimizer
**Purpose:** Determines whether Edge Optimizer can be applied to a request.

If this feature has been enabled, then the following criteria must also be met before the request will be processed by Edge Optimizer:

- The requested content must use an edge CNAME URL.
- The edge CNAME referenced in the URL must correspond to a site whose configuration has been activated in a rule.

This feature requires the ADN platform and the Edge Optimizer feature.

Value|Result
-|-
Enabled|Indicates that the request is eligible for Edge Optimizer processing.
Disabled|Restores the default behavior. The default behavior is to deliver content over the ADN platform without any additional processing.

**Default Behavior:** Disabled
 

### Edge Optimizer - Instantiate Configuration
**Purpose:** Instantiates or activates the Edge Optimizer configuration associated with a site.

This feature requires the ADN platform and the Edge Optimizer feature.

Key information:

- Instantiation of a site configuration is required before requests to the corresponding edge CNAME can be processed by Edge Optimizer.
- This instantiation only needs to be performed a single time per site configuration. A site configuration that has been instantiated will remain in that state until the Edge Optimizer – Instantiate Configuration feature that references it is removed from the rule.
- The instantiation of a site configuration does not mean that all requests to the corresponding edge CNAME will automatically be processed by Edge Optimizer. The Edge Optimizer feature determines whether an individual request will be processed.

If the desired site does not appear in the list, then you should edit its configuration and verify that the Active option has been marked.

**Default Behavior:** Site configurations are inactive by default.
--->

## <a name="origin-features"></a>Kaynak özellikleri

Bu özellikler, CDN kaynak sunucu ile nasıl iletişim kurduğu denetlemek için tasarlanmıştır.

Ad | Amaç
-----|--------
[En fazla tutma isteği](#maximum-keep-alive-requests) | Kapalı olduğu önce en fazla istek tutma bağlantı sayısını tanımlar.
[Proxy özel üstbilgileri](#proxy-special-headers) | POP bir kaynak sunucuya iletilir CDN özgü istek üstbilgileri kümesini tanımlar.


## <a name="specialty-features"></a>Özel Özellikler

Bu özellikler, Gelişmiş kullanıcılar için gelişmiş işlevsellik sağlar.

Ad | Amaç
-----|--------
[Önbelleğe alınabilir HTTP yöntemleri](#cacheable-http-methods) | Ağ üzerinde önbelleğe ek HTTP yöntemleri kümesini belirler.
[Önbelleğe alınabilir istek gövdesi boyutu](#cacheable-request-body-size) | Bir POST yanıt önbelleğe olup olmadığını belirlemek için eşiğini tanımlar.
[Kullanıcı değişkeni](#user-variable) | Yalnızca dahili kullanım için.

 
## <a name="url-features"></a>URL özellikleri

Bu özellikleri yeniden yönlendirilen veya farklı bir URL'ye yeniden yazılmıştır isteğine izin verin.

Ad | Amaç
-----|--------
[Yeniden yönlendirmeleri izleyin](#follow-redirects) | İstekleri bir müşteri kaynak sunucu tarafından döndürülen konum üstbilgisi içinde tanımlı ana bilgisayar adı için yeniden yönlendirilen olup olmadığını belirler.
[URL yeniden yönlendirme](#url-redirect) | Konum üstbilgisi keşfi yönlendirir.
[URL yeniden yazma](#url-rewrite)  | İstek URL'sini yeniden yazar.



## <a name="azure-cdn-rules-engine-features-reference"></a>Azure CDN kuralları özellikleri başvurusunu altyapısı

---
### <a name="age-response-header"></a>Age yanıtı üstbilgisi
**Amaç**: Age yanıtı üstbilgisi istemciye gönderilen yanıta dahil edilip edilmeyeceğini belirler.
Değer|Sonuç
--|--
Etkin | Age yanıtı üstbilgisi istemciye gönderilen yanıtı dahil edilir.
Devre dışı | Age yanıtı üstbilgisi istemciye gönderilen yanıtı gelen dışlandı.

**Varsayılan davranış**: devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="bandwidth-parameters"></a>Bant genişliği parametreleri
**Amaç:** bant genişliği azaltma parametreler (örneğin, ec_rate ve ec_prebuf) etkin olup olmadığını belirler.

Bant genişliği azaltma parametreleri, bir istemcinin isteğini için veri aktarım hızı için özel bir oranı sınırlı olup olmadığını belirleyin.

Değer|Sonuç
--|--
Etkin|Bant genişliği azaltma isteklerini kabul etmeniz POP sağlar.
Devre dışı|Bant genişliği azaltma parametreleri yoksaymak POP neden olur. İstenen içerik normalde hizmet (diğer bir deyişle, bant genişliği azaltma olmadan).

**Varsayılan davranış:** etkin.
 
[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="bandwidth-throttling"></a>Bant genişliği azaltma
**Amaç:** POP tarafından sağlanan yanıt için bant genişliği kısıtlar.

Aşağıdaki seçeneklerden her ikisinin bant genişliği azaltma yukarı doğru şekilde ayarlamak için tanımlanmalıdır.

Seçenek|Açıklama
--|--
KB / saniye|Bu seçeneği yanıt sunmak için kullanılabilecek en fazla bant genişliğiyle (Kb / saniye) ayarlayın.
Prebuf saniye|POP bant genişliği daraltma kadar beklenecek saniye sayısı için bu seçeneği belirleyin. Bu süre sınırsız bant genişliği amacı, bant genişliği azaltma nedeniyle görüntüsü gidip gelir veya arabelleğe alma sorunları yaşayan bir medya oynatıcı engellemektir.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="bypass-cache"></a>Önbelleği atlama
**Amaç:** isteği önbelleğe almayı Atla gerekmediğini belirler.

Değer|Sonuç
--|--
Etkin|İçerik, daha önce üzerinde POP önbelleğe olsa bile kaynak sunucuya atlayabilir tüm istekleri neden olur.
Devre dışı|POP önbellek varlıklar, yanıt üstbilgilerini tanımlanmış önbellek İlkesi göre neden olur.

**Varsayılan davranış:**

- **HTTP büyük:** devre dışı

<!---
- **ADN:** Enabled

--->

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cacheable-http-methods"></a>Önbelleğe alınabilir HTTP yöntemleri
**Amaç:** ağda önbelleğe ek HTTP yöntemleri kümesini belirler.

Anahtar bilgileri:

- Bu özellik GET yanıtları her zaman önbelleğe alınacağını varsayar. Sonuç olarak, GET HTTP yöntemini bu özelliği ayarlanırken dahil olmamalıdır.
- Bu özellik yalnızca POST HTTP yöntemini destekler. Bu özellik ayarlayarak POST yanıt önbelleğe almayı etkinleştir `POST`.
- Varsayılan olarak, yalnızca, gövde 14 KB'den küçük istekleri önbelleğe alınır. En büyük istek gövdesi boyutunu ayarlamak için alınabilir istek gövdesi boyutu özelliğini kullanın.

**Varsayılan davranış:** yalnızca GET yanıtlarını önbelleğe alınır.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cacheable-request-body-size"></a>Önbelleğe alınabilir istek gövdesi boyutu
**Amaç:** POST yanıt önbelleğe olup olmadığını belirlemek için eşiğini tanımlar.

Bu eşik en büyük istek gövdesi boyutu belirterek belirlenir. Daha büyük bir istek gövdesini içeren istekleri önbelleğe alınmaz.

Anahtar bilgileri:

- Bu özellik yalnızca POST yanıtlarını önbelleğe alma işlemi için uygun olduğunda geçerlidir. POST isteği önbelleğe almayı etkinleştirmek için alınabilir HTTP yöntemleri özelliğini kullanın.
- İstek gövdesi için dikkate alınır:
    - x-www-form-urlencoded değerleri
    - Benzersiz bir önbellek anahtar sağlama
- Büyük en fazla istek gövdesi boyutu tanımlama veri teslim performansını etkileyebilir.
    - **Önerilen değer:** 14 Kb
    - **Minimum değer:** 1 Kb

**Varsayılan davranış:** 14 Kb

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-control-header-treatment"></a>Cache-Control üstbilgisi işleme
**Amaç:** oluşturulmasını denetler `Cache-Control` dış Max-Age özelliği etkin olduğunda POP tarafından üstbilgileri.

Bu tür bir yapılandırma elde etmek için en kolay yolu dış Max-Age ve Cache-Control üstbilgisi işleme özellikleri aynı deyiminde yerleştirmektir.

Değer|Sonuç
--|--
Üzerine Yaz|Aşağıdaki eylemler gerçekleşir sağlar:<br/> -Üzerine yazar `Cache-Control` kaynak sunucu tarafından üretilen üstbilgi. <br/>-Ekler `Cache-Control` üstbilgi üretilen yanıta dış Max-Age özelliğiyle.
Doğrudan Geçiş|Sağlar `Cache-Control` dış Max-Age özelliği tarafından üretilen üstbilgi hiçbir zaman yanıta eklenir. <br/> Kaynak sunucu oluşturursa bir `Cache-Control` üstbilgisi, bunu geçirir aracılığıyla son kullanıcıya. <br/> Kaynak sunucu üretmek değil, bir `Cache-Control` üstbilgi sonra bu seçeneği değil içerecek şekilde yanıt üstbilgisi neden olabilir bir `Cache-Control` üstbilgi.
Eksik varsa ekleyin.|Varsa bir `Cache-Control` üstbilgi kaynak sunucudan alınmadı sonra bu seçeneği ekler `Cache-Control` üstbilgi dış Max-Age özelliği tarafından üretilen. Bu seçenek, tüm varlıklar atanan sağlamak için yararlıdır bir `Cache-Control` üstbilgi.
Kaldır| Bu seçenek sağlar bir `Cache-Control` başlık üstbilgisi Yanıtla dahil değildir. Varsa bir `Cache-Control` üstbilgi zaten atanmış sonra üstbilgi yanıttan kaldırılır.

**Varsayılan davranış:** üzerine yazın.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-key-query-string"></a>Önbellek anahtarı sorgu dizesi
**Amaç:** önbellek anahtarını içerir veya dışlar bir istekle ilişkili sorgu dizesi parametreleri belirler.

Anahtar bilgileri:

- Bir veya daha fazla sorgu dizesi parametresi adları belirtin ve her parametre adı tek bir boşlukla ayırın.
- Bu özellik, sorgu dizesi parametreleri dahil veya önbellek anahtarından hariç olup olmadığını belirler. Aşağıdaki tabloda her seçenek için ek bilgiler sağlanmaktadır.

Tür|Açıklama
--|--
 Ekle|  Belirtilen her parametre önbellek anahtarında dahil olduğunu gösterir. Bu özelliği tanımlı bir sorgu dizesi parametresi için benzersiz bir değer içeren her bir istek için benzersiz bir önbellek anahtarı oluşturulur. 
 Tüm içerir  |Benzersiz sorgu dizesi içeren bir varlık için her istek için benzersiz bir önbellek anahtarını oluşturulduğunu gösterir. Küçük bir önbellek isabet yüzdesi neden olabilir çünkü bu yapılandırma türü genellikle önerilmez. Daha fazla isteklere hizmet gerekir çünkü düşük bir önbellek isabet sayısı kaynak sunucu üzerindeki yükü artırır. Bu yapılandırma "benzersiz önbelleği" sorgu dizesi önbelleğe alma sayfasında olarak bilinen önbelleğe alma davranışını çoğaltır. 
 Dışla | Yalnızca belirtilen parametreler önbellek anahtarından dışlandı gösterir. Diğer tüm sorgu dizesi parametreleri önbellek anahtarında bulunur. 
 Tüm Dışla  |Tüm sorgu dizesi parametreleri önbellek anahtarından hariç tutulan gösterir. Bu yapılandırma, önbelleğe alma davranışı sorgu dizesini önbelleğe alma sayfasında "standart-cache" varsayılan çoğaltır.  

Kurallar altyapısı içinde sorgu dizesini önbelleğe alma uygulanan şekilde özelleştirmenizi sağlar. Örneğin, sorgu dizesi önbelleğe alma'nün yalnızca belirli konumlara veya dosya türleri üzerinde gerçekleştirilir belirtebilirsiniz.

"No-cache" sorgu dizesini önbelleğe alma davranışı sorgu dizesini önbelleğe alma sayfasında çoğaltmak için bir URL sorgu joker karakter eşleştirme koşul ve bir atlama önbellek özelliği içeren bir kural oluşturun. URL sorgu joker karakter eşleştirme koşulu için bir yıldız işareti (*) belirtin.

>[!IMPORTANT] 
> Bu hesaptaki herhangi bir yol için belirteci yetkilendirme etkinleştirilirse, standart Önbellek modu sorgu dizesini önbelleğe alma işlemi için kullanılan tek modudur. Daha fazla bilgi için bkz. [Sorgu dizeleri içeren Azure CDN önbelleğe alma davranışını kontrol etme](cdn-query-string-premium.md).

#### <a name="sample-scenarios"></a>Örnek senaryolar

Aşağıdaki örnek kullanım bu özellik için bir örnek istek ve varsayılan önbellek anahtarını sağlar:

- **Örnek istek:** http://wpc.0001.&lt; Etki alanı&gt;/800001/Origin/folder/asset.htm?sessionid=1234 & dili tr & UserID = 01 =
- **Varsayılan önbellek anahtarını:** /800001/Origin/folder/asset.htm

##### <a name="include"></a>Ekle

Örnek Yapılandırması:

- **Tür:** içerir
- **Parametre:** dili

Bu tür bir yapılandırma aşağıdaki sorgu dizesi parametresi önbellek anahtarını oluşturur:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Tüm içerir

Örnek Yapılandırması:

- **Tür:** tüm içerir

Bu tür bir yapılandırma aşağıdaki sorgu dizesi parametresi önbellek anahtarını oluşturur:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Dışla

Örnek Yapılandırması:

- **Tür:** Dışla
- **Parametre:** SessionID UserID

Bu tür bir yapılandırma aşağıdaki sorgu dizesi parametresi önbellek anahtarını oluşturur:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Tüm Dışla

Örnek Yapılandırması:

- **Tür:** tüm Dışla

Bu tür bir yapılandırma aşağıdaki sorgu dizesi parametresi önbellek anahtarını oluşturur:

    /800001/Origin/folder/asset.htm

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-key-rewrite"></a>Önbellek anahtarı yeniden yazma
**Amaç:** bir istekle ilişkili önbellek anahtarını yeniden yazar.

Önbellek anahtarı önbelleğe alma amacıyla bir varlığı tanımlayan göreli bir yoldur. Diğer bir deyişle, sunucuları, yolunu göre bir varlık önbelleğe alınmış bir sürümü için önbellek-anahtara göre tanımlanan denetleyin.

Bu özellik, aşağıdaki seçeneklerden her ikisinin tanımlayarak yapılandırın:

Seçenek|Açıklama
--|--
Orijinal yol| Göreli yol önbellek anahtarını yeniden yazılmıştır istek türlerini tanımlayın. Göreli bir yol, temel kaynak yolu seçerek ve ardından bir normal ifade deseni tanımlayan tanımlanabilir.
Yeni bir yol|Yeni önbellek anahtarı için göreli yol tanımlayın. Göreli bir yol, temel kaynak yolu seçerek ve ardından bir normal ifade deseni tanımlayan tanımlanabilir. Bu göreli yol HTTP değişkenleri kullanımı ile dinamik olarak oluşturulabilir
**Varsayılan davranış:** bir isteğin önbellek anahtar isteğin URI tarafından belirlenir.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="comment"></a>Açıklama
**Amaç:** içindeki bir kural eklemek Not sağlar.

Bu özellik için bir kullanım, bir kural veya neden belirli bir koşul eşleşmiyor veya özellik kuralına eklenen genel amacı hakkında ek bilgi sağlamaktır.

Anahtar bilgileri:

- En fazla 150 karakter belirtilebilir.
- Yalnızca alfasayısal karakterler kullanın.
- Bu özellik, kural davranışını etkilemez. Yalnızca Burada ileride veya bu bilgileri kural gidermede yardımcı olabilecek sağlayabilir bir alanı sağlamak üzere tasarlanmıştır.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="complete-cache-fill"></a>Önbellek dolgu tamamlayın
**Amaç:** Kısmi önbellek isabetsizliği POP üzerinde bir istek sonuçlanır ne olacağını belirler.

Kısmi önbellek isabetsizliği tamamen POP'a yüklenmedi bir varlık için önbellek durumunu açıklar. Bir varlık üzerinde POP yalnızca kısmen önbelleğe alınmışsa, bu varlık için bir sonraki istekte yeniden kaynak sunucuya iletilir.
<!---
This feature is not available for the ADN platform. The typical traffic on this platform consists of relatively small assets. The size of the assets served through these platforms helps mitigate the effects of partial cache misses, since the next request will typically result in the asset being cached on that POP.

--->
Kısmi önbellek isabetsizliği genellikle bir kullanıcı bir indirme durdurur sonra veya yalnızca HTTP Aralık isteklerini kullanarak istenen varlıklar için oluşur. Bu özellik genellikle (örneğin, videoları) başından yüklenmez büyük varlıklar için kullanışlıdır. Sonuç olarak, bu özellik HTTP büyük platform üzerinde varsayılan olarak etkindir. Diğer tüm platformlarda devre dışı bırakılır.

Çünkü müşteri kaynak sunucu üzerindeki yükü azaltır ve hangi müşterilerin içeriğinizi karşıdan yükle hızını artırır HTTP büyük bir platform için varsayılan yapılandırmayı tutun.

Değer|Sonuç
--|--
Etkin|Varsayılan davranışını geri yükler. Varlık kaynak sunucusundan bir arka planda getirmeye başlatmak için POP zorlamak için varsayılan davranıştır. Sonrasında, varlık POP'ın yerel önbellekteki olacaktır.
Devre dışı|POP varlık için bir arka planda getirmeye gerçekleştirmesini engeller. Sonuç, bu bölgedeki bu varlık için bir sonraki istekte müşteri kaynak sunucudan istemek POP neden olur.

**Varsayılan davranış:** etkin.

#### <a name="compatibility"></a>Uyumluluk
Hangi önbelleğinde ayarları izlenen şekilde nedeniyle, bu özellik aşağıdaki eşleşme koşullarla ilişkili olamaz: 
- Sayı olarak
- İstemci IP Adresi
- Tanımlama bilgisi parametresi
- Tanımlama bilgisi parametresi Regex
- Ülke
- Cihaz
- Edge Cname
- Başvuran etki alanı
- İstek üstbilgisi değişmez değeri
- İstek üstbilgisi Regex
- İstek üstbilgisi joker karakter
- İstek yöntemi
- İstek düzeni
- URL sorgu değişmez değeri
- URL sorgu Regex
- URL sorgu joker karakter
- URL sorgu parametresi

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="compress-file-types"></a>Sıkıştırma dosya türleri
**Amaç:** sunucuda sıkıştırılan dosyalar için dosya biçimlerini tanımlar.

Bir dosya biçimi, Internet medya türünü (örneğin, Content-Type) kullanılarak belirtilebilir. Internet medya türü belirli bir varlık dosya biçimi tanımlamak sunucuların sağlar platformdan bağımsız meta verilerdir. Ortak Internet medya türleri listesi aşağıda verilmiştir.

Internet medya türü|Açıklama
--|--
Metin/düz|Düz metin dosyaları
metin/html| HTML dosyaları
metin/css|Geçişli stil sayfaları (CSS)
Uygulama/x-javascript|Javascript
Uygulama/javascript|Javascript
Anahtar bilgileri:

- Birden çok Internet medya türü, her biri tek bir boşluk ile sınırlandırma tarafından belirtin. 
- Bu özellik yalnızca, boyutu 1 MB'tan az varlıkları sıkıştırır. Daha büyük varlıklar sunucuları tarafından sıkıştırılmaz.
- Görüntü, video, gibi içerik ve ses ortam varlıkları (örneğin, JPG, MP3, MP4, vb.), belirli türde zaten sıkıştırılır. Varlık türlerinin ek sıkıştırmayı dosya boyutu önemli ölçüde etkilememesini nedeniyle bunları sıkıştırmayı etkinleştirmemeniz önerilir.
- Yıldız işareti gibi joker karakterler desteklenmez.
- Bu özellik için bir kural eklemeden önce bu kuralın uygulanacağı platform için sıkıştırma sayfasında sıkıştırma devre dışı seçeneğini ayarlamak emin olun.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="custom-log-field-1"></a>Özel günlük alanı 1
**Amaç:** biçimi ve ham bir günlük dosyası özel günlük alanına atanan içeriği belirler.

Bu özel alan hangi istek ve yanıt üstbilgi değerleri, günlük dosyalarında depolanır belirlemenize olanak sağlar.

Varsayılan olarak, özel günlük alan "x-ec_custom-1." olarak adlandırılır Bu alanın adını ham günlük ayarları sayfasından özelleştirilebilir.

İstek ve yanıt üst bilgileri belirtmek için biçim şu şekilde tanımlanır:

Üstbilgi türü|Biçimlendir|Örnekler
-|-|-
İstek üstbilgisi|%{[RequestHeader]()}[t]() | % {Kabul-Encoding} t <br/> {Başvuran} t <br/> % {Yetkilendirme} i
Yanıt Üst Bilgisi|%{[ResponseHeader]()}[o]()| % {Yaş} o <br/> % {Content-Type} o <br/> % {Tanımlama bilgisi} o

Anahtar bilgileri:

- Özel günlük alan üstbilgi alanları ve düz metin herhangi bir birleşimini içerebilir.
- Bu alanın geçerli karakterler aşağıdaki gibidir: alfasayısal (0-9, a-z ve A-Z), tire, iki nokta üst üste, noktalı, kesme, virgül, nokta, alt çizgi, eşittir işareti, parantez, köşeli ayraçlar ve boşluk. Süslü ayraçlar ve yüzde simge yalnızca bir üstbilgi alanı belirtmek için kullanılan olduğunda izin verilir.
- Yazım denetimi her belirtilen üstbilgi alanı istenen istek/yanıt üstbilgisi adı eşleşmelidir.
- Birden çok üstbilgi belirtmek istiyorsanız, bir ayırıcı her üstbilgisi belirtmek için kullanın. Örneğin, her başlığı için bir kısaltma kullanabilirsiniz:
    - AE: % {kabul-Encoding} i A: % {Yetkilendirme} i u: % {Content-Type} o 

**Varsayılan değer:** -

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="debug-cache-response-headers"></a>Önbellek yanıt üstbilgilerini hata ayıklama
**Amaç:** yanıt için önbellek ilkesini için istenen varlık bilgileri sağlayan X EC Debug yanıt üst bilgisi dahil olup olmadığını belirler.

Aşağıdakilerin her ikisi de true olduğunda üstbilgilerini yanıta dahil önbellek yanıtı hata ayıklama:

- Hata ayıklama önbellek yanıt üstbilgileri özelliğinin istenen isteği etkinleştirildi.
- Yukarıdaki istek yanıta dahil hata ayıklama önbellek yanıt üstbilgilerini kümesini tanımlar.

Hata ayıklama önbellek yanıt üstbilgileri aşağıdaki üstbilgi ve istenen yönergeleri istekte ekleyerek istenebilir:

X-EC-Debug: _Directive1_,_Directive2_,_DirectiveN_

**Örnek:**

X EC Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state

Değer|Sonuç
-|-
Etkin|Hata ayıklama önbellek yanıt üstbilgileri için istekleri X EC Debug üstbilgi içeren bir yanıt döndürür.
Devre dışı|X EC Debug yanıt üstbilgisi yanıttan edilmeyecek.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="default-internal-max-age"></a>Varsayılan iç Max-Age
**Amaç:** varsayılan max-age aralığı POP için kaynak sunucusu önbellek COLLECTION belirler. Diğer bir deyişle, POP önce geçecek süreyi önbelleğe alınmış bir varlık kaynak sunucuda depolanan varlık eşleşip eşleşmediğini kontrol eder.

Anahtar bilgileri:

- Bu eylem yalnızca yanıtlar için bir kaynak sunucudan bir max-age göstergesi atamadığınız gerçekleşecek `Cache-Control` veya `Expires` üstbilgi.
- Bu eylem bağlantısı alınabilir olarak kabul edilen olmayan varlıklar için olmayacaktır.
- Bu eylem, POP önbellek revalidations tarayıcıya etkilemez. Bu tür revalidations tarafından belirlenen `Cache-Control` veya `Expires` dış Max-Age özelliğiyle özelleştirilebilir tarayıcıya gönderilen üstbilgileri.
- Bu eylem sonuçlarını bir observable yanıt üstbilgileri ve içeriğiniz için POP döndürülen içeriği üzerindeki etkisi, ancak kaynak sunucunuz POP gönderilen COLLECTION trafik miktarı üzerinde bir etkisi olabilir.
- Bu özellik tarafından yapılandırın:
    - Bir varsayılan iç Maksimum yaş uygulanabilir durum kodu seçme.
    - Bir tamsayı değeri belirtme ve istediğiniz zaman birimi (örneğin, saniye, dakika, saat, vb.) seçme. Bu değer varsayılan iç max-age aralığı tanımlar.

- Zaman birimi "Off" ayarını bir varsayılan iç max-age aralığı 7 gün istekleri için bir Maksimum yaş göstergesi atanan değil atamak kendi `Cache-Control` veya `Expires` üstbilgi.

**Varsayılan değer:** 7 gün

#### <a name="compatibility"></a>Uyumluluk
Hangi önbelleğinde ayarları izlenen şekilde nedeniyle, bu özellik aşağıdaki eşleşme koşullarla ilişkili olamaz: 
- Sayı olarak
- İstemci IP Adresi
- Tanımlama bilgisi parametresi
- Tanımlama bilgisi parametresi Regex
- Ülke
- Cihaz
- Edge Cname
- Başvuran etki alanı
- İstek üstbilgisi değişmez değeri
- İstek üstbilgisi Regex
- İstek üstbilgisi joker karakter
- İstek yöntemi
- İstek düzeni
- URL sorgu değişmez değeri
- URL sorgu Regex
- URL sorgu joker karakter
- URL sorgu parametresi

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="deny-access-403"></a>(403) erişimini engelle
**Amaç**: 403 Yasak yanıtta reddedilen tüm isteği olup olmadığını belirler.

Değer | Sonuç
------|-------
Etkin| 403 Yasak yanıtta reddedilir eşleştirme ölçütü karşılayan tüm isteklerin neden olur.
Devre dışı| Varsayılan davranışını geri yükler. Döndürülecek yanıt türünü belirlemek kaynak sunucunun izin vermek için varsayılan davranıştır.

**Varsayılan davranış**: devre dışı

> [!TIP]
   > Satır içi bağlantıları içeriğinize kullanarak HTTP başvuran erişimi engellemek için bir istek üstbilgisi eşleşme koşulu ile ilişkilendirmek için bu özellik için bir olası kullanımı içindir.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="expires-header-treatment"></a>Üstbilgi işleme süresi
**Amaç:** oluşturulmasını denetler `Expires` dış Max-Age özelliği etkin olduğunda POP tarafından üstbilgileri.

Bu tür bir yapılandırma elde etmek için en kolay yolu dış Max-Age ve üstbilgisi işleme süresi özellikleri aynı deyiminde yerleştirmektir.

Değer|Sonuç
--|--
Üzerine Yaz|Aşağıdaki eylemler gerçekleşir sağlar:<br/>-Üzerine yazar `Expires` kaynak sunucu tarafından üretilen üstbilgi.<br/>-Ekler `Expires` üstbilgi üretilen yanıta dış Max-Age özelliğiyle.
Doğrudan Geçiş|Sağlar `Expires` dış Max-Age özelliği tarafından üretilen üstbilgi hiçbir zaman yanıta eklenir. <br/> Kaynak sunucu oluşturursa bir `Expires` üstbilgisi, onu geçecek aracılığıyla son kullanıcıya. <br/>Kaynak sunucu üretmek değil, bir `Expires` üstbilgi sonra bu seçeneği değil içerecek şekilde yanıt üstbilgisi neden olabilir bir `Expires` üstbilgi.
Eksik varsa ekleyin.| Varsa bir `Expires` üstbilgi kaynak sunucudan alınmadı sonra bu seçeneği ekler `Expires` üstbilgi dış Max-Age özelliği tarafından üretilen. Bu seçenek, tüm varlıklar atanacak sağlamak için yararlıdır bir `Expires` üstbilgi.
Kaldır| Sağlar bir `Expires` başlık üstbilgisi Yanıtla dahil değildir. Varsa bir `Expires` üstbilgi zaten atanmış sonra üstbilgi yanıttan kaldırılır.

**Varsayılan davranış:** üzerine yaz

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="external-max-age"></a>Dış Maksimum yaş
**Amaç:** POP önbellek COLLECTION tarayıcıya max-age aralığını belirler. Diğer bir deyişle, bir tarayıcı önce geçecek süreyi POP öğesinden bir varlık yeni bir sürümünü denetleyebilir.

Bu özelliği etkinleştirmek oluşturacağını `Cache-Control: max-age` ve `Expires` POP üstbilgileri ve HTTP istemciye göndermek. Varsayılan olarak, bu üstbilgileri kaynak sunucu tarafından oluşturulanlar üzerine yazar. Ancak, Cache-Control üstbilgisi işleme ve üstbilgisi işleme süresi özellikleri bu davranışı değiştirmek için kullanılabilir.

Anahtar bilgileri:

- Bu eylem, kaynak sunucu önbelleği revalidations POP etkilemez. Bu tür revalidations tarafından belirlenen `Cache-Control` ve `Expires` üstbilgi kaynak sunucudan alınan ve varsayılan iç Max-Age ve zorla iç Max-Age özelliklerle özelleştirilebilir.
- Bu özellik bir tamsayı değeri belirterek ve istediğiniz zaman birimi (örneğin, saniye, dakika, saat, vb.) seçerek yapılandırın.
- Neden olan POP göndermek bu özelliği negatif bir değere ayarlama bir `Cache-Control: no-cache` ve bir `Expires` her yanıtı tarayıcıya ile geçmişte ayarlamak zaman. Bir HTTP istemci yanıt önbelleğe almaz karşın, bu ayar POP kaynak sunucudan yanıt önbelleğe alma yeteneğini etkilemez.
- Zaman birimi "Off" ayarı, bu özellik devre dışı bırakır. `Cache-Control` Ve `Expires` kaynak sunucunun yanıt ile önbelleğe alınmış üstbilgileri geçecek aracılığıyla tarayıcıya.

**Varsayılan davranış:** devre dışı

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="follow-redirects"></a>Yeniden yönlendirmeleri izleyin
**Amaç:** istekleri müşteri kaynak sunucu tarafından döndürülen konum üstbilgisi içinde tanımlı ana bilgisayar adı için yeniden yönlendirilen olup olmadığını belirler.

Anahtar bilgileri:

- İstekler, aynı platform için karşılık gelen CNAME'ler kenara yalnızca yönlendirilebilir.

Değer|Sonuç
-|-
Etkin|İstekler yönlendirilebilir.
Devre dışı|İstekleri yönlendirilir değil.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="force-internal-max-age"></a>İç Max-Age zorla
**Amaç:** , max-age aralığı POP için kaynak sunucusu önbellek COLLECTION belirler. Diğer bir deyişle, POP önce geçecek süreyi önbelleğe alınmış bir varlık kaynak sunucuda depolanan varlık eşleşip eşleşmediğini kontrol edebilirsiniz.

Anahtar bilgileri:

- Bu özellik, max-age aralığı içinde tanımlı geçersiz kılar `Cache-Control` veya `Expires` bir kaynak sunucudan oluşturulan üstbilgileri.
- Bu özellik, POP önbellek revalidations tarayıcıya etkilemez. Bu tür revalidations tarafından belirlenen `Cache-Control` veya `Expires` tarayıcıya gönderilen üstbilgileri.
- Bu özellik tarafından POP istemciye teslim yanıtta observable bir etkisi yoktur. Bununla birlikte, POP kaynak sunucuya gönderilen COLLECTION trafik miktarı üzerinde bir etkisi olabilir.
- Bu özellik tarafından yapılandırın:
    - Bir iç max-age uygulanır durum kodu seçme.
    - Bir tamsayı değeri belirtme ve istediğiniz zaman birimi (örneğin, saniye, dakika, saat, vb.) seçme. Bu değer isteğin, max-age aralığı tanımlar.

- Zaman birimi "Off" ayarı bu özelliği devre dışı bırakır. Bir iç max-age aralık istenilen varlıkların atanmamış. Özgün üstbilgisi önbelleğe alma yönergeleri içermiyorsa, ardından varlık varsayılan iç Max-Age özelliği etkin ayarına göre önbelleğe alınır.

**Varsayılan davranış:** devre dışı

#### <a name="compatibility"></a>Uyumluluk
Hangi önbelleğinde ayarları izlenen şekilde nedeniyle, bu özellik aşağıdaki eşleşme koşullarla ilişkili olamaz: 
- Sayı olarak
- İstemci IP Adresi
- Tanımlama bilgisi parametresi
- Tanımlama bilgisi parametresi Regex
- Ülke
- Cihaz
- Edge Cname
- Başvuran etki alanı
- İstek üstbilgisi değişmez değeri
- İstek üstbilgisi Regex
- İstek üstbilgisi joker karakter
- İstek yöntemi
- İstek düzeni
- URL sorgu değişmez değeri
- URL sorgu Regex
- URL sorgu joker karakter
- URL sorgu parametresi

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="h264-support-http-progressive-download"></a>H.264 desteği (HTTP aşamalı indirme)
**Amaç:** içerik akışı sağlama için kullanılabilir H.264 dosya biçimleri türlerini belirler.

Anahtar bilgileri:

- İzin verilen H.264 dosya adı uzantıları boşlukla ayrılmış bir dizi dosya uzantılarını seçeneğinde tanımlayın. Dosya uzantıları seçeneği varsayılan davranışı geçersiz kılar. Bu dosya adı uzantıları dahil ederek, bu seçeneği ayarlarken MP4 ve F4V desteği korur. 
- Bir süre eklediğinizden emin olun (örneğin, .mp4 .f4v) her dosya adı uzantısı belirtirken.

**Varsayılan davranış:** HTTP aşamalı indirme MP4 ve F4V medya varsayılan olarak destekler.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="honor-no-cache-request"></a>Uy No Cache isteği
**Amaç:** bir HTTP istemcisi no-cache mi istekleri iletilir kaynak sunucuya belirler.

HTTP istemcisi gönderdiğinde no-cache isteği gerçekleştiği bir `Cache-Control: no-cache` ve/veya `Pragma: no-cache` HTTP isteği üstbilgisi.

Değer|Sonuç
--|--
Etkin|Kaynak sunucusuna yönlendirmek için bir HTTP istemcinin no-önbellek ister ve kaynak sunucunun yanıt üstbilgileri ve gövde POP aracılığıyla HTTP istemciye döndürülecek sağlar.
Devre dışı|Varsayılan davranışını geri yükler. Kaynak sunucusuna iletilen no-cache isteklerini engellemek için varsayılan davranıştır.

Tüm üretim trafiği için bu özellik devre dışı varsayılan durumundayken bırakmayı kullanmamanız önerilir. Aksi halde, kaynak sunucuları son kullanıcılardan, web sayfaları yenilerken birçok Hayır önbellek isteği yanlışlıkla tetikleyebilir veya no-cache üstbilgisi video her istek ile göndermek için kodlanmış birçok popüler medya oynatıcıları tam korumalı değil. Bununla birlikte, bu özellik belirli hazırlama veya isteğe bağlı kaynak sunucudan alınmasını yeni içerik izin vermek üzere dizinleri, sınama üretim dışı uygulamak yararlı olabilir.

Bu özellik nedeniyle bir kaynak sunucuya iletilebilmesi için izin verilen bir istek için bildirilen önbellek durumu TCP_Client_Refresh_Miss olur. Modül raporlama çekirdek kullanılabilir önbellek durumları rapor istatistiksel bilgileri önbelleği durumuna göre sağlar. Bu, bu özellik nedeniyle bir kaynak sunucuya sayısını ve iletilen isteklerin izlemenize olanak sağlar.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="ignore-origin-no-cache"></a>Kaynak No-Cache yoksay
**Amaç:** CDN bir kaynak sunucudan sunulan aşağıdaki yönergeleri dikkate alıp almayacağını belirler:

- `Cache-Control: private`
- `Cache-Control: no-store`
- `Cache-Control: no-cache`
- `Pragma: no-cache`

Anahtar bilgileri:

- Bu özellik, yukarıdaki yönergeleri yoksayılacak durum kodları boşlukla ayrılmış bir listesi tanımlayarak yapılandırın.
- Bu özellik için geçerli durum kodları kümesidir: 200, 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 ve 505.
- Bu özellik boş bir değerine ayarlayarak devre dışı bırakın.

**Varsayılan davranış:** varsayılan davranış, yukarıdaki yönergeleri dikkate almaktır.

#### <a name="compatibility"></a>Uyumluluk
Hangi önbelleğinde ayarları izlenen şekilde nedeniyle, bu özellik aşağıdaki eşleşme koşullarla ilişkili olamaz: 
- Sayı olarak
- İstemci IP Adresi
- Tanımlama bilgisi parametresi
- Tanımlama bilgisi parametresi Regex
- Ülke
- Cihaz
- Edge Cname
- Başvuran etki alanı
- İstek üstbilgisi değişmez değeri
- İstek üstbilgisi Regex
- İstek üstbilgisi joker karakter
- İstek yöntemi
- İstek düzeni
- URL sorgu değişmez değeri
- URL sorgu Regex
- URL sorgu joker karakter
- URL sorgu parametresi

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="ignore-unsatisfiable-ranges"></a>Unsatisfiable aralıkları yoksay 
**Amaç:** bir istek 416 İstenen aralık değil sağlanabilir durum kodu oluşturduğunda istemcilere döndürülen yanıt belirler.

Belirtilen bayt aralığı isteği POP tarafından karşılanamayan ve IF-Range isteği üstbilgisi alanının belirtilmedi varsayılan olarak, bu durum kodu döndürülür.

Değer|Sonuç
-|-
Etkin|POP 416 İstenen aralık yeterli değil bir durum koduna sahip bir geçersiz bayt aralığı isteğine yanıt vermesini engeller. Bunun yerine sunucuları istenen varlık teslim etmek ve bir 200 Tamam istemciye döndür.
Devre dışı|Varsayılan davranışını geri yükler. 416 İstenen aralık değil sağlanabilir durum kodu vermenizin varsayılan davranıştır.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="internal-max-stale"></a>İç Max-eski
**Amaç:** ne kadar süreyle önbelleğe alınan varlık sunulan POP POP kaynak sunucu ile önbelleğe alınmış varlık düzeltin erişemediğinde normal sona erme süresini geçen kontrol eder.

Normalde, bir varlığın, max-age süresi dolduğunda, POP kaynak sunucuya yeniden doğrulanması isteği gönderir. Kaynak sunucu sonra ya da bir 304 ile Yanıtla POP baştan vermek için değişiklik kira önbelleğe alınan varlık üzerinde or else 200 Tamam POP önbelleğe alınan varlık güncelleştirilmiş bir sürümünü sağlamak için.

POP bu dahili Max eski özellik olup, nasıl uzun denetler ve ardından bu tür bir COLLECTION çalışırken kaynak sunucu ile bağlantı kuramadı ise, POP şimdi eski varlık sunmaya devam edebilir.

Varlığın, max-age dolduğunda değil başarısız COLLECTION oluştuğunda bu zaman aralığı başlatır. Bu nedenle, hangi sırasında başarılı COLLECTION bir varlık sunulabilecek en uzun süresi, max-age artı max eski birleşimi tarafından belirtilen zaman miktarıdır. Bir varlık 9:00 30 dakika cinsinden maksimum yaş ve en çok eski 15 dakika ile önbelleğe alınmışsa, örneğin, ardından 9:44 başarısız COLLECTION teşebbüs 9:46 başarısız COLLECTION teşebbüs tr oluşturacağı sırada eski önbelleğe alınan varlık alma bir son kullanıcı neden olacak d kullanıcı 504 ağ geçidi zaman aşımı alma.

Bu özellik yerine geçen için yapılandırılan herhangi bir değer `Cache-Control: must-revalidate` veya `Cache-Control: proxy-revalidate` kaynak sunucudan alınan üstbilgileri. Bu üstbilgiler birini alındığında, kaynak sunucudan bir varlık başlangıçta önbelleğe alındığında POP eski bir önbelleğe alınan varlık görecek değil. POP varlığın, max-age aralığı sona erdiğinde kaynağa düzeltin kaydedemediği böyle bir durumda, POP 504 ağ geçidi zaman aşımı hatası döndürür.

Anahtar bilgileri:

- Bu özellik tarafından yapılandırın:
    - Max eski uygulanır durum kodu seçme.
    - Bir tamsayı değeri belirtme ve istediğiniz zaman birimi (örneğin, saniye, dakika, saat, vb.) seçme. Bu değer iç max-uygulanacak olan eski tanımlar.

- Zaman birimi "Off" ayarı, bu özellik devre dışı bırakır. Önbelleğe alınan bir varlık, kendi normal sona erme zamanı sunulmayacak.

**Varsayılan davranış:** iki dakika

#### <a name="compatibility"></a>Uyumluluk
Hangi önbelleğinde ayarları izlenen şekilde nedeniyle, bu özellik aşağıdaki eşleşme koşullarla ilişkili olamaz: 
- Sayı olarak
- İstemci IP Adresi
- Tanımlama bilgisi parametresi
- Tanımlama bilgisi parametresi Regex
- Ülke
- Cihaz
- Edge Cname
- Başvuran etki alanı
- İstek üstbilgisi değişmez değeri
- İstek üstbilgisi Regex
- İstek üstbilgisi joker karakter
- İstek yöntemi
- İstek düzeni
- URL sorgu değişmez değeri
- URL sorgu Regex
- URL sorgu joker karakter
- URL sorgu parametresi

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="log-query-string"></a>Günlük sorgu dizesi
**Amaç:** bir sorgu dizesi erişim günlükleri URL'de birlikte depolanan olup olmadığını belirler.

Değer|Sonuç
-|-
Etkin|Sorgu dizeleri depolama URL'leri bir erişim günlüğe kaydederken sağlar. Bir URL bir sorgu dizesi içermiyorsa, sonra bu seçeneği bir etkisi olmayacaktır.
Devre dışı|Varsayılan davranışını geri yükler. URL bir erişim günlüğe kaydederken sorgu dizelerini yoksaymak için varsayılan davranıştır.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="maximum-keep-alive-requests"></a>En fazla tutma isteği
**Amaç:** , kapatılmış olmasından önce en fazla istek tutma bağlantı sayısını tanımlar.

En fazla istek sayısını düşük bir değere ayarlamak, kesinlikle önerilmez ve performans düşüşüne neden olabilir.

Anahtar bilgileri:

- Bu değer bir tamsayı belirtin.
- Virgül veya nokta belirtilen değeri içermez.

**Varsayılan değer:** 10.000 istekleri

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="modify-client-request-header"></a>İstemci istek üstbilgisi değiştirme
**Amaç:** her istek açıkladığı istek üstbilgileri kümesi içerir. Bu özellik şunlardan birini yapabilirsiniz:

- Append veya bir istek üstbilgisini atanan değer üzerine yazabilirsiniz. Belirtilen istek üstbilgisi mevcut değilse, sonra bu özellik, isteği ekler.
- Bir istek üstbilgisini istekten silin.

Bir kaynak sunucusuna iletilen istekleri bu özellik tarafından yapılan değişiklikleri yansıtır.

Aşağıdaki eylemlerden birini istek üst bilgisinde gerçekleştirilebilir:

Seçenek|Açıklama|Örnek
-|-|-
Ekle|Belirtilen değer var olan istek üstbilgi değerinin sonuna eklenir.|**Üstbilgi değeri (istemci) istek:**Value1 <br/> **Üstbilgi değeri (HTTP kurallar altyapısı) istek:** Value2 <br/>**Yeni istek üstbilgi değeri:** Value1Value2
Üzerine Yaz|İstek üstbilgisi değeri belirtilen değere ayarlanır.|**Üstbilgi değeri (istemci) istek:**Value1 <br/>**Üstbilgi değeri (HTTP kurallar altyapısı) istek:** Value2 <br/>**Yeni istek üstbilgi değeri:** Value2 <br/>
Sil|Belirtilen istek üstbilgisi siler.|**Üstbilgi değeri (istemci) istek:**Value1 <br/> **İstemci istek üstbilgisi yapılandırmasını Değiştir:** söz konusu istek üstbilgisi silin. <br/>**Sonuç:** belirtilen istek üstbilgisi kaynak sunucusuna iletilen değil.

Anahtar bilgileri:

- Adı seçeneğinde belirtilen değeri istenen istek üstbilgisi için tam bir eşleşme olduğundan emin olun.
- Servis talebi üstbilgi tanımlamak amacıyla dikkate alınmaz. Aşağıdaki değişkenleri, örneğin, birini `Cache-Control` üstbilgi adı tanımlamak için kullanılabilir:
    - ön bellek denetimi
    - CACHE-CONTROL
    - cachE-Control
- Bir üstbilgi adı belirtirken, yalnızca alfasayısal karakterler, tire ve alt çizgiler kullanın.
- Üstbilgi silme POP'ları bir kaynak sunucusuna iletilen engeller.
- Aşağıdaki üst bilgiler ayrılmış ve bu özellik tarafından değiştirilemez:
    - iletilen
    - konak
    - aracılığıyla
    - uyarı
    - x-iletilen-için
    - "X-AB" ile başlayan tüm başlığı adları ayrılmıştır.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="modify-client-response-header"></a>İstemci yanıt üstbilgisi değiştirme
Her yanıtı açıkladığı yanıt üstbilgilerini kümesini içerir. Bu özellik şunlardan birini yapabilirsiniz:

- Append veya bir yanıt üstbilgisi atanan değer üzerine yazabilirsiniz. Belirtilen yanıt üstbilgisi mevcut değilse, sonra bu özellik, yanıta ekler.
- Bir yanıt üstbilgisi yanıttan silin.

Varsayılan olarak, yanıt üstbilgi değerleri POP ve kaynak sunucu tarafından tanımlanır.

Aşağıdaki eylemlerden birini bir yanıt üstbilgisi gerçekleştirilebilir:

Seçenek|Açıklama|Örnek
-|-|-
Ekle|Belirtilen değer var olan yanıt üstbilgi değeri sonuna eklenir.|**Yanıt üstbilgi değeri (istemci):**Value1 <br/> **Yanıt üstbilgi değeri (HTTP kurallar altyapısı):** Value2 <br/>**Yeni yanıt üstbilgi değeri:** Value1Value2
Üzerine Yaz|Yanıt üstbilgi değeri belirtilen değere ayarlanır.|**Yanıt üstbilgi değeri (istemci):**Value1 <br/>**Yanıt üstbilgi değeri (HTTP kurallar altyapısı):** Value2 <br/>**Yeni yanıt üstbilgi değeri:** Value2 <br/>
Sil|Belirtilen yanıt üst bilgisi siler.|**Yanıt üstbilgi değeri (istemci):** Value1 <br/> **İstemci yanıt üstbilgisi yapılandırmasını Değiştir:** söz konusu yanıt üstbilgisi silin. <br/>**Sonuç:** belirtilen yanıt üst bilgisi istemciye iletilecek değil.

Anahtar bilgileri:

- Adı seçeneğinde belirtilen değeri istenen yanıt üst bilgisi için tam bir eşleşme olduğundan emin olun. 
- Servis talebi üstbilgi tanımlamak amacıyla dikkate alınmaz. Aşağıdaki değişkenleri, örneğin, birini `Cache-Control` üstbilgi adı tanımlamak için kullanılabilir:
    - ön bellek denetimi
    - CACHE-CONTROL
    - cachE-Control
- Üstbilgi silinmesi, bu istemciye iletilen önler.
- Aşağıdaki üst bilgiler ayrılmış ve bu özellik tarafından değiştirilemez:
    - kabul kodlama
    - geçerlilik süresi
    - bağlantı
    - İçerik kodlama
    - içerik uzunluğu
    - İçerik aralığı
    - tarih
    - sunucu
    - toplamı
    - Transfer-encoding
    - Yükseltme
    - değişir
    - aracılığıyla
    - uyarı
    - "X-AB" ile başlayan tüm başlığı adları ayrılmıştır.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="partial-cache-sharing"></a>Kısmi önbellek paylaşımı
**Amaç:** istek kısmen önbelleğe alınmış içeriği oluşturmak olup olmadığını belirler.

Bu kısmi önbellek, ardından istenen içeriği tam olarak önbelleğe kadar bu içerik için yeni isteklerini karşılamak için kullanılabilir.

Değer|Sonuç
-|-
Etkin|İstekleri kısmen önbelleğe alınmış içeriği oluşturabilirsiniz.
Devre dışı|İstekleri, istenen içerik tam olarak önbelleğe alınan bir sürümü yalnızca oluşturabilir.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="prevalidate-cached-content"></a>Önbelleğe alınmış içeriği prevalidate
**Amaç:** TTL'si süresi dolmadan önce önbelleğe alınmış içeriği erken yeniden doğrulanması için uygun olup olmadığını belirler.

İstenen içeriğin TTL, erken yeniden doğrulanması uygun olacağı süre sonundan önce süreyi tanımlayın.

Anahtar bilgileri:

- "Kapalı" zaman birimi önbelleğe alınan içeriğin sonra gerçekleşmesi COLLECTION gerektirdiğinden seçerek TTL süresi doldu. Saat belirtilmemesi gerekir ve göz ardı edilir.

**Varsayılan davranış:** devre dışı. Önbelleğe alınan içeriğin TTL süresi dolduktan sonra yeniden doğrulanması yalnızca yer alabilir.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="proxy-special-headers"></a>Proxy özel üstbilgileri
**Amaç:** POP bir kaynak sunucuya iletilir CDN özgü istek üstbilgileri kümesini tanımlar.

Anahtar bilgileri:

- Bu özellik tanımlanan her CDN özgü istek üstbilgisi bir kaynak sunucuya iletilir.
- CDN özel istek üstbilgisi bir kaynak sunucu için bu listeden kaldırarak iletilmelerini önleyebilirsiniz.

**Varsayılan davranış:** tüm CDN özgü istek üstbilgileri kaynak sunucuya iletilir.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="refresh-zero-byte-cache-files"></a>Sıfır bayt önbellek dosyaları Yenile
**Amaç:** 0 bayt önbellek varlık için bir HTTP istemcinin isteğini POP tarafından nasıl işlendiğini belirler.

Geçerli değerler şunlardır:

Değer|Sonuç
--|--
Etkin|Varlık ve kaynak sunucudan yeniden getirmesi POP'a neden olur.
Devre dışı|Varsayılan davranışını geri yükler. İstek üzerine geçerli önbellek varlıklar sunmak için varsayılan davranıştır.
Bu özellik doğru önbelleğe alma ve içerik dağıtımı için gerekli değildir, ancak geçici bir çözüm olarak yararlı olabilir. Örneğin, kaynak sunucularda dinamik içerik oluşturucuları yanlışlıkla 0 baytlık yanıtları Pop'lere gönderilen neden olabilir. Bu tür yanıtları genellikle POP tarafından önbelleğe alınır. 0-bayt yanıt hiçbir zaman geçerli bir yanıt olduğunu biliyorsanız 

Bu tür içerik için daha sonra bu özellik Varlık türlerinin istemcilerinize hizmet engelleyebilir.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="set-cacheable-status-codes"></a>Önbelleğe alınabilir durum kodları
**Amaç:** önbelleğe alınmış içeriği sonuçlanabilir durum kodları kümesini tanımlar.

Varsayılan olarak, önbelleğe alma 200 Tamam yanıtlar için yalnızca etkinleştirilir.

İstenen durum kodları boşlukla ayrılmış bir kümesini tanımlar.

Anahtar bilgileri:

- Kaynak No-Cache yoksay özelliğini etkinleştirin. Bu özellik etkin değilse, ardından 200 Tamam yanıtlarını önbelleğe değil.
- Bu özellik için geçerli durum kodları kümesidir: 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 ve 505.
- Bu özellik, 200 Tamam durum kodu oluşturan yanıtlar için önbelleğe almayı devre dışı bırakmak için kullanılamaz.

**Varsayılan davranış:** önbelleğe alma, yalnızca 200 Tamam durum kodu oluşturan yanıtlar için etkinleştirildi.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="set-client-ip-custom-header"></a>İstemci IP özel üstbilgi ayarlayın
**Amaç:** isteyen istemci isteği IP adresine göre tanımlayan bir özel üst bilgi ekler.

Üstbilgi adı seçeneği istemcinin IP adresini depolandığı özel istek üstbilgisi adını tanımlar.

Bu özellik, bir müşteri sağlar istemci IP bulmak için kaynak sunucu adresleri özel istek üstbilgisi. Önbellekten isteği sunan, kaynak sunucu istemcinin IP adresini bildirilmez. Bu nedenle, bu özellik önbelleğe olmayan varlıklarla kullanılması önerilir.

Belirtilen üstbilgi adı aşağıdaki adlarının herhangi biri eşleşmediğinden emin olun:

- Standart istek üstbilgisi adları. Standart üstbilgi adlarının bir listesini bulunabilir [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).
- Ayrılmış üstbilgi adları:
    - iletilen için
    - konak
    - değişir
    - aracılığıyla
    - uyarı
    - x-iletilen-için
    - "X-AB" ile başlayan tüm başlığı adları ayrılmıştır.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="stale-content-delivery-on-error"></a>Eski içerik teslim hata
**Amaç:** istenen içerik müşteri kaynak sunucudan alınırken bir hata önbellek yeniden doğrulanması sırasında veya ortaya çıktığında süresi dolan önbelleğe alınan içerik teslim olup olmadığını belirler.

Değer|Sonuç
-|-
Etkin|Bir kaynak sunucuya bağlanma sırasında bir hata oluştuğunda eski içerik istemciye hizmet verir.
Devre dışı|Kaynak sunucunun hata istemciye iletilir.

**Varsayılan davranış:** devre dışı

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="stale-while-revalidate"></a>Revalidate sırasında eski
**Amaç:** POP COLLECTION gerçekleştirilirken istemciye eski içerik sunmanızı sağlayarak performansı geliştirir.

Anahtar bilgileri:

- Bu özellik davranışını seçilen zaman birimi göre değişir.
    - **Zaman birimi:** eski içerik teslim izin vermek için bir zaman birimi (örneğin, saniye, dakika, saat, vb.) seçin ve bir süre belirtin. Bu tür kurulum teslim edebilir süreyi uzatmak CDN doğrulama aşağıdaki formülü göre istemeden önce içerik verir: **TTL** + **eski sırada düzeltin zaman** 
    - **Kapalı:** seçin "kapalı" Eski içerik sunulması için önce bir isteği yeniden doğrulanması gerektirir.
        - Uygulanamaz ve yok sayılacak bu yana bir süre boyunca belirtmeyin.

**Varsayılan davranış:** devre dışı. İstenen içerik sunulabilen önce yeniden doğrulanması gerçekleşmesi gerekir.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth"></a>Belirteç kimlik doğrulama
**Amaç:** belirteç tabanlı kimlik doğrulaması için bir istek uygulanmış olup olmadığını belirler.

Belirteç tabanlı kimlik doğrulaması etkinleştirilirse, şifrelenmiş bir simge sağlayan ve belirtecini tarafından belirtilen gereksinimler için uymaları yalnızca istekleri kullanılacaktır.

Şifrelemek ve simge değerlerini şifresini çözmek için kullanılan şifreleme anahtarını birincil anahtar ve belirteç kimlik doğrulama sayfasında yedekleme anahtar seçenekleri tarafından belirlenir. Şifreleme anahtarları platforma özgü göz önünde bulundurun.

**Varsayılan davranış:** devre dışı bırakılmış.

Bu özellik URL yeniden yazma özelliği hariç olmak üzere çoğu özellikleri daha önceliklidir.

Değer | Sonuç
------|---------
Etkin | İstenen içerik belirteç tabanlı kimlik doğrulaması ile korur. Yalnızca geçerli bir belirteci sağlayan ve kendi gereksinimlerine istemcilerinden gelen istekleri kullanılacaktır. FTP hareketler belirteç tabanlı kimlik doğrulamasını bırakılır.
Devre dışı| Varsayılan davranışını geri yükler. Bir isteğin güvenli olup olmadığını belirlemek belirteç tabanlı kimlik doğrulaması yapılandırmanıza izin vermek için varsayılan davranıştır.

#### <a name="compatibility"></a>Uyumluluk
Belirteç kimlik doğrulama her zaman eşleştirme koşulla birlikte kullanmayın. 

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-denial-code"></a>Belirteç kimlik doğrulama reddi kodu
**Amaç:** belirteç tabanlı kimlik doğrulaması nedeniyle bir istek reddedildiğinde kullanıcıya döndürülecek yanıt türünü belirler.

Kullanılabilir yanıt kodları aşağıdaki tabloda listelenmiştir.

Yanıt Kodu|Yanıt adı|Açıklama
-------------|-------------|--------
301|Kalıcı olarak taşındı|Bu durum kodu yetkisiz kullanıcıların konumu üstbilgisinde belirtilen URL'ye yeniden yönlendirir.
302|Bulundu|Bu durum kodu yetkisiz kullanıcıların konumu üstbilgisinde belirtilen URL'ye yeniden yönlendirir. Bu durum kodu, bir yeniden yönlendirme gerçekleştiren endüstri standart yöntemidir.
307|Geçici yeniden yönlendirme|Bu durum kodu yetkisiz kullanıcıların konumu üstbilgisinde belirtilen URL'ye yeniden yönlendirir.
401|Yetkilendirilmemiş|Bu durum kodu ile WWW-Authenticate yanıt üstbilgisi birleştirme, bir kullanıcıdan kimlik doğrulaması için olanak sağlar.
403|Yasak|Bu ileti, yetkisiz bir kullanıcının korumalı içeriği erişmeye çalışırken göreceği standart 403 Yasak durum iletisi içindir.
404|Dosya Bulunamadı|Bu durum kodu HTTP istemcisi sunucuyla iletişim kuramıyor, ancak istenen içerik bulunamadı gösterir.

#### <a name="compatibility"></a>Uyumluluk
Belirteç kimlik doğrulama reddi kodu bir her zaman eşleştirme koşulunu kullanmayın. Bunun yerine, kullanın **reddi özel işleme** bölümüne **belirteci Auth** sayfasında **Yönet** portal. Daha fazla bilgi için bkz: [belirteci kimlik doğrulaması ile güvenli hale getirme Azure CDN varlıklar](cdn-token-auth.md).

#### <a name="url-redirection"></a>URL yeniden yönlendirme

3xx durum kodu döndürmek için yapılandırıldığında bu özellik, kullanıcı tanımlı bir URL yeniden yönlendirme URL'si destekler. Bu kullanıcı tarafından tanımlanan URL'yi, aşağıdaki adımları gerçekleştirerek belirtilebilir:

1. Belirteç kimlik doğrulama reddi kod özelliği için bir 3xx yanıt kodu seçin.
2. "Konum" isteğe bağlı üstbilgi adı seçeneğini seçin.
3. İsteğe bağlı üstbilgi değeri seçeneği istenen URL'sine ayarlayın.

Bir URL için 3xx durum kodu tanımlanmazsa 3xx durum kodu için standart yanıt sayfa kullanıcıya döndürülür.

URL yeniden yönlendirme yalnızca 3xx yanıt kodları için geçerlidir.

İsteğe bağlı üstbilgi değeri seçenek alfasayısal karakterler, tırnak işaretleri ve alanları destekler.

#### <a name="authentication"></a>Kimlik Doğrulaması

Bu özellik, WWW-Authenticate üstbilgisi belirteç tabanlı kimlik doğrulaması ile korunan içerik için yetkisiz bir isteğe yanıt verirken ekleyin özelliği destekler. WWW-Authenticate üstbilgisi yapılandırmanızda "temel" olarak ayarlanmışsa, yetkisiz kullanıcı hesabı kimlik bilgileri istenir.

Yukarıdaki yapılandırma aşağıdaki adımları gerçekleştirerek elde edilebilir:

1. Belirteç kimlik doğrulama reddi kod özelliği için yanıt kodunu olarak "401" seçin.
2. "WWW-Authenticate" isteğe bağlı üstbilgi adı seçeneğini seçin.
3. "Temel" isteğe bağlı üstbilgi değeri seçeneğine ayarlı

WWW-Authenticate üstbilgisi yalnızca 401 yanıt kodları için geçerlidir.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-ignore-url-case"></a>Belirteç kimlik doğrulama URL çalışması yoksay
**Amaç:** belirteç tabanlı kimlik doğrulaması ile yapılan URL karşılaştırmaları büyük küçük harfe duyarlı olup olmadığını belirler.

Bu özellik tarafından etkilenen Parametreler şunlardır:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Geçerli değerler şunlardır:

Değer|Sonuç
---|----
Etkin|POP durumu URL'ler için belirteç tabanlı kimlik doğrulama parametreleri karşılaştırılırken yoksay neden olur.
Devre dışı|Varsayılan davranışını geri yükler. Belirteç kimlik doğrulamasının büyük küçük harfe duyarlı olması URL karşılaştırmaları için varsayılan davranıştır.

**Varsayılan davranış:** devre dışı bırakılmış.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-parameter"></a>Belirteç kimlik doğrulama parametresi
**Amaç:** belirteç tabanlı kimlik doğrulaması sorgu dizesi parametresi yeniden adlandırılmış olup olmadığını belirler.

Anahtar bilgileri:

- Değer seçeneği bir belirteç belirtilebilir sorgu dizesi parametresinin adını tanımlar.
- Değer seçeneği "ec_token" ayarlanamaz
- Değer seçeneğinde tanımlanan ad URL yalnızca geçerli karakterleri içerdiğinden emin olun.

Değer|Sonuç
----|----
Etkin|Değer seçeneği belirteçleri tanımlanmalıdır sorgu dizesi parametresinin adını tanımlar.
Devre dışı|Bir belirteç istek URL'sindeki tanımsız sorgu dizesi parametresi olarak belirtilebilir.

**Varsayılan davranış:** devre dışı bırakılmış. Bir belirteç istek URL'sindeki tanımsız sorgu dizesi parametresi olarak belirtilebilir.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="url-redirect"></a>URL yeniden yönlendirme
**Amaç:** istekleri konum üstbilgisi aracılığıyla yeniden yönlendirir.

Bu özellik yapılandırması, aşağıdaki seçenekleri ayarlama gerektirir:

Seçenek|Açıklama
-|-
Kod|İstemciye döndürülecek yanıt kodu seçin.
Kaynak & düzeni| Bu ayarları yeniden yönlendirilen istekleri türünü tanımlayan bir istek URI düzeni tanımlayın. Yalnızca istek URL'si hem de aşağıdaki ölçütleri karşılayan yönlendirilir: <br/> <br/> **Kaynak (veya içerik erişim noktası):** bir kaynak sunucuyu tanımlar göreli bir yol seçin. Bu yol _/XXXX/_ bölümü ve uç nokta adınız. <br/> **Kaynak (desen):** göreli yolu tarafından istekleri tanımlayan bir desen tanımlanması gerekir. Bu normal ifade deseni doğrudan başlatır (yukarıya bakın) sonra daha önce seçilen içerik erişim noktası bir yolu tanımlamanız gerekir. <br/> -Daha önce tanımlanan istek URI ölçütlerini (diğer bir deyişle, kaynak & düzeni) çakışmadığını, bu özellik için tanımlı hiçbir eşleşme koşullarla emin olun. <br/> -Bir desen belirtin; boş bir değer deseni olarak kullanırsanız, tüm dizeleri eşleştirilir.
Hedef| Yukarıdaki istekleri yönlendirilecek URL tanımlayın. <br/> Dinamik olarak bu URL'yi kullanarak oluşturun: <br/> -Bir normal ifade deseni <br/>-HTTP değişkenleri <br/> Kaynak desende kullanarak $ hedef modele yakalanmış değerlerinizi yerleştirin_n_ nerede _n_ bu yakalanan sıraya göre bir değer tanımlar. Örneğin, $1 $2 ikinci değer temsil ederken, kaynak desende yakalanan ilk değerini temsil eder. <br/> 
Mutlak bir URL kullanmak için önerilir. Göreli bir URL kullanımı için geçersiz bir yol CDN URL'leri yönlendirmek.

**Örnek senaryo**

Bu örnek, bir sınır bu temel CDN URL'ye çözümler CNAME URL yeniden yönlendirme hakkında gösterir: http://marketing.azureedge.net/brochures

Bu temel kenar CNAME URL istekleri uygun yönlendirilir: http://cdn.mydomain.com/resources

Bu URL yeniden yönlendirme aşağıdaki yapılandırma elde edilebilir: ![](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Önemli noktaları:**

- İstek URL yeniden yönlendirme özelliğini tanımlar yönlendirilecek URL. Sonuç olarak, ek eşleme koşulları gerekli değildir. Eşleşme koşul "her zaman" tanımlandı ancak yalnızca "broşürler" klasörüne "Pazarlama" Müşteri kaynağındaki istekleri yönlendirilir. 
- Eşleşen tüm istekleri hedef seçeneğinde CNAME URL tanımlanan ucunu yönlendirilir. 
    - Örnek Senaryo #1: 
        - Örnek istek (CDN URL): http://marketing.azureedge.net/brochures/widgets.pdf 
        - İstek URL'si (sonra yeniden yönlendirme): http://cdn.mydomain.com/resources/widgets.pdf  
    - Örnek Senaryo #2: 
        - Örnek istek (Kenar CNAME URL): http://marketing.mydomain.com/brochures/widgets.pdf 
        - (Sonra yeniden yönlendirme) istek URL'si: http://cdn.mydomain.com/resources/widgets.pdf örnek senaryosu
    - Örnek Senaryo #3: 
        - Örnek istek (Kenar CNAME URL): http://brochures.mydomain.com/campaignA/final/productC.ppt 
        - İstek URL'si (sonra yeniden yönlendirme): http://cdn.mydomain.com/resources/campaignA/final/productC.ppt  
- İsteğin düzenini yeniden yönlendirmeden sonra değişmeden kalmasını sağlar hedef seçeneğinde istek düzeni (% {Şeması}) değişkeni yararlanır.
- İstekten yakalanan URL kesimleri "$1." aracılığıyla yeni bir URL'ye eklenir

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="url-rewrite"></a>URL yeniden yazma
**Amaç:** istek URL'si yeniden yazar.

Anahtar bilgileri:

- Bu özellik yapılandırması, aşağıdaki seçenekleri ayarlama gerektirir:

Seçenek|Açıklama
-|-
 Kaynak & düzeni | Bu ayarları yeniden yazılmıştır istekleri türünü tanımlayan bir istek URI düzeni tanımlayın. Yalnızca istek URL'si hem de aşağıdaki ölçütleri karşılayan yazılacaktır: <br/>     - **Kaynak (veya içerik erişim noktası):** bir kaynak sunucuyu tanımlar göreli bir yol seçin. Bu yol _/XXXX/_ bölümü ve uç nokta adınız. <br/> - **Kaynak (desen):** göreli yolu tarafından istekleri tanımlayan bir desen tanımlanması gerekir. Bu normal ifade deseni doğrudan başlatır (yukarıya bakın) sonra daha önce seçilen içerik erişim noktası bir yolu tanımlamanız gerekir. <br/> Önceden tanımlanmış istek URI ölçütleri (diğer bir deyişle, kaynak & düzeni) çakışmadığını, bu özellik için tanımlanan eşleşme koşullardan herhangi biri ile doğrulayın. Bir desen belirtin; boş bir değer deseni olarak kullanırsanız, tüm dizeleri eşleştirilir. 
 Hedef  |Yukarıdaki istekleri için tarafından yazılacak göreli URL tanımlayın: <br/>    1. Kaynak sunucu tanımlayan bir içerik erişim noktası seçme. <br/>    2. Göreli yolu kullanarak tanımlama: <br/>        -Bir normal ifade deseni <br/>        -HTTP değişkenleri <br/> <br/> Kaynak desende kullanarak $ hedef modele yakalanmış değerlerinizi yerleştirin_n_ nerede _n_ bu yakalanan sıraya göre bir değer tanımlar. Örneğin, $1 $2 ikinci değer temsil ederken, kaynak desende yakalanan ilk değerini temsil eder. 
 Bu özellik, geleneksel bir yeniden yönlendirme yapmadan URL yeniden yazma POP sağlar. Diğer bir deyişle, istek sahibinin yeniden URL istenen gibi aynı yanıt kodu alır.

**Örnek Senaryo 1**

Bu örnek, bir sınır bu temel CDN URL'ye çözümler CNAME URL yeniden yönlendirme gösterilmiştir: http://marketing.azureedge.net/brochures/

Bu temel kenar CNAME URL istekleri uygun yönlendirilir: http://MyOrigin.azureedge.net/resources/

Bu URL yeniden yönlendirme aşağıdaki yapılandırma elde edilebilir: ![](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Örnek Senaryo 2**

Bu örnekte, normal ifadeler kullanarak küçük büyük harf CNAME URL'den kenar yönlendirmek gösterilmiştir.

Bu URL yeniden yönlendirme aşağıdaki yapılandırma elde edilebilir: ![](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)


**Önemli noktaları:**

- İstek URL yeniden yazma özelliği tanımlar yazılacak URL'leri. Sonuç olarak, ek eşleme koşulları gerekli değildir. Eşleşme koşul "her zaman" tanımlandı ancak yalnızca "broşürler" klasörüne "Pazarlama" Müşteri kaynağındaki isteklerini yazılacaktır.

- İstekten yakalanan URL kesimleri "$1." aracılığıyla yeni bir URL'ye eklenir

#### <a name="compatibility"></a>Uyumluluk
Bu özellik için bir istek uygulanmadan önce karşılanması gereken ölçütlerle eşleşen içerir. Çakışan eşleşme ölçütlerini ayarlama önlemek için bu özellik aşağıdaki eşleşme koşullar ile uyumlu değil:

- Sayı olarak
- CDN kaynak
- İstemci IP Adresi
- Müşteri kaynağı
- İstek düzeni
- URL yolu dizini
- URL yolu genişletme
- URL yolu dosya
- URL yolu değişmez değeri
- URL yolu Regex
- URL yolu joker karakter
- URL sorgu değişmez değeri
- URL sorgu parametresi
- URL sorgu Regex
- URL sorgu joker karakter

[Başa dön](#azure-cdn-rules-engine-features)

</br>

---
### <a name="user-variable"></a>Kullanıcı değişkeni
**Amaç:** yalnızca iç kullanım için.

[Başa dön](#azure-cdn-rules-engine-features)

</br>

## <a name="next-steps"></a>Sonraki Adımlar
* [Kural altyapısı başvurusu](cdn-rules-engine-reference.md)
* [Kural altyapısı koşullu ifadeleri](cdn-rules-engine-reference-conditional-expressions.md)
* [Kural altyapısı eşleştirme koşulları](cdn-rules-engine-reference-match-conditions.md)
* [Kurallar altyapısı kullanarak HTTP davranışı geçersiz kılma](cdn-rules-engine.md)
* [Azure CDN'ye genel bakış](cdn-overview.md)
