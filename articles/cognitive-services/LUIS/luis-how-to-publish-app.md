---
title: LUIS uygulamanızı yayımlayın | Microsoft Docs
description: Derleme ve Language Understanding (LUIS) kullanarak uygulamanızı test edin sonra azure'da bir web hizmeti olarak yayımlayın.
services: cognitive-services
titleSuffix: Azure
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 43a26f9e81b788c2a110c24bf2e02c56c0714f1e
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988845"
---
# <a name="publish-your-trained-app"></a>Eğitilen uygulamanızı yayımlayın
Oluşturma ve LUIS uygulamanızı test etme bitirdikten sonra yayımlayın. Uygulamayı yayımladıktan sonra yayımlama sayfası, ilişkili tüm HTTP gösterir [uç noktaları](luis-glossary.md#endpoint). Bu uç noktaları başına [bölge](luis-reference-regions.md) ve başına [anahtarı](luis-how-to-manage-keys.md), ardından istemci, sohbet botu veya arka uç uygulamasına tümleştirilmiştir. 

Her zaman [test](interactive-test.md) uygulamanızı yayımlamadan önce. 

## <a name="production-and-staging-slots"></a>Üretim ve hazırlama yuvası
Uygulamanıza yayımlayabilirsiniz **hazırlama yuvası** veya **üretim yuvasına**. İki yayımlama yuvaları kullanarak, bu iki farklı Uç noktalara yayımlanan uç noktaları ile iki farklı sürümlerini veya aynı sürüme sahip olmanızı sağlar. 

<!-- TBD: what is the technical difference? log files, endpoint quota? -->

## <a name="settings-configuration-requires-publishing-model"></a>Yayımlama Modeli ayarları yapılandırması gerektirir
Uç noktaya sonra aşağıdaki ayarlarda yapılan değişiklikleri yayımlayın. 

## <a name="external-services-settings"></a>Dış hizmetler ayarları
Dış hizmet ayarları dahil **[yaklaşım analizi](#enable-sentiment-analysis)** ve  **[konuşma hazırlama](#enable-speech-priming)**.

### <a name="enable-sentiment-analysis"></a>Yaklaşım analizi etkinleştir
İçinde **dış Hizmetleri ayarları**, **yaklaşım analizi etkinleştirme** LUIS ile tümleştirmek onay kutusunu sağlayan [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) yaklaşımını ve anahtar tümcecik sağlamak için analizi. Metin analizi anahtarı belirtmeniz gerekmez ve Azure hesabınızda bu hizmet için fatura ücret alınmaz. Bu ayarı işaretleyin, sonra kalıcıdır. 

Yaklaşım verilerdir pozitif gösteren 0 ile 1 arasındaki bir puan (1 yakın) veya (0 yakın) negatif yaklaşım veri.

Yaklaşım Analizi ile JSON uç yanıtı hakkında daha fazla bilgi için bkz. [yaklaşım analizi](luis-concept-data-extraction.md#sentiment-analysis)

### <a name="enable-speech-priming"></a>Konuşma Hazırlama işlemi etkinleştir 
İçinde **dış Hizmetleri ayarları**, **etkinleştirme konuşma hazırlama** onay kutusu sizin söylenen utterance bir sohbet Robotu veya arama LUIS uygulaması alın ve bir LUIS almak için tek bir uç nokta içerecek şekilde izin verir tahmini yanıt. Bilişsel hizmet konuşma Hazırlama işlemi kullanır [konuşma tanıma API'si](../Speech-Service/rest-apis.md). 

![Konuşma Hazırlama işlemi onay iletişim kutusunun görüntüsü](./media/luis-how-to-publish-app/speech-prime-modal.png)

Bu özellik etkinleştirildikten sonra uygulamanızı yayımlayın. LUIS uygulamanızı yayımladığınızda, uygulama modelinizi konuşma hizmeti hazırlamak için kendi konuşma hizmeti gönderilir. Model bilgilerdir **değil** dışında kendi hizmeti kullanılır. 

Konuşma Hazırlama işlemi kullanımını tamamlamak için kullanmak için aşağıdaki bilgilere ihtiyacınız vardır. [Speech SDK'sı](../speech-service/speech-sdk-reference.md):
* LUIS uç noktası anahtarı.
* LUIS uygulama kimliği.
* Bir uç nokta etki alanı olarak adlandırılan Speech SDK'sı içinde "ana bilgisayar adı" "westus.api.cognitive.microsoft.com gibi" Burada ilk alt etki alanı burada uygulama yayımlanır bölgedir.

Daha fazla bilgi için [Konuşmayı amaca dönüştürme](http://aka.ms/speechsdk) örnek.

LUIS uygulamanızı silinir veya konuşma hizmeti silindi, model verileri kaldırılır. 

## <a name="endpoint-url-settings"></a>Uç nokta URL'si ayarları
Uç nokta URL'si Hizmetleri ayarları dahil **[saat dilimi](#set-timezone-offset)** uzaklığı,  **[tüm hedefi puanları tahmin](#include-all-predicted-intent-scores)**, ve  **[ Bing yazım denetleyicisi](#enable-bing-spell-checker)**.

### <a name="set-timezone-offset"></a>Saat dilimi uzaklığı ayarlayın 
Zaman dilimi seçiminden yuvası seçimi parçasıdır. LUIS için bu saat dilimi ayarını verir [alter](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) istediğiniz önceden oluşturulmuş datetimeV2 zaman değerleri sırasında tahmin döndürülen varlık verilerini seçili saat dilimine göre doğru olmasını sağlayın. 

### <a name="include-all-predicted-intent-scores"></a>Amaç tüm tahmin edilen puanları içerir
**INCLUDE tüm hedefi puanları tahmin** onay kutusu her amacını tahmin puanını dahil etmek uç nokta sorgu yanıt verir. 

Bu ayar, sohbet botu veya döndürülen ıntents puanları temel alarak programlı karar vermek için arama LUIS uygulamanızı sağlar. Genellikle üst iki amacı en ilginç var. En çok puan hiçbiri hedefi yok hedefi ve diğer yüksek puanlı amacını arasında kesin bir seçim yapar izleme bir soru sorun, sohbet botu seçebilirsiniz ise. 

Hedefleri ve puanlarını Ayrıca, uç nokta günlükleri dahil. Yapabilecekleriniz [dışarı](luis-how-to-start-new-app.md#export-app) Bu günlükleri ve puanları analiz edin. 

```
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
İçinde **uç nokta URL'si ayarları**, **etkinleştirme Bing yazım denetleyicisi** LUIS sözcüklerin tahmin önce düzeltmek onay kutusu sağlar. Oluşturma bir  **[Bing yazım denetimi anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. Anahtar oluşturulduktan sonra iki querystring parametreleri Yayımla sayfasında uç noktası URL'sine eklenir. 

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

## <a name="publish-your-trained-app-to-an-http-endpoint"></a>Bir HTTP uç noktasına eğitilen uygulamanızı yayımlayın
Adını tıklayarak uygulamanızı açın **uygulamalarım** sayfasında ve ardından **Yayımla** üst panelinde. 

![Yayımlama Sayfası-](./media/luis-how-to-publish-app/publish-to-production.png)
 
Uygulamanız başarıyla yayımlandığında bir yeşil bir başarı bildirim tarayıcı üst kısmında görüntülenir. 

* Yayımlamak isteyip istemediğinizi seçin **üretim** veya **hazırlama** aşağı açılan menüden seçerek **Select yuvası**. 

## <a name="assign-key"></a>Tuşu atama

Bir anahtar gösterilen ücretsiz Starter_Key dışında kullanmak isterseniz tıklayın **anahtarı Ekle** düğmesi. Bu eylem, uygulamaya atamak için mevcut bir uç noktası anahtarı seçmenizi sağlayan bir iletişim kutusu açılır. Oluşturma ve uç nokta anahtarları LUIS uygulamanıza ekleme hakkında daha fazla bilgi için bkz. [anahtarlarınızı yönetme](luis-how-to-manage-keys.md).

Uç noktaları ve diğer bölgeler ile ilişkili anahtarlarını görmek için bölgeler geçiş yapmak için radyo düğmelerini kullanın. Her satırda **kaynakları ve anahtarları** tablo hesabınızla ilişkili Azure kaynaklarını ve bu kaynakla ilişkilendirilmiş uç nokta anahtarlarını listeler.

## <a name="endpoint-url-construction"></a>Uç nokta URL'si oluşturma
Uç nokta URL'si, uç nokta anahtar ile ilişkili bir Azure bölgesine karşılık gelir.

Bu tabloda rahatça yayımlama yapılandırmanızı URL uç noktasını rota seçenekleri ve sorgu dize değerleri yansıtır. LUIS arama uygulamanız için uç nokta URL'lerini oluşturmak istiyorsanız bu aynı yollar ve sorgu dizesi değerleri için uç nokta kümesi kullanılan--emin olun ayarlayın.

URL rota, bölge ve uygulama kimliği ile oluşturulur. De başka bölgelerde veya diğer uygulamalarla yayımlıyorsanız, uç nokta URL'si, bölge ve uygulama kimliği değerlerini değiştirerek oluşturulabilir. 

* Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin. Yayımlama başarılı olduğunda LUIS uygulamanıza erişmek için görüntülenen uç nokta URL'sini kullanın. 

### <a name="optional-query-string-parameters"></a>İsteğe bağlı bir sorgu dizesi parametreleri
Aşağıdaki sorgu dizesi parametreleri ile uç nokta URL'sini kullanılabilir:

<!-- TBD: what about speech priming? -->

|Sorgu dizesi|Tür|Örnek değer|Amaç|
|--|--|--|--|
|ayrıntılı|boole|true|Dahil [tüm hedefi puanları](#include-all-predicted-intent-scores) utterance için|
|timezoneOffset|sayı (birimi, dakika)|60|Ayarlama [saat dilimi uzaklığı](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) için [datetimeV2 önceden oluşturulmuş varlıklar](luis-reference-prebuilt-datetimev2.md)|
|Yazım denetimi|boole|true|[Yazım denetimi düzeltme](#enable-bing-spell-checker) utterance--bing-yazım-onay-subscription-key sorgu dizesi parametresi ile birlikte kullanıldığında,|
|Bing yazım-onay-abonelik-anahtar|Abonelik kimliği||Yazım denetimi sorgu dizesi parametresi ile birlikte kullanılan|
|Hazırlama|boole|false|Hazırlık veya üretim uç noktası seçin|
|Günlük|boole|true|Sorgu ve oturum sonuçları Ekle|


## <a name="test-your-published-endpoint-in-a-browser"></a>Yayımlanan uç noktanız bir tarayıcıda test
Yayımlanan uç noktanızı URL'de seçerek test **uç nokta** sütun. Varsayılan tarayıcı ile oluşturulan URL açılır. URL parametresi olarak "& q" test sorgunuzu için. Örneğin, ekleme `&q=Book me a flight to Boston on May 4` URL'si ve Enter tuşuna basın. Tarayıcı JSON yanıtı HTTP uç noktanızın görüntüler. 

![Yayımlanan bir HTTP uç noktasından JSON yanıtı](./media/luis-how-to-publish-app/luis-publish-app-json-response.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [anahtarları Yönet](./luis-how-to-manage-keys.md) anahtarları LUIS uygulamanıza ekleyin ve anahtarları bölgeler ile nasıl eşleştiği hakkında bilgi edinin.
* Bkz: [eğitme ve uygulamanızı test](interactive-test.md) yayımlanan uygulamanızı test konsolunda test etmek yönergeler.
