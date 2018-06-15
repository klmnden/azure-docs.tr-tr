---
title: HALUK uygulamanızı yayınlama | Microsoft Docs
description: Derleme ve dil anlama (HALUK) kullanarak uygulamanızı test sonra Azure üzerinde bir web hizmeti olarak yayımlayın.
services: cognitive-services
titleSuffix: Azure
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 056608e7843c8feab28359de5f2762164287f09d
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356141"
---
# <a name="publish-your-trained-app"></a>Eğitilmiş uygulamanızı yayınlama
Derleme ve HALUK uygulamanızı test etme tamamladığınızda, yayımlayın. Uygulama yayımlandıktan sonra ilişkili tüm HTTP Yayımla Sayfası gösterilir [uç noktaları](luis-glossary.md#endpoint). Bu uç noktalar başına [bölge](luis-reference-regions.md) ve başına [anahtar](Manage-Keys.md), ardından herhangi bir istemci, chatbot veya arka uç uygulama tümleştirilmiştir. 

Her zaman [test](train-test.md) yayımlamadan önce uygulamanızın. 

## <a name="production-and-staging-slots"></a>Üretim ve hazırlama yuvası
Uygulamanıza yayımlayabilirsiniz **hazırlama yuvası** veya **üretim yuvasına**. İki yayımlama yuvaları kullanarak, bu iki farklı bitiş noktasında yayımlanan uç noktaları ile iki farklı sürümü veya aynı sürüme sahip olmanızı sağlar. 

<!-- TBD: what is the technical difference? log files, endpoint quota? -->

## <a name="settings-configuration-requires-publishing-model"></a>Yayımlama modeli ayarlarını yapılandırma gerektirir
Aşağıdaki ayarlarda yapılan değişiklikler sonra uç noktasına yayımlayın. 

## <a name="external-services-settings"></a>Dış Hizmetleri ayarları
Dış hizmet ayarları dahil **[düşünceleri analiz](#enable-sentiment-analysis)** ve  **[konuşma hazırlama](#enable-speech-priming)**.

### <a name="enable-sentiment-analysis"></a>Düşünceleri çözümlemesini etkinleştirin
İçinde **dış Hizmetleri ayarları**, **etkinleştirmek düşünceleri analiz** onay kutusu ile tümleştirmek HALUK verir [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) düşünceleri ve anahtar deyimi sağlamak için Çözümleme. Metin analizi anahtar sağlamak sahip ve Azure hesabınıza bu hizmet için fatura olan herhangi bir ücret alınmaz. Bu ayar işaretlediğinizde kalıcıdır. 

Düşünceleri verilerdir pozitif gösteren 0 ile 1 arasındaki bir puan (1 yakın) veya (0 yakın) negatif verilerin düşünceleri.

Düşünceleri analiz JSON bitiş noktası Yanıtla hakkında daha fazla bilgi için bkz: [düşünceleri çözümleme](luis-concept-data-extraction.md#sentiment-analysis)

### <a name="enable-speech-priming"></a>Konuşma Hazırlama işlemi etkinleştir 
İçinde **dış Hizmetleri ayarları**, **etkinleştirmek konuşma hazırlama** onay kutusunu chatbot ya da HALUK arama uygulama konuşulan utterance almak ve bir HALUK almak için tek bir uç nokta içerecek şekilde sağlar Tahmin yanıtı. Konuşma Hazırlama işlemi Bilişsel hizmetinin kullandığı [konuşma API](../Speech-Service/rest-apis.md). 

![Konuşma Hazırlama işlemi onay iletişim kutusunun görüntüsü](./media/luis-how-to-publish-app/speech-prime-modal.png)

Bu özellik etkinleştirildikten sonra uygulamanızı yayımlayın. HALUK uygulamanızı yayımladığınızda, uygulama modeli konuşma hizmet hazırlamak için kendi konuşma hizmetine gönderilir. Model bilgilerinizin olduğundan **değil** dışında kendi hizmet kullanılır. 

Konuşma Hazırlama işlemi kullanımını tamamlamak için kullanmak için aşağıdaki bilgiler gereklidir. [konuşma SDK](../speech-service/speech-sdk-reference.md):
* HALUK abonelik anahtarı.
* HALUK uygulama kimliği.
* Bir uç nokta etki alanı olarak bilinir konuşma SDK'sındaki "ana bilgisayar adı" "westus.api.cognitive.microsoft.com gibi" ilk alt etki alanı burada uygulama yayımlanan bölge olduğu.

Daha fazla bilgi için bkz: [hedefi konuşma](http://aka.ms/speechsdk) örnek.

HALUK uygulamanızı silinmiş veya konuşma hizmeti silinmiş model verileri kaldırılır. 

## <a name="endpoint-url-settings"></a>Uç nokta URL'si ayarları
Uç nokta URL'si Hizmetleri ayarları dahil **[saat dilimi](#set-timezone-offset)** uzaklığı,  **[tüm hedefi puanları tahmin](#include-all-predicted-intent-scores)**, ve  **[ Bing yazım denetleyicisi](#enable-bing-spell-checker)**.

### <a name="set-timezone-offset"></a>Saat dilimi uzaklığı ayarlayın 
Saat dilimi seçimi yuvası seçimi parçasıdır. Bu saat dilimi ayarı için HALUK sağlar [alter](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) istediğiniz önceden oluşturulmuş datetimeV2 zaman değerleri tahmin sırasında döndürülen varlık verilerini seçili saat dilimine göre doğru olmasını sağlayın. 

### <a name="include-all-predicted-intent-scores"></a>Tüm tahmin edilen hedefi puanlar içerir
**Dahil tüm tahmin hedefi puanları** onay kutusu her hedefi tahmin puanını dahil etmek uç nokta sorgu yanıt verir. 

Bu ayar, chatbot veya döndürülen hedefleri puanları temel alan bir programlama kararı vermeniz HALUK arama uygulamanızı sağlar. Genellikle en ilginç üst iki amacı ' dir. Üst puan None ise hedefi, izleme soru sormak, chatbot seçebilirsiniz, diğer yüksek Puanlama amacıyla hiçbiri hedefi arasındaki kesin bir seçenek yapar. 

Hedefleri ve puanlarını de altındadır uç nokta günlükleri dahil. Yapabilecekleriniz [dışarı](create-new-app.md#export-app) Bu günlükleri ve notların çözümleyebilirsiniz. 

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

### <a name="enable-bing-spell-checker"></a>Bing yazım denetleyicisi etkinleştir 
İçinde **uç nokta URL'si ayarları**, **etkinleştirmek Bing yazım denetleyicisi** onay kutusunu sözcüklerin tahmin önce düzeltmek HALUK sağlar. Bu oluşturmanızı gerektirir bir  **[Bing yazım denetimi anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. Anahtar oluşturulduktan sonra iki sorgu dizesi parametreleri Yayımla sayfasında uç nokta URL'si eklenir. 

HALUK arama uygulamanız için kendi URL'ler oluşturulurken, emin olun **yazım denetimi = true** sorgu dizesi parametresi ve **bing-yazım-onay-subscription-key = {YOUR_BING_KEY_HERE}**. Değiştir `{YOUR_BING_KEY_HERE}` Bing yazım denetleyicisi anahtarınız ile.

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

## <a name="publish-your-trained-app-to-an-http-endpoint"></a>Bir HTTP uç noktasına eğitilen uygulamanızı yayınlama
Uygulamanızı adını tıklayarak açın **My uygulamaları** sayfasında ve ardından **Yayımla** üst panelinde. 

![Yayımla Sayfası-](./media/luis-how-to-publish-app/publish-to-production.png)
 
Uygulamanızı başarıyla yayımlandığında, yeşil başarı bildirimi tarayıcısının üstünde görünür. 

* Yayımla isteyip istemediğinizi seçin **üretim** veya **hazırlama** altında açılan menüden seçerek **Select yuvası**. 

## <a name="assign-key"></a>Anahtar atama

Bir anahtar dışında gösterilen ücretsiz Starter_Key kullanmak istiyorsanız tıklatın **anahtar Ekle** düğmesi. Bu eylem, uygulamaya atamak için mevcut bir uç nokta anahtarı seçmenizi sağlayan bir iletişim kutusunu açar. Oluşturma ve uç nokta anahtarları HALUK uygulamanıza ekleme hakkında daha fazla bilgi için bkz: [tuşlarınızı yönetme](Manage-Keys.md).

Uç noktaları ve diğer bölgeler ile ilişkili tuşlarını görmek için bölgeler geçiş yapmak için radyo düğmelerini kullanın. Her satırda **kaynakları ve anahtarları** tabloda hesabınızla ilişkili Azure kaynaklarını ve bu kaynakla ilişkili uç nokta anahtarları.

## <a name="endpoint-url-construction"></a>Uç nokta URL'si yapımı
Uç nokta URL'sini uç nokta anahtar ile ilişkili Azure bölgesine karşılık gelir.

Bu tabloyu kolaylıkla yayımlama rota seçenekleri ve sorgu dize değerleri ile URL uç yapılandırmanızda yansıtır. HALUK arama uygulamanız için uç nokta URL'leri oluşturmak istiyorsanız bu aynı yolları ve sorgu dizesi değerleri için uç noktaya ayarlama kullanılan--emin olun ayarlayın.

URL rota bölge ve bir uygulama kimliği ile oluşturulur Başka bölgelerde veya diğer uygulamalarla yayımlıyorsa, uç nokta URL'si bölge ve uygulama kimliği değerleri değiştirerek oluşturulabilir. 

* Üretim yuvasına seçin ve **Yayımla** düğmesi. Yayımlama başarılı olduğunda HALUK uygulamanıza erişmek için görüntülenen uç nokta URL'sini kullanın. 

### <a name="optional-query-string-parameters"></a>İsteğe bağlı sorgu dizesi parametreleri
Aşağıdaki sorgu dizesi parametreleri uç nokta URL'si ile kullanılabilir:

<!-- TBD: what about speech priming? -->

|Sorgu dizesi|Tür|Örnek değer|Amaç|
|--|--|--|--|
|Ayrıntılı|boole|true|Dahil [tüm hedefi puanları](#include-all-predicted-intent-scores) utterance için|
|timezoneOffset|sayı (birimi, dakika)|60|Ayarlama [saat dilimi uzaklığı](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) için [datetimeV2 önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md#builtindatetimev2)|
|Yazım denetimi|boole|true|[Yazımı düzeltin](#enable-bing-spell-checker) utterance--bing yazım-onay-abonelik-anahtar sorgu dizesi parametresi ile birlikte kullanıldığında,|
|Bing yazım-onay-abonelik-anahtar|Abonelik kimliği||Yazım denetimi sorgu dizesi parametresi ile birlikte kullanılan|
|Hazırlama|boole|false|Hazırlık veya üretim uç noktası seçin|
|Günlük|boole|true|Sorgu ve sonuçları oturum Ekle|


## <a name="test-your-published-endpoint-in-a-browser"></a>Bir tarayıcıda yayımlanan uç noktanızı test
URL'de seçerek yayımlanan uç noktanızı test **Endpoint** sütun. Varsayılan tarayıcı oluşturulan URL ile açılır. URL parametre kümesini "& q" test sorgunuz için. Örneğin, append `&q=Book me a flight to Boston on May 4` URL ve ENTER tuşuna basın. Tarayıcı, HTTP uç noktası JSON yanıtını görüntüler. 

![Yayımlanan bir HTTP uç noktası yanıtından JSON](./media/luis-how-to-publish-app/luis-publish-app-json-response.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [anahtarları Yönet](./Manage-Keys.md) anahtarları HALUK uygulamanıza ekleyin ve anahtarları bölgelerine nasıl eşleme hakkında bilgi edinin.
* Bkz: [eğitimi ve uygulamanızı test](Train-Test.md) test konsolunda yayımlanan uygulamanızı test etme hakkında yönergeler için.
