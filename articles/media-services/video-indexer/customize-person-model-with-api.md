---
title: Video Indexer API kişi modeli - Azure'ı özelleştirmek için kullanın
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer API ile bir kişi modeli özelleştirme gösterilmektedir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: anzaman
ms.openlocfilehash: e5a34a75c73401c567a0e898a1ce9f85cde96586
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553717"
---
# <a name="customize-a-person-model-with-the-video-indexer-api"></a>Video Indexer API ile bir kişi modeli özelleştirme

Video Indexer, video içeriği için yüz algılama ve ünlü tanıma destekler. Ünlü tanıma özelliği, yaklaşık IMDB Wikipedia ve üst LinkedIn öğrenilenler gibi sık istenen bir veri kaynağına göre bir milyon yüzleri kapsar. Ünlü tanıma özelliği tarafından tanınmayan yüz algılandı; Ancak, sol adlandırılmamış. Görüntü, Video Indexer için karşıya yükleme ve sonuçları geri alma sonra geri dönün ve değil tanındı yüzleri adlandırın. Bir ada sahip bir yüz etiket sonra adı ve yüz hesabınızın kişi modele eklenir. Video Indexer, ardından bu yüz gelecekteki videoları ve son videolar algılar.

Video Indexer API, bu konuda açıklandığı bir video, algılanan yüzeylere düzenlemek için kullanabilirsiniz. Video Indexer Web sitesinde açıklandığı gibi kullanabilirsiniz [özelleştirme kişi modeli Video Indexer Web sitesini kullanarak](customize-person-model-with-api.md).

## <a name="managing-multiple-person-models"></a>Birden çok kişi modeli yönetme 

Video Indexer, hesap başına birden çok kişi modelleri destekler. Bu özellik şu anda yalnızca Video Indexer API kullanılabilir.

Farklı kullanım örneği senaryoları için hesabınızı oluşturabilmesine olanak sağlar, hesap başına birden çok kişi modeller oluşturmak isteyebilirsiniz. Örneğin, içeriğiniz için Spor ilişkili ise, ardından her bir spor (futbol, Basketbol, futbol, vb.) için ayrı bir kişi modeli oluşturabilirsiniz. 

Bir model oluşturulduktan sonra karşıya yükleme/dizin ya da bir video ölçeklemek belirli bir kişi modelin model kimliği sağlayarak kullanabilirsiniz. Video için yeni bir yüz eğitim videosu ile ilişkilendirildi belirli özel model güncelleştirir.

Her hesap 50 kişi modelleri sınırı vardır. Birden çok kişi modeli desteği gerekmiyorsa, bir kişinin karşıya yükleme/dizin oluşturma veya yeniden dizin oluşturmaya videonuza model kimliği atamayın. Bu durumda, Video Indexer, hesabınızdaki varsayılan özel kişi modelini kullanır.

## <a name="create-a-new-person-model"></a>Yeni bir kişi model oluşturma

Yeni bir kişi modeli belirtilen hesabı oluşturun. 

### <a name="request-url"></a>İstek URL'si

Bir POST isteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?name={name}&accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X POST "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?name={name}&accessToken={accessToken}"
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-Person-Model?).

### <a name="request-parameters"></a>İstek parametreleri 

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ad|string|Evet|Kişi model adı|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Oluşturulan model kişinin Kimliğini ve adını, yeni oluşturduğunuz aşağıdaki örnekte biçimi aşağıdaki model isteğin yanıtını verir.

```json
{
    "id": "227654b4-912c-4b92-ba4f-641d488e3720",
    "name": "Example Person Model"
}
```

Ardından kullanmalısınız **kimliği** değerini **personModelId** parametre olduğunda [dizinine bir video karşıya](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) veya [video ölçeklemek](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?).

## <a name="delete-a-person-model"></a>Bir kişi modeli Sil

Özel bir kişi model belirtilen hesaptan silin. 

Kişi model başarıyla silinirse, silinen modeli kullandığınız videolarınızı geçerli dizini bunları yeniden kadar değişmeden kalır. Ancak temel silinen modelde adlandırılmıştı yüzleri Video Indexer tarafından geçerli videolarınızı bu modeli kullanarak sıralanan tanınmayacak; Ancak, yine de bu yüzeyleri algılanır. Silinen modelini kullanarak sıralanan videolarınızı geçerli artık hesabınıza ait varsayılan Kişi modeli kullanır. Silinen modelinden yüzleri hesabınızın varsayılan modelinde olarak da adlandırılır, bu yüzeyleri videoları tanınması devam eder.

### <a name="request-url"></a>İstek URL'si

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels/{id}?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.
```curl
curl -v -X DELETE "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels/{id}?accessToken={accessToken}"
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Person-Model?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|id|string|Evet|Kişi model kimliği (kişi modeli oluşturduğunuzda oluşturulur)|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Kişi model başarıyla silindiğinde döndürülen içerik yok.

## <a name="get-all-person-models"></a>Tüm kişi modelleri Al

Tüm kişi modelleri belirtilen hesapta alın. 

### <a name="request-call"></a>İstek araması

Bir GET isteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?accessToken={accessToken}"
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Person-Models?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Yanıt kişi modelleri (varsayılan Kişi modeli belirtilen hesapta dahil), hesabınızdaki tüm ve her biri kendi adları ve aşağıdaki örnekte biçimi aşağıdaki kimlikleri listesini sağlar.

```json
[
    {
        "id": "59f9c326-b141-4515-abe7-7d822518571f",
        "name": "Default"
    }, 
    {
        "id": "9ef2632d-310a-4510-92e1-cc70ae0230d4",
        "name": "Test"
    }
]
```

Hangi modeli kullanarak bir video için kullanmak istediğiniz seçebilirsiniz **kimliği** için kişi model değerini **personModelId** parametre olduğunda [dizinine bir video karşıya](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) veya [video ölçeklemek](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?).

## <a name="update-a-face"></a>Bir yüzü güncelleştir

Bu komut, videoda bir yüzün videonun kimliği ve yüz Kimliğini kullanarak bir ad ile güncelleştirmenizi sağlar. Bu videoyu karşıya yükleme/dizin oluşturma veya yeniden dizin oluşturmaya ilişkili kişi model güncelleştirir. Hiçbir kişi modeli atanmışsa, hesabın varsayılan Kişi modelini güncelleştirir. 

Bu gerçekleştikten sonra aynı yüz aynı kişi modeli paylaşan diğer geçerli videolarınızı, oluşumunu tanır. Diğer geçerli videolarınızı bir yüz tanıma, bir toplu işlem olarak etkili olması için biraz zaman alabilir.

Video Indexer, yeni bir adla bir ünlü tanınan bir yazıtipi güncelleştirebilirsiniz. Size yeni bir ad yerleşik ünlü tanıma öncelikli olur.

### <a name="request-call"></a>İstek araması

Bir POST isteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Videos/{videoId}/Index/Faces/{faceId}?accessToken={accessToken}&newName={newName}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X PUT "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Videos/{videoId}/Index/Faces/{faceId}?accessToken={accessToken}&newName={newName}"
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Video-Face?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|videoId|string|Evet|Güncelleştirmek istediğiniz yüzü göründüğü videonun kimliği. Bu video karşıya yüklendi ve dizini oluşturulur.|
|Faceıd|integer|Evet|Güncelleştirilecek yüz kimliği. Video dizinden Faceıd alabilirsiniz|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|
|ad|string|Evet|Yüz tanıma güncelleştirmek için yeni adı.|

İki farklı verirseniz aynı kişinin yüzlerini aynı modellemek için adları kişi modelleri için benzersiz **adı** parametre değeri, Video Indexer'ın aynı kişi yüzleri görünümleri ve videonuzu yeniden sonra bunları uygun sonuç verir. 

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Yüz tanıma başarıyla güncelleştirildiğinde döndürülen içerik yok.

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer Web sitesini kullanarak kişi modeli özelleştirme](customize-person-model-with-website.md)
