---
title: QnA bot Azure Bot hizmet - Azure Bilişsel hizmetler oluşturma | Microsoft Docs
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: fc430bf3aa7cad279d7a93bb6892aa19abee3378
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109278"
---
# <a name="create-a-qna-bot-with-azure-bot-service"></a>Azure Bot hizmetiyle QnA Bot oluşturma
Bu öğreticide, Azure Portal'da Azure Bot hizmetiyle QnA bot oluşturma sürecinde açıklanmaktadır.

## <a name="prerequisite"></a>Önkoşul
Oluşturmadan önce adımları [Bilgi Bankası oluşturun](../How-To/create-knowledge-base.md) sorular ve yanıtlar QnA Maker bir hizmet oluşturmak için.

Bot QnAMakerDialog oluşturulan Bilgi Bankası gelen sorulara yanıt verir.

## <a name="create-a-qna-bot"></a>QnA Bot oluşturma
1. İçinde [Azure portal](https://portal.azure.com)seçin **oluşturma** menü dikey ve ardından yeni bir kaynak **tümünü görmek**.

    ![bot hizmet oluşturma](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Arama kutusuna arama **Web uygulaması Bot**.

    ![bot hizmet seçimi](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. İçinde **Bot hizmeti dikey**, gerekli bilgileri sağlayın ve seçin **oluşturma**. Bu oluşturur ve Azure'a QnAMakerDialog bot hizmetiyle dağıtır.

    - Ayarlama **uygulama adı** bot kişinin adı. Bot (örneğin, mynotesbot.azurewebsites.net) buluta dağıtıldığında alt etki alanı adı kullanılır.
    - Abonelik, kaynak grubu, uygulama hizmeti planı ve konumu seçin.
    - Seçin **soru ve yanıt** Bot şablon alanını (Node.js veya C#) şablonu.
    - Yasal bildirim için onay kutusunu seçin. Yasal bildirim koşullarını onay kutusu var.

        ![bot hizmet seçimi](../media/qnamaker-tutorials-create-bot/bot-service-qna-template.PNG)

4. Bot hizmet dağıtıldığını doğrulayın.

    - Seçin **bildirimleri** (Azure portalı üst kenarına bulunan zil simgesine). Bildirim değiştirilecek **dağıtım başladı** için **dağıtım başarılı**.
    - Bildirim değişiklikler yapıldıktan sonra **dağıtım başarılı**seçin **kaynağa gidin** bu bildirim.

## <a name="chat-with-the-bot"></a>Bot ile Sohbet
Seçme **kaynağa gidin** bot ait kaynak dikey penceresine yönlendirilirsiniz.

Bot kaydedildiğinde, tıklatın **Web sohbeti testinde** Web sohbet bölmesini açmak için. "Web sohbet hello" yazın.

![QnA bot web Sohbeti](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

Bot "Lütfen QnAKnowledgebaseId ve QnASubscriptionKey uygulama ayarları ayarlamanız ile yanıt verir. Onları alma hakkında bilgi https://aka.ms/qnaabssetup". Bu yanıt QnA Bot bir ileti aldı, ancak hiçbir QnA Maker henüz ilişkili Bilgi Bankası yok onaylar. Sonraki adımda bunu.

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Bot QnA Maker Bilgi Bankası Bağlan

1. Açık **uygulama ayarları** ve düzenleme **QnAKnowledgebaseId**, **QnAAuthKey**ve **QnAEndpointHostName** içeren alanlar QnA Maker Bilgi Bankası değerleri.

    ![Uygulama ayarları](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

2. Bilgi Bankası'nda Ayarlar sekmesinde, Bilgi Bankası kimliği, ana bilgisayar URL'si ve uç noktası anahtarı almanız https://qnamaker.ai.
    - Oturum [QnA Maker](https://qnamaker.ai)
    - Bilgi Bankası'na gidin
    - Tıklayın **ayarları** sekmesi
    - **Yayımlama** , bunu yapmadıysanız Bilgi Bankası,

    ![QnA Maker değerlerin](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Bilgi Bankası Önizleme sürümü ile QnA bot bağlanmak isterseniz değerini ayarlamak **Apim abonelik anahtar Ocp** için **QnAAuthKey**. Bırakın **QnAEndpointHostName** boş.

## <a name="test-the-bot"></a>Bot test
Azure portalında tıklayın **Test Web sohbet** bot test etmek için. 

![QnA Maker bot](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

QnA Bot, Bilgi Bankası artık yanıtlar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnA Maker ve HALUK tümleştirme](./integrate-qnamaker-luis.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bilgi Bankası yönetme](https://qnamaker.ai)
- [Farklı kanaldaki, bot etkinleştir](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
