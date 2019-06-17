---
title: Azure API Management ilkeleri | Microsoft Docs
description: Azure API Management'ta kullanılabilecek ilkeler hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: e27829fe5ebf57552ef4e97a2bfc7b6aefd81dc8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66254396"
---
# <a name="api-management-policies"></a>API Management ilkeleri
Bu bölüm, aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](api-management-howto-policies.md).  
  
 İlkeler yayımcının API configuration aracılığıyla davranışını değiştirmek sistemin güçlü özellikleridir. İstekte sırayla yürütülen deyimlerin bir koleksiyon veya bir API yanıtı ilkelerdir. Sık kullanılan deyimler, biçim dönüştürme, XML'den json'a içerir ve bir geliştiriciden gelen çağrıların miktarını sınırlamak için hız sınırı çağırın. Kullanıma hazır çok daha fazla ilke kullanılabilir.  
  
 İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. [Akışı denetle](api-management-advanced-policies.md#choose) ve [Değişken ayarla](api-management-advanced-policies.md#set-variable) gibi bazı ilkeler ilke ifadelerini temel alır. Daha fazla bilgi için bkz. [Gelişmiş ilkeler](api-management-advanced-policies.md#AdvancedPolicies) ve [İlke ifadeleri](api-management-policy-expressions.md).  
  
##  <a name="ProxyPolicies"></a> İlkeleri  
  
-   [Erişimi kısıtlama ilkeleri](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
    -   [Onay HTTP üst bilgisi](api-management-access-restriction-policies.md#CheckHTTPHeader) -varlığı ve/veya HTTP üstbilgisi değerini zorunlu kılar.  
    -   [Abonelik yoluyla çağrı hızını sınırla](api-management-access-restriction-policies.md#LimitCallRate) -engeller API kullanımı ani olarak abonelik başına çağrı hızını sınırlama.  
    -   [Anahtara göre çağrı hızını sınırla](api-management-access-restriction-policies.md#LimitCallRateByKey) -engeller API kullanımı ani başına anahtar başına çağrı hızını sınırlama tarafından.  
    -   [Arayan IP kısıtlama](api-management-access-restriction-policies.md#RestrictCallerIPs) -belirli IP adreslerinin ve/veya adres aralıkları filtreleri (sağlayan/engeller) çağırır.  
    -   [Abonelik tarafından kullanım kotası ayarla](api-management-access-restriction-policies.md#SetUsageQuota) -yenilenebilir veya ömrü çağrı birim ve/veya bant genişliği kota, abonelik başına temelinde zorunlu tutmanıza olanak tanır.  
    -   [Anahtara göre kullanım kotası ayarla](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -yenilenebilir veya ömrü çağrı birim ve/veya bant genişliği kota anahtarı başına temelinde zorunlu tutmanıza olanak tanır.  
    -   [JWT doğrulama](api-management-access-restriction-policies.md#ValidateJWT) -varlığı ve belirtilen bir HTTP üstbilgisi veya belirtilen sorgu parametresi ayıklanan JWT'nin geçerliliğini zorunlu kılar.  
-   [Gelişmiş ilkeler](api-management-advanced-policies.md#AdvancedPolicies)  
    -   [Denetim akışı](api-management-advanced-policies.md#choose) - koşullu ilke bildirimlerine Boolean ifadeler değerlendirmeye göre uygulanır.  
    -   [İstek](api-management-advanced-policies.md#ForwardRequest) -isteği arka uç hizmetine iletir.
    -   [Eşzamanlılık sınırlamak](api-management-advanced-policies.md#LimitConcurrency) -engeller içine birden çok istekleri belirtilen sayıda tarafından aynı anda yürütülmesini ilkeleri.
    -   [Olay Hub'ına](api-management-advanced-policies.md#log-to-eventhub) -gönderdiği iletileri belirtilen biçimde bir Günlükçü varlık tarafından tanımlanan bir ileti hedef.
    -   [Sahte yanıt](api-management-advanced-policies.md#mock-response) -iptalleri işlem hattı yürütme ve sahte yanıt doğrudan çağırana döner.
    -   [Yeniden deneme](api-management-advanced-policies.md#Retry) -varsa ve koşul yerine getirilene kadar iliştirilmiş ilke bildirimlerine yürütülmesi yeniden dener. Yürütme, belirtilen zaman aralıklarında yineleyin ve belirtilen en fazla yeniden deneme sayısı.  
    -   [Yanıt](api-management-advanced-policies.md#ReturnResponse) -iptalleri işlem hattı yürütme ve belirtilen yanıtını çağırana doğrudan döndürür.  
    -   [One-way isteğini göndermek](api-management-advanced-policies.md#SendOneWayRequest) -bir istek için yanıt beklemeden belirtilen URL'ye gönderir.  
    -   [İsteği Gönder](api-management-advanced-policies.md#SendRequest) -bir istek belirtilen URL'ye gönderir.
    -   [HTTP proxy ayarlamak](api-management-advanced-policies.md#SetHttpProxy) -bir HTTP Ara sunucusu üzerinden iletilen istekleri sağlar.
    -   [Değişken Ayarla](api-management-advanced-policies.md#set-variable) -kalıcı bir adlandırılmış bağlam değişkeni daha sonra erişmek için bir değer.  
    -   [İstek yöntemini ayarla](api-management-advanced-policies.md#SetRequestMethod) -bir isteğin HTTP yöntemi değiştirmenize izin verir.  
    -   [Durum kodu ayarlamak](api-management-advanced-policies.md#SetStatus) -HTTP durum kodunu belirtilen değere değişir.  
    -   [İzleme](api-management-advanced-policies.md#Trace) -bir dizeye ekler [API denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) çıktı.  
    -   [Bekleyin](api-management-advanced-policies.md#Wait) -içine için bekleyeceği [gönderme isteği](api-management-advanced-policies.md#SendRequest), [değeri önbellekten alma](api-management-caching-policies.md#GetFromCacheByKey), veya [denetim akışı](api-management-advanced-policies.md#choose) devam etmeden önce tamamlanması ilkeleri.  
-   [Kimlik doğrulama ilkeleri](api-management-authentication-policies.md#AuthenticationPolicies)  
    -   [Temel kimlik doğrulaması](api-management-authentication-policies.md#Basic) -temel kimlik doğrulaması kullanarak arka uç hizmeti ile kimlik doğrulaması.  
    -   [İstemci sertifikası ile kimlik doğrulaması](api-management-authentication-policies.md#ClientCertificate) -istemci sertifikalarını kullanan bir arka uç hizmeti ile kimlik doğrulaması.  
    -   [Yönetilen kimliği ile kimlik doğrulaması](api-management-authentication-policies.md#ManagedIdentity) -kimlik doğrulaması kullanarak bir arka uç hizmeti bir [yönetilen kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).  
-   [Önbelleğe alma ilkeleri](api-management-caching-policies.md#CachingPolicies)  
    -   [Önbellekten alma](api-management-caching-policies.md#GetFromCache) -önbellek gerçekleştirmek aramak ve kullanılabilir olduğunda geçerli önbelleğe alınan yanıt verin.  
    -   [Önbellek için Store](api-management-caching-policies.md#StoreToCache) -belirtilen önbellek denetimi yapılandırmasına yanıtı önbelleğe alır.  
    -   [Önbelleğe alınan değer elde](api-management-caching-policies.md#GetFromCacheByKey) -anahtar tarafından önbelleğe alınan öğe Al.  
    -   [Değer önbellekte Store](api-management-caching-policies.md#StoreToCacheByKey) -bir öğeyi anahtara göre önbellekte Store.  
    -   [Değer önbelleğinden kaldırmak](api-management-caching-policies.md#RemoveCacheByKey) -bir öğeyi anahtara göre önbellekte kaldırma.  
-   [Çapraz etki alanı ilkeleri](api-management-cross-domain-policies.md#CrossDomainPolicies)  
    -   [Etki alanları arası çağrılar izin](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -API Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirir.  
    -   [CORS](api-management-cross-domain-policies.md#CORS) -çıkış noktaları arası kaynak paylaşımı (CORS) bir işlem veya tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için API desteği ekler.  
    -   [JSONP](api-management-cross-domain-policies.md#JSONP) -işlem veya JavaScript tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için bir API (JSONP) doldurma desteğiyle JSON ekler.  
-   [Dönüştürme ilkeleri](api-management-transformation-policies.md#TransformationPolicies)  
    -   [JSON XML'ye dönüştürür](api-management-transformation-policies.md#ConvertJSONtoXML) - dönüştürür istek veya yanıt gövdesi JSON'dan XML.  
    -   [XML, JSON biçimine Dönüştür](api-management-transformation-policies.md#ConvertXMLtoJSON) - dönüştürür istek veya yanıt gövdesi XML'den JSON'a.  
    -   [Bul ve Değiştir gövdedeki dizeyi](api-management-transformation-policies.md#Findandreplacestringinbody) - istek veya yanıtı alt dizeyi bulur ve farklı bir dizeyle değiştirir.  
    -   [Maske URL'leri içeriğinde](api-management-transformation-policies.md#MaskURLSContent) -yanıta bağlantı (maskeleri)'yeniden yazar, böylece eşdeğer bağlantı ağ geçidi üzerinden işaret gövde.  
    -   [Arka uç Hizmeti'ni](api-management-transformation-policies.md#SetBackendService) -arka uç hizmeti gelen bir istek için değiştirir.  
    -   [Gövde ayarlamak](api-management-transformation-policies.md#SetBody) -ileti gövdesini gelen ve giden istekler için ayarlar.  
    -   [Set HTTP üstbilgisi](api-management-transformation-policies.md#SetHTTPheader) - bir var olan yanıt ve/veya istek üst bilgisi için bir değer atar veya yeni bir yanıt ve/veya istek üstbilgisi ekler.  
    -   [Sorgu dizesi parametresini ayarlayın](api-management-transformation-policies.md#SetQueryStringParameter) - ekler, değerini değiştirir veya istek sorgu dizesi parametresi siler.  
    -   [URL yeniden yazma](api-management-transformation-policies.md#RewriteURL) -istek URL'si, genel formu web hizmeti tarafından beklenen biçime dönüştürür.  
    -   [Bir XSLT kullanarak XML dönüştürme](api-management-transformation-policies.md#XSLTransform) -XSL dönüşümü için XML istek veya yanıt gövdesi içinde geçerlidir.  



## <a name="next-steps"></a>Sonraki adımlar
İlkeleriyle çalışma hakkında bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API'leri dönüştürme](transform-api.md)
+ [İlke örnekleri](policy-samples.md)   
