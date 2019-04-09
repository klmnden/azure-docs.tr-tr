---
title: Soru-cevap Robotu - Azure robot hizmeti - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Yayımlama sayfasında var olan bir Bilgi Bankası için soru-cevap sohbet Robotu oluşturun. Bu bot, Bot Framework SDK v4 kullanır. Bot oluşturmak için kod yazmanız gerekmez, tüm kod size sağlanır.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/08/2019
ms.author: tulasim
ms.openlocfilehash: 85b0004288a06a834b61f6e3d50017d35d66ce86
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59263885"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v4"></a>Öğretici: Azure ile soru-cevap Robotu oluşturun v4 Bot hizmeti

Gelen bir soru-cevap sohbet Robotu oluşturun **Yayımla** var olan bir Bilgi Bankası için sayfa. Bu bot, Bot Framework SDK v4 kullanır. Bot oluşturmak için kod yazmanız gerekmez, tüm kod size sağlanır.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Azure Bot hizmeti var olan bir Bilgi Bankası oluşturma
> * Kod çalışıp çalışmadığını denetlemek için bot ile sohbet edin 

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için yayımlanan Bilgi Bankası olması gerekir. Biri yoksa adımları [oluştur ve KB gelen yanıt](create-publish-query-in-portal.md) sorular ve cevaplar ile soru-cevap Oluşturucu Bilgi Bankası oluşturmak için öğretici.

<a name="create-a-knowledge-base-bot"></a>

## <a name="create-a-qna-bot"></a>Soru-cevap Robotu oluşturun

Bilgi Bankası için bir istemci uygulaması olarak bir bot oluşturulabilir. 

1. Soru-cevap Oluşturucu Portalı'nda Git **Yayımla** sayfasında ve, Bilgi Bankası yayımlama. Seçin **Bot oluşturma**. 

    ![Soru-cevap Oluşturucu Portalı'nda yayımlama sayfasına gidin ve kendi Bilgi Bankası yayımlama. Bot Oluştur'u seçin.](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base-page.png)

    Azure portal ile bot oluşturma yapılandırma açar.

1.  Bot oluşturma ayarlarını girin:

    |Ayar|Değer|Amaç|
    |--|--|--|
    |Robot adı|`my-tutorial-kb-bot`|Azure kaynağı adı için robot budur.|
    |Abonelik|Amaç bakın.|Aynı abonelik, soru-cevap Oluşturucu kaynakları oluşturmak için kullanılan seçin.|
    |Kaynak grubu|`my-tutorial-rg`|Tüm bot ile ilgili Azure kaynakları için kullanılan kaynak grubu.|
    |Konum|`west us`|Botun Azure kaynak konumu.|
    |Fiyatlandırma katmanı|`F0`|Ücretsiz katmanı için Azure bot hizmeti.|
    |Uygulama adı|`my-tutorial-kb-bot-app`|Bu yalnızca botunuzun desteklemek için bir web uygulamasıdır. Soru-cevap Oluşturucu hizmetinizi kullanır bu aynı uygulama adı olmamalıdır. Soru-cevap Oluşturucu'nın web uygulamasını başka bir kaynakla desteklenmiyor.|
    |SDK dil|C#|Bot framework SDK'sı tarafından kullanılan temel programlama dili budur. Seçenekleriniz şunlardır C# veya Node.js.|
    |Soru-cevap kimlik doğrulama anahtarı|**Değiştirme**|Bu değer sizin için doldurulur.|
    |App service planı/konumu|**Değiştirme**|Bu öğretici için konum önemli değildir.|
    |Azure Storage|**Değiştirme**|Konuşma verileri Azure depolama tablolarında depolanır.|
    |Application Insights|**Değiştirme**|Günlük kaydı Application Insights'a gönderilir.|
    |Microsoft uygulama kimliği|**Değiştirme**|Active directory kullanıcı adı ve parola gereklidir.|

    ![Bilgi Bankası bot bu ayarlarla oluşturun.](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base.png)

    Birkaç dakika bot oluşturma işlemi bildirimi başarılı sonuç raporlar kadar bekleyin.

<a name="test-the-bot"></a>

## <a name="chat-with-the-bot"></a>Bot ile sohbet edin

1. Azure portalında yeni bot kaynak gelen bildirim açın. 

    ![Azure portalında yeni bot kaynak gelen bildirim açın.](../media/qnamaker-tutorials-create-bot/azure-portal-notifications.png)

1. Gelen **Bot Yönetim**seçin **Test Web sohbeti** girin: `How large can my KB be?`. Bot ile yanıt verir: 


    `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.`


    ![Yeni Bilgi Bankası bot test edin.](../media/qnamaker-tutorial-create-publish-query-in-portal/test-bot-in-web-chat-in-azure-portal.png)

    Azure bot hakkında daha fazla bilgi için bkz: [soruları yanıtlamak için kullanım soru-cevap Oluşturucu](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs)

## <a name="related-to-qna-maker-bots"></a>Soru-cevap Oluşturucu botlar için ilgili

* Soru-cevap Oluşturucu Portalı'nda kullanılan soru-cevap Oluşturucu Yardım bot olarak kullanılabilir bir [bot örnek](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-support-bot).
    ![Soru-cevap Oluşturucu Yardım bot kırmızı robot simgedir](../media/qnamaker-tutorials-create-bot/answer-bot-icon.PNG)
* [Sağlık Hizmeti botlarınız](https://docs.microsoft.com/HealthBot/qna_model_howto) soru-cevap Oluşturucu birini kullanın. kendi [dil modellerini](https://docs.microsoft.com/HealthBot/qna_model_howto).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin bot ile işiniz bittiğinde, Azure portalında bot kaldırın. 

Yeni bir kaynak grubu botun kaynaklar oluşturduysanız, kaynak grubunu silin. 

Yeni bir kaynak grubu oluşturmadıysanız, bot ile ilişkili kaynakları bulmak gerekir. Bot ve bot uygulaması adına göre aramak için en kolay yolu olan. Bot kaynakları şunlardır:

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
