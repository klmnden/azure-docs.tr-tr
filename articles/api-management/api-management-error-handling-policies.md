---
title: "Azure API Management ilkeleri işleme hatası | Microsoft Docs"
description: "Azure API Management'te isteklerinin işlenmesi sırasında ortaya çıkabilecek hata koşullarını yanıt öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 6bc71c0745493d52128553a78a31c45a3bca30f8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="error-handling-in-api-management-policies"></a>API Management ilkeleri işleme hatası
Azure API Management sağlayarak proxy'sine isteklerini işleme sırasında ortaya çıkabilecek hata koşullarını yanıtlamak yayımcılar sağlayan bir `ProxyError` nesnesi. `ProxyError` Nesnesi aracılığıyla erişilir [bağlamı. Son hata](api-management-policy-expressions.md#ContextVariables) özelliği ve ilkeleri tarafından kullanılan `on-error` ilke bölümü. Bu konuda bir başvuru hatası için Azure API Management'te işleme yetenekleri sağlar.  
  
## <a name="error-handling-in-api-management"></a>API Management'te işleme hatası  
 Azure API Management ilkeleri uygulamasına bölünen `inbound`, `backend`, `outbound`, ve `on-error` bölümler aşağıdaki örnekte gösterildiği gibi.  
  
```xml  
<policies>  
  <inbound>  
    <!-- statements to be applied to the request go here -->  
  </inbound>  
  <backend>  
    <!-- statements to be applied before the request is   
         forwarded to the backend service go here -->  
    </backend>  
    <outbound>  
      <!-- statements to be applied to the response go here -->  
    </outbound>  
    <on-error>  
        <!-- statements to be applied if there is an error   
             condition go here -->  
  </on-error>  
</policies>  
```  
  
 Bir isteğin işlenmesi sırasında istek için kapsamdaki tüm ilkeleri ile birlikte yerleşik adımları yürütülür. Bir hata oluşursa, hemen işleme için atlar `on-error` ilke bölümü. `on-error` İlkesi bölümünde herhangi bir kapsamda kullanılabilir ve API yayımcıların event hubs'a hata günlüğü veya çağırana dönmek için yeni bir yanıt oluşturma gibi özel davranışını yapılandırabilir.  
  
> [!NOTE]
>  `on-error` Bölümü ilkeleri varsayılan olarak mevcut değil. Eklenecek `on-error` bölümünde bir ilke, ilke düzenleyicisinde istenen ilke gözatın ve bunu ekleyin. İlkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/).  
>   
>  Varsa hiçbir `on-error` bölümünde arayanlar alacağı 400 veya 500 HTTP yanıt iletilerini bir hata koşulu ortaya çıkarsa.  
  
### <a name="policies-allowed-in-on-error"></a>Üzerinde hatada izin ilkeleri  
 Aşağıdaki ilkeleri kullanılabilir `on-error` ilke bölümü.  
  
-   [seçin](api-management-advanced-policies.md#choose)  
-   [değişken Ayarla](api-management-advanced-policies.md#set-variable)  
-   [Bul ve Değiştir](api-management-transformation-policies.md#Findandreplacestringinbody)  
-   [return yanıt](api-management-advanced-policies.md#ReturnResponse)  
-   [set üstbilgisi](api-management-transformation-policies.md#SetHTTPheader)  
-   [Set yöntemi](api-management-advanced-policies.md#SetRequestMethod)  
-   [durumunu ayarla](api-management-advanced-policies.md#SetStatus)  
-   [Gönderme isteği](api-management-advanced-policies.md#SendRequest)  
-   [bir şekilde İsteği Gönder](api-management-advanced-policies.md#SendOneWayRequest)  
-   [Günlük eventhub](api-management-advanced-policies.md#log-to-eventhub)  
-   [JSON xml](api-management-transformation-policies.md#ConvertJSONtoXML)  
-   [XML json](api-management-transformation-policies.md#ConvertXMLtoJSON)  
  
## <a name="lasterror"></a>Son hata  
 Ne zaman bir hata oluşur ve denetim görüşmek için `on-error` İlkesi bölümünde hata depolanır [bağlamı. Son hata](api-management-policy-expressions.md#ContextVariables) ilkeleri tarafından erişilen özellik `on-error` bölümünde ve aşağıdaki özelliklere sahiptir.  
  
|Ad|Tür|Açıklama|Gerekli|  
|----------|----------|-----------------|--------------|  
|Kaynak|Dize|Hatanın oluştuğu öğe adları. İlke veya yerleşik ardışık düzen adım adı olabilir.|Evet|  
|Neden|Dize|Hata işleme kullanılabilir makine kolay hata kodu.|Hayır|  
|İleti|Dize|Okunabilir hata açıklaması.|Evet|  
|Kapsam|Dize|Burada hata oluştu ve "Genel", "Ürün", "API" veya "işlem" biri olabilir kapsam adı|Hayır|  
|Section|Dize|Burada bir hata oluştu ve aşağıdakilerden birini "gelen" bölüm adı, "arka uç", "giden" veya "hata".|Hayır|  
|Yol|Dize|İç içe geçmiş İlkesi, örneğin belirtir "[3] seçin / ne zaman [2]".|Hayır|  
|Policyıd|Dize|Değeri `id` hatanın oluştuğu ilkedeki müşteri tarafından belirtilmişse özniteliği|Hayır|  
  
> [!NOTE]
>  Tüm ilkeler isteğe sahip `id` ilke kök öğesinin eklenebilir özniteliği. Bir hata koşulu oluştuğunda bu öznitelik bir ilke varsa, özniteliğinin değeri kullanılarak alınabilir `context.LastError.PolicyId` özelliği.  
  
## <a name="predefined-errors-for-built-in-steps"></a>Yerleşik adımlar için önceden tanımlanmış hataları  
 Aşağıdaki hatalar yerleşik işleme adımları değerlendirme sırasında ortaya çıkabilecek hata koşullarını önceden tanımlanmıştır.  
  
|Kaynak|Koşul|Neden|İleti|  
|------------|---------------|------------|-------------|  
|yapılandırma|URI herhangi bir API veya işlemi eşleşmiyor|OperationNotFound|Gelen istek için bir işlem eşleştirilemiyor.|  
|Yetkilendirme|Abonelik anahtarı sağlanmadı|SubscriptionKeyNotFound|Erişim abonelik anahtarı eksik nedeniyle reddedildi. Bu API için istekleri yaparken, abonelik anahtarı eklediğinizden emin olun.|  
|Yetkilendirme|Abonelik anahtarı değeri geçersiz|SubscriptionKeyInvalid|Erişim reddedildi nedeniyle geçersiz abonelik anahtarı. Etkin bir aboneliğiniz için geçerli bir anahtar sağladığınızdan emin olun.|  
  
## <a name="predefined-errors-for-policies"></a>İlkeleri için önceden tanımlanmış hataları  
 Aşağıdaki hatalar İlkesi değerlendirmesi sırasında ortaya çıkabilecek hata koşullarını önceden tanımlanmıştır.  
  
|Kaynak|Koşul|Neden|İleti|  
|------------|---------------|------------|-------------|  
|hız sınırı|Hızı sınırı aşıldı|RateLimitExceeded|Hız sınırı aşıldı|  
|Kota|Kotası aşıldı|QuotaExceeded|Çağrı hacmi kotası aşıldı. Kota xx:xx:xx içinde yenilenmesi. - veya - bant genişliği kota dışında. Kota xx:xx:xx içinde yenilenmesi.|  
|jsonp|Geri çağırma parametresi değeri geçersiz (hatalı karakterler içerir)|CallbackParameterInvalid|{Geri arama parametresi adı} geri çağırma parametresinin değeri geçerli bir JavaScript tanımlayıcısı değil.|  
|IP Filtresi|İstek çağıran IP ayrıştırılamadı|FailedToParseCallerIP|IP adresi için arayan kuramadı. Erişim engellendi.|  
|IP Filtresi|Arayan IP içinde değil izin verilen listesine|CallerIpNotAllowed|Arayan IP adresi {IP adresi} izin verilmiyor. Erişim engellendi.|  
|IP Filtresi|Arayan IP Engellenenler listesinde değil|CallerIpBlocked|Arayan IP adresi engellendi. Erişim engellendi.|  
|onay üstbilgisi|Gerekli üstbilgisi değil sunulan veya değer eksik|HeaderNotFound|Üstbilgi {üstbilgi-adı} istekte bulunamadı. Erişim engellendi.|  
|onay üstbilgisi|Gerekli üstbilgisi değil sunulan veya değer eksik|HeaderValueNotAllowed|{Üstbilgi değeri} {üstbilgi-adı} üstbilgi değerini izin verilmiyor. Erişim engellendi.|  
|doğrulama jwt|Jwt belirteci istekte eksik.|TokenNotFound|JWT istekte bulunamadı. Erişim engellendi.|  
|doğrulama jwt|İmza doğrulaması başarısız oldu|TokenSignatureInvalid|< İleti jwt kitaplığından\>. Erişim engellendi.|  
|doğrulama jwt|Geçersiz hedef kitle|TokenAudienceNotAllowed|< İleti jwt kitaplığından\>. Erişim engellendi.|  
|doğrulama jwt|Geçersiz veren|TokenIssuerNotAllowed|< İleti jwt kitaplığından\>. Erişim engellendi.|  
|doğrulama jwt|Belirtecin süresi|TokenExpired|< İleti jwt kitaplığından\>. Erişim engellendi.|  
|doğrulama jwt|İmza anahtarı kimliğine göre çözümlenemedi|TokenSignatureKeyNotFound|< İleti jwt kitaplığından\>. Erişim engellendi.|  
|doğrulama jwt|Gerekli talepleri belirtecinden eksik|TokenClaimNotFound|JWT belirteci aşağıdaki talep eksik: < c1\>, < c2\>,... Erişim engellendi.|  
|doğrulama jwt|Talep değerleri eşleşmiyor|TokenClaimValueNotAllowed|{Talep değeri} {talep-adı} talep değeri izin verilmiyor. Erişim engellendi.|  
|doğrulama jwt|Diğer doğrulama hataları|JwtInvalid|< İleti jwt kitaplığı\>|

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma daha fazla bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API dönüştürme](transform-api.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   