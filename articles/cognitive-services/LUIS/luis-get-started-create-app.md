---
title: 'Hızlı Başlangıç: uygulama oluşturma'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Işıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturun. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 71f3084be697dd84f3f262d2a79cd04a0ba76d8e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086819"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Hızlı başlangıç: Önceden oluşturulmuş ev otomasyonu uygulamasını kullanma

Bu hızlı başlangıçta ışıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturacaksınız. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu makale için [https://www.luis.ai](https://www.luis.ai) adresindeki LUIS portalından oluşturulan ücretsiz bir LUIS hesabına ihtiyacınız olacak. 

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
**Uygulamalarım** sayfasından uygulama oluşturabilir ve yönetebilirsiniz. 

1. LUIS portalında oturum açın.

2. **Yeni uygulama oluştur**'u seçin.

    [![Uygulama listesinin ekran görüntüsü](media/luis-quickstart-new-app/app-list.png "uygulama listesinin ekran görüntüsü")](media/luis-quickstart-new-app/app-list.png)

3. İletişim kutusunda uygulamanıza "Home Automation" adını verin.

    [![Yeni uygulama açılır iletişim Oluştur ekran görüntüsü](media/luis-quickstart-new-app/create-new-app-dialog.png "yeni uygulama açılır iletişim Oluştur ekran görüntüsü")](media/luis-quickstart-new-app/create-new-app-dialog.png)

4. Uygulamanızın kültürünü seçin. Bu Home Automation uygulaması için İngilizce seçeneğini belirleyin. Ardından **Bitti**'yi seçin. LUIS, Home Automation uygulamasını oluşturur. 

    >[!NOTE]
    >Uygulama oluşturduktan sonra kültür değiştirilemez. 

## <a name="add-prebuilt-domain"></a>Önceden oluşturulmuş etki alanını ekleme

Sol taraftaki gezinti bölmesinden **Önceden oluşturulmuş etki alanları**'nı seçin. Ardından "Home" araması yapın. **Etki alanı ekle**'yi seçin.

[![Ekran görüntüsü, giriş Otomasyon etki alanı çekilerek önceden oluşturulmuş etki alanı menüde](media/luis-quickstart-new-app/home-automation.png "ekran görüntüsü, giriş Otomasyon etki alanı çekilerek önceden oluşturulmuş etki alanı menüsü")](media/luis-quickstart-new-app/home-automation.png)

Etki alanı başarıyla eklendiğinde önceden oluşturulmuş etki alanı kutusunda **Etki alanını kaldır** düğmesi görüntülenir.

[![Kaldır düğmesinin Ekran görüntüsü, giriş Otomasyon etki](media/luis-quickstart-new-app/remove-domain.png "Kaldır düğmesinin Ekran görüntüsü, giriş Otomasyon etki alanı")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Amaçlar ve varlıklar

HomeAutomation etki alanının amaçlarını incelemek için sol taraftaki gezinti bölmesinden **Amaçlar**'ı seçin. Her amaçta örnek konuşmalar bulunur.

> [!NOTE]
> **Hiçbiri**, tüm LUIS uygulamaları tarafından sağlanan bir amaçtır. Uygulamanızın sağladığı işlevleri karşılamayan konuşmaların işlenmesi için bunu seçersiniz. 

**HomeAutomation.TurnOff** amacını seçin. Amaçta varlıklarla etiketlenmiş olan konuşmaların bir listesini görebilirsiniz.

[![Ekran görüntüsü, HomeAutomation.TurnOff hedefi](media/luis-quickstart-new-app/home-automation-turnon.png "ekran görüntüsü, HomeAutomation.TurnOff hedefi")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Uygulamanızı test etme
Uygulamanızı eğittikten sonra test edebilirsiniz. Üst gezinti bölmesinde **Test**'i seçin. Etkileşimli Test bölmesine "Turn off the lights" (Işıkları kapat) gibi bir test konuşması yazın. 

```
Turn off the lights
```

En yüksek puana sahip olan amacın test konuşması için beklediğiniz amaca karşılık geldiğinden emin olun.

Bu örnekte "Turn off the lights" (Işıkları kapat), olması gereken şekilde "HomeAutomation.TurnOff" için en yüksek puana sahip amaç olarak tanımlanır.

[![Utterance vurgulandığı ekran görüntüsü, Test paneliyle](media/luis-quickstart-new-app/test.png "utterance vurgulandığı ekran görüntüsü, Test paneli")](media/luis-quickstart-new-app/test.png)


Test bölmesini daraltmak için yeniden **Test**'i seçin. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Adresteki URL'nin sonuna gidin ve `turn off the living room light` tümcesini girip Enter tuşuna basın. Tarayıcıda HTTP uç noktanızın JSON yanıtı görüntülenir.

    [![Tarayıcı ile JSON sonuç görüntüsü algılar hedefi kapatma](media/luis-quickstart-new-app/turn-off-living-room.png "tarayıcı ile JSON sonuç görüntüsü algılar hedefi kapatma")](media/luis-quickstart-new-app/turn-off-living-room.png)
    
## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Uç noktayı kod ile çağırabilirsiniz:

> [!div class="nextstepaction"]
> [Kod kullanarak bir LUIS uç noktasını çağırma](luis-get-started-cs-get-intent.md)
