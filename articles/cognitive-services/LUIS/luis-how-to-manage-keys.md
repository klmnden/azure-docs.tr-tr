---
title: LUIS yazma ve uç nokta anahtarları yönetme
titleSuffix: Azure Cognitive Services
description: Azure portalında bir LUIS uç noktası anahtarı oluşturduktan sonra anahtar LUIS uygulaması için atama ve doğru uç nokta URL'sini alın. LUIS Öngörüler almak için bu uç nokta URL'sini kullanın.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 62081f96e2081833eb705992914899a6764bd792
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033218"
---
# <a name="add-an-azure-luis-resource-to-app"></a>Bir Azure LUIS kaynak uygulamaya ekleme

Azure portalında bir LUIS kaynak oluşturduktan sonra LUIS uygulaması için kaynak atayın ve doğru uç nokta URL'sini alın. LUIS Öngörüler almak için bu uç nokta URL'sini kullanın.

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
<a name="key-concepts"></a>
<a name="authoring-key"></a>
<a name="create-and-use-an-endpoint-key"></a>
<a name="assign-endpoint-key"></a>

## <a name="assign-resource"></a>Kaynak atama

1. Bir LUIS anahtar oluşturmak [Azure portalında](https://portal.azure.com). Daha fazla bilgi için bkz. [Azure'ı kullanarak bir uç noktası anahtarı oluşturma](luis-how-to-azure-subscription.md).
 
2. Seçin **Yönet** bölmenin sağ üst menüdeki içinde seçip **anahtarları ve uç noktaları**.

    [ ![Anahtarlar ve uç noktaları sayfası](./media/luis-manage-keys/keys-and-endpoints.png) ](./media/luis-manage-keys/keys-and-endpoints.png#lightbox)

3. LUIS eklemek için seçin **Kaynak Ata +**.

    ![Uygulamanıza bir kaynak atayın](./media/luis-manage-keys/assign-key.png)

4. LUIS Web sitesine ile oturum açma e-posta adresiyle ilişkili iletişim kutusunda bir kiracı seçin.  

5. Seçin **abonelik adı** eklemek istediğiniz Azure kaynakla ilişkilendirilmiş.

6. Seçin **LUIS kaynak adı**. 

7. Seçin **atama kaynak**. 

8. Tabloya yeni satır bulun ve uç nokta URL'sini kopyalayın. Doğru bir tahmin için LUIS uç noktasına bir HTTP GET isteği yapmak için oluşturulur. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="unassign-resource"></a>Kaynak atamasını Kaldır
Uç nokta ataması, Azure'dan silinmez. Yalnızca LUIS bağlantısı kesilebilir. 

Uç noktası anahtarı atanmamış veya uygulamaya atanmamış herhangi için uç nokta URL'sini, bir hata döndürür istek: `401 This application cannot be accessed with the current subscription`. 

## <a name="include-all-predicted-intent-scores"></a>Amaç tüm tahmin edilen puanları içerir
**INCLUDE tüm hedefi puanları tahmin** onay kutusu her amacını tahmin puanını dahil etmek uç nokta sorgu yanıt verir. 

Bu ayar, sohbet botu veya döndürülen ıntents puanları temel alarak programlı karar vermek için arama LUIS uygulamanızı sağlar. Genellikle üst iki amacı en ilginç var. En çok puan hiçbiri hedefi yok hedefi ve diğer yüksek puanlı amacını arasında kesin bir seçim yapar izleme bir soru sorun, sohbet botu seçebilirsiniz ise. 

Hedefleri ve puanlarını Ayrıca, uç nokta günlükleri dahil. Yapabilecekleriniz [dışarı](luis-how-to-start-new-app.md#export-app) Bu günlükleri ve puanları analiz edin. 

```JSON
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

## <a name="enable-bing-spell-checker"></a>Bing yazım denetimi etkinleştir 
İçinde **uç nokta URL'si ayarları**, **Bing yazım denetleyicisi** geçiş sözcüklerin tahmin önce düzeltmek LUIS sağlar. Oluşturma bir  **[Bing yazım denetimi anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. 

Ekleme **yazım denetimi = true** querystring parametresi ve **bing-yazım-onay-subscription-key = {YOUR_BING_KEY_HERE}** . Değiştirin `{YOUR_BING_KEY_HERE}` Bing yazım denetleyicisi anahtarınızı.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```


## <a name="publishing-regions"></a>Yayımlama bölgeleri

Yayımlama hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md) yayımlama çalıştırmasına dahil olmak üzere [Avrupa](luis-reference-regions.md#publishing-to-europe), ve [Avustralya](luis-reference-regions.md#publishing-to-australia). Yayımlama bölgeler bölge geliştirme farklıdır. Yazma bölgesinde sorgu uç nokta için istediğiniz yayımlama bölgeyi karşılık gelen bir uygulama oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Anahtarınızı uygulamanızda yayımlayacağınızı **uygulamayı Yayımla** sayfası. Yayımlama hakkında yönergeler için bkz: [uygulamayı Yayımla](luis-how-to-publish-app.md).

Bkz: [LUIS anahtarlarında](luis-concept-keys.md) LUIS yazma ve uç nokta temel kavramları anlamak için.