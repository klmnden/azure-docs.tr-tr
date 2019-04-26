---
title: Sözlük
titleSuffix: Language Understanding - Azure Cognitive Services
description: Terimler sözlüğü açıklanmaktadır LUIS API'si hizmeti ile çalışırken karşılaşabileceğiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: ced83b83c2d64ba5c4816f378c66dae9f4210391
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60193637"
---
# <a name="language-understanding-glossary-of-common-vocabulary-and-concepts"></a>Dil anlama sözlüğü yaygın kelime ve kavramları
LUIS API'si hizmeti ile çalışırken, Language Understanding (LUIS), koşulları sözlüğü açıklar karşılaşabilirsiniz.

## <a name="active-version"></a>Etkin sürümü

Etkin LUIS sürüm model değişiklikleri alan sürümdür. İçinde [LUIS](luis-reference-regions.md) Web sitesi, etkin bir sürüm değil bir sürüm olarak yapmanız gerekiyorsa, önce o sürümü etkin olarak ayarlanacak.

## <a name="authoring"></a>Yazma

Yazımı yolunda oluşturma, yönetme ve dağıtma olanağı bir [LUIS uygulaması](#luis-app), ya da [LUIS](luis-reference-regions.md) Web sitesi veya [yazma API'leri](https://aka.ms/luis-authoring-api).

## <a name="authoring-key"></a>Anahtar yazma

Daha önce "Programlama" anahtar adı. Uygulama yazmak için kullanılır. Üretim düzeyinde uç nokta sorgular için kullanılmaz. Daha fazla bilgi için [anahtar sınırları](luis-boundaries.md#key-limits).   

## <a name="batch-test-json-file"></a>Toplu metin JSON dosyası

Toplu iş dosyası, bir JSON dizisidir. Dizideki her öğe üç özelliğe sahiptir: `text`, `intent`, ve `entities`. `entities` Bir dizi bir özelliktir. Dizi, boş olabilir. Varsa `entities` varlıkları doğru şekilde belirlemek gereken, dizi, boş değil.

```JSON
[
    {
        "text": "drive me home",
        "intent": "None",
        "entities": []
    },
    {
        "text": "book a flight to orlando on the 25th",
        "intent": "BookFlight",
        "entities": [
            {
                "entity": "orlando",
                "type": "Location",
                "startIndex": 18,
                "endIndex": 25
            }
        ]
    }
]

```


## <a name="collaborator"></a>Ortak çalışanı

Bir ortak çalışanı değil [sahibi](#owner) uygulamanın, ancak ekleme, düzenleme ve silme amacı, varlıkları, konuşma için aynı izinlere sahip.

## <a name="currently-editing"></a>Şu anda düzenleme

Aynı [etkin sürümü](#active-version)

## <a name="domain"></a>Etki alanı

LUIS bağlamında bir **etki alanı** bilgi alanıdır. Etki alanınız, Bilgi Bankası uygulama bölgesine özeldir. Bu, seyahat aracı uygulaması gibi genel bir alan olabilir. Bir seyahat aracı uygulaması belirli alanlara yalnızca belirli coğrafi konumlar, diller ve Hizmetleri gibi şirketinizin bilgi olabilir.

## <a name="endpoint"></a>Uç noktası

[LUIS uç nokta](https://aka.ms/luis-endpoint-apis) URL'dir sonra LUIS sorguları gönderdiğinizde burada [LUIS uygulaması](#luis-app) yazılan ve yayımlandı. Yayımlanmış uygulama ve bunun yanı sıra uygulama kimliği bölge uç nokta URL'sini içerir Uç nokta bulabilirsiniz **[anahtarları ve uç noktaları](luis-how-to-azure-subscription.md)** sayfasında, uygulamanızı veya uç nokta URL'sini alabilirsiniz [uygulama bilgi al](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37) API.

Örnek uç nokta şu şekilde görünür:

`https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscriptionID>&verbose=true&timezoneOffset=0&q=<utterance>`

|QueryString parametresi|açıklama|
|--|--|
|bölge| [yayımlanan bölge](luis-reference-regions.md#publishing-regions) |
|Uygulama Kimliği | LUIS uygulama kimliği |
|Subscriptionıd | Azure portalında oluşturulan LUIS (abonelik) uç noktası anahtarı |
|q | Utterance |
|timezoneOffset| minutes|

## <a name="entity"></a>Varlık

[Varlıkları](luis-concept-entity-types.md) önemli sözcüklerdir [konuşma](luis-concept-utterance.md) ilgili bilgileri açıklayan [hedefi](luis-concept-intent.md), ve bazen için gerekli olan. Veri türü LUIS, aslında bir varlıktır.

## <a name="f-measure"></a>F ölçümü

İçinde [toplu test](luis-interactive-test.md#batch-testing), testin doğruluğu ölçüsü.

## <a name="false-negative"></a>Hatalı negatif (TN)

İçinde [toplu test](luis-interactive-test.md#batch-testing), konuşma, uygulamanızı yanlış tahmin edilen hedef hedefi/varlık olmaması veri noktalarını temsil eder.

## <a name="false-positive"></a>Hatalı pozitif sonuç (TP)

İçinde [toplu test](luis-interactive-test.md#batch-testing), veri noktalarının konuşma, uygulamanızı yanlış tahmin edilen hedef hedefi/varlık varlığını temsil eder.

## <a name="features"></a>Özellikleri

Machine learning'de bir [özellik](luis-concept-feature.md) bir ayırt edici nitelik veya sisteminizin gözlemler veri özniteliği.

## <a name="intent"></a>Hedefi

Bir [hedefi](luis-concept-intent.md) bir görev veya eylem gerçekleştirmek isteyen kullanıcı temsil eder. Bir amaç veya hedefin bir uçuştaki kayıt, fatura ödeme veya haber makalesinin bulma gibi bir kullanıcının giriş, ifade olduğundan. LUIS hedefi tahmin tüm utterance üzerinde temel alır. Varlıklar, buna karşılık olarak bir utterance parçalarıdır.

## <a name="labeling"></a>Etiketleme

Bir sözcük veya tümcecik bir amaç'ın içinde ilişkilendirme işlemi olduğundan etiketleme [utterance](#utterance) ile bir [varlık](#entity) (veri türü).

## <a name="luis-app"></a>LUIS uygulaması

Bir LUIS uygulaması için doğal dil işleme dahil olmak üzere bir eğitilen veri modeli, [hedefleri](#intent), [varlıkları](#entity)ve etiketli [konuşma](#utterance).

## <a name="owner"></a>Sahibi

Her uygulamanın, uygulamayı oluşturan kişi olan bir sahip vardır. Sahibi ekleyebilir [ortak çalışanlar](#collaborator).

## <a name="pattern"></a>Desenleri
Önceki deseni özelliği ile değiştirilir [desenleri](luis-concept-patterns.md). Desenler, daha az eğitim örnekleri sağlayarak tahmin doğruluğunu artırmak için kullanın.

## <a name="phrase-list"></a>İfade listesi

A [tümcecik listesi](luis-concept-feature.md#what-is-a-phrase-list-feature) aynı sınıfa ait benzer şekilde (örneğin, şehirler ya da ürün adlarını) işlenmesi gereken değerleri (sözcük ve tümcecikleri) bir grup içerir. Değiştirilebilir bir listesi, eş anlamlı sözcükler kabul edilir.

## <a name="prebuilt-domains"></a>Önceden oluşturulmuş etki alanı

A [önceden oluşturulmuş etki alanı](luis-how-to-use-prebuilt-domains.md) ev Otomasyonu (HomeAutomation) ya da Restoran ayırmalar (RestaurantReservation) gibi belirli bir etki alanı için yapılandırılmış bir LUIS uygulaması. Hedefleri, konuşma ve varlıklar, bu etki alanı için yapılandırılır.

## <a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık

A [önceden oluşturulmuş varlık](luis-prebuilt-entities.md) LUIS, genel türleri sayısı, URL ve e-posta gibi bilgileri sağlayan bir varlıktır. Uygulamanız için önceden oluşturulmuş bir varlık eklemek seçin.

## <a name="precision"></a>Duyarlık
İçinde [toplu test](luis-interactive-test.md#batch-testing), duyarlık (pozitif Tahmine dayalı olarak da bilinir) olan alınan konuşma arasında ilgili konuşma bölümü.

## <a name="programmatic-key"></a>Programlı anahtarı

Olarak yeniden adlandırıldı [anahtar yazma](#authoring-key).

## <a name="publish"></a>Yayımlama

Bir LUIS yapma anlamına gelir yayımlama [etkin sürüm](#active-version) hazırlama veya üretim kullanılabilir [uç nokta](#endpoint).  

## <a name="quota"></a>Kota

LUIS kotası ise SORUMLULUĞUN [Azure aboneliği katmanı](https://aka.ms/luis-price-tier). Her iki istek / saniye (HTTP durum 429) ve toplam istek (HTTP durum 403) ayda LUIS kota sınırlı olabilir.

## <a name="recall"></a>Geri çağırma
İçinde [toplu test](luis-interactive-test.md#batch-testing), geri çağırma (duyarlılık da bilinir), genelleştirmek LUIS olanağıdır.

## <a name="semantic-dictionary"></a>Anlam sözlüğü
İfade listesi sayfası yanı sıra listesi varlık sayfası üzerinde bir anlam sözlük sağlanır. Anlam sözlük bir kelimelerin geçerli kapsamda önerileri sağlar.

## <a name="sentiment-analysis"></a>Yaklaşım analizi
Yaklaşım analizi pozitif veya negatif değerleri tarafından sağlanan bir konuşma sağlayan [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/).

## <a name="speech-priming"></a>Konuşma Hazırlama işlemi

Konuşma Hazırlama işlemi aracılığıyla LUIS modelinize primed, konuşma hizmeti sağlar.

## <a name="spelling-correction"></a>Yazım denetimi

Bing yazım denetleyicisi önce tahmin uzunluğu yanlış yazılan sözcükleri düzeltmek etkinleştirin.

## <a name="starter-key"></a>Başlangıç anahtarı

Aynı [programlı anahtarı](#programmatic-key), yazma anahtarı olarak yeniden adlandırıldı.

## <a name="subscription-key"></a>Abonelik anahtarı

Abonelik anahtarı **uç nokta** LUIS hizmeti ile ilişkilendirilen anahtar [Azure'da oluşturduğunuz](luis-how-to-azure-subscription.md). Bu anahtarı değil [anahtar yazma](#programmatic-key). Bir uç noktası anahtarı varsa, tüm uç nokta istekleri yazma anahtarı yerine kullanılmalıdır. Uç nokta URL'SİNİN sonuna içinde geçerli uç nokta anahtarınızı görebilirsiniz [ **anahtarları ve uç noktaları** sayfa](luis-how-to-azure-subscription.md) içinde [LUIS](luis-reference-regions.md) Web sitesi. Bu değeri **abonelik anahtarı** ad/değer çifti.

## <a name="test"></a>Test

[Test](luis-interactive-test.md#test-your-app) sonuçları bir utterance LUIS için geçirme ve JSON görüntüleme LUIS uygulaması anlamına gelir.

## <a name="timezoneoffset"></a>Saat dilimi uzaklığı

Uç nokta timezoneOffset içerir. Bu ekleyin veya datetimeV2 kaldırmak istediğiniz dakika sayısıdır önceden oluşturulmuş varlık. Utterance Örneğin, "ne zaman artık sağlıyor?", döndürülen datetimeV2 istemci isteği için geçerli zaman ise. Bir bot veya botunuzun ait kullanıcı ile aynı değil başka bir uygulama, istemci istek geliyorsa, bot ve kullanıcı uzaklığı geçmelidir.

Bkz: [önceden oluşturulmuş datetimeV2 varlığın saat dilimini değiştirme](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity).

## <a name="token"></a>Belirteç
Bir belirteç varlık etiketli en küçük birimdir. Simgeleştirme uygulamanın üzerinde temel [kültür](luis-language-support.md#tokenization).

## <a name="train"></a>Eğitme

Eğitim olan herhangi bir değişiklik hakkında LUIS eğitiminde işleminin [etkin sürüm](#active-version) son eğitim itibaren.

## <a name="true-negative"></a>TRUE negatif (TN)

İçinde [toplu test](luis-interactive-test.md#batch-testing), konuşma, uygulamanızın doğru şekilde tahmin edilen hedef hedefi/varlık olmaması veri noktalarını temsil eder.

## <a name="true-positive"></a>Gerçek pozitif sonuç (TP)

İçinde [toplu test](luis-interactive-test.md#batch-testing), veri noktalarının konuşma, uygulamanızın doğru şekilde tahmin edilen hedef hedefi/varlık varlığını temsil eder.

## <a name="utterance"></a>Utterance

Bir utterance, "Seattle sonraki Salı kitap 2 bilet" gibi doğal dil bir terimdir. Örnek konuşma ıntent'e eklenir.

## <a name="version"></a>Sürüm

Bir LUIS [sürüm](luis-how-to-manage-versions.md) LUIS uygulama kimliği ve yayımlanan uç noktası ile ilişkili bir özel veri modeli. Her LUIS uygulaması en az bir sürüm var.
