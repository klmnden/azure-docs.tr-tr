---
title: Azure robot hizmeti - soru-cevap Oluşturucu ile soru-cevap Robotu
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 74c7bc5c601cd36a8dd2454506745406bc00dac0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031297"
---
# <a name="create-a-qna-bot-with-azure-bot-service-v3"></a>Azure ile soru-cevap Robotu oluşturun Bot hizmeti v3
Bu öğretici Azure portalında Azure Bot hizmeti v3 ile soru-cevap bot oluşturma sürecinde kılavuzluk eder.

## <a name="prerequisite"></a>Önkoşul
Derlemeden önce adımları [Bilgi Bankası oluşturma](../How-To/create-knowledge-base.md) sorular ve cevaplar ile soru-cevap Oluşturucu hizmetini oluşturmak için.

Bot QnAMakerDialog oluşturulan Bilgi Bankası gelen sorulara yanıt verir.

## <a name="create-a-qna-bot"></a>Soru-cevap Robotu oluşturun
1. İçinde [Azure portalında](https://portal.azure.com)seçin **Oluştur** yeni bir kaynak seçin ve menü dikey **tümünü gör**.

    ![bot hizmeti oluşturma](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Arama kutusuna arama **Web App Botu**.

    ![bot hizmeti seçimi](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. İçinde **Bot hizmeti dikey**, gerekli bilgileri sağlayın:

    - Ayarlama **uygulama adı** botunuzun kişinin adı. Botunuzun (örneğin, mynotesbot.azurewebsites.net) buluta dağıtıldığında alt etki alanı adı kullanılır.
    - Abonelik, kaynak grubu, App service planı ve konumu seçin.

4. Soru-cevap Robotu ile SDK v4 oluşturmaya yönelik yönergelere bakın - görmek için [soru-cevap v4 bot şablon](https://aka.ms/qna-bot-v4). V3 şablonlarını kullanmak için SDK'sı sürümünü seçin **SDK v3** ve SDK'sı dilini **C#** veya **Node.js**.

    ![bot sdk ayarları](../media/qnamaker-tutorials-create-bot/bot-v3.png)

5. Seçin **soru ve yanıt** şablonu için robot şablon alanını, ardından seçerek şablonu ayarlarını kaydedin **seçin**.

    ![bot hizmeti seçimi](../media/qnamaker-tutorials-create-bot/bot-v3-template.png)

6. Ayarlarınızı gözden geçirin ve ardından **Oluştur**. Bu, oluşturur ve QnAMakerDialog ile bot hizmeti, Azure'a dağıtır.

    ![bot hizmeti seçimi](../media/qnamaker-tutorials-create-bot/bot-blade-settings-v3.png)

7. Bot hizmeti dağıtıldığını doğrulayın.

    - Seçin **bildirimleri** (Azure portalının üst kenarı bulunan zil simgesi). Bildirim değiştirilecek **dağıtım başlatıldı** için **dağıtım başarılı**.
    - Bildirim değişiklikler yapıldıktan sonra **dağıtım başarılı**seçin **kaynağa Git** bu bildirim.

## <a name="chat-with-the-bot"></a>Bot ile sohbet edin
Seçme **kaynağa Git** botun kaynak dikey penceresine götürür.

Bot kaydedildikten sonra tıklayın **Test Web sohbeti** Web Chat bölmesinde açmak için. "Web Chat hello" yazın.

![Soru-cevap Robotu web Sohbeti](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

Bot yanıt veren "Lütfen QnAKnowledgebaseId ve QnASubscriptionKey uygulama ayarlarında kümesi. Onları almayı öğrenme https://aka.ms/qnaabssetup". Bu yanıt, soru-cevap Robotu bir ileti aldı ancak henüz ilişkili soru-cevap Oluşturucu Bilgi Bankası olduğunu onaylar. Sonraki adımda bunu.

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Soru-cevap Oluşturucu Bankası için robot bağlanma

1. Açık **uygulama ayarları** ve düzenleme **QnAKnowledgebaseId**, **QnAAuthKey**ve **QnAEndpointHostName** içeren alanlar soru-cevap Oluşturucu bankanızı değerleri.

    ![Uygulama ayarları](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

2. Bilgi Bankası'ndaki Ayarlar sekmesinde, Bilgi Bankası kimliği, ana bilgisayar URL'si ve uç noktası anahtarı alın https://qnamaker.ai.
    - Oturum [soru-cevap Oluşturucu](https://qnamaker.ai)
    - Bilgi Bankası'na gidin
    - Tıklayarak **ayarları** sekmesi
    - **Yayımlama** bunu yapmadıysanız bilgi bankanızı,

    ![Soru-cevap Oluşturucu değerleri](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Bilgi Bankası Önizleme sürümü ile soru-cevap Robotu bağlanmak istiyorsanız, değerini ayarlamak **Ocp-Apim-Subscription-Key** için **QnAAuthKey**. Bırakın **QnAEndpointHostName** boş.

## <a name="test-the-bot"></a>Bot test
Azure portalında tıklayarak **Test Web sohbeti içinde** bot test etmek için. 

![Soru-cevap Oluşturucu Robotu](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

Soru-cevap Botunuzun, Bilgi Bankası artık yanıt verir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap oluşturucu ve LUIS tümleştirin](./integrate-qnamaker-luis.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bilgi bankanızı yönetme](https://qnamaker.ai)
- [Farklı kanallar botunuzun insana](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
