---
title: Çok Aç konuşmaları
titleSuffix: Azure Cognitive Services
description: Bağlam ve istekleri robotunuza ilişkin bir soru alanından diğerine çok dönüş olarak bilinir, birden çok kapatır yönetmek için kullanın. Çok sıranız özelliği bir geri ve İleri önceki sorusunun bağlam burada etkiler sonraki soru ve yanıt konuşma.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 05/14/2019
ms.author: diberry
ms.openlocfilehash: f12b55e9b00e933e13f84832b8cc36267a1da05f
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954878"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Birden çok kapatır konuşmanın oluşturmak için istemleri takip kullanın

Bağlam ve izleme istekleri olarak bilinen birden çok kapatır yönetmek için kullanmak _çok Aç_, robotunuza ilişkin bir soru alanından diğerine.

Uzmanlardan [tanıtım videosu](https://aka.ms/multiturnexample).

## <a name="what-is-a-multi-turn-conversation"></a>Bir çok Aç konuşma nedir?

Konuşma bazı türleri tek bir sırayla tamamlanamıyor. İstemci uygulaması (Sohbet Robotu) konuşmalarınızı tasarlarken, bir kullanıcı filtrelenmiş ya da doğru yanıtı belirleyebilmesi iyileştirilmektedir gereken soru sorun. Kullanıcıyla sunarak bu soruları akışla mümkündür **izleme ister**.

Kullanıcı bir soru sorduğunda, soru-cevap Oluşturucu yanıt verir _ve_ herhangi bir izleme istemi. Bu seçenek olarak takip soruları göstermenizi sağlar. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Örnek çok Aç Konuşma ile Sohbet Robotu

Sohbet Robotu, konuşma, soru soru son yanıt belirlemek için kullanıcı yönetir.

![Damıtarak konuşma bağlamında kullanılabilen akışın içinde görüşme durumu Konuşmaya devam seçeneği olarak sunulan soruların yanıtlarını içinde uyarılar sağlayarak bir çok Aç iletişim sisteminde yönetin.](../media/conversational-context/conversation-in-bot.png)

Önceki resimde, kullanıcının soru yanıt döndürmeden önce iyileştirilmektedir gerekir. Bilgi Bankası ' (#1) sorunun sohbet Robotu dört seçimden (2) sunulan dört izleme istemleri sahiptir. 

Kullanıcı bir seçim (3) seçtiğinde, ardından İleri iyileştirme seçenekleri (4) listesi sunulur. Bu durum, doğru ve son yanıt (6) belirlenir kadar (5) devam edebilir.

Bağlamsal konuşma yönetmek için istemci uygulamanızı değiştirmeniz gerekebilir.

## <a name="create-a-multi-turn-conversation-from-documents-structure"></a>Belgenin yapısından çok Aç konuşma oluşturma
Bilgi Bankası oluşturduğunuzda, bir isteğe bağlı-çok Aç ayıklama etkinleştirmek için onay kutusunu görürsünüz. Bir belge içeri aktarırken bu seçeneği belirlerseniz, çok Aç konuşma yapısından örtük. Bu yapıyı varsa soru-cevap Oluşturucu izleme komut istemi soru-cevap çiftlerini oluşturur. Çok Aç yapısı yalnızca nelze odvodit URL'leri, PDF veya DOCX dosyaları. 

Microsoft Surface, aşağıdaki görüntüde [PDF dosyası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf) el ile kullanılmak üzere tasarlanmıştır. 

![! [Bir belge içe aktarırsanız, bağlamsal konuşma yapısından ima. Bu yapıyı varsa soru-cevap Oluşturucu izleme komut istemi soru-cevap çiftlerini belge alma işleminin bir parçası olarak sizin için oluşturur.] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

PDF belgesinin içeri aktarırken, soru-cevap Oluşturucu damıtarak konuşma bağlamında kullanılabilen akışı oluşturmak için yapı izleme istemleri belirler. 

![! [PDF belgesinin içeri aktarırken, soru-cevap Oluşturucu damıtarak konuşma bağlamında kullanılabilen akışı oluşturmak için yapı izleme istemleri belirler. ](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

## <a name="show-questions-and-answers-with-context"></a>Sorular ve cevaplar bağlamla Göster

1. Bağlamsal konuşmaları yalnızca olanlar için görüntülenen soru ve yanıt çiftleri azaltın. Seçin **görüntüleme seçenekleri**, ardından **Show bağlam (Önizleme)**. İlk soru ve yanıt çifti izleme istemiyle ekleyene kadar listesi boş olur. 

    ![Soru filtrelemek ve bağlamsal konuşmaları tarafından çiftleri yanıt](../media/conversational-context/filter-question-and-answers-by-context.png)

## <a name="add-new-qna-pair-as-follow-up-prompt"></a>Yeni soru-cevap çift olarak izleme istemi ekleyin

1. Seçin **ekleme soru-cevap çifti**. 
1. Yeni soru metnini girin `Give feedback.` bir yanıt `What kind of feedback do you have?`.

1. İçinde **yanıt** bu soruyu seçin için sütun **Ekle izleme istemi**. 
1. **İzleme istemi (Önizleme)** açılır pencere arama için mevcut bir soru veya yeni bir soru girin olanak tanır. Aşağıdaki değerler girerek yeni bir istemi oluşturun: 

    |Metin alanı|Değer|
    |--|--|
    |**Metin görüntüleme**|`Feedback on an QnA Maker service`|
    |**Yanıt için bağlantı**|`How would you rate QnA Maker??`|
    |||

    ![Yeni komut istemi soru-cevap oluşturma](../media/conversational-context/create-child-prompt-from-parent.png)

1. Denetleme **yalnızca bağlam**. **Yalnızca bağlam** seçeneğin belirtilmediğini bu kullanıcı metni anlaşılması _yalnızca_ önceki soruyu yanıtlayarak verildiyse. Bu senaryo için istem metnini herhangi bir tek başına soru olarak anlam ifade etmez, yalnızca önceki soruyu bağlamında mantıklıdır.
1. Seçin **Yeni Oluştur** seçip **Kaydet**. 

    Bu yeni bir soru-cevap çifti oluşturulur ve izleme istemi olarak seçilen sorunun bağlı. **Bağlam** sütununda, her iki sorular için izleme bir komut istemi ilişkileri gösterir. 

    ![! [Bağlam sütununda, her iki sorular için izleme bir komut istemi ilişkiyi gösterir.] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Seçin **Ekle izleme istemi** için `Give feedback` soru başka bir izleme istemi ekleyin. Bu açılır **izleme istemi (Önizleme)** açılır pencere.

1. Aşağıdaki değerler girerek yeni bir istemi oluşturun:

    |Metin alanı|Değer|
    |--|--|
    |**Metin görüntüleme**|`Feedback on an existing feature`|
    |**Yanıt için bağlantı**|`Which feature would you like to give feedback on?`|
    |||

1. Denetleme **yalnızca bağlam**. **Yalnızca bağlam** seçeneğin belirtilmediğini bu kullanıcı metni anlaşılması _yalnızca_ önceki soruyu yanıtlayarak verildiyse. Bu senaryo için istem metnini herhangi bir tek başına soru olarak anlam ifade etmez, yalnızca önceki soruyu bağlamında mantıklıdır.

1. **Kaydet**’i seçin. 

    Bu yeni bir soru oluşturulur ve izleme komut istemi soru olarak soru bağlı `Give feedback` soru.
    
    Bu noktada, önceki soruya beğenmediğinizi iki izleme istemleri üst soru sahip `Give feedback`.

    ![! [Bu noktada, önceki 'geri bildirimde bulunun' soruya, beğenmediğinizi iki izleme istemleri üst soru sahiptir.] (.. / media/conversational-context/all-child-prompts-created.png)](../media/conversational-context/all-child-prompts-created.png#lightbox)

1. Seçin **kaydedin ve eğitme** yeni sorular ile Bilgi Bankası eğitmek için. 

## <a name="add-existing-qna-pair-as-follow-up-prompt"></a>İzleme istemi var olan soru-cevap çifti Ekle

1. Bir izleme istemi olarak var olan bir soru-cevap çifti bağlamak isterseniz, soru ve yanıt çifti için bir satır seçin.
1. Seçin **Ekle izleme istemi** söz konusu satırdaki.
1. İçinde **izleme istemi (Önizleme)** açılır pencere, arama kutusuna yanıt metni girin. Tüm eşleşmeleri döndürülür. Yanıt izlenmesini istediğiniz ve kontrol seçin **yalnızca bağlam**, ardından **Kaydet**. 

    ![İzleme istemi ait bağlantı yanıt metni kullanarak yanıt iletişim var olan bir yanıt arayın.](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

    İzleme istemi ekledikten sonra seçeneğini belirlemeyi unutmayın **kaydedin ve eğitme**.
  
<!--

## To find best prompt answer, add metadata to follow-up prompts 

If you have several follow-up prompts for a given QnA pair, but you know as the knowledge base manager, that not all prompts should be returned, use metadata to categorize the prompts in the knowledge base, then send the metadata from the client application as part of the GenerateAnswer request.

In the knowledge base, when a question-and-answer pair is linked to follow-up prompts, the metadata filters are applied first, then the follow-ups are returned.

1. For the two follow-up QnA pairs, add metadata to each one:

    |Question|Add metadata|
    |--|--|
    |`Feedback on an QnA Maker service`|"Feature":"all"|
    |`Feedback on an existing feature`|"Feature":"one"|
    
    ![Add metadata to follow-up prompt so it can be filtered in conversation response from service](../media/conversational-context/add-metadata-feature-to-follow-up-prompt.png) 

1. Save and train. 

    When you send the question `Give feedback` with the metadata filter `Feature` with a value of `all`, only the QnA pair with that metadata will be returned. Both QnA pairs are not returned because they both do not match the filter. 

-->

## <a name="test-the-qna-set-to-get-all-the-follow-up-prompts"></a>Test izleme almak için soru-cevap ayarlamak ister

Ne zaman takip sorusu test ister içinde **Test** bölmesinde **etkinleştirmek çok Aç**, sorunuzu girin. Yanıtta izleme yönergeleri içerir.

![Test Bölmesi'nde soruyu test ederken yanıt izleme yönergeleri içerir.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

## <a name="json-request-to-return-initial-answer-and-follow-up-prompts"></a>İlk yanıt ve istemleri takip döndürülecek JSON isteği

Boş kullanmak `context` kullanıcının sorusuna verilen yanıt istemek ve istemleri takip dahil etmek için nesne. 

```JSON
{
  "question": "accounts and signing in",
  "top": 30,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="json-response-to-return-initial-answer-and-follow-up-prompts"></a>İlk yanıt ve istemleri takip döndürülecek JSON yanıtı

Önceki bölümde yanıt ve izleme sizden istenen `Accounts and signing in`. Yanıt adresinde yer alan bilgilerin istemi içeriyor `answers[0].context`, kullanıcıya görüntülenecek metni içerir. 

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way they like. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 86.96,
            "id": 37,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 38,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    },
                    {
                        "displayOrder": 1,
                        "qnaId": 39,
                        "qna": null,
                        "displayText": "Use Windows Hello to sign in"
                    },
                    {
                        "displayOrder": 2,
                        "qnaId": 40,
                        "qna": null,
                        "displayText": "Sign out"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 32.27,
            "id": 38,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start , and right-click your name. Then select Sign out. ",
            "score": 27.0,
            "id": 40,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ],
    "debugInfo": null
}
```

`prompts` Dizisi sağlar metinde `displayText` özelliği ve `qnaId` konuşma akıştaki sonraki görüntülenen seçenek olarak bu yanıtlar göstermek için değer. 

## <a name="json-request-to-return-non-initial-answer-and-follow-up-prompts"></a>JSON istek olmayan ilk yanıt ve istemleri takip döndürmek için

Dolgu `context` önceki bağlam eklenecek nesne.

Aşağıdaki JSON istekte geçerli soru şudur `Use Windows Hello to sign in` ve önceki soruyu `Accounts and signing in`. 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 30,
  "userId": "Default",
  "isTest": false,
  "qnaId": 39,
  "context": {
    "previousQnAId": 37,
    "previousUserQuery": "accounts and signing in"
  }
}
``` 

##  <a name="json-response-to-return-non-initial-answer-and-follow-up-prompts"></a>Olmayan ilk yanıt ve istemleri takip döndürülecek JSON yanıtı

Soru-cevap Oluşturucu _GenerateAnswer_ JSON yanıtı içeren izleme istemleri `context` listedeki ilk öğe özelliği `answers` nesnesi:

```JSON
{
    "answers": [
        {
            "questions": [
                "Give feedback"
            ],
            "answer": "What kind of feedback do you have?",
            "score": 100.0,
            "id": 288,
            "source": "Editorial",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 291,
                        "qna": null,
                        "displayText": "Feedback on an QnA Maker service"
                    },
                    {
                        "displayOrder": 0,
                        "qnaId": 292,
                        "qna": null,
                        "displayText": "Feedback on an existing feature"
                    }
                ]
            }
        }
    ]
}
```

## <a name="displaying-prompts-and-sending-context-in-the-client-application"></a>Komut istemlerini görüntüleme ve istemci uygulamasında bağlamı gönderme 

İstemleri, Bilgi Bankası'ndaki ekledik ve akış test Bölmesi'nde test ettikten istemleri otomatik olarak istemci uygulamalarında görünmeye başlar. Size istemleri önerilen eylemleri veya düğme olarak kullanıcının sorgu yanıtı bir parçası olarak istemci uygulamaları dahil ederek gösterebilir [Bot Framework örnek](https://aka.ms/qnamakermultiturnsample) kodunuzda. İstemci uygulaması geçerli soru-cevap kimliği ve kullanıcı sorgusu depolamak ve bunları geçmesi [GenerateAnswer API bağlam nesnesinin](#json-request-to-return-non-initial-answer-and-follow-up-prompts) ileri kullanıcı sorgusu için.

## <a name="display-order-supported-in-api"></a>Desteklenen API görüntülenme sırasını

JSON yanıtta döndürülen görüntülenme sırasını yalnızca API tarafından düzenleme için desteklenir. 

## <a name="next-steps"></a>Sonraki adımlar

Gelen bağlamsal konuşmalara hakkında daha fazla bilgi [iletişim örnek](https://aka.ms/qnamakermultiturnsample) veya daha fazla bilgi edinin [kavramsal bot tasarım çok Aç konuşmaları](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Bilgi Bankası geçirme](../Tutorials/migrate-knowledge-base.md)
