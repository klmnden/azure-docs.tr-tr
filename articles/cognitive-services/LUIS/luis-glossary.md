---
title: Dil anlama (HALUK) API'si hizmeti için Sözlük | Microsoft Docs
description: Terimler sözlüğü açıklanmaktadır HALUK API hizmetiyle çalışacak şekilde karşılaşabileceğiniz.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: d1b83432e925e4f6fd439ac78acfde56ad31ba52
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128411"
---
# <a name="glossary"></a>Sözlük

## <a name="active-version"></a>Etkin sürüm

Etkin HALUK sürüm model değişiklikleri alan sürümdür. İçinde [HALUK](luis-reference-regions.md) Web sitesi, etkin sürümü olmayan bir sürüm olarak değişiklik yapmak istiyorsanız, önce bu sürüm etkin olarak ayarlamak için ihtiyacınız. 

## <a name="authoring"></a>Yazma

Yazma yeteneğidir oluşturmak, yönetmek ve dağıtmak bir [HALUK uygulama](#luis-app), her iki kullanarak [HALUK](luis-reference-regions.md) Web sitesi veya [API'lerini geliştirme](https://aka.ms/luis-authoring-api). 

## <a name="authoring-key"></a>Anahtar yazma

Daha önce "Programmatic" anahtarı adı. Uygulama yazmak için kullanılır. Üretim düzeyi endpoint sorgular için kullanılmaz. Daha fazla bilgi için bkz: [anahtar sınırları](luis-boundaries.md#key-limits).   

## <a name="batch-test-json-file"></a>Toplu iş metin JSON dosyası

Toplu iş dosyasını bir JSON dizisidir. Dizideki her öğe üç özelliklere sahiptir: `text`, `intent`, ve `entities`. `entities` Bir dizi bir özelliktir. Dizi boş olabilir. Varsa `entities` dizi boş değil, varlıkları doğru bir şekilde tanımlamak gerekiyor.

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

Bir ortak çalışanı değil [sahibi](#owner) uygulamasının, ancak ekleme, düzenleme ve silme hedefleri, varlıklar, utterances için aynı izinleri vardır.

## <a name="currently-editing"></a>Şu anda düzenleme

Aynı [etkin sürüm](#active-version)

## <a name="domain"></a>Etki alanı

HALUK bağlamında bir **etki alanı** bilgi alanıdır. Etki alanınızı bilgi için uygulama alan özeldir. Bu, seyahat aracı uygulaması gibi genel bir alan olabilir. Seyahat aracı uygulama yalnızca bilgi için şirketinizin belirli coğrafi konumlar, diller ve hizmetler gibi alanlarının özgü olabilir. 

## <a name="endpoint"></a>Uç noktası

[HALUK endpoint](https://aka.ms/luis-endpoint-apis) URL'dir sonra HALUK sorguları göndermek nerede [HALUK uygulama](#luis-app) yazılan ve yayımlanan. Uç nokta URL'sini bölge, bir uygulama kimliği yanı sıra yayımlanan uygulama içerir Uç nokta bulabilirsiniz **[Yayımla](publishapp.md)** sayfası, uygulamanızın kaynakları ve anahtarları tablo ya da, uç nokta URL'sini alabilirsiniz [uygulama bilgi al](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37) API.

Bir örnek uç nokta şuna benzer:

`https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscriptionID>&verbose=true&timezoneOffset=0&q=<utterance>`

|Sorgu dizesi parametresi|açıklama|
|--|--|
|bölge| [yayımlanan bölge](luis-reference-regions.md#publishing-regions) |
|AppID | HALUK uygulama kimliği |
|Subscriptionıd | Azure portalında oluşturulan HALUK uç noktası (abonelik) anahtarı |
|q | utterance |
|timezoneOffset| minutes|

## <a name="entity"></a>Varlık

[Varlıkları](luis-concept-entity-types.md) önemli sözcüklerdir [utterances](luis-concept-utterance.md) ilgili bilgileri açıklamak [hedefi](luis-concept-intent.md), ve bazen için önemli olduğunu. Bir varlık temelde datatype HALUK içinde değil. 

## <a name="f-measure"></a>F ölçümü

İçinde [toplu sınama][batch-testing], testin doğruluğu ölçüsü.

## <a name="false-negative"></a>False negatif (TN)

İçinde [toplu sınama][batch-testing], veri noktalarını, uygulamanızı yanlış tahmin hedef amacı/varlık yokluğu utterances temsil eder.

## <a name="false-positive"></a>Yanlış pozitif (TP)

İçinde [toplu sınama][batch-testing], veri noktalarını utterances, uygulamanızı yanlış tahmin hedef amacı/varlık varlığını temsil eder.

## <a name="features"></a>Özellikleri

Machine learning'de bir [özelliği](luis-concept-feature.md) ayırt bir ayırdedici nitelik ya da sisteminizi gözlemleyen veri özniteliği.

## <a name="intent"></a>Hedefi

Bir [hedefi](luis-concept-intent.md) bir görev ya da kullanıcının istediği gerçekleştirmek için eylem temsil eder. Bir amaç veya hedef bir uçuş kayıt, bir fatura ödeme ya da bir haber makalesi bulma gibi bir kullanıcının giriş ifade değil. HALUK hedefi tahmin tüm utterance temel alır. Varlıklar, karşılaştırma bir utterance parçalarıdır.

## <a name="labeling"></a>Etiketleme

Etiketleme olan bir sözcük veya tümcecik amacına 's içinde ilişkilendirme işlemi [utterance](#utterance) ile bir [varlık](#entity) (datatype). 

## <a name="luis-app"></a>HALUK uygulama

Doğal dil işleme dahil etmek için bir eğitilen veri modeli HALUK uygulamadır [hedefleri](#intent), [varlıklar](#entity)ve etiketli [utterances](#utterance).

## <a name="owner"></a>Sahibi

Her uygulamanın uygulama oluşturulan kişi olan bir sahip vardır. Sahibi ekleyebilir [ortak](#collaborator).

## <a name="pattern"></a>Desenler
Önceki deseni özelliği ile değiştirilir [desenleri](luis-concept-patterns.md). Modelleri daha az eğitim örnekleri sağlayarak tahmin doğruluğunu artırmak için kullanın. 

## <a name="phrase-list"></a>Tümcecik listesi

A [tümcecik listesi](luis-concept-feature.md#what-is-a-phrase-list-feature) değerlerin aynı sınıfa ait olan ve benzer şekilde (örneğin, şehirler veya ürün adlarını) değerlendirilmesi gerekir (kelimeler ve ifadeler) bir grubu kapsar. Birbirinin yerine bir liste eş anlamlıları kabul edilir. 

## <a name="prebuilt-domains"></a>Önceden oluşturulmuş bir etki alanı

A [önceden oluşturulmuş bir etki alanı](luis-how-to-use-prebuilt-domains.md) ev Otomasyon (HomeAutomation) veya Restoran ayırmaları (RestaurantReservation) gibi belirli bir etki alanı için yapılandırılmış bir HALUK uygulama. Hedefleri, utterances ve varlıkları bu etki alanı için yapılandırılır. 

## <a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık

A [önceden oluşturulmuş varlık](pre-builtentities.md) HALUK genel türleri sayısı, URL ve e-posta gibi bilgileri sağlayan bir varlıktır. Uygulamanıza önceden oluşturulmuş bir varlık eklemek için seçin. 

## <a name="precision"></a>Duyarlılık
İçinde [toplu sınama][batch-testing], duyarlık (pozitif Tahmine dayalı olarak da bilinir) olan alınan utterances arasında ilgili utterances kesir.

## <a name="programmatic-key"></a>Programsal anahtarı

İçin yeniden adlandırılmış [anahtar yazma](#authoring-key). 

## <a name="publish"></a>Yayımlama

Bir HALUK yapmadan anlamına gelir yayımlama [etkin sürüm](#active-version) hazırlık veya üretim kullanılabilir [endpoint](#endpoint).  

## <a name="quota"></a>Kota

HALUK kotası ise sınırlandırılmasıdır [Azure aboneliği katmanı](https://aka.ms/luis-price-tier). HALUK kota hem istekleri / saniye (HTTP durum 429) ve toplam istek (HTTP durum 403) aydaki sınırlı olabilir. 

## <a name="recall"></a>Geri çağırma
İçinde [toplu sınama][batch-testing], geri çağırma (duyarlılık da bilinir), genelleştirmek HALUK yeteneği. 

## <a name="semantic-dictionary"></a>Anlam sözlük
Bir anlam sözlük tümcecik listesi sayfası yanı sıra listesi varlık sayfası üzerinde sağlanır. Anlam sözlük geçerli kapsamına göre sözcüklerin öneriler sağlar.

## <a name="sentiment-analysis"></a>Düşünceleri çözümleme
Düşünceleri analizini sağlar tarafından sağlanan utterances pozitif veya negatif değerleri [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/). 

## <a name="speech-priming"></a>Konuşma Hazırlama işlemi

Konuşma Hazırlama işlemi, konuşma hizmetinin HALUK modelinizi primed izin verir. Bkz: [konuşma Hazırlama işlemi etkinleştir ](publishapp.md#enable-speech-priming).

## <a name="spelling-correction"></a>Yazım denetimi

Yayımla sayfasında etkinleştir [Bing yazım denetleyicisi](publishapp.md#enable-bing-spell-checker) tahmin önce utterances yanlış yazılmış sözcüklerin düzeltmek için. 

## <a name="starter-key"></a>Başlangıç anahtarı

Aynı [programlı anahtar](#programmatic-key), yazma anahtarına olarak yeniden adlandırıldı.

## <a name="subscription-key"></a>Abonelik anahtarı

Abonelik anahtarı **endpoint** HALUK hizmetiyle ilişkili anahtar [Azure'da oluşturulan](luis-how-to-azure-subscription.md). Bu anahtar [anahtar yazma](#programmatic-key). Bir uç noktası anahtarı varsa, tüm uç nokta istekler geliştirme anahtar yerine kullanılmalıdır. Geçerli uç nokta anahtarınızı alt kısmındaki uç nokta URL'si içinde gördüğünüz [ **uygulama yayımlama** sayfa](publishapp.md) içinde [HALUK](luis-reference-regions.md) Web sitesi. Bu değeri **abonelik anahtarı** ad/değer çifti. 

## <a name="test"></a>test etme

[Sınama](interactive-test.md#test-your-app) HALUK uygulama sonuçları bir utterance için HALUK geçirme ve JSON görüntüleme anlamına gelir.

## <a name="timezoneoffset"></a>Saat dilimi uzaklığı

Uç nokta timezoneOffset içerir. Bu datetimeV2 ekleyip istediğiniz dakika sayısıdır önceden oluşturulmuş varlık. Utterance ise, örneğin, "ne zaman şimdi nedir?", döndürülen datetimeV2 geçerli istemci isteği zamanı. İstemci isteği bir bot veya bot ait kullanıcı ile aynı değil başka bir uygulama geliyorsa bot ve kullanıcı uzaklığı geçirmelisiniz. 

Bkz: [önceden oluşturulmuş datetimeV2 varlığın saat dilimini değiştir](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity).

## <a name="token"></a>Belirteç
Varlık etiketli en küçük birim belirtecidir. Simgeleştirme uygulamanın üzerinde temel [kültür](luis-supported-languages.md#tokenization).

## <a name="train"></a>Tren

Eğitim olan değişiklikler hakkında HALUK eğitme işlemi [etkin sürüm](#active-version) son eğitim itibaren.

## <a name="true-negative"></a>Doğru negatif (TN)

İçinde [toplu sınama][batch-testing], veri noktalarını, uygulamanızı doğru şekilde tahmin hedef amacı/varlık yokluğu utterances temsil eder.

## <a name="true-positive"></a>Doğru pozitif (TP)

İçinde [toplu sınama][batch-testing], veri noktalarını utterances, uygulamanızın doğru şekilde tahmin hedef amacı/varlık varlığını temsil eder.

## <a name="utterance"></a>utterance

Bir utterance "Seattle sonraki Salı defteri 2 bilet" gibi bir doğal dil terimdir. Örnek utterances hedefi eklenir. 

## <a name="version"></a>Sürüm

Bir HALUK [sürüm](luis-how-to-manage-versions.md) HALUK uygulama kimliği ve yayımlanan uç ile ilişkili bir özel veri model. Her HALUK uygulamanın en az bir sürüme sahip.

[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/interactive-test#batch-testing
