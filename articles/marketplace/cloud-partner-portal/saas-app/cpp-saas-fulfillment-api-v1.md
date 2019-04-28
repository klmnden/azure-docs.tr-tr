---
title: SaaS yerine getirme API V1 - Azure Marketi | Microsoft Docs
description: Azure Marketi'nde ilişkili yerine getirme V1 API'leri kullanarak bir SaaS teklif oluşturma açıklanmaktadır.
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
ms.topic: reference
ms.date: 03/28/2019
ms.author: pbutlerm
ROBOTS: NOINDEX
ms.openlocfilehash: 4908233280c69a37ea470eed2ef077cb220a7930
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62101107"
---
# <a name="saas-fulfillment-apis-version-1--deprecated"></a>SaaS yerine getirme API sürümü 1 (kullanım dışı)

Bu makalede, API'leri ile SaaS teklifi oluşturmak açıklanmaktadır. API uç noktaları, REST yöntemleri ve oluşan sahip satmak, seçili Azure SaaS teklifinizle aboneliklere izin vermek için gereklidir.  

> [!WARNING]
> SaaS yerine getirme API bu ilk sürümünde kullanım dışı; Bunun yerine, [SaaS yerine getirme API V2](./cpp-saas-fulfillment-api-v2.md).  Bu API, yalnızca mevcut yayımcıların Hizmet şu anda sağlanıyor. 

Aşağıdaki API, SaaS hizmetinizin Azure ile tümleştirmenize yardımcı olmak için verilmiştir:

-   Çözümleme
-   Abone olun
-   Dönüştür
-   Abonelikten çık


## <a name="api-methods-and-endpoints"></a>API yöntemleri ve uç noktaları

Aşağıdaki bölümlerde, API yöntemleri ve bir SaaS teklifi için abonelikleri etkinleştirmek için kullanılabilir uç noktalar açıklanmaktadır.


### <a name="marketplace-api-endpoint-and-api-version"></a>Market API uç noktası ve API sürümü

Azure Market API'si için uç nokta `https://marketplaceapi.microsoft.com`.

Geçerli API sürümü `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Abonelik çözümleyin

GÖNDERİSİNİ eyleme çözmek bir kalıcı kaynak kimliği için bir Market belirteç çözmek kullanıcıların uç nokta sağlar  Kaynak Kimliği SAAS abonelik için benzersiz tanımlayıcısıdır. 

Bir kullanıcı, bir ISV Web sitesine yönlendirilir, sorgu parametrelerinde bir belirteç URL'sini içerir. Bu belirteci kullanmasına ve bu sorunu çözmek için bir istekte bulunmak için ISV bekleniyor. Yanıtın benzersiz SAAS abonelik kimliği, adı, Teklif kimliği ve kaynak planlama içerir. Bu belirteci yalnızca bir saat için geçerli değil.

*İstek*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Parametre adı** |     **Açıklama**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  API sürümü        |  Bu istek için kullanılacak işlem sürümü.   |
|  |  |


*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu istemci işlemi tüm olayları sunucu tarafında olaylarıyla ilişkilendirir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| içerik türü       | Evet          | `application/json`                                        |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
| x-ms-Pazar-token| Evet| Kullanıcı, Azure SaaS ISV Web sitesine yönlendirilir, URL'deki belirteci sorgu parametresi. **Not:** Bu belirteç, yalnızca 1 saat boyunca geçerlidir. Ayrıca, URL kod çözme tarayıcıdan belirteç değeri kullanmadan önce.|
|  |  |  |
  

*Yanıt gövdesi*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **Parametre adı** | **Veri türü** | **Açıklama**                       |
|--------------------|---------------|---------------------------------------|
| id                 | String        | SaaS aboneliğin kimliği.          |
| subscriptionName| String| SaaS hizmetine abone sırasında azure'da kullanıcı tarafından ayarlanan SaaS abonelik adı.|
| OfferId            | String        | Teklif kullanıcıya abone kimliği. |
| Planıd             | String        | Bir kullanıcı abone kimliği planlayın.  |
|  |  |  |


*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Belirteç başarıyla çözümlendi.                                                            |
| 400                  | `BadRequest`         | Ya da, üst bilgiler eksik veya geçersiz bir API sürümü belirtilen gereklidir. Belirteci hatalı biçimlendirilmiş ya da süresi dolmuş ya da belirteç olduğu için çözümlenemedi (belirteç yalnızca bir kez oluşturulan 1 saat boyunca geçerlidir). |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                                 |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                                |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                                        |
|  |  |  |


*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi durumda bu değer iletilmezse server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu kimlik tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Hayır           | Bu değer yalnızca bir 429 yanıtı için ayarlanır.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Abone olun

Abone uç noktası bir SaaS hizmetine belirli bir plan için bir abonelik başlatmak ve faturalandırma ticaret sisteminde etkinleştirmesine olanak sağlar.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{Subscriptionıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Benzersiz kimliği, SaaS çözmek API aracılığıyla belirteç çözdükten sonra elde edilen abonelik.                              |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

|  **Üstbilgi anahtarı**        | **Gerekli** |  **Açıklama**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   Hayır         | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| x-ms-bağıntı kimliği     |   Hayır         | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| IF-Match/If-None-Match |   Hayır         |   Güçlü Doğrulayıcı ETag değeri.                                                          |
| içerik türü           |   Evet        |    `application/json`                                                                   |
|  Yetkilendirme         |   Evet        |    JSON web token (JWT) taşıyıcı belirteç.                                               |
| x-ms-Pazar-oturum-modu| Hayır | Bir SaaS teklife abone sırasında prova modunu etkinleştirmek için bayrak. Ayarlanırsa, abonelik ücret uygulanmayan durumunda. ISV senaryolarını test etmek için kullanışlıdır. Lütfen kümesine **'prova'**|
|  |  |  |

*Gövde*

``` json
{
    "lanId": "",
}
```

| **Öğe adı** | **Veri türü** | **Açıklama**                      |
|------------------|---------------|--------------------------------------|
| Planıd           | (Gerekli) Dize        | SaaS hizmeti kullanıcı plan kimliği için abone oluyor.  |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme için belirli bir plan aldı.                   |
| 400                  | `BadRequest`         | Ya da üst bilgileri eksik olan veya JSON gövdesi yanlış biçimlendirilmiş gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                   |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                  |
| 409                  | `Conflict`           | Abonelik üzerinde başka bir işlem sürüyor.                     |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                          |
|  |  |  |

'İşlemi-konum' üst bilgisi, istek işlemin durumu için 202 yanıt, iletişim kurun. Kimlik doğrulaması diğer Market API'leri ile aynıdır.

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi durumda bu değer iletilmezse server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu değer, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Evet          | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
| İşlem konumu | Evet          | İşlem durumunu almak için bir kaynak bağlayın.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Değişiklik planı uç noktası

Değişiklik uç noktası şu anda abone planlarına dönüştürmek için yeni bir plan izin verir.

**DÜZELTME EKİ**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{Subscriptionıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Abonelik kimliği, SaaS.                              |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**          | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | Hayır           | İstemciden istek izleme için benzersiz bir dize değeri. Bir GUID önerilir. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.   |
| x-ms-bağıntı kimliği      | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| IF-Match /If-None-Match | Hayır           | Güçlü Doğrulayıcı ETag değeri.                              |
| içerik türü            | Evet          | `application/json`                                        |
| Yetkilendirme           | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
|  |  |  |

*Gövde*

```json
{
    "planId": ""
}
```

|  **Öğe adı** |  **Veri türü**  | **Açıklama**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  Planıd           |  (Gerekli) Dize         | SaaS hizmeti kullanıcı plan kimliği için abone oluyor.          |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme için belirli bir plan aldı.                   |
| 400                  | `BadRequest`         | Ya da üst bilgileri eksik olan veya JSON gövdesi yanlış biçimlendirilmiş gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                   |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                  |
| 409                  | `Conflict`           | Abonelik üzerinde başka bir işlem sürüyor.                     |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                          |
|  |  |  |

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi durumda bu değer iletilmezse server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu değer, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Evet          | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
| İşlem konumu | Evet          | İşlem durumunu almak için bir kaynak bağlayın.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Aboneliği silin

Abone uç noktası silme eylemini belirtilen kimliğe sahip bir aboneliği silme açmasına olanak sağlar.

*İstek*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{Subscriptionıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Abonelik kimliği, SaaS.                              |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden istek izleme için benzersiz bir dize değeri. Bir GUID önerilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.                                                           |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | SaaS abonelik etkinleştirme için belirli bir plan aldı.                   |
| 400                  | `BadRequest`         | Ya da üst bilgileri eksik olan veya JSON gövdesi yanlış biçimlendirilmiş gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                   |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                  |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işlemekle meşgul, lütfen daha sonra yeniden deneyin.                  |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak kullanılamıyor. Lütfen daha sonra yeniden deneyin.                          |
|  |  |  |

'İşlemi-konum' üst bilgisi, istek işlemin durumu için 202 yanıt, iletişim kurun. Kimlik doğrulaması diğer Market API'leri ile aynıdır.

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi takdirde bu aktarılırsa server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Evet          | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
| İşlem konumu | Evet          | İşlem durumunu almak için bir kaynak bağlayın.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>İşlem durumunu Al

Bu uç noktayı (abonelik/Aboneliği Kaldır/Değiştir planı) bir tetiklenen zaman uyumsuz işlemin durumunu izlemek kullanıcı sağlar.

*İstek*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/*{Operationıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Tetiklenen işlem benzersiz kimliği.                |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden istek izleme için benzersiz bir dize değeri. Bir GUID önerilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.   |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.  |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
|  |  |  | 

*Yanıt gövdesi*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı** | **Veri türü** | **Açıklama**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | String        | İşlemin kimliği.                                                                      |
| durum             | Sabit listesi          | İşlem durumu aşağıdakilerden biri: `In Progress`, `Succeeded`, veya `Failed`.          |
| resourceLocation   | String        | Oluşturulan veya değiştirilen aboneliğinize bağlayın. Bu, istemci güncelleştirilen durumu post işlemi almak için yardımcı olur. Bu değer için ayarlanmadı `Unsubscribe` operations. |
| oluşturuldu            | DateTime      | İşlem oluşturma zamanı UTC diliminde saat.                                                           |
| Son değiştirme       | DateTime      | Son güncelleştirme işlemi UTC diliminde saat.                                                      |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Get isteği başarıyla çözümlendi ve yanıt gövdesi içerir.    |
| 400                  | `BadRequest`         | Ya da, üst bilgiler eksik veya geçersiz bir API sürümü belirtildi gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                      |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                     |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                             |
|  |  |  |

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi takdirde bu aktarılırsa server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Evet          | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Aboneliği edinin

Get eylemini abone uç noktası ile belirtilen kaynak tanımlayıcı bir abonelik almak olanak tanır.

*İstek*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{Subscriptionıd}*?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Abonelik kimliği, SaaS.                              |
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemci, tercihen bir GUID istek izleme için benzersiz bir dize değeri. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.                                                           |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                                                                    |
|  |  |  |

*Yanıt gövdesi*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Azure abonelik kaynak kimliği, SaaS.    |
| offerId                | String        | Teklif kullanıcıya abone kimliği.         |
| Planıd                 | String        | Bir kullanıcı abone kimliği planlayın.          |
| saasSubscriptionName   | String        | SaaS abonelik adı.                |
| saasSubscriptionStatus | Sabit listesi          | İşlem durumu.  Aşağıdakilerden biri:  <br/> - `Subscribed`: Abonelik etkin değil.  <br/> - `Pending`: Kullanıcı kaynak oluşturabilirsiniz, ancak ISV tarafından etkinleştirilmez.   <br/> - `Unsubscribed`: Kullanıcı iptal etti.   <br/> - `Suspended`: Kullanıcı aboneliği askıya aldı.   <br/> - `Deactivated`:  Azure abonelik askıya alındı.  |
| oluşturuldu                | DateTime      | Abonelik oluşturma UTC zaman damgası değeri. |
| Son değiştirme           | DateTime      | Abonelik UTC zaman damgası değeri değiştirildi. |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Get isteği başarıyla çözümlendi ve yanıt gövdesi içerir.    |
| 400                  | `BadRequest`         | Ya da, üst bilgiler eksik veya geçersiz bir API sürümü belirtildi gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                      |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                     |
| 429                  | `RequestThrottleId`  | Hizmetidir istekleri işlemekle meşgul, daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Hizmet olduğu aşağı geçici olarak, daha sonra yeniden deneyin.                             |
|  |  |  |

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi takdirde bu aktarılırsa server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Hayır           | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
| eTag               | Evet          | İşlem durumunu almak için bir kaynak bağlayın.                                                        |
|  |  |  |

### <a name="get-subscriptions"></a>Abonelikleri Al

Abonelik uç noktasında alma işlemi ISV tüm abonelikler için tüm teklifleri almak açmasına olanak sağlar.

*İstek*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Parametre adı**  | **Açıklama**                                       |
|---------------------|-------------------------------------------------------|
| API sürümü         | Bu istek için kullanılacak işlem sürümü. |
|  |  |

*Üst Bilgiler*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Hayır           | İstemciden istek izleme için benzersiz bir dize değeri. Bir GUID önerilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan.             |
| x-ms-bağıntı kimliği | Hayır           | İstemci üzerinde işlem için benzersiz bir dize değeri. Bu değer, tüm istemci işlemi olayları sunucu tarafında olaylarla ilişkilendirmek için kullanılabilir. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üst bilgilerinde sağlanan. |
| Yetkilendirme      | Evet          | JSON web token (JWT) taşıyıcı belirteç.                    |
|  |  |  |

*Yanıt gövdesi*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Azure abonelik kaynak kimliği, SaaS.    |
| offerId                | String        | Teklif kullanıcıya abone kimliği.         |
| Planıd                 | String        | Bir kullanıcı abone kimliği planlayın.          |
| saasSubscriptionName   | String        | SaaS abonelik adı.                |
| saasSubscriptionStatus | Sabit listesi          | İşlem durumu.  Aşağıdakilerden biri:  <br/> - `Subscribed`: Abonelik etkin değil.  <br/> - `Pending`: Kullanıcı kaynak oluşturabilirsiniz, ancak ISV tarafından etkinleştirilmez.   <br/> - `Unsubscribed`: Kullanıcı iptal etti.   <br/> - `Suspended`: Kullanıcı aboneliği askıya aldı.   <br/> - `Deactivated`:  Azure abonelik askıya alındı.  |
| oluşturuldu                | DateTime      | Abonelik oluşturma UTC zaman damgası değeri. |
| Son değiştirme           | DateTime      | Abonelik UTC zaman damgası değeri değiştirildi. |
|  |  |  |

*Yanıt kodları*

| **HTTP durum kodu** | **Hata kodu**     | **Açıklama**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Get isteği başarıyla çözümlendi ve yanıt gövdesi içerir.    |
| 400                  | `BadRequest`         | Ya da, üst bilgiler eksik veya geçersiz bir API sürümü belirtildi gereklidir. |
| 403                  | `Forbidden`          | Çağıranın bu işlemi gerçekleştirmek için yetkili değil.                      |
| 404                  | `NotFound`           | Aboneliğiniz ile sağlanan kimliği bulunamadı                                     |
| 429                  | `RequestThrottleId`  | Hizmet istekleri işlemekle meşgul, lütfen daha sonra yeniden deneyin.                     |
| 503                  | `ServiceUnavailable` | Hizmet geçici olarak kullanılamıyor. Lütfen daha sonra yeniden deneyin.                             |
|  |  |  |

*Yanıt Üstbilgileri*

| **Üstbilgi anahtarı**     | **Gerekli** | **Açıklama**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Evet          | İstek istemciden alınan kimliği.                                                                   |
| x-ms-bağıntı kimliği | Evet          | Bağıntı kimliği istemci tarafından aksi takdirde bu aktarılırsa server bağıntı kimliğidir.                   |
| x-ms-etkinlik kimliği    | Evet          | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır. |
| Yeniden deneme sonrasında        | Hayır           | Zaman aralığı ile hangi istemci durumu kontrol edebilirsiniz.                                                       |
|  |  |  |

### <a name="saas-webhook"></a>SaaS Web kancası

Bir SaaS Web kancası değişiklikleri SaaS hizmeti için proaktif olarak bildirmek için kullanılır. Bu GÖNDERİ API kimliği doğrulanmamış olması beklenir ve Microsoft hizmeti tarafından çağrılır. SaaS hizmeti doğrulamak ve Web kancası bildirim eylemi gerçekleştirmeden önce onları yetkilendirmek için API işlemleri çağırmak için bekleniyor. 

*Gövde*

``` json
  {
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "action": "Subscribe", // Subscribe/Unsubscribe/ChangePlan
    "operationRequestSource":"Azure",
    "timeStamp":"2018-12-01T00:00:00"
  }
```

| **Parametre adı**     | **Veri türü** | **Açıklama**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | String       | Tetiklenen işlem benzersiz kimliği.                |
| activityId   | String        | Hizmet isteği izlemek için benzersiz bir dize değeri. Bu, tüm Mutabakatları için kullanılır.               |
| subscriptionId                     | String        | Azure abonelik kaynak kimliği, SaaS.    |
| offerId                | String        | Teklif kullanıcıya abone kimliği. Yalnızca "Güncelleştir" eylemi ile sağlanır.        |
| publisherId                | String        | Yayımcı kimliği SaaS teklifi         |
| Planıd                 | String        | Bir kullanıcı abone kimliği planlayın. Yalnızca "Güncelleştir" eylemi ile sağlanır.          |
| action                 | String        | Bu bildirim tetikleme eylem. Olası değerler - etkinleştirme, silme, askıda kalma, eski duruma getirme, güncelleştirme          |
| Zaman damgası                 | String        | Bu bildirim tetiklendiğinde UTC zaman damgası değeri.          |
|  |  |  |


## <a name="next-steps"></a>Sonraki adımlar

Geliştiriciler ayrıca programlı bir şekilde alabilir ve işleme iş yükleri, teklifler ve yayımcı profilini kullanarak [bulut iş ortağı portalı REST API'lerini](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
