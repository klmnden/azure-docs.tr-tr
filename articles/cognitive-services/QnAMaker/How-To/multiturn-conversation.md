---
title: Çok Aç konuşmaları
titleSuffix: Azure Cognitive Services
description: Bağlam ve istekleri robotunuza ilişkin bir soru alanından diğerine çok dönüş olarak bilinir, birden çok kapatır yönetmek için kullanın. Çok sıranız özelliği bir geri ve İleri önceki sorusunun bağlam burada etkiler sonraki soru ve yanıt konuşma.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/26/2019
ms.author: diberry
ms.openlocfilehash: a126456159776254408df8325f97fcee967835e2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442729"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Birden çok kapatır konuşmanın oluşturmak için istemleri takip kullanın

Bağlam ve izleme istekleri olarak bilinen birden çok kapatır yönetmek için kullanmak _çok Aç_, robotunuza ilişkin bir soru alanından diğerine.

Nasıl yapıldığını görmek için aşağıdaki tanıtım videosunu izleyin.

[![](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample).

## <a name="what-is-a-multi-turn-conversation"></a>Bir çok Aç konuşma nedir?

Tek bir sırayla bazı soruların yanıtlanması olamaz. İstemci uygulaması (Sohbet Robotu) konuşmalarınızı tasarlarken, bir kullanıcı filtrelenmiş ya da doğru yanıtı belirleyebilmesi iyileştirilmektedir gereken soru sorun. Kullanıcıyla sunarak bu soruları akışla mümkündür **izleme ister**.

Kullanıcı bir soru sorduğunda, soru-cevap Oluşturucu yanıt verir _ve_ herhangi bir izleme istemi. Bu seçenek olarak takip soruları göstermenizi sağlar. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Örnek çok Aç Konuşma ile Sohbet Robotu

Son yanıt belirlemek için soru soru kullanıcı ile görüşme bir sohbet Robotu yönetir.

![Damıtarak konuşma bağlamında kullanılabilen akışın içinde görüşme durumu Konuşmaya devam seçeneği olarak sunulan soruların yanıtlarını içinde uyarılar sağlayarak bir çok Aç iletişim sisteminde yönetin.](../media/conversational-context/conversation-in-bot.png)

Yukarıdaki görüntüde, kullanıcının girdiği `My account`. Bilgi Bankası 3 bağlantılı soru-cevap çiftlerini sahiptir. Yanıt iyileştirmek için üç seçeneklerden birini seçmek kullanıcının olmalıdır. Bilgi Bankası ' (#1) sorunun sohbet Robotu üç seçenek (2) sunulan üç izleme istemleri sahiptir. 

Kullanıcı bir seçim (3) seçtiğinde, ardından İleri iyileştirme seçenekleri (4) listesi sunulur. Bu durum, doğru ve son yanıt (6) belirlenir kadar (5) devam edebilir.

Önceki resimle **etkinleştirmek çok Aç** edebilmek için görüntülenen yönergeleri. 

### <a name="using-multi-turn-in-a-bot"></a>İçinde bir bot çok dönüş kullanma

Bağlamsal konuşma yönetmek için istemci uygulamanızı değiştirmeniz gerekebilir. Eklemeniz gerekecektir [botunuzun kodu](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-prompting) yönergeleri görmek için.  

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Bir belgenin yapısından çok Aç konuşma oluşturma

Bilgi Bankası oluşturduğunuzda, bir isteğe bağlı-çok Aç ayıklama etkinleştirmek için onay kutusunu görürsünüz. 

![Bilgi Bankası oluşturduğunuzda, bir isteğe bağlı-çok Aç ayıklama etkinleştirmek için onay kutusunu görürsünüz.](../media/conversational-context/enable-multi-turn.png)

Bir belge içeri aktarırken bu seçeneği belirlerseniz, çok Aç konuşma yapısından örtük. Bu yapıyı varsa soru-cevap Oluşturucu izleme komut istemi soru-cevap çiftlerini oluşturur. 

Çok Aç yapısı yalnızca nelze odvodit URL'leri, PDF veya DOCX dosyaları. 

Microsoft Surface, aşağıdaki görüntüde [PDF dosyası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf) el ile kullanılmak üzere tasarlanmıştır. Bu PDF dosyasının boyutu nedeniyle, Azure soru-cevap Oluşturucu kaynak fiyatlandırma katmanı b arama gerektirir (15 dizinler) veya daha büyük. 

![! [Bir belge içe aktarırsanız, bağlamsal konuşma yapısından ima. Bu yapıyı varsa soru-cevap Oluşturucu izleme komut istemi soru-cevap çiftlerini belge alma işleminin bir parçası olarak sizin için oluşturur.] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

PDF belgesinin içeri aktarırken, soru-cevap Oluşturucu damıtarak konuşma bağlamında kullanılabilen akışı oluşturmak için yapı izleme istemleri belirler. 

1. İçinde **1. adım**seçin **Bilgi Bankası oluşturma** üst gezinti bölmesinden.
1. İçinde **2. adım**oluşturun veya mevcut bir soru-cevap hizmetini kullanın. Soru-cevap hizmeti B'bir arama hizmeti ile kullandığınızdan emin olun (15 dizinler) veya Surface el ile PDF dosyasının daha küçük bir katman için çok büyük olduğundan daha yüksek.
1. İçinde **3. adım**, gibi Bilgi Bankası için bir ad girin `Surface manual`.
1. İçinde **4. adım**seçin **etkinleştirmek, URL'lerden çok Aç ayıklama .pdf veya .docx dosyaları.** Yüzey el ile için URL'yi seçin

    ```text
    https://github.com/Azure-Samples/cognitive-services-sample-data-files/raw/master/qna-maker/data-source-formats/product-manual.pdf
    ```

1. Seçin **, KB oluşturma** düğmesi. 

    Bilgi Bankası oluşturulduktan sonra soru ve yanıt çiftleri görünümünü görüntüler.

## <a name="show-questions-and-answers-with-context"></a>Sorular ve cevaplar bağlamla Göster

Bağlamsal konuşmaları yalnızca olanlar için görüntülenen soru ve yanıt çiftleri azaltın. 

1. Seçin **görüntüleme seçenekleri**, ardından **Show bağlam (Önizleme)** . Soru ve yanıt çiftleri izleme yönergeleri içeren liste gösterir. 

    ![Soru filtrelemek ve bağlamsal konuşmaları tarafından çiftleri yanıt](../media/conversational-context/filter-question-and-answers-by-context.png)

2. İlk sütunda çok Aç bağlamı görüntüler.

    ![! [PDF belgesinin içeri aktarırken, soru-cevap Oluşturucu damıtarak konuşma bağlamında kullanılabilen akışı oluşturmak için yapı izleme istemleri belirler. ](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

    Önceki görüntüde #1 sütununda, geçerli soru belirten kalın metin gösterir. Üst soru sıradaki ilk öğedir. Aşağıdaki sorular bağlantılı soru ve yanıt çiftleridir. Bu öğeler, diğer bağlam öğeleri hemen dönebilmesi seçilebilir. 

## <a name="add-existing-qna-pair-as-follow-up-prompt"></a>İzleme istemi var olan soru-cevap çifti Ekle

Özgün soru `My account` izleme istemleri gibi sahip `Accounts and signing in`. 

![Özgün soru 'Hesabımdaki' doğru bir şekilde döndürür 'Hesaplar ve oturum açma' yanıt ve bağlı izleme istemleri zaten sahip.](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Bir izleme istemi bağlı olmayan mevcut bir soru-cevap çifti ekleyin. Soru-cevap çiftinde soru bağlı değil çünkü geçerli görünüm ayarını değiştirmesi gerekir.

1. Mevcut bir soru-cevap çift izleme istemi olarak bağlamak için soru ve yanıt çifti için bir satır seçin. Yüzey el ile için arama `Sign out` listenin azaltmak için.
1. Satırında `Signout`seçin **Ekle izleme istemi** gelen **yanıt** sütun.
1. İçinde **izleme istemi (Önizleme)** açılır penceresinde aşağıdakileri girin:

    |Alan|Değer|
    |--|--|
    |Metin görüntüleme|`Turn off the device`. Bu izleme istemi görüntülemeyi özel metindir.|
    |Yalnızca bağlam|Seçili. Sorunun bağlamı belirtiyorsa, bu yanıt yalnızca döndürülür.|
    |Yanıt Bağla|Girin `Use the sign-in screen` var olan soru-cevap çifti bulunamadı.|


1.  Bir eşleşme döndürülmez. Bu yanıt izleme seçip **Kaydet**. 

    ![İzleme istemi ait bağlantı yanıt metni kullanarak yanıt iletişim var olan bir yanıt arayın.](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. İzleme istemi ekledikten sonra seçeneğini belirlemeyi unutmayın **kaydedin ve eğitme** üst gezintideki.
  
### <a name="edit-the-display-text"></a>Görüntülenecek metni Düzenle 

Ne zaman bir izleme istemi oluşturulur ve var olan bir soru-cevap çifti olarak seçilen **yanıt bağlantı**, yeni girebilirsiniz **ekran metni**. Bu metin, var olan soru değiştirmez ve yeni bir alternatif soru eklemez. Bu değerleri ayrıdır. 

1. Görüntü metnini düzenlemek için aramak ve soruda seçmek **bağlam** alan.
1. Bu sorunun satırda izleme istemi yanıt sütunu seçin. 
1. Görünen metni düzenleyin ve ardından seçin **Düzenle**.

    ![Düzenlemek istediğiniz görüntü metnini seçin ve Düzenle'yi seçin.](../media/conversational-context/edit-existing-display-text.png)

1. **İzleme istemi** açılır mevcut görünen metni değiştirme olanak tanır. 
1. İşiniz bittiğinde, görüntülenecek metni düzenleme seçin **Kaydet**. 
1. Seçeneğini belirlemeyi unutmayın **kaydedin ve eğitme** üst gezintideki.


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

## <a name="add-new-qna-pair-as-follow-up-prompt"></a>Yeni soru-cevap çift olarak izleme istemi ekleyin

Yeni bir soru-cevap çift Bilgi Bankası'na ekleyin. Soru-cevap çifti için mevcut bir soru bir izleme istemi bağlantılı olması gerekir.

1. Bilgi Bankası'nın araç çubuğundan aramak ve seçmek için var olan soru-cevap çifti `Accounts and Signing In`. 

1. İçinde **yanıt** bu soruyu seçin için sütun **Ekle izleme istemi**. 
1. **İzleme istemi (Önizleme)** , aşağıdaki değerler girerek yeni bir izleme istemi oluşturun: 

    |Metin alanı|Değer|
    |--|--|
    |**Metin görüntüleme**|`Create a Windows Account`. Bu izleme istemi görüntülemeyi özel metindir.|
    |**Yalnızca bağlam**|Seçili. Sorunun bağlamı belirtiyorsa, bu yanıt yalnızca döndürülür.|
    |**Yanıt için bağlantı**|Yanıt olarak aşağıdaki metni girin:<br>`[Create](https://account.microsoft.com/) a Windows account with a new or existing email account.`<br>Kaydet ve veritabanı eğitmek, bu metin dönüştürülür |
    |||

    ![Yeni komut istemi soru-cevap oluşturma](../media/conversational-context/create-child-prompt-from-parent.png)


1. Seçin **Yeni Oluştur** seçip **Kaydet**. 

    Bu yeni bir soru-cevap çifti oluşturulur ve izleme istemi olarak seçilen sorunun bağlı. **Bağlam** sütununda, her iki sorular için izleme bir komut istemi ilişkileri gösterir. 

1. Değişiklik **görüntüleme seçenekleri** için [bağlam Göster](#show-questions-and-answers-with-context).

    Yeni soru nasıl bağlı gösterir.

    ![Yeni bir izleme istemi oluşturma ](../media/conversational-context/new-qna-follow-up-prompt.png)

    Üst soru yeni soru seçimlerini biri olarak gösterir.

    ![! [Bağlam sütununda, her iki sorular için izleme bir komut istemi ilişkiyi gösterir.] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. İzleme istemi ekledikten sonra seçeneğini belirlemeyi unutmayın **kaydedin ve eğitme** üst gezintideki.

## <a name="enable-multi-turn-when-testing-follow-up-prompts"></a>Test izleme, ister çok Aç'ı etkinleştir

Ne zaman takip sorusu test ister içinde **Test** bölmesinde **etkinleştirmek çok Aç**, sorunuzu girin. Yanıtta izleme yönergeleri içerir.

![Test Bölmesi'nde soruyu test ederken yanıt izleme yönergeleri içerir.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Çok Aç etkinleştirmezseniz, yanıt döndürülür ancak izleme istemleri alınmadı.

## <a name="json-request-to-return-initial-answer-and-follow-up-prompts"></a>İlk yanıt ve istemleri takip döndürülecek JSON isteği

Boş kullanmak `context` kullanıcının sorusuna verilen yanıt istemek ve istemleri takip dahil etmek için nesne. 

```JSON
{
  "question": "accounts and signing in",
  "top": 10,
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
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way he or she likes. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 100.0,
            "id": 15,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    }
                ]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start , and right-click your name. Then select Sign out. ",
            "score": 38.01,
            "id": 18,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Turn off the device"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 27.53,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

`prompts` Dizisi sağlar metinde `displayText` özelliği ve `qnaId` konuşma sonraki görüntülenen seçenek olarak bu yanıtlar göstermek için değer akış, seçili gönderebilirsiniz `qnaId` soru-cevap Oluşturucu aşağıdaki istek geri dön . 

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="json-request-to-return-non-initial-answer-and-follow-up-prompts"></a>JSON istek olmayan ilk yanıt ve istemleri takip döndürmek için

Dolgu `context` önceki bağlam eklenecek nesne.

Aşağıdaki JSON istekte geçerli soru şudur `Use Windows Hello to sign in` ve önceki soruyu `Accounts and signing in`. 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "qnaId": 17,
  "context": {
    "previousQnAId": 15,
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
                "Use Windows Hello to sign in"
            ],
            "answer": "**Use Windows Hello to sign in**\n\nSince Surface Pro 4 has an infrared (IR) camera, you can set up Windows Hello to sign in just by looking at the screen. \n\nIf you have the Surface Pro 4 Type Cover with Fingerprint ID (sold separately), you can set up your Surface sign you in with a touch. \n\nFor more info, see What is Windows Hello? on Windows.com. ",
            "score": 100.0,
            "id": 17,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Meet Surface Pro 4"
            ],
            "answer": "**Meet Surface Pro 4**\n\nGet acquainted with the features built in to your Surface Pro 4. \n\nHere’s a quick overview of Surface Pro 4 features: \n\n\n\n\n\n\n\nPower button \n\n\n\n\n\nPress the power button to turn your Surface Pro 4 on. You can also use the power button to put it to sleep and wake it when you’re ready to start working again. \n\n\n\n\n\n\n\nTouchscreen \n\n\n\n\n\nUse the 12.3” display, with its 3:2 aspect ratio and 2736 x 1824 resolution, to watch HD movies, browse the web, and use your favorite apps. \n\nThe new Surface G5 touch processor provides up to twice the touch accuracy of Surface Pro 3 and lets you use your fingers to select items, zoom in, and move things around. For more info, see Surface touchscreen on Surface.com. \n\n\n\n\n\n\n\nSurface Pen \n\n\n\n\n\nEnjoy a natural writing experience with a pen that feels like an actual pen. Use Surface Pen to launch Cortana in Windows or open OneNote and quickly jot down notes or take screenshots. \n\nSee Using Surface Pen (Surface Pro 4 version) on Surface.com for more info. \n\n\n\n\n\n\n\nKickstand \n\n\n\n\n\nFlip out the kickstand and work or play comfortably at your desk, on the couch, or while giving a hands-free presentation. \n\n\n\n\n\n\n\nWi-Fi and Bluetooth® \n\n\n\n\n\nSurface Pro 4 supports standard Wi-Fi protocols (802.11a/b/g/n/ac) and Bluetooth 4.0. Connect to a wireless network and use Bluetooth devices like mice, printers, and headsets. \n\nFor more info, see Add a Bluetooth device and Connect Surface to a wireless network on Surface.com. \n\n\n\n\n\n\n\nCameras \n\n\n\n\n\nSurface Pro 4 has two cameras for taking photos and recording video: an 8-megapixel rear-facing camera with autofocus and a 5-megapixel, high-resolution, front-facing camera. Both cameras record video in 1080p, with a 16:9 aspect ratio. Privacy lights are located on the right side of both cameras. \n\nSurface Pro 4 also has an infrared (IR) face-detection camera so you can sign in to Windows without typing a password. For more info, see Windows Hello on Surface.com. \n\nFor more camera info, see Take photos and videos with Surface and Using autofocus on Surface 3, Surface Pro 4, and Surface Book on Surface.com. \n\n\n\n\n\n\n\nMicrophones \n\n\n\n\n\nSurface Pro 4 has both a front and a back microphone. Use the front microphone for calls and recordings. Its noise-canceling feature is optimized for use with Skype and Cortana. \n\n\n\n\n\n\n\nStereo speakers \n\n\n\n\n\nStereo front speakers provide an immersive music and movie playback experience. To learn more, see Surface sound, volume, and audio accessories on Surface.com. \n\n\n\n\n",
            "score": 21.92,
            "id": 3,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 4,
                        "qna": null,
                        "displayText": "Ports and connectors"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 19.04,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

## <a name="query-the-knowledge-base-with-the-qna-id"></a>Bilgi Bankası soru-cevap Kimliğine sahip sorgu

İlk sorunun yanıtı, sizden izleme ve onun ilişkili `qnaId` döndürülür. Kimliğine sahip, bu izleme istemi ait istek gövdesinde geçirebilirsiniz. İstek gövdesi içeriyorsa `qnaId`ve (önceki soru-cevap özelliklerini içeren) bağlam nesnesi GenerateAnswer tam soru ve Kimliğe göre sıralama algoritması tarafından Soru metni yanıt bulmak için kullanmak yerine döndürecektir. 

## <a name="displaying-prompts-and-sending-context-in-the-client-application"></a>Komut istemlerini görüntüleme ve istemci uygulamasında bağlamı gönderme 

Bilgi Bankası'ndaki yönergeleri eklediniz ve akış test Bölmesi'nde test. Artık istemci uygulamasında bu yönergeleri kullanmanız gerekir. Bot Framework için istemleri otomatik olarak istemci uygulamalarında görünmeye başlar. Size istemleri önerilen eylemleri veya düğme olarak kullanıcının sorgu yanıtı bir parçası olarak istemci uygulamaları dahil ederek gösterebilir [Bot Framework örnek](https://aka.ms/qnamakermultiturnsample) kodunuzda. İstemci uygulaması geçerli soru-cevap kimliği ve kullanıcı sorgusu depolamak ve bunları geçmesi [GenerateAnswer API bağlam nesnesinin](#json-request-to-return-non-initial-answer-and-follow-up-prompts) ileri kullanıcı sorgusu için. 

## <a name="display-order-supported-in-api"></a>Desteklenen API görüntülenme sırasını

[Metni görüntülemek ve görüntüleme sırası](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto), JSON yanıtta döndürülen, tarafından düzenleme için desteklenen [güncelleştirme API'si](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). 

<!--

FIX - Need to go to parent, then answer column, then edit answer. 

-->

## <a name="create-knowledge-base-with-multi-turn-prompts-with-the-create-api"></a>Bilgi Bankası çok bırakma yönergeleri ile oluşturmak API ile oluşturun.

İle çok bırakma yönergeleri kullanarak bir Bilgi Bankası çalışması oluşturabilirsiniz [soru-cevap Oluşturucu API'si oluşturma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create). Komut istemlerini eklemekte olduğunuz `context` özelliğin `prompts` dizisi. 


## <a name="add-or-delete-multi-turn-prompts-with-the-update-api"></a>Ekleme veya güncelleştirme API'si ile çok Aç istemleri silme

Ekleme veya silme çok bırakma yönergeleri kullanarak [soru-cevap Oluşturucu güncelleştirme API'si](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update).  Komut istemlerini eklemekte olduğunuz `context` özelliğin `promptsToAdd` dizi ve `promptsToDelete` dizisi. 


## <a name="next-steps"></a>Sonraki adımlar

Gelen bağlamsal konuşmalara hakkında daha fazla bilgi [iletişim örnek](https://aka.ms/qnamakermultiturnsample) veya daha fazla bilgi edinin [kavramsal bot tasarım çok Aç konuşmaları](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Bilgi Bankası geçirme](../Tutorials/migrate-knowledge-base.md)
