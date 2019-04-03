---
title: Soru-cevap Robotu - Azure robot hizmeti - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Bu öğretici Azure portalında Azure Bot hizmeti v3 ile soru-cevap bot oluşturma sürecinde kılavuzluk eder.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/02/2019
ms.author: tulasim
ms.openlocfilehash: 218103f2c75ec1016a997c259767ccd011191fab
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879620"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v3"></a>Öğretici: Azure ile soru-cevap Robotu oluşturun Bot hizmeti v3

Bu öğreticide, Azure robot hizmeti v3 ile bir soru-cevap Robotu oluştururken size yol gösteren [Azure portalında](https://portal.azure.com) herhangi bir kod yazmadan. Yayımlanmış bir Bilgi Bankası (KB) bağlanan bir bot için robot uygulama ayarlarını değiştirme olarak kadar kolaydır. 

> [!Note] 
> Bu konuda Bot SDK'sı için 3 sürümüdür. Sürüm 4 bulabilirsiniz [burada](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs). 

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Soru-cevap Oluşturucu şablonu ile bir Azure Bot hizmeti oluşturma
> * Kod çalışıp çalışmadığını denetlemek için bot ile sohbet edin 
> * İçin robot, yayımlanan KB bağlanma
> * Bir soru ile bot test

Bu makale için serbest soru-cevap Oluşturucu kullanabilirsiniz [hizmet](../how-to/set-up-qnamaker-service-azure.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için yayımlanan Bilgi Bankası olması gerekir. Biri yoksa adımları [Bilgi Bankası oluşturma](../How-To/create-knowledge-base.md) sorular ve cevaplar ile soru-cevap Oluşturucu hizmetini oluşturmak için.

## <a name="create-a-qna-bot"></a>Soru-cevap Robotu oluşturun

1. Azure portalda **Kaynak oluştur**’u seçin.

    ![bot hizmeti oluşturma](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Arama kutusuna arama **Web App Botu**.

    ![bot hizmeti seçimi](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. **Robot Hizmeti**'nde gerekli bilgileri sağlayın:

    - Ayarlama **uygulama adı** botunuzun kişinin adı. Botunuzun (örneğin, mynotesbot.azurewebsites.net) buluta dağıtıldığında alt etki alanı adı kullanılır.
    - Abonelik, kaynak grubu, App service planı ve konumu seçin.

4. V3 şablonlarını kullanmak için SDK'sı sürümünü seçin **SDK v3** ve SDK'sı dilini **C#** veya **Node.js**.

    ![bot sdk ayarları](../media/qnamaker-tutorials-create-bot/bot-v3.png)

5. Seçin **soru ve yanıt** şablonu için robot şablon alanını, ardından seçerek şablonu ayarlarını kaydedin **seçin**.

    ![bot hizmeti şablonunu Seçimi Kaydet](../media/qnamaker-tutorials-create-bot/bot-v3-template.png)

6. Ayarlarınızı gözden geçirin ve ardından **Oluştur**. Bu, oluşturur ve bot hizmeti ile Azure'a dağıtır.

    ![bot oluşturma](../media/qnamaker-tutorials-create-bot/bot-blade-settings-v3.png)

7. Bot hizmeti dağıtıldığını doğrulayın.

    - Seçin **bildirimleri** (Azure portalının üst kenarı bulunan zil simgesi). Bildirim değiştirilecek **dağıtım başlatıldı** için **dağıtım başarılı**.
    - Bildirim değişiklikler yapıldıktan sonra **dağıtım başarılı**seçin **kaynağa Git** bu bildirim.

## <a name="chat-with-the-bot"></a>Bot ile sohbet edin

Seçme **kaynağa Git** botun kaynağı alır.

Seçin **Test Web sohbeti** Web Chat bölmesinde açmak için. "Hi" Web Chat içinde yazın.

![Soru-cevap Robotu web Sohbeti](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

Bot yanıt veren "Lütfen QnAKnowledgebaseId ve QnASubscriptionKey uygulama ayarlarında kümesi. Bu yanıt, soru-cevap Robotu bir ileti aldı ancak henüz ilişkili soru-cevap Oluşturucu Bilgi Bankası olduğunu onaylar. 

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Soru-cevap Oluşturucu Bankası için robot bağlanma

1. Açık **uygulama ayarları** ve düzenleme **QnAKnowledgebaseId**, **QnAAuthKey**ve **QnAEndpointHostName** içeren alanlar soru-cevap Oluşturucu bankanızı değerleri.

    ![Uygulama ayarları](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

1. Soru-cevap Oluşturucu Portalı'nda, Bilgi Bankası Ayarlar sekmesinde, Bilgi Bankası kimliği, ana bilgisayar URL'si ve uç noktası anahtarı alın.

   - Oturum [soru-cevap Oluşturucu](https://qnamaker.ai)
   - Bilgi Bankası'na gidin
   - Seçin **ayarları** sekmesi
   - **Yayımlama** bunu yapmadıysanız bilgi bankanızı,

     ![Soru-cevap Oluşturucu değerleri](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

## <a name="test-the-bot"></a>Bot test

Azure portalında **Test Web sohbeti içinde** bot test etmek için. 

![Soru-cevap Oluşturucu Robotu](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

Soru-cevap Botunuzun, Bilgi Bankası yanıtlar.

## <a name="related-to-qna-maker-bots"></a>Soru-cevap Oluşturucu botlar için ilgili

* Soru-cevap Oluşturucu Portalı'nda kullanılan soru-cevap Oluşturucu Yardım bot olarak kullanılabilir bir [bot örnek](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-support-bot).
    ![Soru-cevap Oluşturucu Yardım bot kırmızı robot simgedir](../media/qnamaker-tutorials-create-bot/answer-bot-icon.PNG)
* [Sağlık Hizmeti botlarınız](https://docs.microsoft.com/HealthBot/qna_model_howto) soru-cevap Oluşturucu birini kullanın. kendi [dil modellerini](https://docs.microsoft.com/HealthBot/qna_model_howto).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin bot ile işiniz bittiğinde, Azure portalında bot kaldırın. Bot hizmetleri şunlardır:

* App Service planı
* Arama hizmeti
* Bilişsel hizmet
* App service
* İsteğe bağlı olarak, bu da application ınsights hizmeti ve depolama için application ınsights verilerini içerebilir

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kavramı: Bilgi Bankası](../concepts/knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bilgi bankanızı yönetme](https://qnamaker.ai)
- [Farklı kanallar botunuzun insana](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
