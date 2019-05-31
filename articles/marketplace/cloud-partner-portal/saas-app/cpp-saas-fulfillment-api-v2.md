---
title: SaaS yerine getirme API V2 | Azure Market
description: Bir SaaS teklifi ilişkili yerine getirme V2 API kullanarak Azure Market ve AppSource üzerinde oluşturma açıklanır.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: pabutler
ms.openlocfilehash: 432fef4c5dec697fecce694e251dd533aa1cbf07
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418007"
---
# <a name="saas-fulfillment-apis-version-2"></a>SaaS yerine getirme API sürüm 2 

Bu makalede, Azure Market ve AppSource bağımsız yazılım satıcıları (ISV'ler) SaaS uygulamalarına satmak sağlayan API ayrıntıları. Azure Market ve AppSource üzerinde transactable SaaS gereksinimini sunan bu API'dir.

> [!IMPORTANT] 
> SaaS teklif işlevselliği geçirildiğini [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory).  Tüm yeni yayımcılar, iş ortağı merkezi yeni SaaS teklifleri oluşturma ve mevcut teklifler yönetmek için kullanmanız gerekir.  SaaS teklifleri ile geçerli yayımcılar iş ortağı Merkezi'ne batchwise bulut iş ortağı Portalı'ndan geçiriliyor.  Bulut iş ortağı portalı belirli mevcut teklifler zaman geçirilmiş belirtmek için durum iletilerini görüntüler.
> Daha fazla bilgi için [yeni SaaS teklifi oluşturma](../../partner-center-portal/create-new-saas-offer.md).

## <a name="managing-the-saas-subscription-lifecycle"></a>SaaS abonelik yaşam döngüsü yönetimi

Microsoft SaaS Service SaaS aboneliği satın aldığınız tüm yaşam döngüsü yönetir ve gerçek yerine getirme sürücü için bir mekanizma değiştikçe planlara ve ISV ile aboneliği silme işlemini tamamlama API'sini kullanır. Müşteri, Microsoft'un SaaS abonelik durumuna göre faturalandırılır. Aşağıdaki diyagram, durumları ve durumları değişiklikleri sürücü işlemleri gösterir.

![SaaS abonelik yaşam döngüsü durumları](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Bir SaaS abonelik durumları

Aşağıdaki tablo, bir açıklama ve sıralı diyagramı her biri için (varsa) da dahil olmak üzere bir SaaS abonelik için sağlama durumları listeler. 

#### <a name="provisioning"></a>Sağlama

Bir müşteri satın başlattığında, ISV bu bilgileri bir müşteri etkileşimli bir URL parametresi kullanarak web sayfası üzerinde bir kimlik doğrulama kodu alır. Örneğin: `https://contoso.com/signup?token=..`, iş ortağı Merkezi giriş sayfası URL sağlayıcısında `https://contoso.com/signup`. Kimlik doğrulama kodu doğrulanır ve gidermek API'sini çağırarak sağlanması için gerekenler ilişkin ayrıntılar için değiştirilen.  SaaS hizmeti sağlama tamamlandığında, bir etkinleştirme çağrı yerine getirme tamamlandıktan ve müşteri faturalandırılır sinyal gönderir.  Aşağıdaki diyagramda bir sağlama senaryosu için API çağrıları dizisini gösterir.  

![Bir SaaS hizmet sağlanması için API çağrısı.](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Sağlanan

Bu durum sağlanan bir hizmet kararlı durumudur.

#### <a name="provisioning-for-update"></a>Güncelleştirme için sağlama
(Market'ten) 

Bu durum, mevcut bir güncelleştirme temsil eden hizmet bekliyor. Böyle bir güncelleştirme Market veya SaaS hizmetine (yalnızca için doğrudan gelen müşteri hareketlerini.) müşteri tarafından başlatılan Marketten bir güncelleştirme başlatıldığında eylemleri Aşağıdaki diyagramda gösterilmektedir.

![Market'ten güncelleştirme başlatıldığında API'sini çağırır.](./media/saas-update-api-v2-calls-from-marketplace-a.png)

#### <a name="provisioning-for-update"></a>Güncelleştirme için sağlama  
(SaaS hizmetinden) 

Aşağıdaki diyagramda bir güncelleştirme SaaS hizmeti tarafından başlatılan Eylemler gösterilir. (Web kancası çağrısı SaaS hizmeti tarafından başlatılan bir abonelik için bir güncelleştirme ile değiştirilir. 

![Güncelleştirme SaaS hizmeti tarafından başlatıldığında API'sini çağırır.](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Askıya alındı

Bu durum, bir müşterinin Ödeme alınan taşınmadığından gösterir. İlke tarafından abonelik unfulfilling önce bir yetkisiz kullanım süresi müşteri sağlayacağız. Bu durumda bir aboneliğiniz olduğunda: 

- Bir ISV olarak düşebilir veya hizmet kullanıcının erişimi engellemek tercih edebilirsiniz. 
- Abonelik ayarları ya da veri kaybı olmadan tam işlevselliğini geri yüklemek bir kurtarılabilir durumda tutulması gerekir. 
- Eski duruma getirme isteği yerine getirme API aracılığıyla bu abonelik için veya bir veritabanının sağlama isteği yetkisiz kullanım süresi sonunda almak bekleyebilirsiniz. 

#### <a name="unsubscribed"></a>Aboneliği 

Abonelikler, yanıt bir açık müşteri isteği veya ödeme nedeniyle, yanıt olarak bu durum ulaşın. ISV gelen Müşteri'nin veri kurtarma isteğinde en az X gün için saklanır ve ardından silinir beklenir. 


## <a name="api-reference"></a>API başvurusu

Bu bölümde SaaS belgeleri *abonelik API* ve *Operations API'si*.  Değerini `api-version` API sürüm 2 için parametredir `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Bir parametre ve tanımları

Aşağıdaki tabloda ortak parametrelerini ve yerine getirme API'leri tarafından kullanılan varlıkları tanımlarını listeler.

|     Varlık/parametre     |     Tanım                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | Bir SaaS kaynak GUID tanımlayıcısı  |
| `name`                   | Bu kaynak için müşteri tarafından sağlanan kolay adı |
| `publisherId`            | Örneğin "contoso" her yayımcı için benzersiz bir dize tanımlayıcı |
| `offerId`                | Örneğin "offer1" her teklif için benzersiz bir dize tanımlayıcısı  |
| `planId`                 | Her planı/SKU'yu, örneğin "Gümüş" için benzersiz bir dize tanımlayıcı |
| `operationId`            | Belirli bir işlem için GUID tanımlayıcısı  |
|  `action`                | Bir kaynakta ya da gerçekleştirilmekte olan eylemin `subscribe`, `unsubscribe`, `suspend`, `reinstate`, veya `changePlan`, `changeQuantity`, `transfer`  |
|   |   |

Genel olarak benzersiz tanımlayıcıları ([GUID'leri](https://en.wikipedia.org/wiki/Universally_unique_identifier)) genellikle otomatik olarak oluşturulan 128-bit (32 onaltılık) sayılardır. 

#### <a name="resolve-a-subscription"></a>Bir abonelik çözümleyin 

Bir kalıcı kaynak kimliği için bir Market belirteci çözülemedi yayımcı Çözümle uç nokta sağlar Kaynak Kimliği SAAS abonelik için benzersiz tanımlayıcısıdır.  Bir kullanıcı, bir ISV Web sitesine yönlendirilir, sorgu parametrelerinde bir belirteç URL'sini içerir. Bu belirteci kullanmasına ve bu sorunu çözmek için bir istekte bulunmak için ISV bekleniyor. Yanıtın benzersiz SAAS abonelik kimliği, adı, Teklif kimliği ve kaynak planlama içerir. Bu belirteci yalnızca bir saat için geçerli değil. 

**Yayınla:<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      | `application/json` |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci işlemi için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |  [JSON web token (JWT) taşıyıcı belirteci alma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) |
|  x-ms-Pazar-token  |  Kullanıcı, Azure SaaS ISV Web sitesine yönlendirilir, URL'deki belirteci sorgu parametresi (için örn: `https://contoso.com/signup?token=..`). *Not:* URL, kullanmadan önce tarayıcıdan belirteç değeri kodunu çözer.  |

*Yanıt kodları:*

Kod: 200<br>
Donuk belirteç SaaS aboneliği çözümler.<br>

```json
Response body:
{
    "id": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" // This will not be returned if the "quantity" = 1
}
```

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek. x-ms-Pazar-belirteci eksik, hatalı veya süresi dolmuş.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç sunucu hatası

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

**Al:<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  Bu istek için kullanılacak işlem sürümü.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
| İçerik türü       |  `application/json`  |
| x-ms-requestid     |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| x-ms-bağıntı kimliği |  İstemci işlemi için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
| Yetkilendirme      |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Yanıt kodları:*

Kod: 200 <br/>
Kimlik doğrulama belirteci temel alınarak, yayımcı ve yayımcının tüm teklifler için karşılık gelen abonelikler alın.<br> Yanıt yükü:<br>

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
          ], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation. [Provisioning, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

Devamlılık belirteci yalnızca alınacak planları, ek "sayfalar" mevcut olacaktır. 

Kod: 403 <br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor. 

Kod: 500 İç Sunucu Hatası

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

**Al:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Belirteç çözmek API aracılığıyla çözdükten sonra elde edilen SaaS abonelik benzersiz tanıtıcısı   |
|  ApiVersion        |   Bu istek için kullanılacak işlem sürümü   |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      |  `application/json`  |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci işlemi için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Yanıt kodları:*

Kod: 200<br>
SaaS abonelik tanımlayıcıdan alır<br> Yanıt yükü:<br>

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
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Kod: 404<br>
Bulunamadı<br> 

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç sunucu hatası<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Kullanılabilir planlar listesi

Geçerli yayımcı için herhangi bir private/public teklif olup olmadığını öğrenmek için bu çağrıyı kullanın.

**Al:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   Bu istek için kullanılacak işlem sürümü  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik türü     |  `application/json` |
|   x-ms-requestid   |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği  | İstemci işlemi için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  Yetkilendirme     |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) |

*Yanıt kodları:*

Kod: 200<br>
Bir müşteri için uygun bir plan listesini alın.<br>

Yanıt gövdesi:

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
Bulunamadı<br> 

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor. <br> 

Kod: 500<br>
İç sunucu hatası<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Bir aboneliği etkinleştirin

**Yayınla:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanıtıcısı  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      | `application/json`  |
|  x-ms-requestid    | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği  | İstemci işlemi için benzersiz bir dize değeri. Bu dize istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) |

*İstek:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Yanıt kodları:*

Kod: 200<br>
Abonelik etkinleştirir.<br>

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç sunucu hatası

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

**Düzeltme Eki:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      | `application/json` |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.    |
| Yetkilendirme      |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

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
| İşlem konumu | İşlemin durumunu almak için bir kaynak bağlayın.   |

*Yanıt kodları:*

Kod: 202<br>
Planı değiştirmek için istek kabul edildi. Başarı/başarısızlık durumu belirlemek için işlem konumu yoklamak için ISV bekleniyor. <br>

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları.

>[!Note]
>Bir plan veya miktarı yalnızca tek seferde düzeltme, ikisi birden değil. Bir Abonelikteki ile düzenler **güncelleştirme** değil `allowedCustomerOperations`.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç sunucu hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-quantity-on-the-subscription"></a>Abonelik üzerinde miktarı Değiştir

Abonelik miktarındaki güncelleştirin.

**Düzeltme Eki:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      | `application/json` |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.    |
| Yetkilendirme      |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

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
Kabul edildi. Miktar değiştirme isteğini kabul etti. Başarı/başarısızlık durumu belirlemek için işlem konumu yoklamak için ISV bekleniyor. <br>

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları.

>[!Note]
>Bir plan veya miktarı yalnızca tek seferde düzeltme, ikisi birden değil. Bir Abonelikteki ile düzenler **güncelleştirme** değil `allowedCustomerOperations`.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç sunucu hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="delete-a-subscription"></a>Aboneliği silme

Aboneliği iptal et ve belirtilen abonelik silin.

**Sil:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId> ?api-version=<ApiVersion>`**

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
|  Yetkilendirme     |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Yanıt kodları:*

Kod: 202<br>
ISV tarafından başlatılan çağrı belirtmek için bir SaaS abonelikte aboneliği.<br>

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Bir abonelikle silmek **Sil** değil `allowedCustomerOperations`.

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç sunucu hatası

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

**Al:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   Bu istek için kullanılacak işlem sürümü.                |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   İçerik türü     |  `application/json` |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Yanıt kodları:*

Kod: 200<br> Bekleyen işlemler bir Abonelikteki listesini alır.<br>
Yanıt yükü:

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

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç sunucu hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>İşlem durumunu Al

Belirtilen tetiklenen zaman uyumsuz işlemin durumunu izlemek yayımcı sağlar (abone / Aboneliği Kaldır / Değiştir planlama / değiştirme miktarı).

**Al:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  İçerik türü      |  `application/json`   |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |[JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Yanıt kodları:* Kod: 200<br> Belirtilen SaaS işlem alır<br>
Yanıt yükü:

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

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.
 
Kod: 500<br> İç sunucu hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>Bir işlemin durumunu güncelleştirme

Başarı/başarısızlık sağlanan değerlerle belirtmek için bir işlemin durumunu güncelleştirin.

**Düzeltme Eki:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

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
|   x-ms-requestid   |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  Yetkilendirme     |  [JSON web token (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*İstek yükü:*

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Yanıt kodları:*

Kod: 200<br> ISV tarafında işleminin tamamlanma bildirmek için çağırın. Örneğin, bu yanıt bilgisayar lisansı/planları değişikliği sinyal.

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları

Kod: 403<br>
Yetkisiz. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli yayımcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 409<br>
Çakışma oluştu. Örneğin, daha yeni bir işlem zaten karşılamış

Kod: 500<br> İç sunucu hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="webhook-on-the-saas-service"></a>Web kancası SaaS hizmeti hakkında

Yayımcı, Web kancası proaktif olarak kendi hizmetindeki değişiklikler kullanıcılara bildirmek için bu SaaS hizmetinde uygulamalıdır. API kimliği doğrulanmamış olması beklenir ve Microsoft SaaS hizmeti tarafından çağrılır. SaaS hizmeti doğrulamak ve Web kancası bildirim eylemi gerçekleştirmeden önce onları yetkilendirmek için API işlemleri çağırmak için bekleniyor.

```json
{
    "operationId": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",
    "planId": "silver",
    "quantity": "20"  ,
    "action": "Subscribe",
    "timeStamp": "2018-12-01T00:00:00"
}
```

Burada eylem bunlardan biri olabilir: 
- `Subscribe`  (Kaynak ne zaman etkinleştirildi)
- `Unsubscribe` (Kaynak ne zaman silinmiş)
- `ChangePlan` (Değişiklik planı işlemi tamamlandığında)
- `ChangeQuantity` (Değişiklik miktar işlemi tamamlandığında)
- `Suspend` (Zaman kaynak askıya alındı)
- `Reinstate` (Zaman kaynak sonra askıya alma uzatılamaz)


## <a name="mock-api"></a>Sahte API

Özellikle prototip oluşturma, geliştirme ile çalışmaya başlamanıza yardımcı olmak için sahte Apı'lerimizi kullanın ve test projeleri. 

Konak uç noktası: `https://marketplaceapi.microsoft.com/api` <br/>
API sürümü: `2018-09-15` <br/>
Kimlik doğrulama gerekmiyor <br/>
Örnek URI: `https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

Sahne ve gerçek API'ler API uç nokta yolları aynıdır, ancak API sürümlerini farklıdır. Sahte için 2018-09-15 ve uygulamanın üretim sürümü için 2018-08-31 sürümüdür. 

Herhangi bir API çağrısı bu makalede sahte konak uç noktaya yapılabilir. Sahte verileri yanıt olarak geri almak bekleyebilirsiniz. Genel olarak sahte veri yanıt olarak geri almak bekleyebilirsiniz. Sahte API üzerinde güncelleştirme abonelik yöntemlere yapılan çağrılar, her zaman 500 döndürür. 

## <a name="next-steps"></a>Sonraki adımlar

Geliştiriciler ayrıca programlı bir şekilde alabilir ve işleme iş yükleri, teklifler ve yayımcı profilini kullanarak [bulut iş ortağı portalı REST API'lerini](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
