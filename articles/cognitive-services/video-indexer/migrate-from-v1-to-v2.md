---
title: Azure Video Indexer API v1'den v2'ye geçirme | Microsoft Docs
description: Bu konuda, Azure Video Indexer API v1'den v2'ye geçirme açıklanmaktadır.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 1883c449def44f6c54c6f20317de39ada405643f
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388979"
---
# <a name="migrate-from-the-video-indexer-api-v1-to-v2"></a>Video Indexer API v1'den v2'ye geçirme

> [!Note]
> Video Indexer V1 API, 1 Ağustos 2018'de kullanım dışı bırakıldı. Artık, Video Indexer v2 API'si kullanmanız gerekir. <br/>Video Indexer v2 API'leri ile geliştirme için lütfen bulunan yönergeleri okuyun [burada](https://api-portal.videoindexer.ai/). 

Bu makalede, v2'de sunulan değişiklikler açıklanmaktadır.  

## <a name="api-changes"></a>API değişiklikleri

### <a name="authorization-and-operations"></a>Yetkilendirme ve işlemler

V2 sürümü, Video Indexer API kimlik doğrulama ve yetkilendirme modeli değişti. İki API kümesi vardır: 

* Yetkilendirme 
* İşlemler

**Yetkilendirme** API çağırmak için erişim belirteçleri almak için kullanılan **işlemleri** API. **İşlemleri** API içeren tüm Video Indexer API, karşıya video öngörüleri alın ve diğer işlemler gibi.

Sonra [abone](video-indexer-get-started.md) için **yetkilendirme** API oluşturabileceksiniz (v1'de yaptığınız gibi.) abonelik anahtarınızı geçirerek erişim belirteçleri almak

## <a name="getting-and-using-the-access-token-for-operations-apis"></a>Alma ve API işlemleri için erişim belirtecini kullanarak

Çağrılırken **işlemleri** API'leri, abonelik anahtarı kalmaz kullanılabilir artık. Bunun yerine, erişim belirteçleri elde geçer **yetkilendirme** API. 

Her istek, aradığınız API erişim düzeyini eşleşen geçerli bir belirteç olması gerekir. Örneğin, bir kullanıcı erişim belirteci, kullanıcı hesaplarınızı alma gibi işlemleri gerektirir. Hesap işlemleri düzey olan tüm videoları listesi gibi bir hesap erişim belirtecini gerektirir. Videolar, reindex video gibi işlemler, bir hesap erişim belirtecini ya da bir video erişim belirteci gerektirir.

İşleri kolaylaştırmak için kullanabileceğiniz **yetkilendirme** API > **GetAccounts** hesaplarınızı kullanıcı sağlanmadan önce belirteci alma işlemi. Aynı zamanda geçerli belirteçleriyle hesaplarını almak bir hesap belirteci almak için ek bir arama atlamanızı etkinleştirme sorabilirsiniz.

Farklı bir erişim belirteci hakkında daha fazla bilgi için bkz: [Azure Video Indexer API kullanma](video-indexer-use-apis.md).

### <a name="locations"></a>Konumlar

Video Indexer hesabınız konumunu API'sine yapılan her çağrı içermelidir. Konum olmadan veya yanlış bir konum API çağrıları başarısız olur.

Aşağıdaki tabloda açıklanan değerlere uygulanır. **Parametre değerine** ne zaman geçirdiğiniz değer API kullanıyor.

|**Ad**|**Parametre değeri**|**Açıklama**|
|---|---|---|
|Deneme|deneme|Deneme hesapları için kullanılır. Örneğin: https://api.videoindexer.ai/trial/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Batı ABD|westus2|Azure Batı ABD 2 bölgesinde için kullanılır.  Örneğin: https://api.videoindexer.ai/westus2/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Kuzey Avrupa |northeurope|Azure Kuzey Avrupa bölgesinde için kullanılır. Örneğin:  https://api.videoindexer.ai/northeurope/Accounts/{accountId}/Videos/{videoId}/Index?language=English. |
|Doğu Asya|eastasia|Azure Doğu Asya bölgesi kullanılır. Örneğin:  https://api.videoindexer.ai/eastasia/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|

### <a name="data-model"></a>Veri modeli

Video Indexer, artık çok daha net öngörüleri sunmak için Basitleştirilmiş veri modeline sahiptir. V2 API'si tarafından oluşturulan çıktı hakkında daha fazla bilgi için bkz: [v2 API'si tarafından üretilen Video dizinleyici çıktısını İnceleme](video-indexer-output-json-v2.md).

### <a name="swagger"></a>Swagger

Video Indexer API tanımlarını buna uygun olarak güncelleştirildi ve aracılığıyla indirilebilir [API portalı](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).


### <a name="v1-vs-v2-examples"></a>V1 ve V2 örnekleri

Yeni API'leri V2, 3 ana parametreleri içerir:

1. [Yukarıda açıklanan şekilde konumu] -. Deneme, westus2, northeurope ya da ping'in ekran.
2. [YOUR_ACCOUNT_ID] - hesabınızın kimliği bir GUID. Tüm hesaplar (aşağıda açıklanmıştır) alırken alınır.
3. [YOUR_VIDEO_ID] - videonuzu (örneğin, "d4fa369abc") kimliği. Bir video karşıya yüklenirken veya videolar için arama yaparken döndürdü.

#### <a name="uploading-a-video-in-v1"></a>V1 bulunan bir videoyu karşıya yükleniyor:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=some_name&description=some_description&privacy=private&videoUrl=http://URL_TO_YOUR_VIDEO
```

#### <a name="uploading-a-video-in-v2"></a>V2'de bir video karşıya yükleniyor:

1. Erişim için karşıya yükleme isteği belirteci alma:

  Ya da tüm hesapları ve bunların erişim belirteçleri elde edebilirsiniz:

    ```
    https://api.videoindexer.ai/auth/[LOCATION]/Accounts?generateAccessTokens=true&allowEdit=true
    ```

  Veya özel bir hesap erişim belirteci alma:
  
  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/AccessToken?allowEdit=true
  ```
2. Videoyu karşıya yükle:

  ```
  POST https://api.videoindexer.ai/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos?name=MySample&description=MySampleDescription&videoUrl=[URL_ENCODED_VIDEO_URL]&accessToken=eyJ0eXAiOiJ...
  ```

#### <a name="getting-insights-in-v1"></a>V1'de öngörü alma:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]
```
  
#### <a name="getting-insights-in-v2"></a>V2'de öngörü alma:

1. Hesap erişim belirtecini kullanabilir veya bir video düzeyinde erişim belirteci alın:

  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/AccessToken
  ```
  
2. Öngörüler elde edin:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

#### <a name="getting-video-processing-state-in-v1"></a>V1 alınırken video işleme durumu:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]/State
```
  
#### <a name="getting-video-processing-state-in-v2"></a>V2'de başlangıç video işleme durumu:

API v2'de, işleme durumunu alma Video dizini API'si bir parçası olarak döndürülür.

1. Hesap erişim belirtecini kullanabilir veya bir video düzeyinde erişim belirteci alın:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```
  
2. Öngörüler elde edin:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Video dizinleyici API'sini kullanma](video-indexer-use-apis.md)

