---
title: "Azure API Management ilke başvurusu"
description: "API Management yapılandırmak amacıyla kullanılabilir ilkeleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: adc0c4415e10ddd0b4994cecef17f026546e91a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-policy-reference"></a>Azure API Management ilke başvurusu
Bu bölümde ilkelerinde dizin sağlayan [API Management ilke başvurusu][API Management policy reference]. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri][Policies in API Management].

İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. Gibi bazı ilkeler [kontrol akışı] [ Control flow] ve [değişken Ayarla] [ Set variable] ilkeler ilke ifadelerini temel alarak. Daha fazla bilgi için bkz: [ilkeleri Gelişmiş] [ Advanced policies] ve [ilke ifadeleri][Policy expressions]

## <a name="policy-reference-index"></a>İlke başvuru dizini
* [Erişimi kısıtlama ilkeleri][Access restriction policies]
  * [Onay HTTP üstbilgisi] [ Check HTTP header] -varlığı ve/veya bir HTTP üstbilgisi değerini zorlar.
  * [Abonelik tarafından çağrı hızını sınırla] [ Limit call rate by subscription] -engeller API kullanımı ani başına abonelik başına çağrı hızını sınırlayarak.
  * [Anahtara göre çağrı hızını sınırla](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -engeller API kullanımı ani başına anahtar başına çağrı hızını sınırlayarak.
  * [Arayan IP'leri kısıtlamak] [ Restrict caller IPs] -filtreleri (verir/engellediği) çağrılarından belirli IP adreslerini ve/veya adres aralığı.
  * [Abonelik tarafından kullanım kotası ayarla] [ Set usage quota by subscription] -yenilenebilir veya ömrü çağrısı birim ve/veya bant genişliği kota, abonelik başına temelinde zorunlu tutmanıza olanak tanır.
  * [Anahtara göre kullanım kotası ayarla](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -yenilenebilir veya ömrü çağrısı birim ve/veya bant genişliği kota anahtarı başına temelinde zorunlu tutmanıza olanak tanır.
  * [JWT doğrulama] [ Validate JWT] -varlığı ve belirtilen bir HTTP üstbilgisi veya belirtilen sorgu parametresi ayıklanan JWT geçerliliğini zorlar.
* [Gelişmiş ilkeleri][Advanced policies]
  * [Denetim akışı] [ Control flow] - koşullu ilke deyimleri Boole değerlendirmesi sonuçlarına dayalı uygular [ifadeleri][expressions].
  * [İstek] [ Forward request] -arka uç hizmetine isteği iletir.
  * [Olay Hub'ına günlük] [ Log to Event Hub] -gönderdiği iletileri belirtilen biçimde tarafından tanımlanan bir ileti hedef bir [Günlükçü](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) varlık.
  * [Yeniden deneme](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -değilse ve koşul yerine getirilene kadar iliştirilmiş ilke deyimleri yürütülmesi yeniden dener. Yürütme belirtilen zaman aralıklarında yineleyin ve belirtilen en fazla yeniden deneme sayısı.
  * [Yanıt](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -iptalleri potansiyel satış, yürütme ve doğrudan çağırana belirtilen yanıt verir.
  * [Tek yönlü İsteği Gönder](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -bir istek için yanıt beklemeden belirtilen URL'ye gönderir.
  * [İsteği Gönder](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -belirtilen URL'ye bir isteği gönderir.
  * [İstek yöntemini ayarla](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -bir istek için HTTP yöntemini değiştirmenize izin verir.
  * [Durum Ayarla](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -HTTP durum kodu için belirtilen değer değiştirir.
  * [Değişken Ayarla] [ Set variable] -adlandırılmış bir değer kalıcı [bağlamı] [ context] sonraki erişim için değişken.
  * [İzleme](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -bir dizeye ekler [API denetçisi](api-management-howto-api-inspector.md) çıktı.
  * [Bekleyin](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) -kapalı gönderme bekler, önbellek, Get değerinden istek veya denetim devam etmeden önce tamamlanmasını akış ilkeleri.
* [Kimlik doğrulama ilkeleri][Authentication policies]
  * [Basic ile kimlik doğrulaması] [ Authenticate with Basic] -temel kimlik doğrulaması kullanarak arka uç hizmeti ile kimlik doğrulaması.
  * [İstemci sertifikası ile kimlik doğrulaması] [ Authenticate with client certificate] -istemci sertifikalarını kullanan bir arka uç hizmeti ile kimlik doğrulaması.
* [Önbelleğe alma ilkeleri][Caching policies] 
  * [Önbellekten alma] [ Get from cache] -önbellek gerçekleştirmek aramak ve geçerli bir önbelleğe alınan yanıt kullanılabilir olduğunda döndürür.
  * [Önbellek deposuna] [ Store to cache] -belirtilen önbellek denetim yapılandırmasında göre yanıtı önbelleğe alır.
  * [Önbelleğe alınan değer alma](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -anahtar tarafından önbelleğe alınmış bir öğeyi geri almak.
  * [Değer önbellekte depolamak](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -bir öğe anahtarı tarafından önbellekte depolamak.
  * [Önbelleğe alınan değer kaldırmak](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -öğenin önbellekte anahtarının kaldırın.
* [Etki alanı ilkelerini arası][Cross domain policies] 
  * [Etki alanları arası çağrılar izin] [ Allow cross-domain calls] -API Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirir.
  * [CORS] [ CORS] -çıkış noktaları arası kaynak paylaşımı (CORS) desteklemek için bir işlem veya tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için bir API ekler.
  * [JSONP] [ JSONP] -bir işlem veya tarayıcı tabanlı JavaScript istemcilerden etki alanları arası çağrılarına izin vermek için bir API (JSONP) doldurma desteğiyle JSON ekler.
* [Dönüştürme ilkelerini][Transformation policies] 
  * [XML ve JSON Dönüştür] [ Convert JSON to XML] - dönüştürür isteği veya yanıtı XML ve JSON öğesinden gövde.
  * [XML JSON biçimine Dönüştür] [ Convert XML to JSON] - dönüştürür isteği veya yanıtı için JSON XML'den gövde.
  * [Bulma ve değiştirme dizesi gövdesi içinde] [ Find and replace string in body] - bir istek veya yanıt alt dizeyi bulur ve farklı bir dizeyle değiştirir.
  * [İçeriği URL'leri maske] [ Mask URLs in content] -yanıta bağlantı (maskeleri)'yeniden yazar gövde böylece bunlar ağ geçidi aracılığıyla eşdeğer bağlantı üzerine gelin.
  * [Arka uç hizmetini ayarlamak] [ Set backend service] -gelen bir istek için arka uç hizmeti değiştirir.
  * [Gövde ayarlamak] [ Set body] -ileti gövdesi gelen ve giden istekleri için ayarlar.
  * [HTTP üstbilgisi kümesi] [ Set HTTP header] - değer atayan bir varolan yanıt ve/veya istek üstbilgisi veya yeni bir yanıt ve/veya istek üstbilgisi ekler.
  * [Sorgu dizesi parametresi kümesi] [ Set query string parameter] - ekler, değiştirir değerini veya istek sorgu dizesi parametresi siler.
  * [URL yeniden yazma] [ Rewrite URL] -bir istek URL'sini ortak formundan web hizmeti tarafından beklenen biçime dönüştürür.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki videoda İlkesi ifadeleri hakkında daha fazla bilgi için bkz.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log to Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON to XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML to JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx


