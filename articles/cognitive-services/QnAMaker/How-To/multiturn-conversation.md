---
title: Çok Aç konuşmaları
titleSuffix: Azure Cognitive Services
description: Bağlam ve istekleri robotunuza ilişkin bir soru alanından diğerine çok dönüş olarak bilinir, birden çok kapatır yönetmek için kullanın. Sonraki Soru ve yanıt önceki sorusunun bağlam burada etkiler geri yönlü konuşma sahip olma yeteneği çok sıranız.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/26/2019
ms.author: diberry
ms.openlocfilehash: 10249375922b47a40f71a60938cdd12ffe0f9b54
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508148"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Birden çok kapatır konuşmanın oluşturmak için istemleri takip kullanın

Bağlam ve izleme istekleri olarak bilinen birden çok kapatır yönetmek için kullanmak _çok Aç_, robotunuza ilişkin bir soru alanından diğerine.

Çok Aç nasıl çalıştığını görmek için aşağıdaki tanıtım videosunu görüntüleyin:

[![Soru-cevap Oluşturucu konuşmada çok Aç](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample)

## <a name="what-is-a-multi-turn-conversation"></a>Bir çok Aç konuşma nedir?

Tek bir sırayla bazı soruların yanıtlanması olamaz. İstemci uygulaması (Sohbet Robotu) konuşmalarınızı tasarlarken, bir kullanıcı filtrelenmiş ya da doğru yanıtı belirlemek için iyileştirilmiştir gereken soru sorun. Bu akış aracılığıyla soruları kullanıcıyla sunarak olası yaptığınız *izleme ister*.

Bir kullanıcı bir soru sorduğunda, soru-cevap Oluşturucu yanıt verir _ve_ herhangi bir izleme istemi. Bu yanıt seçenek olarak takip soruları göstermenizi sağlar. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Örnek çok Aç Konuşma ile Sohbet Robotu

Çok bırakma ile Sohbet Robotu aşağıdaki görüntüde gösterildiği gibi son yanıt belirlemek için bir kullanıcı ile görüşme yönetir:

![Bir kullanıcının konuşma aracılığıyla Kılavuzu yönergeleri içeren bir çok Aç iletişim kutusu](../media/conversational-context/conversation-in-bot.png)

Önceki görüntüde girerek bir kullanıcı bir konuşma başlayıp **Hesabımı**. Bilgi Bankası üç bağlantılı soru-cevap çiftlerini sahiptir. Yanıt daraltmak için kullanıcı seçeneklerden biri üç Bilgi Bankası'ndaki seçer. Sohbet Robotu üç seçenekten (2) sunulan üç izleme istemleri (#1) sorunun var. 

Kullanıcı bir seçenek (3) seçtiğinde, İleri iyileştirme seçenekleri (4) listesi gösterilir. Kullanıcı doğru son (6) yanıt belirler kadar bu dizisi (5) devam eder.

> [!NOTE]
> Yukarıdaki görüntüde, **etkinleştirmek çok Aç** istemleri görüntülendiğinden emin olmak için onay kutusunun seçilmiş. 

### <a name="use-multi-turn-in-a-bot"></a>Bir bot çok sırayla kullanın

Bağlamsal konuşma yönetmek için istemci uygulamanız tarafından değiştirme [botunuz için kod ekleme](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-prompting). Kod ekleme yönergeleri görmelerini sağlar.  

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Bir belgenin yapısından çok Aç konuşma oluşturma

Bir Bilgi Bankası oluşturduğunuzda **, KB doldurmak** bölümünde görüntüler bir **etkinleştirmek, URL'lerden çok Aç ayıklama .pdf veya .docx dosyaları** onay kutusu. 

![Çok Aç ayıklama etkinleştirmek için onay kutusu](../media/conversational-context/enable-multi-turn.png)

İçeri aktarılan bir belge için bu seçeneği belirlediğinizde, çok Aç konuşma belge yapısından örtük. Bu yapıyı varsa soru-cevap Oluşturucu izleme istemi bu çiftleri sorularını ve yanıtlarını içeri aktarma işleminin bir parçası olarak oluşturur. 

Çok Aç yapısı yalnızca URL'lerden, PDF dosyaları, çıkarılan veya DOCX dosyaları. Yapısı örneği için görüntüyü bir [Microsoft Surface kullanıcı el ile PDF dosyası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf). Bu PDF dosyasının boyutu nedeniyle, soru-cevap Oluşturucu kaynak gerektiren bir **arama fiyatlandırma katmanı** , **B** (15 dizinler) veya daha büyük. 

![! [Kullanıcı kılavuzuna yapısında örneği] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

PDF belgesinin içeri aktardığınızda, soru-cevap Oluşturucu, izleme yapısından damıtarak konuşma bağlamında kullanılabilen akışı oluşturmak ister belirler. 

1. Soru-cevap oluşturucu içinde seçin **Bilgi Bankası oluşturma**.
1. Oluşturun veya mevcut bir soru-cevap Oluşturucu hizmetini kullanın. PDF dosyasının daha küçük bir katman için çok büyük olduğundan, Microsoft Surface önceki örnekte soru-cevap Oluşturucu hizmeti ile kullanın. bir **arama hizmetinizi** , **B** (15 dizinler) veya daha büyük.
1. Gibi Bilgi Bankası için bir ad girin **yüzey el ile**.
1. Seçin **etkinleştirmek, URL'lerden çok Aç ayıklama .pdf veya .docx dosyaları** onay kutusu. 
1. Yüzey el ile URL'yi seçin **https://github.com/Azure-Samples/cognitive-services-sample-data-files/raw/master/qna-maker/data-source-formats/product-manual.pdf** .

1. Seçin **, KB oluşturma** düğmesi. 

    Bilgi Bankası oluşturulduktan sonra soru-cevap çiftlerini görünümü görüntülenir.

## <a name="show-questions-and-answers-with-context"></a>Sorular ve cevaplar bağlamla Göster

Bağlamsal konuşmaları yalnızca olanlar için görüntülenen soru-cevap çiftlerini azaltın. 

Seçin **görüntüleme seçenekleri**ve ardından **Show bağlam (Önizleme)** . İzleme komut istemlerini içeren soru-cevap çiftlerini listesini görüntüler. 

![Soru-cevap çiftlerini bağlamsal konuşmaları göre filtrele](../media/conversational-context/filter-question-and-answers-by-context.png)

Çok Aç bağlam ilk sütunda görüntülenir.

![! ["Bağlam (Önizleme)" sütun] (.. / media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

Yukarıdaki görüntüde, **#1** sütununda, geçerli soru belirten kalın metin gösterir. Üst soru sıradaki ilk öğedir. Aşağıdaki sorular bağlantılı soru-cevap çiftleridir. Diğer bağlam öğeleri hemen gidebilirsiniz bu öğeleri seçilebilir, içindir. 

## <a name="add-an-existing-question-and-answer-pair-as-a-follow-up-prompt"></a>Bir izleme istemi var olan bir soru-cevap çifti Ekle

Özgün soru **Hesabımı**, izleme istemleri gibi sahip **hesaplar ve oturum açma**. 

!["Hesaplar ve oturum açma" yanıtlar ve izleme istemleri](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Bir izleme istemi bağlı olmayan mevcut bir soru-cevap çifti ekleyin. Soru-cevap çiftinde soru bağlı değil çünkü geçerli görünüm ayarını değiştirilmesi gerekir.

1. Mevcut bir soru-cevap çift izleme istemi olarak bağlamak için soru-cevap çifti için bir satır seçin. Yüzey el ile için arama **oturumunuzu** listenin azaltmak için.
1. Satırında **Signout**, **yanıt** sütunundaki **Ekle izleme istemi**.
1. Alanları **izleme istemi (Önizleme)** açılır penceresinde, aşağıdaki değerleri girin:

    |Alan|Değer|
    |--|--|
    |Metin görüntüleme|Girin **cihazı kapatıp**. Bu izleme istemi görüntülenecek özel metindir.|
    |Yalnızca bağlam| Bu onay kutusunu seçin. Sorunun bağlamı belirtiyorsa bir yanıt döndürülür.|
    |Yanıt için bağlantı|Girin **oturum açma ekranı kullanın** mevcut soru/yanıt çifti bulunamadı.|


1.  Bir eşleşme döndürülmez. Bu yanıt bir izleme seçin ve ardından **Kaydet**. 

    !["Takip istemi (Önizleme)" sayfası](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. İzleme istemi ekledikten sonra seçin **kaydedin ve eğitme** üst gezintideki.
  
### <a name="edit-the-display-text"></a>Görüntülenecek metni Düzenle 

Ne zaman bir izleme istemi oluşturulur ve var olan bir soru-cevap çifti olarak girilir **yanıt bağlantı**, yeni girebilirsiniz **ekran metni**. Bu metin, var olan soru yerini almaz ve yeni bir alternatif soru eklemez. Bu değerleri ayrıdır. 

1. Görüntü metnini düzenlemek için aramak ve soruda seçmek **bağlam** alan.
1. Bu soruyu satır izleme istemine yanıt sütun seçin. 
1. Görünen metni düzenleyin ve ardından istediğiniz seçin **Düzenle**.

    ![Düzenle komutu için görüntülenecek metin](../media/conversational-context/edit-existing-display-text.png)

1. İçinde **izleme istemi** açılır pencerede görünen metni değiştirme. 
1. Bitirdiğinizde, görüntülenecek metni düzenleme seçin **Kaydet**. 
1. Üst gezinti çubuğunda **kaydedin ve eğitme**.


<!--

## To find the best prompt answer, add metadata to follow-up prompts 

If you have several follow-up prompts for a specific question-and-answer pair but you know, as the knowledge base manager, that not all prompts should be returned, use metadata to categorize the prompts in the knowledge base. You can then send the metadata from the client application as part of the GenerateAnswer request.

In the knowledge base, when a question-and-answer pair is linked to follow-up prompts, the metadata filters are applied first, and then the follow-ups are returned.

1. Add metadata to each of the two follow-up question-and-answer pairs:

    |Question|Add metadata|
    |--|--|
    |*Feedback on a QnA Maker service*|"Feature":"all"|
    |*Feedback on an existing feature*|"Feature":"one"|
    
    ![The "Metadata tags" column for adding metadata to a follow-up prompt](../media/conversational-context/add-metadata-feature-to-follow-up-prompt.png) 

1. Select **Save and train**. 

    When you send the question **Give feedback** with the metadata filter **Feature** with a value of **all**, only the question-and-answer pair with that metadata is returned. QnA Maker doesn't return both question-and-answer pairs, because both don't match the filter. 

-->

## <a name="add-a-new-question-and-answer-pair-as-a-follow-up-prompt"></a>Bir izleme istemi yeni bir soru-cevap çifti Ekle

Bilgi Bankası'na yeni bir soru-cevap çift eklediğinizde, her bir çifti için mevcut bir soru bir izleme istemi bağlantılı olması gerekir.

1. Bilgi Bankası araç çubuğunda aramak ve seçmek için mevcut soru/yanıt çifti **hesaplar ve oturum açma**. 

1. İçinde **yanıt** bu soruyu seçin için sütun **Ekle izleme istemi**. 
1. Altında **izleme istemi (Önizleme)** , aşağıdaki değerler girerek yeni bir izleme istemi oluşturun: 

    |Alan|Değer|
    |--|--|
    |Metin görüntüleme|*Bir Windows hesabı oluşturma*. İzleme istemi görüntülenecek özel metin.|
    |Yalnızca bağlam|Bu onay kutusunu seçin. Sorunun bağlamı belirtiyorsa, bu yanıt döndürülür.|
    |Yanıt için bağlantı|Yanıt olarak aşağıdaki metni girin:<br>*[Oluşturma](https://account.microsoft.com/) yeni veya var olan e-posta hesabı olan bir Windows hesabı*.<br>Bu metin, kaydedin ve veritabanı eğitmek, dönüştürülür. |
    |||

    ![Yeni bir komut istemi soru ve yanıt oluşturma](../media/conversational-context/create-child-prompt-from-parent.png)


1. Seçin **Yeni Oluştur**ve ardından **Kaydet**. 

    Bu eylem, bir yeni soru-cevap eşleştirme ve bağlantıları seçilen sorunun bir izleme istemi oluşturur. **Bağlam** sütununda, her iki sorular için izleme bir komut istemi ilişkileri gösterir. 

1. Seçin **görüntüleme seçenekleri**ve ardından [ **Show bağlam (Önizleme)** ](#show-questions-and-answers-with-context).

    Yeni soru nasıl bağlı gösterir.

    ![Yeni bir izleme istemi oluşturma](../media/conversational-context/new-qna-follow-up-prompt.png)

    Yeni bir soru üst soru seçimlerini biri olarak görüntüler.

    ![! [Bağlam sütunu, her iki sorular için izleme bir komut istemi ilişki gösterir] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. İzleme istemi ekledikten sonra seçin **kaydedin ve eğitme** üst gezinti çubuğunda.

## <a name="enable-multi-turn-during-testing-of-follow-up-prompts"></a>İzleme istekleri test sırasında çok Aç'ı etkinleştir

Sorunun izleme ile test ettiğinizde, ister de **Test** bölmesinde **etkinleştirmek çok Aç**, sorunuzu girin. Yanıtta izleme yönergeleri içerir.

![Yanıt izleme yönergeleri içeriyor](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Çok Aç etkinleştirmezseniz, yanıt döndürülür ancak izleme istemleri alınmadı.

## <a name="a-json-request-to-return-an-initial-answer-and-follow-up-prompts"></a>İlk yanıt ve istemleri takip döndürülecek JSON isteği

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

## <a name="a-json-response-to-return-an-initial-answer-and-follow-up-prompts"></a>İlk yanıt ve istemleri takip döndürmek için bir JSON yanıtı

Önceki bölümde yanıt ve izleme sizden istenen **hesaplar ve oturum açma**. Yanıt şu konumdadır istemi bilgileri içeriyor *yanıtlar [0] .context*ve kullanıcıya görüntülenecek metin. 

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
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start, and right-click your name. Then select Sign out. ",
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

`prompts` Dizisi sağlar metinde `displayText` özelliği ve `qnaId` değeri. Konuşma içindeki sonraki görüntülenen seçimler akış ve daha sonra seçilen göndermek gibi bu yanıtlar gösterebilirsiniz `qnaId` soru-cevap Oluşturucu aşağıdaki istek dönün. 

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts"></a>JSON isteği olmayan ilk yanıt ve istemleri takip döndürür

Dolgu `context` önceki bağlam eklenecek nesne.

Aşağıdaki JSON istekte geçerli soru şudur *kullanım Windows oturum açmak için Hello* ve önceki soruyu *hesaplar ve oturum açma*. 

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

##  <a name="a-json-response-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Olmayan ilk yanıt ve istemleri takip döndürmek için bir JSON yanıtı

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

## <a name="query-the-knowledge-base-with-the-qna-maker-id"></a>Soru-cevap Oluşturucu Kimliği ile Bilgi Bankası sorgulama

İlk sorunun yanıtı, sizden izleme ve onun ilişkili `qnaId` döndürülür. Kimliğine sahip, bu izleme istemi ait istek gövdesinde geçirebilirsiniz. İstek gövdesi içeriyorsa `qnaId`ve (önceki soru-cevap Oluşturucu özelliklerini içeren) bağlam nesnesi GenerateAnswer tam soru ve Kimliğe göre sıralama algoritması tarafından Soru metni yanıt bulmak için kullanmak yerine döndürecektir. 

## <a name="display-prompts-and-send-context-in-the-client-application"></a>Görüntüler ve istemci uygulamasında bağlamı gönderme 

Bilgi Bankası'ndaki yönergeleri eklediniz ve akış test Bölmesi'nde test. Artık istemci uygulamasında bu yönergeleri kullanmanız gerekir. Bot Framework yönergeleri istemci uygulamalarında otomatik olarak görüntülenmez. Komut istemlerini önerilen eylemleri veya düğme olarak istemci uygulamalarında kullanıcının sorgu yanıtı bir parçası olarak dahil ederek görüntüleyebileceğiniz [Bot Framework örnek](https://aka.ms/qnamakermultiturnsample) kodunuzda. İstemci uygulaması geçerli soru-cevap Oluşturucu Kimliği ve kullanıcı sorgusu depolamak ve bunları geçmesi [GenerateAnswer API bağlam nesnesinin](#a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts) ileri kullanıcı sorgusu için. 

## <a name="display-order-is-supported-in-the-update-api"></a>Görüntülenme sırasını güncelleştirmesi API'de desteklenir

[Metni görüntülemek ve görüntüleme sırası](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto), JSON yanıtta döndürülen, tarafından düzenleme için desteklenen [güncelleştirme API'si](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). 

<!--

FIX - Need to go to parent, then answer column, then edit answer. 

-->

## <a name="create-knowledge-base-with-multi-turn-prompts-with-the-create-api"></a>Bilgi Bankası çok bırakma yönergeleri ile oluşturmak API ile oluşturun.

İle çok bırakma yönergeleri kullanarak bir Bilgi Bankası çalışması oluşturabilirsiniz [soru-cevap Oluşturucu API'si oluşturma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create). Komut istemlerini eklemekte olduğunuz `context` özelliğin `prompts` dizisi. 


## <a name="add-or-delete-multi-turn-prompts-with-the-update-api"></a>Ekleme veya güncelleştirme API'si ile çok Aç istemleri silme

Ekleme veya silme çok bırakma yönergeleri kullanarak [soru-cevap Oluşturucu güncelleştirme API'si](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update).  Komut istemlerini eklemekte olduğunuz `context` özelliğin `promptsToAdd` dizi ve `promptsToDelete` dizisi. 


## <a name="next-steps"></a>Sonraki adımlar

Bu bağlamsal görüşmeler hakkında daha fazla bilgi [iletişim örnek](https://aka.ms/qnamakermultiturnsample) veya daha fazla bilgi edinin [kavramsal bot tasarım çok Aç konuşmaları](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Bilgi Bankası geçirme](../Tutorials/migrate-knowledge-base.md)
