---
title: SaaS yerine getirme API V2 - Azure Marketi | Microsoft Docs
description: Bir SaaS teklifi ilişkili yerine getirme V2 API kullanarak Azure Marketi'nde oluşturma açıklanır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/27/2019
ms.author: pbutlerm
ms.openlocfilehash: 6d18adfaec965d858bdcb1f74ebcea89f57eea39
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878035"
---
# <a name="saas-fulfillment-api"></a>SaaS yerine getirme API

Bu makalede, bağımsız yazılım satıcıları (ISV'ler) SaaS uygulamalarını tümleştirme sağlayan Azure Marketi'nde API'SİYLE ayrıntıları. Bu API, tüm etkin ticaret kanallarında katılmak ISV uygulamaları sağlar: doğrudan, iş ortağı destekli (satıcı) ve alan öncülüğünde.  Bu API, Azure Marketi'nde transactable SaaS sunar olmasa listeleme için gereklidir.


## <a name="managing-the-saas-subscription-lifecycle"></a>SaaS abonelik yaşam döngüsü yönetimi

Microsoft SaaS Service SaaS aboneliği satın aldığınız tüm yaşam döngüsü yönetir ve gerçek yerine getirme sürücü için bir mekanizma değiştikçe planlara ve ISV ile aboneliği silme işlemini tamamlama API'sini kullanır. Müşteri, Microsoft'un SaaS abonelik durumuna göre faturalandırılır. Aşağıdaki diyagram, durumları ve durumları değişiklikleri sürücü işlemleri gösterir.

![SaaS abonelik yaşam döngüsü durumları](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Bir SaaS abonelik durumları

Aşağıdaki tablo, bir açıklama ve sıralı diyagramı her biri için (varsa) da dahil olmak üzere bir SaaS abonelik için sağlama durumları listeler. 

#### <a name="provisioning"></a>Sağlama

Bir müşteri satın başlattığında, ISV bir AuthCode bir müşteri etkileşimli bir URL parametresi kullanarak web sayfası üzerinde bu bilgileri alır. AuthCode doğrulanır ve sağlanması gereken Ayrıntılar için değiştirilen.  SaaS hizmeti sağlama tamamlandığında, bir etkinleştirme çağrı yerine getirme tamamlandıktan ve müşteri faturalandırılır sinyal gönderir.  Aşağıdaki diyagramda bir sağlama senaryosu için API çağrıları dizisini gösterir.  

![Bir SaaS hizmet sağlanması için API çağrısı.](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Sağlandı

Bu durum sağlanan bir hizmet kararlı durumudur.

#### <a name="provisioning-for-update"></a>Güncelleştirme için sağlama
(Market'ten) 

Bu durum, mevcut bir güncelleştirme temsil eden hizmet bekliyor. Böyle bir güncelleştirme Market veya SaaS hizmetine (yalnızca için doğrudan gelen müşteri hareketlerini.) müşteri tarafından başlatılan Marketten bir güncelleştirme başlatıldığında eylemleri Aşağıdaki diyagramda gösterilmektedir.

![Market'ten güncelleştirme başlatıldığında API'sini çağırır.](./media/saas-update-api-v2-calls-from-marketplace-a.png)

#### <a name="provisioning-for-update"></a>Güncelleştirme için sağlama  
(SaaS hizmetinden) 

Aşağıdaki diyagramda bir güncelleştirme SaaS hizmeti tarafından başlatılan Eylemler gösterilir. (Web kancası çağrısı SaaS hizmeti tarafından başlatılan bir abonelik için bir güncelleştirme ile değiştirilir. 

![Güncelleştirme SaaS hizmeti tarafından başlatıldığında API'sini çağırır.](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Askıya Alındı

Bu durum, bir müşterinin Ödeme alınan taşınmadığından gösterir. İlke tarafından abonelik unfulfilling önce bir yetkisiz kullanım süresi müşteri sağlayacağız. Bu durumda bir aboneliğiniz olduğunda: 

- Bir ISV olarak düşebilir veya hizmet kullanıcının erişimi engellemek tercih edebilirsiniz. 
- Abonelik ayarları ya da veri kaybı olmadan tam işlevselliğini geri yüklemek bir kurtarılabilir durumda tutulması gerekir. 
- Eski duruma getirme isteği yerine getirme API aracılığıyla bu abonelik için veya bir veritabanının sağlama isteği yetkisiz kullanım süresi sonunda almak bekleyebilirsiniz. 

#### <a name="unsubscribed"></a>Aboneliği silindi 

Abonelikler, yanıt bir açık müşteri isteği veya ödeme nedeniyle, yanıt olarak bu durum ulaşın. ISV gelen Müşteri'nin veri kurtarma isteğinde en az X gün için saklanır ve ardından silinir beklenir. 

## <a name="api-reference"></a>API başvurusu

Bu bölümde SaaS belgeleri *abonelik API* ve *Operations API'si*.


### <a name="subscription-api"></a>Abonelik API

Abonelik API aşağıdaki HTTPS işlemleri destekler: **Alma**, **Post**, **düzeltme eki**, ve **Sil**.

#### <a name="list-subscriptions"></a>Abonelikleri listele

Bir yayımcı tüm SaaS abonelikleri listeler.

**Al:<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  Bu istek için kullanılacak işlem sürümü.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Type       |  `application/json`  |
| x-ms-requestid     |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| x-ms-bağıntı kimliği |  İstemci işlemi için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
| Yetkilendirme      |  JSON web token (JWT) taşıyıcı belirteç.  |

*Yanıt kodları:*

Kod: 200<br>
Kimlik doğrulama belirteci alın yayımcı ve karşılık gelen abonelikler yayımcının tüm teklifler için temel.<br> Yanıt yükü:<br>

```json
{
  "subscriptions": [
      {
          "id": "",
          "name": "CloudEndure for Production use",
          "publisherId": "cloudendure",
          "offerId": "ce-dr-tier2",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "cc906b16-1991-4b6d-a5a4-34c66a5202d7"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "0396833b-87bf-4f31-b81c-c67f88973512"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "status": "Subscribed" // Indicates the status of the operation. [Provisioning, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

Kod: 403 <br>
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor. 

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
|  Content-Type      |  `application/json`  |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci işlemi için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |  JSON web token (JWT) taşıyıcı belirteci  |

*Yanıt kodları:*

Kod: 200<br>
Saas abonelik tanımlayıcıdan alır<br> Yanıt yükü:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"CloudEndure for Production use",
        "publisherId": "cloudendure",
        "offerId": "ce-dr-tier2",
        "planId": "silver",
        "quantity": "10"",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "cc906b16-1991-4b6d-a5a4-34c66a5202d7"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "0396833b-87bf-4f31-b81c-c67f88973512"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Kod: 404<br>
Bulunamadı<br> 

Kod: 403<br>
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç Sunucu Hatası<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Kullanılabilir planlar listesi

Geçerli kullanıcı için herhangi bir private/public teklif olup olmadığını öğrenmek için bu çağrıyı kullanın.

**Al:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   Bu istek için kullanılacak işlem sürümü  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|   x-ms-requestid   |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği  | İstemci işlemi için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  Yetkilendirme     |  JSON web token (JWT) taşıyıcı belirteci |

*Yanıt kodları:*

Kod: 200<br>
Bir müşteri için uygun bir plan listesini alın.<br>

```json
Response Body:
[{
    "planId": "silver",
    "displayName": "Silver",
    "isPrivate": false
},
{
    "planId": "silver-private",
    "displayName": "Silver-private",
    "isPrivate": true
}]
```

Kod: 404<br>
Bulunamadı<br> 

Kod: 403<br>
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor. <br> 

Kod: 500<br>
İç Sunucu Hatası<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="resolve-a-subscription"></a>Bir abonelik çözümleyin 

Bir kalıcı kaynak kimliği için bir Market belirteç çözmek kullanıcıların Çözümle uç nokta sağlar Kaynak Kimliği SAAS abonelik için benzersiz tanımlayıcısıdır.  Bir kullanıcı, bir ISV Web sitesine yönlendirilir, sorgu parametrelerinde bir belirteç URL'sini içerir. Bu belirteci kullanmasına ve bu sorunu çözmek için bir istekte bulunmak için ISV bekleniyor. Yanıtın benzersiz SAAS abonelik kimliği, adı, Teklif kimliği ve kaynak planlama içerir. Bu belirteci yalnızca bir saat için geçerli değil. 

**Yayınla:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci işlemi için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |  JSON web token (JWT) taşıyıcı belirteci  |
|  x-ms-Pazar-token  |  Kullanıcı, Azure SaaS ISV Web sitesine yönlendirilir, URL'deki belirteci sorgu parametresi. *Not:* URL, kullanmadan önce tarayıcıdan belirteç değeri kodunu çözer. |

*Yanıt kodları:*

Kod: 200<br>
Donuk belirteç SaaS aboneliği çözümler.<br>

```json
Response body:
{
    "subscriptionId": "cd9c6a3a-7576-49f2-b27e-1e5136e57f45",  
    "subscriptionName": "My Saas application",
    "offerId": "ce-dr-tier2",
    "planId": "silver",
    "quantity": "20",
    "operationId": " be750acb-00aa-4a02-86bc-476cbe66d7fa"  
}
```

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları

Kod: 403<br>
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç Sunucu Hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
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
|  Content-Type      | `application/json`  |
|  x-ms-requestid    | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği  | İstemci işlemi için benzersiz bir dize değeri. Bu istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |  JSON web token (JWT) taşıyıcı belirteci |

*İstek:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Yanıt kodları:*

Kod: 202<br>
Abonelik etkinleştirir.<br>

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları

Kod: 403<br>
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç Sunucu Hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="update-a-subscription"></a>Aboneliği güncelleştirme

Güncelleştirme veya bir abonelik planı sağlanan değerlerle değiştirin.

**Düzeltme Eki:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.    |
| Yetkilendirme      |  JSON web token (JWT) taşıyıcı belirteç.  |

*İstek yükü:*

```json
Request Body:
{
    "planId": "gold",
    "quantity": ""
}
```

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
| İşlem konumu | İşlemin durumunu almak için bir kaynak bağlayın.   |

*Yanıt kodları:*

Kod: 202<br>
Planı Değiştir veya değişiklik miktarını ISV başlatır. <br>

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları.

>[!Note]
>Bir plan veya miktarı yalnızca tek seferde düzeltme, ikisi birden değil. Bir Abonelikteki ile düzenler **güncelleştirme** değil `allowedCustomerOperations`.

Kod: 403<br>
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç Sunucu Hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="delete-a-subscription"></a>Aboneliği sil

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
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.   |
|  x-ms-bağıntı kimliği  |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.   |
|  Yetkilendirme     |  JSON web token (JWT) taşıyıcı belirteç.   |

*Yanıt kodları:*

Kod: 200<br>
ISV tarafından başlatılan çağrı belirtmek için bir SaaS abonelikte aboneliği.<br>

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Bir abonelikle silmek **Sil** değil `allowedCustomerOperations`.

Kod: 403<br>
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç Sunucu Hatası

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


#### <a name="update-a-subscription"></a>Aboneliği güncelleştirme

Bir abonelik, sağlanan değerlerle güncelleştirin.

**Düzeltme Eki:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operation/<operationId>?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  Bu istek için kullanılacak işlem sürümü.  |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |
|  operationId       | Tamamlanmakta işlem. |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     | `application/json`   |
|   x-ms-requestid   |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
|  Yetkilendirme     |  JSON web token (JWT) taşıyıcı belirteç.  |

*İstek yükü:*

```json
{
    "planId": "",
    "quantity": "",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}
```

*Yanıt kodları:*

Kod: 200<br> ISV tarafında işleminin tamamlanma bildirmek için çağırın. Örneğin, bu bilgisayar lisansı/planları değişikliği olabilir.

Kod: 404<br>
Bulunamadı

Kod: 400<br>
Hatalı istek doğrulama hataları

Kod: 403<br>
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 409<br>
Çakışma oluştu. Örneğin, daha yeni bir işlem zaten karşılamış

Kod: 500<br> İç Sunucu Hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="list-outstanding-operations"></a>Bekleyen işlemlerin listesi 

Geçerli kullanıcı için bekleyen işlemleri listeler. 

**Al:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   Bu istek için kullanılacak işlem sürümü.                |
| subscriptionId     | Çözmek API'sini kullanarak belirteci çözdükten sonra elde edilen SaaS aboneliği benzersiz tanımlayıcısı.  |

*İstek bağlıkları:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |  İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     |  JSON web token (JWT) taşıyıcı belirteç.  |

*Yanıt kodları:*

Kod: 200<br> Bekleyen işlemler bir Abonelikteki listesini alır.<br>
Yanıt yükü:

```json
[{
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",  
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId": "cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",  
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
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor.

Kod: 500<br>
İç Sunucu Hatası

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>İşlem durumunu Al

Kullanıcının tetiklenen zaman uyumsuz işlemin (abonelik/Aboneliği Kaldır/Değiştir planı) durumunu izlemenizi sağlar.

**Al:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

*Sorgu parametreleri:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Bu istek için kullanılacak işlem sürümü.  |

*İstek bağlıkları:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`   |
|  x-ms-requestid    |   İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  x-ms-bağıntı kimliği |  İstemci üzerinde işlem için benzersiz bir dize değeri. Bu parametre istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer belirtilmezse, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
|  Yetkilendirme     | JSON web token (JWT) taşıyıcı belirteç.  |

*Yanıt kodları:* Kod: 200<br> Tüm bekleyen SaaS işlemlerin listesini alır<br>
Yanıt yükü:

```json
Response body:
[{
    "id  ": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",  
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
Yetkilendirilmedi. Kimlik doğrulama belirteci sağlanmadı, geçersiz veya istek geçerli kullanıcıya ait olmayan bir alım erişmeye çalışıyor.
 
Kod: 500<br> İç Sunucu Hatası

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
    "operationId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",
    "planId": "silver",
    "quantity": "20"  ,
    "action": "Activate",   // Activate/Delete/Suspend/Reinstate/Change[new]  
    "timeStamp": "2018-12-01T00:00:00"
}

```

<!-- Review following, might not be needed when this publishes -->


## <a name="mock-api"></a>Sahte API

Özellikle, prototip oluşturma ve test projeleri geliştirme ile çalışmaya başlamanıza yardımcı olmak için sahte Apı'lerimizi kullanabilirsiniz. 

Konak uç noktası: https://marketplaceapi.microsoft.com/api API sürümü: 2018-09-15 hiçbir kimlik doğrulaması örnek URI'si gereklidir: https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15

Herhangi bir API çağrısı bu makalede sahte konak uç noktaya yapılabilir. Sahte verileri yanıt olarak geri almak bekleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Geliştiriciler ayrıca programlı bir şekilde alabilir ve işleme iş yükleri, teklifler ve yayımcı profilini kullanarak [bulut iş ortağı portalı REST API'lerini](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
