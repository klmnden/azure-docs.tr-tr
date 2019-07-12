---
title: Abonelik anahtarları
titleSuffix: Language Understanding - Azure Cognitive Services
description: Ücretsiz ilk 1000 uç nokta sorgularınızı kullanmak için Abonelik anahtarları oluşturma gerekmez. Alırsanız bir _kotasından_ hata formunda bir HTTP 403 ve 429 gereken bir anahtar oluşturun ve uygulamanıza atayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/10/2019
ms.author: diberry
ms.openlocfilehash: dedc498ebc910b448b1684136c288b2045780e00
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797944"
---
# <a name="using-subscription-keys-with-your-luis-app"></a>LUIS uygulamanız ile abonelik anahtarlarını kullanma

Language Understanding (LUIS) ilk kez kullandığınızda, Abonelik anahtarları oluşturma gerekmez. 1000 uç nokta sorguları başlangıç olarak verilir. 

Test ve yalnızca prototip için ücretsiz katman (F0) kullanın. Üretim sistemleri için bir [Ücretli](https://aka.ms/luis-price-tier) katmanı. Kullanmayın [anahtar yazma](luis-concept-keys.md#authoring-key) için üretim uç noktası sorgular.


<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"/>

## <a name="create-prediction-endpoint-runtime-resource-in-the-azure-portal"></a>Azure portalında tahmin uç çalışma zamanı kaynağı oluşturma

Oluşturduğunuz [tahmin uç nokta kaynağı](get-started-portal-deploy-app.md#create-the-endpoint-resource) Azure portalında. Bu kaynak, yalnızca uç nokta tahmin sorguları için kullanılmalıdır. Bu kaynak, uygulama geliştirme değişiklikler için kullanmayın.

Language Understanding kaynak veya Bilişsel hizmetler kaynağı oluşturabilirsiniz. Language Understanding kaynak oluşturuyorsanız, bir kaynak türü için kaynak adı postpend alışkanlıktır. 

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

### <a name="using-resource-from-luis-portal"></a>Kaynak LUIS portalından kullanma

LUIS portalından kaynak kullanıyorsanız, konum ve anahtar bilmeniz gerekmez. Bunun yerine, kaynak Kiracı, abonelik ve kaynak adını bilmeniz gerekir.

Sonra [atama](#assign-resource-key-to-luis-app-in-luis-portal) kaynağınıza LUIS uygulamanızı LUIS portal, anahtar ve konumda Yönet bölümünün içinde sorgu tahmin uç nokta URL'SİNİN bir parçası olarak sağlanan **anahtarları ve uç nokta ayarları** sayfası.
 
### <a name="using-resource-from-rest-api-or-sdk"></a>Kaynak REST API veya SDK'sını kullanma

SDK'sını ve REST API(s) kaynak kullanıyorsanız, konum ve anahtar bilmeniz gerekir. Bu bilgiler Yönet bölümünün içinde sorgu tahmin uç nokta URL'SİNİN bir parçası olarak sağlanan **anahtarları ve uç nokta ayarları** de Azure portalı, kaynağın genel bakış ve anahtarları sayfalarında olduğu gibi sayfa.

## <a name="assign-resource-key-to-luis-app-in-luis-portal"></a>Kaynak anahtarı LUIS Portalı'nda LUIS uygulama atama

LUIS için yeni bir kaynak oluşturmak her zaman, istediğiniz [LUIS uygulaması için kaynak atayın](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal). Atandıktan sonra yeni bir kaynak oluşturmadığınız sürece bu adımı tekrar yapmanız gerekmez. Uygulamanızı bölgeleri genişletin veya daha yüksek bir sayı tahmin sorguları desteklemek için yeni bir kaynak oluşturabilir.

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
İçinde **uç nokta URL'si ayarları**, **Bing yazım denetleyicisi** geçiş sözcüklerin tahmin önce düzeltmek LUIS sağlar. Oluşturma bir  **[Bing yazım denetimi anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)** . 

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

Bir CI/CD işlem hattı gibi Otomasyon amacıyla bir LUIS uygulaması LUIS kaynağa atamasını otomatik hale getirmek isteyebilirsiniz. Bunu yapmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

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

## <a name="change-pricing-tier"></a>Fiyatlandırma katmanını değiştirme

1.  İçinde [Azure](https://portal.azure.com), LUIS aboneliğinizi bulun. LUIS aboneliği seçin.
    ![LUIS aboneliğinizi bulun](./media/luis-usage-tiers/find.png)
1.  Seçin **fiyatlandırma katmanı** kullanılabilir fiyatlandırma katmanlarına görmek için. 
    ![Fiyatlandırma katmanları görüntüleyin](./media/luis-usage-tiers/subscription.png)
1.  Fiyatlandırma katmanı seçin seçip **seçin** değişikliği kaydetmek için. 
    ![LUIS ödeme katmanınızı değiştirin](./media/luis-usage-tiers/plans.png)
1.  Fiyat değişikliği tamamlandıktan sonra bir açılır pencere yeni fiyatlandırma katmanına doğrular. 
    ![LUIS ödeme katmanınızı doğrulayın](./media/luis-usage-tiers/updated.png)
1. Unutmayın [Bu uç noktası anahtarı atama](#assign-endpoint-key) üzerinde **Yayımla** sayfasında ve tüm uç nokta sorguları kullanın. 

## <a name="fix-http-status-code-403-and-429"></a>HTTP durum kodu 403 ve 429 Düzelt

Saniyede veya işlem / ay için fiyatlandırma katmanınızı aşması durumunda, durum kodları 403 ve 429 hatasını alırsınız.

### <a name="when-you-receive-an-http-403-error-status-code"></a>Bir HTTP 403 hatası durum kodu aldığınızda

Tüm bu ücretsiz 1000 uç nokta sorgular kullandığınızda veya fiyatlandırma katmanın aylık işlem kotayı aştığınız bir HTTP 403 hatası durum kodu alırsınız. 

Bu hatayı düzeltmek için aşağıdakilerden birini yapmalısınız [fiyatlandırma katmanınızı değiştirerek](luis-how-to-azure-subscription.md#change-pricing-tier) daha yüksek bir katmana veya [yeni kaynak Oluştur](get-started-portal-deploy-app.md#create-the-endpoint-resource) ve [uygulamanıza atama](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal).

Bu hata için çözümleri şunlardır:

* İçinde [Azure portalında](https://portal.azure.com), kaynak, üzerinde anlama, dil **kaynak yönetimi fiyatlandırma Katmanı ->** , daha yüksek bir TPS katman için fiyatlandırma katmanınızı değiştirin. Language Understanding uygulamanıza kaynağınızın zaten atanmışsa, Language Understanding Portalı'nda herhangi bir şey yapmanız gerekmez.
*  Kullanım en yüksek fiyatlandırma katmanı aşarsa, bir yük dengeleyici bulundurmanıza bunları Language Understanding kaynak daha ekleyin. [Language Understanding kapsayıcı](luis-container-howto.md) Kubernetes veya Docker Compose ile bu konuda yardımcı olabilir.

### <a name="when-you-receive-an-http-429-error-status-code"></a>Bir HTTP 429 hatası durum kodu aldığınızda

Ne zaman bu durum kodu döndürülmesine, saniyede fiyatlandırma katmanınızı aşıyor.  

Çözümleri şunlardır:

* Yapabilecekleriniz [fiyatlandırma katmanınızı artırmak](#change-pricing-tier), en yüksek katman üzerinden değilse.
* Kullanım en yüksek fiyatlandırma katmanı aşarsa, bir yük dengeleyici bulundurmanıza bunları Language Understanding kaynak daha ekleyin. [Language Understanding kapsayıcı](luis-container-howto.md) Kubernetes veya Docker Compose ile bu konuda yardımcı olabilir.
* İstemci uygulama isteklerinizi geçit oluşturan bir [yeniden deneme ilkesi](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults#general-guidelines) bu durum kodu aldığınızda kendiniz uygulayacaksınız. 

## <a name="viewing-summary-usage"></a>Özet kullanımı görüntüleme
Azure'da LUIS kullanım bilgilerini görüntüleyebilirsiniz. **Genel bakış** sayfası çağrı ve hata dahil olmak üzere son Özet bilgilerini gösterir. Bir LUIS uç nokta isteği yapıyorsa, hemen izleyin **genel bakış sayfasında**, gösterilecek kullanım beş dakika bekleyin.

![Özet kullanımı görüntüleme](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Kullanım grafikleri özelleştirme
Ölçümleri verilerine daha ayrıntılı bir görünüm sağlar.

![Varsayılan ölçümleri](./media/luis-usage-tiers/metrics-default.png)

Zaman dilimi ve ölçü türü için ölçüm grafikleri yapılandırabilirsiniz. 

![Özel ölçümler](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Toplam işlem eşiği Uyarısı
Belirli bir işlem eşiği, örneğin 10.000 işlem ulaştığınız zaman bilmek istiyorsanız, bir uyarı oluşturabilirsiniz. 

![Varsayılan Uyarıları](./media/luis-usage-tiers/alert-default.png)

İçin ölçüm uyarısı Ekle **toplam çağrıları** belirli bir süre ölçümü. Uyarı alması gereken tüm kişilerin e-posta adreslerini ekleyin. Uyarı alması gereken tüm sistemleri için Web kancaları ekleyin. Uyarı tetiklendiğinde bir mantıksal uygulama da çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Nasıl kullanacağınızı öğrenin [sürümleri](luis-how-to-manage-versions.md) değişiklikleri LUIS uygulamanızı yönetme.
