---
title: LUIS tahminleri - Azure geliştirmek için desenler kullanma Öğreticisi | Microsoft Docs
titleSuffix: Cognitive Services
description: Bu öğreticide, hedefleri için düzeni LUIS amacı ve varlık Öngörüler geliştirmek için kullanın.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/30/2018
ms.author: diberry
ms.openlocfilehash: 355c1edd4fa7433e68a9c0e903f4f782203326fe
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365887"
---
# <a name="tutorial-improve-app-with-patterns"></a>Öğretici: uygulama desenleri ile geliştirin

Bu öğreticide, hedefi ve varlık tahmin artırmak için desenleri kullanın.  

> [!div class="checklist"]
* Bir desen uygulamanıza yardımcı olduğunu belirleme
* Bir düzen oluşturma
* Desen tahmin geliştirmeleri doğrulama

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Başlamadan önce

İnsan Kaynakları uygulamadan yoksa [toplu test](luis-tutorial-batch-testing.md) öğreticide [alma](luis-how-to-start-new-app.md#import-new-app) JSON'a yeni bir uygulama [LUIS](luis-reference-regions.md#luis-website) Web sitesi. İçeri aktarılacak uygulamasını bulunan [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-batchtest-HumanResources.json) GitHub deposu.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `patterns` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Desenler LUIS daha az sayıda örnek ile ortak konuşma öğretin.

İnsan Kaynakları etki alanı yapısı nedeniyle, kuruluşların çalışan ilişkiler hakkında soran birkaç genel yol vardır. Örneğin:

|Konuşmalar|
|--|
|Jill Jones kimin için rapor?|
|Kimin için Jill Jones raporları?|

Bu konuşma yakın her birçok utterance örneği sağlamadan bağlamsal benzersizlik belirleyin. Bir amaç için bir düzen ekleyerek, birçok utterance örneği sağlamadan LUIS bir amaç için ortak utterance düzenlerini öğrenir. 

Örnek şablon konuşma niyetini şunlar:

|Örnek şablon konuşma|
|--|
|{Çalışan} mu rapor?|
|Kimin {çalışana} raporları?|

Desen, varlıkları ve Ignorable metin tanımlamak için söz dizimi içeren bir şablon utterance örnek olarak sağlanır. Bir desen, normal ifade eşleştirme ve makine öğrenimi birleşimidir.  Şablon utterance örnek hedefi konuşma yanı sıra, amaç hangi konuşma uygun daha iyi LUIS verin.

Bir utterance için eşleştirilecek desen için sırada utterance varlıkları şablon utterance varlıklarda ilk eşleşmesi gerekir. Ancak, şablon varlıklar, yalnızca ıntents tahmin yardımcı olmaz. 

**Desenler varlıkları değil algılanırsa, daha az örnek konuşma sağlamanıza olanak tanırken desenle eşleşmez.**

Çalışanların içinde oluşturulduğunu unutmayın [listesi varlık öğretici](luis-quickstart-intent-and-list-entity.md).

## <a name="create-new-intents-and-their-utterances"></a>Yeni hedefleri ve kullanıcıların konuşma oluşturma

İki yeni hedef ekleme: `OrgChart-Manager` ve `OrgChart-Reports`. Bir kez LUIS tahmin hedefi adı istemci uygulama istemci uygulamasında bir işlev adı olarak kullanılabilir ve bu işlev bir parametre olarak çalışan varlık kullanılabilir döndürür.

```Javascript
OrgChart-Manager(employee){
    ///
}
```

1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

2. **Intents** (Amaçlar) sayfasında **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

3. Açılan iletişim kutusuna `OrgChart-Manager` girip **Done** (Bitti) öğesini seçin.

    ![Yeni bir ileti açılan pencere oluşturma](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

4. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |John W. Smith alt kimdir?|
    |John W. Smith kimin için rapor?|
    |John W. Smith'in manager kimdir?|
    |Kimin Jill Jones doğrudan rapor?|
    |Jill Jones gözetmen kimdir?|

    [![Intent'e yeni Konuşma ekleme LUIS ekran](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "ıntent'e yeni Konuşma ekleme LUIS ekran görüntüsü")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

    Anahtar cümlesi varlık çalışan varlık yerine hedefinin konuşma etiketlenmişse endişelenmeyin. Her ikisi de doğru Test bölmesinde ve uç noktasında tahmin. 

5. Sol gezinti bölmesinden **Intents** (Amaçlar) öğesini seçin.

6. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

7. Açılan iletişim kutusuna `OrgChart-Reports` girip **Done** (Bitti) öğesini seçin.

8. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |John W. Smith'in Astları kim?|
    |Kimin John W. Smith için raporları?|
    |John W. Smith yöneten?|
    |Jill Jones bağlı çalışanları kim?|
    |Kimin Jill Jones gözetmenlik?|

## <a name="caution-about-example-utterance-quantity"></a>Örnek utterance miktarı hakkında uyarı

Bu hedefleri, örnek konuşma miktarını LUIS düzgün şekilde ayarlayabilmesi yeterli değil. Gerçek bir uygulamada, her amaç 15 konuşma birçok seçenek ve utterance sözcük uzunluğu en az olmalıdır. Bu birkaç konuşma özellikle desenleri vurgulamak için seçilir. 

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Who is the boss of Jill Jones?` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.353984952
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.353984952
            },
            {
                "intent": "OrgChart-Reports",
                "score": 0.214128986
            },
            {
                "intent": "EmployeeFeedback",
                "score": 0.08434003
            },
            {
                "intent": "MoveEmployee",
                "score": 0.019131
            },
            {
                "intent": "GetJobInformation",
                "score": 0.004819009
            },
            {
                "intent": "Utilities.Confirm",
                "score": 0.0043958663
            },
            {
                "intent": "Utilities.StartOver",
                "score": 0.00312064588
            },
            {
                "intent": "Utilities.Cancel",
                "score": 0.002265454
            },
            {
                "intent": "Utilities.Help",
                "score": 0.00133465114
            },
            {
                "intent": "None",
                "score": 0.0011388344
            },
            {
                "intent": "Utilities.Stop",
                "score": 0.00111166481
            },
            {
                "intent": "FindForm",
                "score": 0.0008900076
            },
            {
                "intent": "ApplyForJob",
                "score": 0.0007836131
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                }
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

Bu sorgu başarısız oldu? Bu eğitim döngüsü için başarılı olmadı. İki üst ıntents puanları kapat. Çünkü LUIS eğitim tam olarak aynı her zaman, biraz yoktur, bir sonraki eğitim döngüde bu iki puanları ters çevir. Yanlış amaç döndürülebilen sonucudur. 

Desenleri doğru amaç 's puanı önemli ölçüde daha yüksek yüzdesi cinsinden ve bir sonraki en yüksek puanı uzaklaştıkça yapmak için kullanın. 

Bu ikinci bir tarayıcı penceresi açık bırakın. Bu öğreticide daha sonra yeniden kullanın. 

## <a name="add-the-template-utterances"></a>Şablon Konuşma ekleme

1. Seçin **derleme** üst menüdeki.

2. Sol gezinti bölmesindeki altında **uygulama performansını**seçin **desenleri** sol gezinti bölmesinden.

3. Seçin **Kuruluş Şeması-yönetici** amacı, aşağıdaki şablon konuşma, bir kerede enter, seçtikten sonra her şablon utterance girin:

    |Şablon konuşma|
    |:--|
    |{Çalışan} [?] alt kimdir|
    |{Çalışan} mu rapor [?] için|
    |[?] {[Kullanıcının] çalışan} Yöneticisi kimdir|
    |{Çalışan} mu [?] için'doğrudan rapor|
    |[?] {[Kullanıcının] çalışan} gözetmen kimdir|
    |Patronunuzdan {çalışanın} [?] kimdir|

    `{Employee}` Söz dizimi hangi varlık olarak da öyledir olarak şablon utterance içindeki varlık konum işaretler. 

    Rolleri varlıklarla rol adı içeren sözdizimi kullanın ve ele alınmaktadır bir [rolleri için ayrı öğretici](luis-tutorial-pattern-roles.md). 

    İsteğe bağlı söz dizimi `[]`, sözcük veya isteğe bağlı bir noktalama işaretleri. LUIS, isteğe bağlı bir metin köşeli ayraçlar yoksayılıyor utterance eşleşir.

    Sol küme ayracı girdiğinizde, Doldur LUIS yardımcı olur, şablon utterance varlıkta yazarsanız `{`.

    [![Şablon konuşma amacı için girme ekran görüntüsü](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)

4. Seçin **Kuruluş Şeması-raporları** amacı, aşağıdaki şablon konuşma, bir kerede enter, seçtikten sonra her şablon utterance girin:

    |Şablon konuşma|
    |:--|
    |Astları [?] {[kullanıcının] çalışan} olan|
    |Kimin {çalışan} [?] raporları|
    |{Çalışan} mu Yönetme [?]|
    |{Çalışan} bağlı çalışanları [?] kim|
    |{Çalışan} mu gözetmenlik [?]|
    |{Çalışan} mu Patronunuzdan [?]|

## <a name="query-endpoint-when-patterns-are-used"></a>Uç nokta desenleri kullanıldığında sorgulama

1. Eğitim ve uygulamayı yeniden yayımlayın.

2. Tarayıcı sekmeleri uç nokta URL'si sekmesine geçin.

3. Adres URL'SİNİN sonuna gidin ve girin `Who is the boss of Jill Jones?` utterance olarak. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.9999989
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.9999989
            },
            {
                "intent": "OrgChart-Reports",
                "score": 7.616303E-05
            },
            {
                "intent": "EmployeeFeedback",
                "score": 7.84204349E-06
            },
            {
                "intent": "GetJobInformation",
                "score": 1.20674213E-06
            },
            {
                "intent": "MoveEmployee",
                "score": 7.91245157E-07
            },
            {
                "intent": "None",
                "score": 3.875E-09
            },
            {
                "intent": "Utilities.StartOver",
                "score": 1.49E-09
            },
            {
                "intent": "Utilities.Confirm",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Help",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Stop",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Cancel",
                "score": 1.225E-09
            },
            {
                "intent": "FindForm",
                "score": 1.123077E-09
            },
            {
                "intent": "ApplyForJob",
                "score": 5.625E-10
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                },
                "role": ""
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

Hedefi tahmin artık önemli ölçüde yüksektir.

## <a name="working-with-optional-text-and-prebuilt-entities"></a>İsteğe bağlı bir metin ve önceden oluşturulmuş varlıklarla çalışma

Bu öğreticideki önceki deseni şablon konuşma birkaç örnek verilmiştir, s harfi iyelik kullanımı gibi isteğe bağlı bir metin vardı `'s`ve soru işareti kullanımını `?`. Uç nokta konuşma yöneticileri ve İnsan Kaynakları temsilcileri geçmiş veri aramaya yanı sıra, gelecekteki bir tarihte'olmuyor şirket içinde çalışan taşır planlı Göster varsayalım.

Örnek konuşma şunlardır:

|Hedefi|İsteğe bağlı bir metin ve önceden oluşturulmuş varlıklar ile örnek konuşma|
|:--|:--|
|Kuruluş Şeması-Manager|`Who was Jill Jones manager on March 3?`|
|Kuruluş Şeması-Manager|`Who is Jill Jones manager now?`|
|Kuruluş Şeması-Manager|`Who will be Jill Jones manager in a month?`|
|Kuruluş Şeması-Manager|`Who will be Jill Jones manager on March 3?`|

Bu örneklerin her bir fiil geniş zaman kullanılmaktadır `was`, `is`, `will be`, bir tarih yanı sıra `March 3`, `now`, ve `in a month`, LUIS gereken doğru şekilde tahmin edin. Son iki örneği neredeyse aynı metni dışında kullandığına dikkat edin `in` ve `on`.

Örnek şablon konuşma:
|Hedefi|İsteğe bağlı bir metin ve önceden oluşturulmuş varlıklar ile örnek konuşma|
|:--|:--|
|Kuruluş Şeması-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?`]|
|Kuruluş Şeması-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
|Kuruluş Şeması-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
|Kuruluş Şeması-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

Köşeli parantezler, isteğe bağlı söz diziminin kullanılması `[]`, bu isteğe bağlı bir metin şablonu utterance için eklemenizi kolaylaştırır ve ikinci bir düzeyi kadar yuvalanabilir `[[]]`ve varlıklar veya metin ekleyin.

**Soru: Bir tek şablon utterance son iki örnekte konuşma neden birleştirebilir uygulanamadı?** Düzen şablonu veya söz dizimi desteklemiyor. Her ikisi de yakalamak için `in` sürümü ve `on` sürüm, her bir ayrı bir şablon utterance olması gerekir.

**Soru: Neden tümü `w` harf, küçük harf her şablon utterance ilk harfini? Bunlar, isteğe bağlı olarak büyük veya küçük olmamalıdır?** Sorgu bitiş noktasına istemci uygulama tarafından gönderilen utterance küçük dönüştürülür. Şablon utterance büyük veya küçük harf olabilir ve uç nokta utterance ya da olabilir. Karşılaştırma küçük harfe dönüştürme sonra her zaman gerçekleştirilir.

**Soru: Önceden oluşturulmuş numarası neden olmadığını şablonunun parçası Mart 3 ise utterance hem de sayı olarak tahmin edilen `3` ve tarih `March 3`?** Şablon utterance bağlamsal bir tarih ya da tam anlamıyla kullanılarak olan `March 3` veya olarak bulunabilen `in a month`. Bir tarihi bir sayı içerebilir, ancak bir sayı olarak bir tarih mutlaka görülen değil. Her zaman en iyi tahmin JSON sonuçları döndürülmesini istediğiniz türünü temsil eden varlık kullanın.  

**Soru: Hatalı hakkında tümcecik oluşturulmuş konuşma gibi `Who will {Employee}['s] manager be on March 3?`.** Bu nerede gibi bakımından farklı kipleri `will` ve `be` ayrılmış yeni bir şablon utterance olmanız gerekir. Mevcut bir şablonu utterance bunu eşleşmez. Utterance amacı değişmediğinden, ancak word yerleşimden utterance değişti. Bu değişiklik LUIS tahmine etkiler.

**Unutmayın: desenin eşleştiği sonra varlıkları ilk olarak bulunur.**

## <a name="edit-the-existing-pattern-template-utterance"></a>Mevcut deseni şablon utterance Düzenle

1. LUIS Web sitesinde seçin **derleme** üst menüden seçip **desenleri** soldaki menüde. 

2. Mevcut bir şablonu utterance Bul `Who is {Employee}['s] manager[?]`, üç noktayı seçin (***...*** ) sağ. 

3. Seçin **Düzenle** açılır menüden. 

4. Şablon utterance için değiştirin: `who is {Employee}['s] manager [[on]{datetimeV2}?]]`

## <a name="add-new-pattern-template-utterances"></a>Yeni düzen şablonu Konuşma ekleme

1. Yine **desenleri** bölümünü **derleme**, birkaç yeni şablon konuşma deseni ekleyin. Seçin **Kuruluş Şeması-yönetici** hedefi aşağı açılan menüden ve her biri aşağıdaki şablon konuşma girin:

    |Hedefi|İsteğe bağlı bir metin ve önceden oluşturulmuş varlıklar ile örnek konuşma|
    |--|--|
    |Kuruluş Şeması-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
    |Kuruluş Şeması-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
    |Kuruluş Şeması-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
    |Kuruluş Şeması-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

2. Uygulama eğitin.

3. Seçin **Test** test panelini açmak için bölmenin üstünde. 

4. Desenle eşleşen ve hedefi puanı önemli ölçüde yüksek olduğunu doğrulamak için birkaç test konuşma girin. 

    İlk utterance girdikten sonra Seç **inceleyin** altında sonucu tahmin sonuçlarını görebilirsiniz.

    |Konuşma|
    |--|
    |Kimin Jill Jones yöneticisi olacak mı|
    |kimin jill Can'ın yöneticisi olacak mı|
    |Kimin Jill Can'ın yöneticisi olacak mı?|
    |kimin Jill jones Yöneticisi 3 Mart olacak mı|
    |Gelecek ay kimin Jill Jones yöneticisi olacak|
    |Bir ayda kimin Jill Jones yöneticisi olacak?|

Tüm bu konuşma bulunan varlıkların içinde bu nedenle bunlar aynı deseniyle eşleşen ve yüksek tahmin puanı vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Roller bir desen ile kullanmayı öğrenin](luis-tutorial-pattern-roles.md)