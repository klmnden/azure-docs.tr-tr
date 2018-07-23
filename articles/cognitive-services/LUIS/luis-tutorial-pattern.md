---
title: LUIS tahminleri - Azure geliştirmek için desenler kullanma Öğreticisi | Microsoft Docs
titleSuffix: Cognitive Services
description: Bu öğreticide, hedefleri için düzeni LUIS amacı ve varlık Öngörüler geliştirmek için kullanın.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: v-geberr;
ms.openlocfilehash: 5d486272f7f713c5d4e6f7b598073c5c09875d43
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172475"
---
# <a name="tutorial-improve-app-with-patterns"></a>Öğretici: uygulama desenleri ile geliştirin

Bu öğreticide, hedefi ve varlık tahmin artırmak için desenleri kullanın.  

> [!div class="checklist"]
* Bir desen uygulamanıza yardımcı olduğunu belirleme
* Bir düzen oluşturma 
* Desen tahmin geliştirmeleri doğrulama

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS](luis-reference-regions.md) hesabına ihtiyacınız olacak.

## <a name="before-you-begin"></a>Başlamadan önce
İnsan Kaynakları uygulamadan yoksa [toplu test](luis-tutorial-batch-testing.md) öğreticide [alma](luis-how-to-start-new-app.md#import-new-app) JSON'a yeni bir uygulama [LUIS](luis-reference-regions.md#luis-website) Web sitesi. İçeri aktarılacak uygulamasını bulunan [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-batchtest-HumanResources.json) GitHub deposu.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `patterns` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Desenler LUIS daha az sayıda örnek ile ortak konuşma öğretin.
İnsan Kaynakları etki alanı yapısı nedeniyle, kuruluşların çalışan ilişkiler hakkında soran birkaç genel yol vardır. Örneğin:

```
Who does Jill Jones report to?
Who reports to Jill Jones? 
```

Bu konuşma yakın her birçok utterance örneği sağlamadan bağlamsal benzersizlik belirleyin. Bir amaç için bir düzen ekleyerek, birçok utterance örneği sağlamadan LUIS bir amaç için ortak utterance düzenlerini öğrenir. 

Örnek şablon konuşma niyetini şunlar:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

Desen, varlıkları ve Ignorable metin tanımlamak için söz dizimi içeren bir şablon utterance örnek olarak sağlanır. Bir desen, normal ifade eşleştirme ve makine öğrenimi birleşimidir.  Şablon utterance örnek hedefi konuşma yanı sıra, amaç hangi konuşma uygun daha iyi LUIS verin.

Bir utterance için eşleştirilecek desen için sırada utterance varlıkları şablon utterance varlıklarda ilk eşleşmesi gerekir. Ancak, şablon varlıklar, yalnızca ıntents tahmin yardımcı olmaz. 

**Desenler varlıkları değil algılanırsa, daha az örnek konuşma sağlamanıza olanak tanırken desenle eşleşmez.**

Çalışanların içinde oluşturulduğunu unutmayın [listesi varlık öğretici](luis-quickstart-intent-and-list-entity.md).

## <a name="create-new-intents-and-their-utterances"></a>Yeni hedefleri ve kullanıcıların konuşma oluşturma
İki yeni hedef ekleme: `OrgChart-Manager` ve `OrgChart-Reports`. Bir kez LUIS tahmin hedefi adı istemci uygulama istemci uygulamasında bir işlev adı olarak kullanılabilir ve bu işlev bir parametre olarak çalışan varlık kullanılabilir döndürür.

```
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

    [![](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Intent'e yeni Konuşma ekleme LUIS ekran görüntüsü")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

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
Yeni amacı ve konuşma eğitim gerektirir. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Eğitim düğmesinin görüntüsü](./media/luis-tutorial-pattern/hr-train-button.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Başarılı işlem bildirim çubuğunun görüntüsü](./media/luis-tutorial-pattern/hr-trained-inline.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Sohbet botunda veya başka bir uygulamada LUIS tahmini almak için uygulamayı yayımlamanız gerekir. 

1. LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

    [ ![Üretim yuvası düğmesi vurgulanmış şekilde, ekran Yayımla Sayfası Yayımla](./media/luis-tutorial-pattern/hr-publish-to-production.png)](./media/luis-tutorial-pattern/hr-publish-to-production.png#lightbox)

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama
1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    [ ![Uç nokta URL'si vurgulanmış ekran görüntüsü, yayımlama sayfası](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png)](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png#lightbox)


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
    
    Varlıklarla rolleri rol adını içeren sözdizimini kullanın ve ele alınmaktadır bir [rolleri için ayrı öğretici](luis-tutorial-pattern-roles.md). 

    İsteğe bağlı söz dizimi `[]`, sözcük veya isteğe bağlı bir noktalama işaretleri. LUIS, isteğe bağlı bir metin köşeli ayraçlar yoksayılıyor utterance eşleşir.

    Sol küme ayracı girdiğinizde, Doldur LUIS yardımcı olur, şablon utterance varlıkta yazarsanız `{`.

    [ ![Şablon konuşma amacı için girme ekran görüntüsü](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)



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

2. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

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

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için üç noktayı seçin (***...*** ) sağında bulunan uygulama listesinde uygulama adı, seçin **Sil**. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Roller bir desen ile kullanmayı öğrenin](luis-tutorial-pattern-roles.md)