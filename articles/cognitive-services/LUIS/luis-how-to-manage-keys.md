---
title: Anahtarları yönetme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Azure portalında bir LUIS uç noktası anahtarı oluşturduktan sonra anahtar LUIS uygulaması için atama ve doğru uç nokta URL'sini alın. LUIS Öngörüler almak için bu uç nokta URL'sini kullanın.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 0a278ded7ce290347645f345e4eee0b15972787f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53088111"
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
<a name="assign-resource"></a>


## <a name="assign-resource-in-luis-portal"></a>LUIS portalında kaynak atayın

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

### <a name="unassign-resource"></a>Kaynak atamasını Kaldır
Uç nokta ataması, Azure'dan silinmez. Yalnızca LUIS bağlantısı kesilebilir. 

Uç noktası anahtarı atanmamış veya uygulamaya atanmamış herhangi için uç nokta URL'sini, bir hata döndürür istek: `401 This application cannot be accessed with the current subscription`. 

### <a name="include-all-predicted-intent-scores"></a>Amaç tüm tahmin edilen puanları içerir
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

### <a name="enable-bing-spell-checker"></a>Bing yazım denetimi etkinleştir 
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


### <a name="publishing-regions"></a>Yayımlama bölgeleri

Yayımlama hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md) yayımlama çalıştırmasına dahil olmak üzere [Avrupa](luis-reference-regions.md#publishing-to-europe), ve [Avustralya](luis-reference-regions.md#publishing-to-australia). Yayımlama bölgeler bölge geliştirme farklıdır. Yazma bölgesinde sorgu uç nokta için istediğiniz yayımlama bölgeyi karşılık gelen bir uygulama oluşturun.

## <a name="assign-resource-without-luis-portal"></a>LUIS portalı olmadan kaynak atayın

Bir CI/CD işlem hattı gibi Otomasyon amacıyla bir LUIS uygulaması LUIS kaynağa atamasını otomatik hale getirmek isteyebilirsiniz. O sırada, aşağıdaki adımları tamamlamanız gerekir:

1. Azure Resource Manager bu belirteci alma [Web sitesi](https://resources.azure.com/api/token?plaintext=true). Bu belirtecin süresi dolacak şekilde hemen kullanın. İstek, bir Azure Resource Manager belirtecini döndürür.

    ![Azure Resource Manager belirteci istemek ve Azure Resource Manager belirteci alma](./media/luis-manage-keys/get-arm-token.png)

1. LUIS kaynakları abonelikler arasında istek için belirteci kullanın [azure alma LUIS API'si hesapları](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), kullanıcı hesabınızın erişebilir. 

    Bu POST API'sini aşağıdaki ayarları gerektirir:

    |Üst bilgi|Değer|
    |--|--|
    |`Authorization`|Değerini `Authorization` olduğu `Bearer {token}`. Belirteç değeri sözcük gelmelidir fark `Bearer` ve boşluk.| 
    |`Ocp-Apim-Subscription-Key`|[Anahtar yazma](luis-how-to-account-settings.md).|

    Bu API bir LUIS aboneliğiniz, abonelik kimliği, kaynak grubu ve kaynak adı, hesap adı olarak döndürülen dahil olmak üzere JSON nesne dizisi döndürür. LUIS uygulaması için atama LUIS kaynak dizideki bir öğe bulur. 

1. LUIS kaynak belirteci atamak [bir LUIS azure hesapları bir uygulamaya atama](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) API. 

    Bu POST API'sini aşağıdaki ayarları gerektirir:

    |Tür|Ayar|Değer|
    |--|--|--|
    |Üst bilgi|`Authorization`|Değerini `Authorization` olduğu `Bearer {token}`. Belirteç değeri sözcük gelmelidir fark `Bearer` ve boşluk.|
    |Üst bilgi|`Ocp-Apim-Subscription-Key`|[Anahtar yazma](luis-how-to-account-settings.md).|
    |Üst bilgi|`Content-type`|`application/json`|
    |Sorgu dizesi|`appid`|LUIS app kimliği. 
    |Gövde||{"Azuresubscriptionıd": "ddda2925-af7f-4b05-9ba1-2155c5fe8a8e"<br>"ResourceGroup": "resourcegroup-2"<br>"AccountName": "luıs-uswest-S0-2"}|

    Bu API, başarılı olduğunda, 201 - oluşturuldu durumuna döndürür. 

## <a name="next-steps"></a>Sonraki adımlar

Anahtarınızı uygulamanızda yayımlayacağınızı **uygulamayı Yayımla** sayfası. Yayımlama hakkında yönergeler için bkz: [uygulamayı Yayımla](luis-how-to-publish-app.md).

Bkz: [LUIS anahtarlarında](luis-concept-keys.md) LUIS yazma ve uç nokta temel kavramları anlamak için.
