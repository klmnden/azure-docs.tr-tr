---
title: Azure Video dizin oluşturucu API v1 ile v2'ye geçirme | Microsoft Docs
description: Bu konuda, Azure Video dizin oluşturucu API v1 ile v2'ye geçirme açıklanmaktadır.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/13/2018
ms.author: juliako
ms.openlocfilehash: 00f5a0d7c9a37c24866cd5e1259800d5e431ccc3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356474"
---
# <a name="migrate-from-the-video-indexer-api-v1-to-v2"></a>Video dizin oluşturucu API v1 ile v2'ye geçirme

> [!Note]
> Video dizin oluşturucu V1 API'leri artık kullanımdan kaldırıldı ve 1 Ağustos 2018 kaldırılacak. Kesintilerini önlemek için Video dizin oluşturucu v2 API'leri kullanarak başlamanız gerekir.
>
> Video dizin oluşturucu v2 API'leri ile geliştirmek için lütfen bulunan yönergeleri başvurun [burada](https://api-portal.videoindexer.ai/). 

Bu makalede, v2'sunulan değişiklikleri açıklar.  

## <a name="api-changes"></a>API değişiklikleri

### <a name="authorization-and-operations"></a>Yetkilendirme ve işlemleri

V2 sürümünde Video dizin oluşturucu API kimlik doğrulama ve yetkilendirme modeli değişti. API iki kümesi vardır: 

* Yetkilendirme 
* İşlemler

**Yetkilendirme** API çağırma erişim belirteçleri elde etmek için kullanılan **Operations** API. **Operations** API'yi içeren tüm Video dizin oluşturucu API'ler, karşıya yükleme video, Get Öngörüler ve diğer işlemleri gibi.

Bir kez, [abone](video-indexer-get-started.md) için **yetkilendirme** API, görebilirsiniz (yalnızca v1 yaptığınız gibi.) abonelik anahtarınızı geçirerek erişim belirteçleri almak

## <a name="getting-and-using-the-access-token-for-operations-apis"></a>API işlemleri için erişim belirtecini kullanarak ve alma

Çağrılırken **Operations** API'leri, abonelik anahtarı kalmaz kullanılabilir artık. Bunun yerine, elde erişim belirteçleri geçer **yetkilendirme** API. 

Her istek aradığınız API erişim düzeyini eşleşen geçerli bir belirteci olması gerekir. Örneğin, bir kullanıcı erişim belirteci, kullanıcı hesaplarınızı, alma gibi işlemleri gerektirir. Hesap işlemleri düzey olan tüm videoları listesi gibi bir hesap erişim belirteci gerektirir. Videolar arat video gibi işlemleri, bir hesap erişim belirteci ya da bir video erişim belirteci gerektirir.

İşleri kolaylaştırmak için kullanabileceğiniz **yetkilendirme** API > **GetAccounts** hesaplarınızı kullanıcı sağlanmadan önce belirtecini almak için. Aynı zamanda geçerli belirteçlerini hesaplarıyla almak bir hesap belirteç almak üzere ek bir arama atlamak etkinleştirme isteyebilir.

Farklı erişim belirteçleri hakkında daha fazla bilgi için bkz: [Azure Video dizin oluşturucu API kullanın](video-indexer-use-apis.md).

### <a name="locations"></a>Konumlar

Her API çağrısı Video dizin oluşturucu hesabınızı konumunu içermelidir. API çağrıları konumu olmadan veya yanlış konumu başarısız olur.

Aşağıdaki tabloda açıklanan değerleri uygulayın. **Parametre değerine** ne zaman geçirdiğiniz değeri API'yi kullanarak.

|**Ad**|**Parametre değeri**|**Açıklama**|
|---|---|---|
|Deneme|deneme|Deneme hesapları için kullanılır. Örneğin: https://api.videoindexer.ai/trial/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Batı ABD|westus2|Azure Batı ABD 2 bölge için kullanılır.  Örneğin: https://api.videoindexer.ai/westus2/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Kuzey Avrupa |northeurope|Azure Kuzey Avrupa bölge için kullanılır. Örneğin:  https://api.videoindexer.ai/northeurope/Accounts/{accountId}/Videos/{videoId}/Index?language=English. |
|Doğu Asya|eastasia|Azure Doğu Asya bölge için kullanılır. Örneğin:  https://api.videoindexer.ai/eastasia/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|

### <a name="data-model"></a>Veri modeli

Video dizin oluşturucu artık çok daha net Öngörüler teslim etmek için bir Basitleştirilmiş veri modeline sahiptir. V2 API'si tarafından üretilen çıkış hakkında daha fazla bilgi için bkz: [v2 API'si tarafından üretilen Video dizin oluşturucu çıktıyı inceleyin](video-indexer-output-json-v2.md).

### <a name="swagger"></a>Swagger

Video dizin oluşturucu API tanımlarını buna uygun olarak güncelleştirildi ve aracılığıyla indirilebilir [API portal](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).


### <a name="v1-vs-v2-examples"></a>V1 vs V2 örnekleri

Yeni V2 API'leri 3 ana parametreleri içerir:

1. [Yukarıda açıklandığı gibi konumu] -. Deneme, westus2, northeurope ya da eastasia.
2. [YOUR_ACCOUNT_ID] - hesabınızı kimliği bir GUID. (Aşağıda açıklanmıştır) tüm hesapları alırken aldı.
3. [YOUR_VIDEO_ID] - videonuzu (örneğin, "d4fa369abc") kimliği. Bir video karşıya yüklenirken veya videolar için arama yaparken döndürdü.

#### <a name="uploading-a-video-in-v1"></a>V1 videoda karşıya yükleniyor:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=some_name&description=some_description&privacy=private&videoUrl=http://URL_TO_YOUR_VIDEO
```

#### <a name="uploading-a-video-in-v2"></a>V2 videoda karşıya yükleniyor:

1. Karşıya yükleme isteği bir erişim belirteci alın:

  Ya da tüm hesapları ve bunların erişim belirteçleri elde edebilirsiniz:

    ```
    https://api.videoindexer.ai/auth/[LOCATION]/Accounts?generateAccessTokens=true&allowEdit=true
    ```

  Veya belirli hesap erişim belirteci alın:
  
  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/AccessToken?allowEdit=true
  ```
2. Bir video karşıya yükle:

  ```
  POST https://api.videoindexer.ai/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos?name=MySample&description=MySampleDescription&videoUrl=[URL_ENCODED_VIDEO_URL]&accessToken=eyJ0eXAiOiJ...
  ```

#### <a name="getting-insights-in-v1"></a>V1 cihazımdan öngörüleri:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]
```
  
#### <a name="getting-insights-in-v2"></a>Öngörüler V2'alınıyor:

1. Hesap erişim belirteci kullanabilir veya bir video düzeyi erişim belirteci alın:

  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/AccessToken
  ```
  
2. Öngörüler alın:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

#### <a name="getting-video-processing-state-in-v1"></a>V1 alma video işleme durumu:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]/State
```
  
#### <a name="getting-video-processing-state-in-v2"></a>V2 alma video işleme durumu:

API v2'in işleme durumunu Al Video dizin API bir parçası olarak döndürülür.

1. Hesap erişim belirteci kullanabilir veya bir video düzeyi erişim belirteci alın:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```
  
2. Öngörüler alın:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Video dizin oluşturucu API kullanın](video-indexer-use-apis.md)

