---
title: SaaS yerine getirme API v2 | Azure Market
description: Bu makalede, oluşturup bir SaaS teklifi Azure Market ve AppSource ilişkili yerine getirme kullanarak yönetmek açıklanmaktadır v2 API'leri.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 05/23/2019
ms.author: evansma
ms.openlocfilehash: ecee1669c29d7b298741f9e5521de03da6dd7e3b
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331630"
---
# <a name="saas-fulfillment-apis-version-2"></a>SaaS yerine getirme API'leri, sürüm 2 

Bu makalede, SaaS uygulamalarında AppSource markete ve Azure Marketi'nde satmak üzere iş ortakları sağlayan API'leri ayrıntıları. Azure Market ve AppSource üzerinde transactable SaaS gereksinimini sunan bu apı'lerdir.

## <a name="managing-the-saas-subscription-life-cycle"></a>SaaS abonelik yaşam döngüsünü yönetme

Azure SaaS, SaaS aboneliği satın aldığınız tüm yaşam döngüsünü yönetir. Bu, gerçek yerine getirme sürücü için bir mekanizma API'leri yerine getirme kullanır, planlar ve iş ortağı abonelikle silme dönüşür. Müşteri'nin fatura Microsoft'un SaaS aboneliğin durumunu alır. Aşağıdaki diyagram, durumları ve durumları değişiklikleri sürücü işlemleri gösterir.

![SaaS abonelik yaşam döngüsü durumları](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Bir SaaS abonelik durumları

Aşağıdaki tablo, bir açıklama ve sıralı diyagramı her biri için (varsa) da dahil olmak üzere bir SaaS abonelik için sağlama durumları listeler. 

#### <a name="provisioning"></a>Sağlama

Bir müşteri satın başlattığında, iş ortağı bu bilgileri bir URL parametresini kullanan bir müşteri etkileşimli web sayfasındaki bir yetkilendirme kodu alır. Bir örnek `https://contoso.com/signup?token=..`iş ortağı Merkezi'nde giriş sayfası URL'si bilgileriyse `https://contoso.com/signup`. Yetkilendirme kodu doğrulanır ve sağlama hizmetinin ayrıntılarını çözmek API'sini çağırarak değişimi.  Bir SaaS hizmeti sağlama tamamlandığında, bir etkinleştirme çağrı yerine getirme tamamlandıktan ve müşteri faturalandırılır sinyal gönderir. 

Aşağıdaki diyagramda bir sağlama senaryosu için API çağrıları dizisini gösterir.  

![Bir SaaS hizmet sağlanması için API çağrıları](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Sağlanan

Bu durum sağlanan bir hizmet kararlı durumudur.

##### <a name="provisioning-for-update"></a>Güncelleştirme için sağlama 

Bu durum, mevcut bir hizmet için bir güncelleştirme Beklemede olduğunu gösterir. Böyle bir güncelleştirme Market ya da SaaS hizmeti (yalnızca doğrudan müşteri işlemler için) müşteri tarafından başlatılabilir.

##### <a name="provisioning-for-update-when-its-initiated-from-the-marketplace"></a>(Bu marketten başlatıldığında) güncelleştirmesi sağlama

Marketten bir güncelleştirme başlatıldığında bir dizi eylem Aşağıdaki diyagramda gösterilmektedir.

![Market'ten güncelleştirme başlatıldığında API çağrıları](./media/saas-update-api-v2-calls-from-marketplace-a.png)

##### <a name="provisioning-for-update-when-its-initiated-from-the-saas-service"></a>(Bu SaaS hizmeti başlatıldığında) güncelleştirmesi sağlama

Aşağıdaki diyagramda, bir güncelleştirme SaaS hizmeti başlatıldığında eylemleri gösterir. (Web kancası çağrısı SaaS hizmeti tarafından başlatılan bir abonelik için bir güncelleştirme ile değiştirilir.) 

![Güncelleştirme SaaS hizmeti başlatıldığında, API çağrıları](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Askıya alındı

Bu durum, bir müşterinin Ödeme alınan taşınmadığından gösterir. Müşteri aboneliğini önce yetkisiz kullanım süresi bir ilke tarafından sağlarız. Bu durumda bir aboneliğiniz olduğunda: 

- Bir iş ortağı olarak düşebilir veya hizmet kullanıcının erişimi engellemek tercih edebilirsiniz.
- Abonelik ayarları ya da veri kaybı olmadan tam işlevselliğini geri yüklemek bir kurtarılabilir durumda tutulması gerekir. 
- Eski duruma getirme isteği yerine getirme API'ler aracılığıyla bu abonelik ya da serbest sağlama isteği için yetkisiz kullanım süresi sonunda yararlanmayı beklersiniz. 

#### <a name="unsubscribed"></a>Aboneliği 

Abonelikler, yanıt bir açık müşteri isteği veya ücretlerin ödenmemesi nedeniyle, bu durumda ulaşın. Ortaktan müşterinin veri kurtarma isteğinde belirli sayıda gün boyunca saklanır ve ardından silinir beklenir. 


## <a name="api-reference"></a>API başvurusu

Bu bölümde SaaS belgeleri *abonelik API* ve *Operations API'si*.  Değerini `api-version` API sürüm 2 için parametredir `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Bir parametre ve tanımları

Aşağıdaki tabloda ortak parametrelerini ve yerine getirme API'leri tarafından kullanılan varlıkları tanımlarını listeler.

|     Varlık/parametre     |     Tanım                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | Bir SaaS kaynak GUID tanımlayıcısı.  |
| `name`                   | Bu kaynak için müşteri tarafından sağlanan bir kolay ad. |
| `publisherId`            | Her yayımcı için bir benzersiz tanımlayıcı (örneğin: "contoso"). |
| `offerId`                | Her teklif için bir benzersiz dize tanımlayıcı (örneğin: "offer1").  |
| `planId`                 | Her planı/SKU'yu için bir benzersiz tanımlayıcı (örneğin: "Gümüş"). |
| `operationId`            | Belirli bir işlem için GUID tanımlayıcısı.  |
|  `action`                | Bir kaynakta ya da gerçekleştirilmekte olan eylemin `subscribe`, `unsubscribe`, `suspend`, `reinstate`, veya `changePlan`, `changeQuantity`, `transfer`.  |
|   |   |

Genel olarak benzersiz tanımlayıcıları ([GUID'leri](https://en.wikipedia.org/wiki/Universally_unique_identifier)) genellikle otomatik olarak oluşturulan 128-bit (32 onaltılık) sayılardır. 

#### <a name="resolve-a-subscription"></a>Bir abonelik çözümleyin 

Bir kalıcı kaynak kimliği için bir Market belirteci çözülemedi yayımcı Çözümle uç nokta sağlar Kaynak Kimliği SaaS abonelik için benzersiz tanımlayıcısıdır. Bir kullanıcı bir iş ortağının Web sitesine yönlendirilir, sorgu parametrelerinde bir belirteç URL'sini içerir. İş ortağı Bu belirteci kullanmasına ve bu sorunu çözmek için istekte beklenir. Yanıtın benzersiz SaaS abonelik kimliği, adı, Teklif kimliği ve kaynak planlama içerir. Bu belirteci yalnızca bir saat boyunca geçerlidir. 

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsresolveapi-versionapiversion"></a>Yayınla<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      | `application/json` |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  authorization     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |
|  x-ms-Pazar-token  |  Kullanıcı, Azure SaaS iş ortağının Web sitesine yönlendirilir, URL'deki belirteci sorgu parametresi (örneğin: `https://contoso.com/signup?token=..`). *Not:* URL, kullanmadan önce tarayıcıdan belirteç değeri kodunu çözer.  |

*Yanıt kodları:*

Kod: 200<br>
Donuk belirteç SaaS aboneliği çözümler. Yanıt gövdesi:
 

```json
{
    "id": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Kod: 400<br>
Hatalı istek. x-ms-Pazar-belirteci eksik, hatalı veya süresi dolmuş.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 404<br>
Bulunamadı.

Kod: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

### <a name="subscription-api"></a>Abonelik API

Abonelik API aşağıdaki HTTPS işlemleri destekler: **Alma**, **Post**, **düzeltme eki**, ve **Sil**.


#### <a name="list-subscriptions"></a>Liste abonelikler

Bir yayımcı tüm SaaS abonelikleri listeler.

##### <a name="getbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsapi-versionapiversion"></a>Al<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  Bu istek için kullanılacak işlem sürümü.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
| İçerik türü       |  `application/json`  |
| x-ms-requestid     |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
| authorization      |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Yanıt kodları:*

Kod: 200 <br/>
Kimlik doğrulama belirteci temel alınarak, yayımcının tüm teklifler için karşılık gelen abonelikler ve yayımcı alır.
Yanıt yükü:<br>

```json
{
  [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

Devamlılık belirteci almak için planlar, ek "sayfalar" varsa mevcut olacaktır. 

Kod: 403 <br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor. 

Kod: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Abonelik al

Belirtilen SaaS abonelik alır. Bu çağrı, lisans bilgilerini almak ve planlama bilgileri için kullanın.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Al<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Belirteç çözmek API aracılığıyla çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.   |
|  ApiVersion        |   Bu istek için kullanılacak işlem sürümü.   |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      |  `application/json`  |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  authorization     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Yanıt kodları:*

Kod: 200<br>
SaaS abonelik tanımlayıcıdan alır. Yanıt yükü:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"Contoso Cloud Solution",
        "publisherId": "contoso",
        "offerId": "offer1",
        "planId": "silver",
        "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 404<br>
Bulunamadı.<br> 

Kod: 500<br>
İç sunucu hatası.<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Kullanılabilir planlar listesi

Geçerli yayımcı herhangi bir özel veya genel tekliflere olup olmadığını öğrenmek için bu çağrıyı kullanın.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidlistavailableplansapi-versionapiversion"></a>Al<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   Bu istek için kullanılacak işlem sürümü.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik türü     |  `application/json` |
|   x-ms-requestid   |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği  | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  authorization     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Yanıt kodları:*

Kod: 200<br>
Bir müşteri için uygun bir plan listesini alır. Yanıt gövdesi:

```json
{
    "plans": [{
        "planId": "Platinum001",
        "displayName": "Private platinum plan for Contoso",
        "isPrivate": true
    }]
}
```

Kod: 404<br>
Bulunamadı.<br> 

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor. <br> 

Kod: 500<br>
İç sunucu hatası.<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Bir aboneliği etkinleştirin

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidactivateapi-versionapiversion"></a>Yayınla<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      | `application/json`  |
|  x-ms-requestid    | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği  | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu dize istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  authorization     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*İstek yükü:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Yanıt kodları:*

Kod: 200<br>
Abonelik etkinleştirir.<br>

Kod: 400<br>
Hatalı istek: doğrulama hataları.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 404<br>
Bulunamadı.

Kod: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-plan-on-the-subscription"></a>Abonelik planını değiştirin

Abonelik planını güncelleştirin.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Düzeltme Eki<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      | `application/json` |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.    |
| authorization      |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*İstek yükü:*

```json
Request Body:
{
    "planId": "gold"
}
```

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
| İşlem konumu | İşlemin durumunu almak için bir kaynak bağlantısı.   |

*Yanıt kodları:*

Kod: 202<br>
Planı değiştirmek için istek kabul edildi. İş ortağı başarı veya başarısızlığı belirlemek için işlem konum yoklamak için bekleniyor. <br>

Kod: 400<br>
Hatalı istek: doğrulama hataları.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 404<br>
Bulunamadı.

Kod: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Bir plan veya miktarı yalnızca tek seferde düzeltme, ikisi birden değil. Bir Abonelikteki ile düzenler **güncelleştirme** değil `allowedCustomerOperations`.

#### <a name="change-the-quantity-on-the-subscription"></a>Abonelik üzerinde miktarı Değiştir

Abonelik miktarındaki güncelleştirin.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Düzeltme Eki:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      | `application/json` |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.    |
| authorization      |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*İstek yükü:*

```json
Request Body:
{
    "quantity": 5
}
```

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
| İşlem konumu | İşlemin durumunu almak için bir kaynak bağlayın.   |

*Yanıt kodları:*

Kod: 202<br>
Miktar değiştirme isteğini kabul etti. İş ortağı başarı veya başarısızlığı belirlemek için işlem konum yoklamak için bekleniyor. <br>

Kod: 400<br>
Hatalı istek: doğrulama hataları.


Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 404<br>
Bulunamadı.

Kod: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Bir plan veya miktarı yalnızca tek seferde düzeltme, ikisi birden değil. Bir Abonelikteki ile düzenler **güncelleştirme** değil `allowedCustomerOperations`.

#### <a name="delete-a-subscription"></a>Aboneliği silme

Aboneliği iptal et ve belirtilen abonelik silin.

##### <a name="deletebr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionid-api-versionapiversion"></a>Sil<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId> ?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik türü     |  `application/json` |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.   |
|  x-ms-bağıntı kimliği  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.   |
|  authorization     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Yanıt kodları:*

Kod: 202<br>
İş ortağı, SaaS abonelik aboneliğinizi iptal etmek için bir çağrı başlattı.<br>

Kod: 400<br>
Bir abonelikle silmek **Sil** değil `allowedCustomerOperations`.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 404<br>
Bulunamadı.

Kod: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>API işlemleri

Operations API'si aşağıdaki düzeltme eki ve Get işlemleri destekler.

#### <a name="list-outstanding-operations"></a>Bekleyen işlemlerin listesi 

Geçerli yayımcı için bekleyen işlemleri listeler. 

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsapi-versionapiversion"></a>Al<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   Bu istek için kullanılacak işlem sürümü.                |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik türü     |  `application/json` |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  authorization     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Yanıt kodları:*

Kod: 200<br> Bekleyen işlemler bir Abonelikteki listesini alır. Yanıt yükü:

```json
[{
    "id": "<guid>",  
    "activityId": "<guid>",
    "subscriptionId": "<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```


Kod: 400<br>
Hatalı istek: doğrulama hataları.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 404<br>
Bulunamadı.

Kod: 500<br>
İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>İşlem durumunu Al

Belirtilen tetiklenen zaman uyumsuz işlemin durumunu izlemek yayımcı sağlar (gibi `subscribe`, `unsubscribe`, `changePlan`, veya `changeQuantity`).

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Al<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      |  `application/json`   |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  authorization     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Yanıt kodları:*<br>

Kod: 200<br> Belirtilen SaaS işlemi alır. Yanıt yükü:

```json
Response body:
{
    "id  ": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}

```

Kod: 400<br>
Hatalı istek: doğrulama hataları.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.
 
Kod: 404<br>
Bulunamadı.

Kod: 500<br> İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>Bir işlemin durumunu güncelleştirme

Başarı veya başarısızlık sağlanan değerlerle belirtmek için bir işlemin durumunu güncelleştirin.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Düzeltme Eki<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |
|  operationId       | Tamamlanmakta işlem. |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik türü     | `application/json`   |
|   x-ms-requestid   |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  authorization     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*İstek yükü:*

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Yanıt kodları:*

Kod: 200<br> Tamamlama iş ortağı tarafında bir işlemin bildirmek için bir çağrı. Örneğin, bu yanıt, lisans planları veya değişiklik sinyal.

Kod: 400<br>
Hatalı istek: doğrulama hataları.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı veya geçersiz ya da istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 404<br>
Bulunamadı.

Kod: 409<br>
Çakışma oluştu. Örneğin, daha yeni bir işlem zaten yerine getirilir.

Kod: 500<br> İç sunucu hatası.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="implementing-a-webhook-on-the-saas-service"></a>Bir Web kancası SaaS hizmeti üzerinde uygulama

Yayımcı, Web kancası proaktif olarak kendi hizmetindeki değişiklikler kullanıcılara bildirmek için bu SaaS hizmetinde uygulamalıdır. SaaS hizmeti doğrulamak ve Web kancası bildirim eylemi gerçekleştirmeden önce onları yetkilendirmek için API işlemleri çağırmak için bekleniyor.

```json
{
  "id": "<this is a GUID operation id, you can call operations API with this to get status>",
  "activityId": "<this is a Guid correlation id>",
  "subscriptionId": "<Guid to uniquely identify this resource>",
  "publisherId": "<this is the publisher’s name>",
  "offerId": "<this is the offer name>",
  "planId": "<this is the plan id>",
  "quantity": "<the number of seats, will be null if not per-seat saas offer>",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Unsubscribe",
  "status": "NotStarted"  

}
```
Burada eylemi aşağıdakilerden biri olabilir: 
- `subscribe` (kaynak ne zaman etkinleştirildi)
- `unsubscribe` (kaynak ne zaman silinmiş)
- `changePlan` (değişiklik planı işlemi tamamlandığında)
- `changeQuantity` (değişiklik miktar işlemi tamamlandığında)
- `suspend` (zaman kaynak askıya alındı)
- `reinstate` (zaman kaynak sonra askıya alma uzatılamaz)

Burada durumu aşağıdakilerden biri olabilir: 
- **NotStarted** <br>
 - **Devam ediyor** <br>
- **Başarılı oldu** <br>
- **Başarısız** <br>
- **Çakışma** <br>

Eyleme dönüştürülebilir durumları ya da bir Web kancası bildirimde olan **başarılı** ve **başarısız**. Bir işlemin yaşam döngüsü olduğunu **NotStarted** gibi bir terminal durumuna **başarılı**, **başarısız**, veya **çakışma**. Alırsanız **NotStarted** veya **Inprogress**, eylemi gerçekleştirmeden önce işlemi terminal durumuna ulaşana kadar durum alma API aracılığıyla isteği devam. 

## <a name="mock-apis"></a>Sahte API'leri

Sahte API'leri ile geliştirme, prototip oluşturma özellikle de olarak test projeleri başlamanıza yardımcı olmak için kullanabilirsiniz. 

Uç nokta ana bilgisayar: `https://marketplaceapi.microsoft.com/api` (gerekli kimlik doğrulamasız)<br/>
API sürümü: `2018-09-15`<br/>
Örnek URI: `https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

Sahte ve gerçek API'leri API uç nokta yolları aynıdır, ancak API sürümlerini farklıdır. Sürüm `2018-09-15` için sahte bir sürümüne ve `2018-08-31` uygulamanın üretim sürümü için. 

Herhangi bir API çağrısı bu makalede sahte konak uç noktaya yapılabilir. Genel olarak sahte veri yanıt olarak geri almak bekler. Sahte API üzerinde güncelleştirme abonelik yöntemlere yapılan çağrılar, her zaman 500 döndürür. 

## <a name="next-steps"></a>Sonraki adımlar

Geliştiriciler ayrıca programlı bir şekilde alabilir ve iş yükleri, teklifler ve yayımcı profilleri kullanarak işleme [bulut iş ortağı portalı REST API'lerini](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
