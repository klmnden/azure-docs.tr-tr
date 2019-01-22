---
title: Abonelik anahtarları
titleSuffix: Language Understanding - Azure Cognitive Services
description: Ücretsiz ilk 1000 uç nokta sorgularınızı kullanmak için Abonelik anahtarları oluşturma gerekmez. Alırsanız bir _kotasından_ hata formunda bir HTTP 403 ve 429 gereken bir anahtar oluşturun ve uygulamanıza atayın.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/18/2019
ms.author: diberry
ms.openlocfilehash: 7127dad813dc4c026f8feb2840f3a3903e7a3e2c
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423095"
---
# <a name="using-subscription-keys-with-your-luis-app"></a>Abonelik anahtarları LUIS uygulamanızı ile kullanma

Ücretsiz ilk 1000 uç nokta sorgularınızı kullanmak için Abonelik anahtarları oluşturma gerekmez. Bu uç nokta sorgular kullanılan sonra bir Azure kaynağı oluşturma [Azure portalında](http://portal.azure.com), ardından bu kaynak bir LUIS uygulaması atayın [LUIS portalı](https://www.luis.ai).

Alırsanız bir _kotasından_ hata formunda bir HTTP 403 ve 429 gereken bir anahtar oluşturun ve uygulamanıza atayın. 

Test ve yalnızca prototip için ücretsiz katman (F0) kullanın. Üretim sistemleri için bir [Ücretli](https://aka.ms/luis-price-tier) katmanı. Kullanmayın [anahtar yazma](luis-concept-keys.md#authoring-key) için üretim uç noktası sorgular.

<a name="create-luis-service"></a>

## <a name="create-language-understanding-endpoint-key-in-the-azure-portal"></a>Azure portalını kullanarak konuşma tanıma uç noktası anahtarı oluşturma

Bu yordam oluşturur bir **Language Understanding** kaynak. Bilişsel hizmetler kullanılabilir bir kaynak istiyorsanız, hepsi bir arada anahtar oluşturma **[Bilişsel hizmet](../cognitive-services-apis-create-account.md)** Language Understanding kaynak yerine. 

Bu anahtar yalnızca, uç nokta tahmin sorguları için kullanılmalıdır. Emin değil, bu anahtar için model veya uygulama değişiklikleri yapın. 

1. Oturum  **[Azure portalında](https://ms.portal.azure.com/)**. 
1. Yeşili **+** üst sol panelde oturum ve arama `Language Understanding` Market'te seçin **Language Understanding** izleyin  **deneyimi oluşturmak** LUIS abonelik hesabı oluşturmak için. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

1. Abonelik ayarları fiyatlandırma katmanları, vb. hesap adı dahil olmak üzere yapılandırın. 

    ![Azure API seçim](./media/luis-azure-subscription/azure-api-choice.png) 

1. Language Understanding kaynak oluşturduktan sonra oluşturulan erişim anahtarlarını görüntüleyebilir **kaynak yönetimi -> anahtarlar**. Anahtarları yapın. Sonraki bölümde, bu yeni kaynak LUIS portalında LUIS uygulaması bağlanma gösterilmektedir. 3. adımdaki LUIS kaynağın adına ihtiyacınız vardır.

    ![Azure anahtarları](./media/luis-azure-subscription/azure-keys.png)

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


## <a name="assign-resource-key-to-luis-app-in-luis-portal"></a>Kaynak anahtarı LUIS Portalı'nda LUIS uygulama atama

1. LUIS portalında oturum açın, ardından seçmek için yeni bir anahtar eklemek için bir uygulama seçin **Yönet** seçip sağ üst menüsünde **anahtarları ve uç noktaları**.

    [ ![Anahtarlar ve uç noktaları sayfası](./media/luis-manage-keys/keys-and-endpoints.png) ](./media/luis-manage-keys/keys-and-endpoints.png#lightbox)

1. LUIS eklemek için seçin **Kaynak Ata +**.

    ![Uygulamanıza bir kaynak atayın](./media/luis-manage-keys/assign-key.png)

1. İletişim kutusunda bir kiracı seçin, oturum açma ile LUIS Web sitesi e-posta adresiyle ilişkili.  

1. Seçin **abonelik adı** eklemek istediğiniz Azure kaynakla ilişkilendirilmiş.

1. Seçin **LUIS kaynak adı**. 

1. Seçin **atama kaynak**. 

1. Tabloya yeni satır bulun ve uç nokta URL'sini kopyalayın. Doğru bir tahmin için LUIS uç noktasına bir HTTP GET isteği yapmak için oluşturulur. 

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

## <a name="change-pricing-tier"></a>Fiyatlandırma katmanını değiştir

1.  İçinde [Azure](https://portal.azure.com), LUIS aboneliğinizi bulun. LUIS aboneliği seçin.
    ![LUIS aboneliğinizi bulun](./media/luis-usage-tiers/find.png)
1.  Seçin **fiyatlandırma katmanı** kullanılabilir fiyatlandırma katmanlarına görmek için. 
    ![Fiyatlandırma katmanları görüntüleyin](./media/luis-usage-tiers/subscription.png)
1.  Fiyatlandırma katmanı seçin seçip **seçin** değişikliği kaydetmek için. 
    ![LUIS ödeme katmanınızı değiştirin](./media/luis-usage-tiers/plans.png)
1.  Fiyat değişikliği tamamlandıktan sonra bir açılır pencere yeni fiyatlandırma katmanına doğrular. 
    ![LUIS ödeme katmanınızı doğrulayın](./media/luis-usage-tiers/updated.png)
1. Unutmayın [Bu uç noktası anahtarı atama](#assign-endpoint-key) üzerinde **Yayımla** sayfasında ve tüm uç nokta sorguları kullanın. 

## <a name="how-to-fix-out-of-quota-errors-when-the-key-exceeds-pricing-tier-usage"></a>Fiyatlandırma katmanı kullanımı anahtar aştığında, Kota aşımı hataları düzeltmek nasıl
Her katman, uç nokta istekleri LUIS hesabınıza belli bir oranda sağlar. İstekleri oranını tarifeli hesabınızın dakika başına veya aylık izin verilen oranı yüksekse isteklerini HTTP hatası almak "429: Çok fazla istek."

Her katman, aylık biriktirici istek sağlar. Toplam istek sayısı izin verilen hızından daha yüksek olan, istekler HTTP hatası alırsınız "403: Yasak".  

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