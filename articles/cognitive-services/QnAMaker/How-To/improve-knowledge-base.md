---
title: Bilgi Bankası - soru-cevap Oluşturucu geliştirin
titleSuffix: Azure Cognitive Services
description: ''
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/05/2019
ms.author: diberry
ms.openlocfilehash: 7281fb15e91195e1dd20095d9fdf80d3d9894a26
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57433068"
---
# <a name="use-active-learning-to-improve-knowledge-base"></a>Bilgi bankasını geliştirmek için etkin öğrenmeyi kullanma

Etkin öğrenme bilgi bankanızı alternatif sorular, soru ve yanıt çiftinizi için kullanıcı-gönderimler göre önererek kalitesini sağlar. Bu öneriler, ya da sorularınız var ya da bunları reddetmeniz ekleme inceleyin. 

Bilgi bankanızı otomatik olarak değişmez. Herhangi bir değişikliğin etkili olması için sırayla önerileri kabul etmeniz gerekir. Bu öneriler sorular ekleyebilir ancak yoksa değiştirebilir veya de soru kaldırabilirsiniz.

## <a name="active-learning"></a>Etkin öğrenme

Soru-cevap Oluşturucu örtük ve açık bir geri bildirim ile soru çeşitlemesini öğrenir.
 
* Örtük geri bildirim – derecelendiricisini kullanıcı soru çok yakın puanlar ile birden çok yanıtlar olduğunda anlar ve bu geri bildirim göz önünde bulundurur. 
* Bilgi Bankası'ndaki, puanları çok az varyasyonu ile birden çok yanıt döndürüldüğünde açık geri bildirim-doğru soruyu hangi soru ise istemci uygulaması kullanıcıya sorar. Train API'si ile soru-cevap Oluşturucu kullanıcının açık geri bildirim gönderilir. 

Her iki yöntem derecelendiricisini kümelenir benzer sorgularla sağlar.

## <a name="how-active-learning-works"></a>Nasıl etkin works öğrenme

Etkin öğrenme herhangi belirli bir sorgu için soru-cevap Oluşturucu tarafından döndürülen ilk birkaç yanıtların puanları temel tetiklenir. Puan farklılıkları küçük bir aralık içinde yer alan sonra olası sorgusu _öneri_ her olası yanıtlar. 

Tüm öneriler birlikte benzerliğe göre kümelenir ve diğer sorular için en çok istenen önerilerden belirli sorgularının sıklığını üzerinde son kullanıcılar tarafından bağlı görüntülenir. Etkin öğrenme makul bir miktar ve çeşitli sorgular kullanım uç noktalar burada alma durumlarda en iyi olası öneriler sağlar.

5 veya daha fazla benzer sorguları kümelenir, 30 dakikada kabul etmek veya reddetmek için Bilgi Bankası Tasarımcısı kullanıcı tabanlı soruların soru-cevap Oluşturucu önerir.

Sorular soru-cevap Oluşturucu Portalı'nda önerilen sonra gözden geçirip kabul edin ya da bu önerileri Reddet gerekir. 

## <a name="upgrade-version-to-use-active-learning"></a>Etkin öğrenmeyi kullanma sürüme yükseltme

Etkin öğrenme, çalışma zamanı sürümü 4.4.0 ve sonraki sürümlerde desteklenir. Bilgi bankanızı bir önceki sürümünde oluşturulduysa [, çalışma zamanını yükseltme](troubleshooting-runtime.md#how-to-get-latest-qnamaker-runtime-updates) bu özelliği kullanmak için. 

## <a name="best-practices"></a>En iyi uygulamalar

Etkin öğrenme kullanırken en iyi yöntemler için bkz: [en iyi uygulamalar](../Concepts/best-practices.md#active-learning).

## <a name="score-proximity-between-knowledge-base-questions"></a>Bilgi Bankası sorular arasındaki uzaklık Puanlama

Bir sorunun puan % gibi 80, yüksek oranda emin olduğu durumlarda, etkin öğrenim için kabul edilen puanları aralığını yaklaşık %10 içinde geniş. Güvenilirlik puanı azaldıkça %40 gibi puanları aralığını yaklaşık %4 içinde de azaltır. 

Algoritma yakınlık belirlemek için basit bir hesaplama değil. Önceki örneklerde aralıkları sabit değildir ancak algoritma yalnızca etkisini anlamak için kılavuz olarak kullanılmalıdır.

## <a name="turn-on-active-learning"></a>Etkin öğrenmeyi Aç

Varsayılan olarak etkin olarak öğrenmeye kapalıdır. Bu önerilen bu soruları görmek için etkinleştirin. 

1. Seçin **Yayımla** Bilgi Bankası yayımlama. Etkin öğrenme sorguları GenerateAnswer API tahmin uç noktasından yalnızca toplanır. Soru-cevap Oluşturucu Portalı'nda Test bölmesine sorgular, etkin öğrenim etkilemez.

1. Üzerinde öğrenme etkin açmak için tıklayın, **adı**Git [ **hizmet ayarları** ](https://www.qnamaker.ai/UserSettings) sağ üst köşedeki soru-cevap Oluşturucu Portalı'nda.  

    ![Hizmet Ayarları sayfasında, etkin öğrenim'de geçiş](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Soru-cevap Oluşturucu hizmetini bulun, sonra geçiş **etkin olarak öğrenmeye**. 

    [![Hizmet Ayarları sayfasında, etkin öğrenim'de geçiş](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    Bir kez **etkin olarak öğrenmeye** olan etkin, Bilgi Bankası yeni sorular kullanıcı tarafından gönderilen sorular temel alınarak düzenli aralıklarla önerir. Devre dışı bırakabilirsiniz **etkin olarak öğrenmeye** ayarın açılıp tarafından yeniden.

## <a name="add-active-learning-suggestion-to-knowledge-base"></a>Etkin öğrenme öneri Bilgi Bankası'na ekleyin.

1. Önerilen sorular görmek için **Düzenle** Bilgi Bankası sayfasında **önerileri göster**. 

    [![Hizmet Ayarları sayfasında, iki durumlu önerileri göster düğme](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Bilgi Bankası ile soru ve yanıt çiftleri seçerek yalnızca önerileri göstermek için filtre **önerileri filtreyle**.

    [![Görmek için öneriler tarafından hizmet ayarları sayfasında, filtre yalnızca soru/yanıt çifti](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1.  Her soru bölüm önerileri içeren bir onay işareti ile yeni sorular gösterir `✔` , soru kabul etmek için veya bir `x` öneriler reddetmek için. Soru eklemek için onay işaretini seçin. 

    [![Hizmet Ayarları sayfasında, etkin öğrenim'de geçiş](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    Ekleme veya silme _tüm öneriler_ seçerek **tüm eklemek** veya **tümünü Reddet**.

1. Seçin **kaydedin ve eğitme** Bilgi Bankası'na değişiklikleri kaydedin.

1. Seçin **Yayımla** GenerateAnswer API'den kullanılabilir olması değişikliklere izin vermek üzere.

    5 veya daha fazla benzer sorguları kümelenir, 30 dakikada kabul etmek veya reddetmek için Bilgi Bankası Tasarımcısı kullanıcı tabanlı soruların soru-cevap Oluşturucu önerir.

## <a name="determine-best-choice-when-several-questions-have-similar-scores"></a>Benzer puanları birkaç sorularınız olduğunda en iyi seçenek belirleyin

Bir soru diğer sorular için puan içinde çok olduğunda, istemci uygulama geliştiricisine başvurun seçebilirsiniz.

### <a name="use-the-top-property-in-the-generateanswer-request"></a>GenerateAnswer istekte üst özelliğini kullanın

Soru-cevap Oluşturucu bir soruya yanıt gönderirken birden fazla üst yanıt döndürmek için JSON gövde bölümü sağlar:

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

İstemci uygulaması (örneğin, bir sohbet Robotu) yanıtı aldığında, en üst 3 soruyu döndürülür:

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

### <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Benzer puanları sorularınız olduğunda istemci uygulaması izleme

İstemci uygulama, tüm soruları görüntüler çoğu kullanıcının soruyu seçin bir seçenek ile kendi amacınıza temsil eder. 

Kullanıcı mevcut sorulardan birini seçtikten sonra. Kullanıcı geri bildirim, soru-cevap Oluşturucu için 's gönderilen [eğitme](http://www.aka.ms/activelearningsamplebot) geri bildirim etkin olarak öğrenmeye devam etmek için API döngü. 

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

Etkin öğrenme ile kullanma hakkında daha fazla bilgi edinin bir [Azure Bot C# örneği](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-activelearning-bot)

## <a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>Etkin öğrenme dışarı aktarılan uygulamanın tsv dosyasına kaydedilir.

Uygulamanız etkin olarak öğrenmeye etkin olan ve uygulamayı dışarı aktarma `SuggestedQuestions` tsv dosyası sütununda etkin olarak öğrenmeye verilerini korur. 

`SuggestedQuestions` Sütundur bilgileri örtük bir JSON nesnesi (`autosuggested`) ve açık (`usersuggested`) [geri bildirim](#active-learning). Bu JSON nesnesinin tek bir kullanıcı tarafından gönderilen soru için örneği `help` olan:

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

Bu uygulamayı yeniden içeri aktarın, bilgi toplamak ve öneriler, Bilgi Bankası için önermek etkin olarak öğrenmeye devam eder. 

## <a name="next-steps"></a>Sonraki adımlar
 
> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu API'si kullanma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
