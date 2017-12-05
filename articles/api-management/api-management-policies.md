---
title: Azure API Management ilkeleri | Microsoft Docs
description: "Azure API Management'te kullanıma ilkeleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: d0d10096c004b50688ad5e6550bf248ceb5ef878
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-policies"></a>API Management ilkeleri
Bu bölümde aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](api-management-howto-policies.md).  
  
 Yapılandırma yoluyla API'nin davranışını değiştirmek yayımcının sisteminin güçlü bir özellik ilkelerdir. İstek üzerinde sırayla yürütülen deyimlerin bir koleksiyon veya bir API yanıtını ilkelerdir. Sık kullanılan deyimler, XML'den JSON biçimi dönüştürme içerir ve bir geliştiriciden gelen çağrıların miktarını sınırlamak için hız sınırı çağırın. Kutudan çıktığında çok daha fazla ilke kullanılabilir.  
  
 İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. [Akışı denetle](api-management-advanced-policies.md#choose) ve [Değişken ayarla](api-management-advanced-policies.md#set-variable) gibi bazı ilkeler ilke ifadelerini temel alır. Daha fazla bilgi için bkz: [ilkeleri Gelişmiş](api-management-advanced-policies.md#AdvancedPolicies) ve [ilke ifadelerini](api-management-policy-expressions.md).  
  
##  <a name="ProxyPolicies"></a>İlkeleri  
  
-   [Erişimi kısıtlama ilkeleri](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
    -   [Onay HTTP üstbilgisi](api-management-access-restriction-policies.md#CheckHTTPHeader) -varlığı ve/veya bir HTTP üstbilgisi değerini zorlar.  
    -   [Abonelik tarafından çağrı hızını sınırla](api-management-access-restriction-policies.md#LimitCallRate) -engeller API kullanımı ani başına abonelik başına çağrı hızını sınırlayarak.  
    -   [Anahtara göre çağrı hızını sınırla](api-management-access-restriction-policies.md#LimitCallRateByKey) -engeller API kullanımı ani başına anahtar başına çağrı hızını sınırlayarak.  
    -   [Arayan IP'leri kısıtlamak](api-management-access-restriction-policies.md#RestrictCallerIPs) -filtreleri (verir/engellediği) çağrılarından belirli IP adreslerini ve/veya adres aralığı.  
    -   [Abonelik tarafından kullanım kotası ayarla](api-management-access-restriction-policies.md#SetUsageQuota) -yenilenebilir veya ömrü çağrısı birim ve/veya bant genişliği kota, abonelik başına temelinde zorunlu tutmanıza olanak tanır.  
    -   [Anahtara göre kullanım kotası ayarla](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -yenilenebilir veya ömrü çağrısı birim ve/veya bant genişliği kota anahtarı başına temelinde zorunlu tutmanıza olanak tanır.  
    -   [JWT doğrulama](api-management-access-restriction-policies.md#ValidateJWT) -varlığı ve belirtilen bir HTTP üstbilgisi veya belirtilen sorgu parametresi ayıklanan JWT geçerliliğini zorlar.  
-   [Gelişmiş ilkeler](api-management-advanced-policies.md#AdvancedPolicies)  
    -   [Denetim akışı](api-management-advanced-policies.md#choose) - koşullu ilke deyimleri Boole ifadeleri değerlendirmeye dayanarak uygular.  
    -   [İstek](api-management-advanced-policies.md#ForwardRequest) -arka uç hizmetine isteği iletir.  
    -   [Olay Hub'ına günlük](api-management-advanced-policies.md#log-to-eventhub) -gönderdiği iletileri belirtilen biçimde Günlükçü varlık tarafından tanımlanan bir ileti hedef.  
    -   [Yeniden deneme](api-management-advanced-policies.md#Retry) -değilse ve koşul yerine getirilene kadar iliştirilmiş ilke deyimleri yürütülmesi yeniden dener. Yürütme belirtilen zaman aralıklarında yineleyin ve belirtilen en fazla yeniden deneme sayısı.  
    -   [Yanıt](api-management-advanced-policies.md#ReturnResponse) -iptalleri potansiyel satış, yürütme ve doğrudan çağırana belirtilen yanıt verir.  
    -   [Tek yönlü İsteği Gönder](api-management-advanced-policies.md#SendOneWayRequest) -bir istek için yanıt beklemeden belirtilen URL'ye gönderir.  
    -   [İsteği Gönder](api-management-advanced-policies.md#SendRequest) -belirtilen URL'ye bir isteği gönderir.  
    -   [Değişken Ayarla](api-management-advanced-policies.md#set-variable) -adlandırılmış bağlamının sonraki erişim için bir değer kalır.  
    -   [İstek yöntemini ayarla](api-management-advanced-policies.md#SetRequestMethod) -bir istek için HTTP yöntemini değiştirmenize izin verir.  
    -   [Durum kodu ayarlamak](api-management-advanced-policies.md#SetStatus) -HTTP durum kodu için belirtilen değer değiştirir.  
    -   [İzleme](api-management-advanced-policies.md#Trace) -bir dizeye ekler [API denetçisi](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) çıktı.  
    -   [Bekleyin](api-management-advanced-policies.md#Wait) -içine için bekleyeceği [gönderme isteği](api-management-advanced-policies.md#SendRequest), [değeri önbellekten alma](api-management-caching-policies.md#GetFromCacheByKey), veya [kontrol akışı](api-management-advanced-policies.md#choose) devam etmeden önce tamamlanmasını ilkeleri.  
-   [Kimlik doğrulama ilkeleri](api-management-authentication-policies.md#AuthenticationPolicies)  
    -   [Basic ile kimlik doğrulaması](api-management-authentication-policies.md#Basic) -temel kimlik doğrulaması kullanarak arka uç hizmeti ile kimlik doğrulaması.  
    -   [İstemci sertifikası ile kimlik doğrulaması](api-management-authentication-policies.md#ClientCertificate) -istemci sertifikalarını kullanan bir arka uç hizmeti ile kimlik doğrulaması.  
-   [Önbelleğe alma ilkeleri](api-management-caching-policies.md#CachingPolicies)  
    -   [Önbellekten alma](api-management-caching-policies.md#GetFromCache) -önbellek gerçekleştirmek aramak ve geçerli bir önbelleğe alınan yanıt kullanılabilir olduğunda döndürür.  
    -   [Önbellek deposuna](api-management-caching-policies.md#StoreToCache) -belirtilen önbellek denetim yapılandırmasında göre yanıtı önbelleğe alır.  
    -   [Önbelleğe alınan değer alma](api-management-caching-policies.md#GetFromCacheByKey) -anahtar tarafından önbelleğe alınmış bir öğeyi geri almak.  
    -   [Değer önbellekte depolamak](api-management-caching-policies.md#StoreToCacheByKey) -bir öğe anahtarı tarafından önbellekte depolamak.  
    -   [Önbelleğe alınan değer kaldırmak](api-management-caching-policies.md#RemoveCacheByKey) -öğenin önbellekte anahtarının kaldırın.  
-   [Çapraz etki alanı ilkeleri](api-management-cross-domain-policies.md#CrossDomainPolicies)  
    -   [Etki alanları arası çağrılar izin](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -API Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirir.  
    -   [CORS](api-management-cross-domain-policies.md#CORS) -çıkış noktaları arası kaynak paylaşımı (CORS) desteklemek için bir işlem veya tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için bir API ekler.  
    -   [JSONP](api-management-cross-domain-policies.md#JSONP) -bir işlem veya tarayıcı tabanlı JavaScript istemcilerden etki alanları arası çağrılarına izin vermek için bir API (JSONP) doldurma desteğiyle JSON ekler.  
-   [Dönüştürme ilkeleri](api-management-transformation-policies.md#TransformationPolicies)  
    -   [XML ve JSON Dönüştür](api-management-transformation-policies.md#ConvertJSONtoXML) - dönüştürür isteği veya yanıtı XML ve JSON öğesinden gövde.  
    -   [XML JSON biçimine Dönüştür](api-management-transformation-policies.md#ConvertXMLtoJSON) - dönüştürür isteği veya yanıtı için JSON XML'den gövde.  
    -   [Bulma ve değiştirme dizesi gövdesi içinde](api-management-transformation-policies.md#Findandreplacestringinbody) - bir istek veya yanıt alt dizeyi bulur ve farklı bir dizeyle değiştirir.  
    -   [İçeriği URL'leri maske](api-management-transformation-policies.md#MaskURLSContent) -yanıta bağlantı (maskeleri)'yeniden yazar gövde böylece bunlar ağ geçidi aracılığıyla eşdeğer bağlantı üzerine gelin.  
    -   [Arka uç hizmetini ayarlamak](api-management-transformation-policies.md#SetBackendService) -gelen bir istek için arka uç hizmeti değiştirir.  
    -   [Gövde ayarlamak](api-management-transformation-policies.md#SetBody) -ileti gövdesi gelen ve giden istekleri için ayarlar.  
    -   [HTTP üstbilgisi kümesi](api-management-transformation-policies.md#SetHTTPheader) - değer atayan bir varolan yanıt ve/veya istek üstbilgisi veya yeni bir yanıt ve/veya istek üstbilgisi ekler.  
    -   [Sorgu dizesi parametresi kümesi](api-management-transformation-policies.md#SetQueryStringParameter) - ekler, değiştirir değerini veya istek sorgu dizesi parametresi siler.  
    -   [URL yeniden yazma](api-management-transformation-policies.md#RewriteURL) -bir istek URL'sini ortak formundan web hizmeti tarafından beklenen biçime dönüştürür.  
    -   [XSLT kullanarak XML dönüştürme](api-management-transformation-policies.md#XSLTransform) -XSL dönüşümü istek veya yanıt gövdesinde XML için geçerlidir.  

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
İlkeleriyle çalışma daha fazla bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API dönüştürme](transform-api.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   
