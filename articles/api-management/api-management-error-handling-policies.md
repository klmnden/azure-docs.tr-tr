---
title: Azure API Management ilkeleri işleme hata | Microsoft Docs
description: Azure API Yönetimi'nde istek işleme sırasında ortaya çıkabilecek hata koşullarını yanıt öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: apimpm
ms.openlocfilehash: 2bde63bb668188936b3dd3cf5ecbf3b8c604eb95
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564326"
---
# <a name="error-handling-in-api-management-policies"></a>API Management ilkeleri işleme hatası

Sağlayarak bir `ProxyError` nesne, Azure API Management istekleri işleme sırasında ortaya çıkabilecek hata koşullarını, yanıt vermek yayımcılar sağlar. `ProxyError` Nesnesi aracılığıyla erişilen [bağlamı. LastError](api-management-policy-expressions.md#ContextVariables) özelliği ve ilkeleri tarafından kullanılan `on-error` ilke bölümü. Bu makalede bir başvuru için hata işleme özelliklerini Azure API Management'ta sağlar.  
  
## <a name="error-handling-in-api-management"></a>API Yönetimi'nde işleme hatası

 Azure API Management ilkeleri halinde bölünmüştür `inbound`, `backend`, `outbound`, ve `on-error` bölümler aşağıdaki örnekte gösterildiği gibi.  
  
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
  
Bir isteğin işlenmesi sırasında yerleşik adımları, istek için kapsama ilkelerle birlikte yürütülür. Bir hata oluşursa, hemen işlemeyi atlar için `on-error` ilke bölümü.  
`on-error` İlke bölümü herhangi bir kapsamda kullanılabilir. API yayımcıları, event hubs'a hata günlüğü veya çağırana döndürmesi için yeni bir yanıt oluşturma gibi özel davranışını yapılandırabilirsiniz.  
  
> [!NOTE]
>  `on-error` Bölümü ilkeleri varsayılan olarak mevcut değil. Eklenecek `on-error` bölümünde bir ilke için istediğiniz ilkeyi ilke düzenleyicisinde göz atın ve ekleyin. İlkeleri yapılandırma hakkında daha fazla bilgi için bkz. [API Management ilkeleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/).  
>   
>  Yoksa hiçbir `on-error` bölümünde, Arayanların alacağı 400 veya 500 HTTP yanıt iletilerini bir hata durumu oluşursa.  
  
### <a name="policies-allowed-in-on-error"></a>Hata izin verilen ilkeleri

 Aşağıdaki ilkeleri kullanılabilir `on-error` ilke bölümü.  
  
-   [Seçin](api-management-advanced-policies.md#choose)  
-   [değişken Ayarla](api-management-advanced-policies.md#set-variable)  
-   [Bul ve Değiştir](api-management-transformation-policies.md#Findandreplacestringinbody)  
-   [döndürülecek yanıt](api-management-advanced-policies.md#ReturnResponse)  
-   [üst bilgi ayarlama](api-management-transformation-policies.md#SetHTTPheader)  
-   [Set yöntemi](api-management-advanced-policies.md#SetRequestMethod)  
-   [durumunu ayarla](api-management-advanced-policies.md#SetStatus)  
-   [Gönderme isteği](api-management-advanced-policies.md#SendRequest)  
-   [bir şekilde İsteği Gönder](api-management-advanced-policies.md#SendOneWayRequest)  
-   [Günlük eventhub](api-management-advanced-policies.md#log-to-eventhub)  
-   [JSON xml](api-management-transformation-policies.md#ConvertJSONtoXML)  
-   [XML-json](api-management-transformation-policies.md#ConvertXMLtoJSON)  
  
## <a name="lasterror"></a>LastError

 Ne zaman bir hata oluşur ve denetimi atlar için `on-error` İlkesi bölümünde hata depolanan [bağlamı. LastError](api-management-policy-expressions.md#ContextVariables) ilkeleri tarafından erişilen özelliği `on-error` bölümü. LastError aşağıdaki özelliklere sahiptir.  
  
| Ad     | Tür   | Açıklama                                                                                               | Gerekli |
|----------|--------|-----------------------------------------------------------------------------------------------------------|----------|
| Kaynak   | string | Hatanın oluştuğu öğe adları. İlke ya da yerleşik bir işlem hattı adım adı olabilir.     | Evet      |
| Neden   | string | Hata işlemede kullanılan makine kullanımı kolay hata kodu.                                       | Hayır       |
| İleti  | string | Kullanıcı tarafından okunabilen hata açıklaması.                                                                         | Evet      |
| Kapsam    | string | Burada bir hata oluştu ve "Genel", "product", "API" veya "işlem" biri olabilir kapsamı adı | Hayır       |
| Section  | string | Hatanın oluştuğu bölüm adı. Olası değerler: "Giriş", "arka uç", "çıkış" veya "hata".       | Hayır       |
| Yol     | string | İç içe geçmiş ilkesi belirtir, örneğin "[3] seçin / zaman [2]".                                                        | Hayır       |
| PolicyId | string | Değeri `id` hatasının oluştuğu ilkesinde müşteri tarafından belirtilen, öznitelik             | Hayır       |

> [!TIP]
> Durum kodu bağlamı erişebilir. Response.StatusCode.  

> [!NOTE]
> İsteğe bağlı tüm ilkelerin sahip `id` ilkesinin kök öğesine eklenebilir özniteliği. Bir hata koşulu olduğunda bu öznitelik bir ilke varsa, öznitelik değeri kullanılarak alınabilir `context.LastError.PolicyId` özelliği.

## <a name="predefined-errors-for-built-in-steps"></a>Yerleşik adımlar için önceden tanımlanmış hataları  
 Aşağıdaki hatalar yerleşik işleme adımları değerlendirmesi sırasında ortaya çıkabilecek hata koşullarını önceden tanımlanmıştır.  
  
| Kaynak        | Koşul                                 | Neden                  | İleti                                                                                                                |
|---------------|-------------------------------------------|-------------------------|------------------------------------------------------------------------------------------------------------------------|
| yapılandırma | URI, herhangi bir API veya işlemi eşleşmiyor. | OperationNotFound       | Gelen istek bir işlemle eşleşen yapılamıyor.                                                                      |
| Yetkilendirme | Abonelik anahtarı sağlanmadı             | SubscriptionKeyNotFound | Erişim abonelik anahtarı eksik nedeniyle reddedildi. Bu API için istekleri yaparken, abonelik anahtarı eklediğinizden emin olun. |
| Yetkilendirme | Abonelik anahtarı değeri geçersiz         | SubscriptionKeyInvalid  | Erişim geçersiz bir abonelik anahtarı nedeniyle reddedildi. Etkin bir aboneliğiniz için geçerli bir anahtar sağladığınızdan emin olun.            |
  
## <a name="predefined-errors-for-policies"></a>İlkeleri için önceden tanımlanmış hataları  
 Aşağıdaki hatalar İlkesi değerlendirmesi sırasında ortaya çıkabilecek hata koşullarını önceden tanımlanmıştır.  
  
| Kaynak       | Koşul                                                       | Neden                    | İleti                                                                                                                              |
|--------------|-----------------------------------------------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| hız sınırı   | Hız sınırı aşıldı                                             | RateLimitExceeded         | Hız sınırı aşıldı                                                                                                               |
| kota        | Kota aşıldı                                                  | QuotaExceeded             | Çağrı hacmi kotası aşıldı. Kota xx:xx:xx içinde yenilenmesi. - veya - bant genişliği kota dışı. Kota xx:xx:xx içinde yenilenmesi. |
| jsonp        | Geri çağırma parametresi değeri geçersiz (hatalı karakterler içeriyor) | CallbackParameterInvalid  | Geri çağırma parametresi {geri arama-parametre-adı} değeri geçerli bir JavaScript tanımlayıcı değil.                                          |
| IP Filtresi    | Arayan IP istek ayrıştırılamadı                          | FailedToParseCallerIP     | Arayan için IP adresi oluşturulamadı. Erişim engellendi.                                                                        |
| IP Filtresi    | Arayan IP değil izin verilen listesi                                | CallerIpNotAllowed        | Arayan IP adresi {IP adresi} izin verilmez. Erişim engellendi.                                                                        |
| IP Filtresi    | Arayan IP Engellenenler listesinde değil                                    | CallerIpBlocked           | Arayan IP adresine engellenir. Erişim engellendi.                                                                                         |
| Üstbilgi denetimi | Gerekli üstbilgisi değil sunulan veya değer eksik               | HeaderNotFound            | Başlığı {üstbilgisi-adı} istek bulunamadı. Erişim engellendi.                                                                    |
| Üstbilgi denetimi | Gerekli üstbilgisi değil sunulan veya değer eksik               | HeaderValueNotAllowed     | {Üst bilgi değeri} başlığı {üstbilgisi-adı} değerine izin verilmez. Erişim engellendi.                                                          |
| validate-jwt | İstekte Jwt belirteci eksik                                 | TokenNotFound             | JWT istekte bulunamadı. Erişim engellendi.                                                                                         |
| validate-jwt | İmza doğrulama başarısız oldu                                     | TokenSignatureInvalid     | < İleti jwt kitaplığından\>. Erişim engellendi.                                                                                          |
| validate-jwt | Geçersiz hedef kitle                                                | TokenAudienceNotAllowed   | < İleti jwt kitaplığından\>. Erişim engellendi.                                                                                          |
| validate-jwt | Geçersiz veren                                                  | TokenIssuerNotAllowed     | < İleti jwt kitaplığından\>. Erişim engellendi.                                                                                          |
| validate-jwt | Belirtecin süresi doldu                                                   | TokenExpired              | < İleti jwt kitaplığından\>. Erişim engellendi.                                                                                          |
| validate-jwt | İmza anahtarı tarafından kimliği çözümlenemedi                            | TokenSignatureKeyNotFound | < İleti jwt kitaplığından\>. Erişim engellendi.                                                                                          |
| validate-jwt | Gerekli talepleri belirtecinden eksik                          | TokenClaimNotFound        | JWT belirteci aşağıdaki talep eksik: < c1\>, < c2\>,... Erişim engellendi.                                                            |
| validate-jwt | Talep değerleri eşleşmiyor                                           | TokenClaimValueNotAllowed | Talep {talep-adı} değeri {talep değeri} izin verilmez. Erişim engellendi.                                                             |
| validate-jwt | Diğer doğrulama hataları                                       | JwtInvalid                | < İleti jwt kitaplığından\>                                                                                                          |

## <a name="example"></a>Örnek

API İlkesi ayarlamak:

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
    <on-error>
        <set-header name="ErrorSource" exists-action="override">
            <value>@(context.LastError.Source)</value>
        </set-header>
        <set-header name="ErrorReason" exists-action="override">
            <value>@(context.LastError.Reason)</value>
        </set-header>
        <set-header name="ErrorMessage" exists-action="override">
            <value>@(context.LastError.Message)</value>
        </set-header>
        <set-header name="ErrorScope" exists-action="override">
            <value>@(context.LastError.Scope)</value>
        </set-header>
        <set-header name="ErrorSection" exists-action="override">
            <value>@(context.LastError.Section)</value>
        </set-header>
        <set-header name="ErrorPath" exists-action="override">
            <value>@(context.LastError.Path)</value>
        </set-header>
        <set-header name="ErrorPolicyId" exists-action="override">
            <value>@(context.LastError.PolicyId)</value>
        </set-header>
        <set-header name="ErrorStatusCode" exists-action="override">
            <value>@(context.Response.StatusCode.ToString())</value>
        </set-header>
        <base />
    </on-error>
</policies>
```

ve yetkisiz bir isteği gönderirken aşağıdaki yanıta neden olur:

![Yetkisiz hata yanıtı](media/api-management-error-handling-policies/error-response.png)

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma hakkında bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API'leri dönüştürme](transform-api.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   