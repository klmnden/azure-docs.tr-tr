---
title: İlk LUIS uygulamanıza 10 dakika var
titleSuffix: Azure Cognitive Services
description: Işıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturun. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 12a660b49d1a81865c34ceda38f041de9be31eb1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037482"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Hızlı başlangıç: Önceden oluşturulmuş ev otomasyonu uygulamasını kullanma

Bu hızlı başlangıçta ışıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturacaksınız. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu makale için [http://www.luis.ai](http://www.luis.ai) adresindeki LUIS portalından oluşturulan ücretsiz bir LUIS hesabına ihtiyacınız olacak. 

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
**Uygulamalarım** sayfasından uygulama oluşturabilir ve yönetebilirsiniz. 

1. LUIS portalında oturum açın.

2. **Yeni uygulama oluştur**'u seçin.

    [![](media/luis-quickstart-new-app/app-list.png "Uygulama listesinin ekran görüntüsü")](media/luis-quickstart-new-app/app-list.png)

3. İletişim kutusunda uygulamanıza "Home Automation" adını verin.

    [![](media/luis-quickstart-new-app/create-new-app-dialog.png "Create new app (Yeni uygulama oluştur) açılan iletişim kutusunun ekran görüntüsü")](media/luis-quickstart-new-app/create-new-app-dialog.png)

4. Uygulamanızın kültürünü seçin. Bu Home Automation uygulaması için İngilizce seçeneğini belirleyin. Ardından **Bitti**'yi seçin. LUIS, Home Automation uygulamasını oluşturur. 

    >[!NOTE]
    >Uygulama oluşturduktan sonra kültür değiştirilemez. 

## <a name="add-prebuilt-domain"></a>Önceden oluşturulmuş etki alanını ekleme

Sol taraftaki gezinti bölmesinden **Önceden oluşturulmuş etki alanları**'nı seçin. Ardından "Home" araması yapın. **Etki alanı ekle**'yi seçin.

[![](media/luis-quickstart-new-app/home-automation.png "Önceden oluşturulmuş etki alanı menüsünden çağrılan Home Automation etki alanının ekran görüntüsü")](media/luis-quickstart-new-app/home-automation.png)

Etki alanı başarıyla eklendiğinde önceden oluşturulmuş etki alanı kutusunda **Etki alanını kaldır** düğmesi görüntülenir.

[![](media/luis-quickstart-new-app/remove-domain.png "Etki alanını kaldır düğmesinin göründüğü Home Automation etki alanının ekran görüntüsü")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Amaçlar ve varlıklar

HomeAutomation etki alanının amaçlarını incelemek için sol taraftaki gezinti bölmesinden **Amaçlar**'ı seçin. Her amaçta örnek konuşmalar bulunur.

> [!NOTE]
> **Hiçbiri**, tüm LUIS uygulamaları tarafından sağlanan bir amaçtır. Uygulamanızın sağladığı işlevleri karşılamayan konuşmaların işlenmesi için bunu seçersiniz. 

**HomeAutomation.TurnOff** amacını seçin. Amaçta varlıklarla etiketlenmiş olan konuşmaların bir listesini görebilirsiniz.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "HomeAutomation.TurnOff amacının ekran görüntüsü")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Uygulamanızı test etme
Uygulamanızı eğittikten sonra test edebilirsiniz. Üst gezinti bölmesinde **Test**'i seçin. Etkileşimli Test bölmesine "Turn off the lights" (Işıkları kapat) gibi bir test konuşması yazın. 

```
Turn off the lights
```

En yüksek puana sahip olan amacın test konuşması için beklediğiniz amaca karşılık geldiğinden emin olun.

Bu örnekte "Turn off the lights" (Işıkları kapat), olması gereken şekilde "HomeAutomation.TurnOff" için en yüksek puana sahip amaç olarak tanımlanır.

[![](media/luis-quickstart-new-app/test.png "Konuşmanın vurgulandığı Test paneli ekran görüntüsü")](media/luis-quickstart-new-app/test.png)


Test bölmesini daraltmak için yeniden **Test**'i seçin. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Adresteki URL'nin sonuna gidin ve `turn off the living room light` tümcesini girip Enter tuşuna basın. Tarayıcıda HTTP uç noktanızın JSON yanıtı görüntülenir.

    [![](media/luis-quickstart-new-app/turn-off-living-room.png "TurnOff amacını belirleyen JSON sonucunun gösterildiği tarayıcı ekran görüntüsü")](media/luis-quickstart-new-app/turn-off-living-room.png)
    
## <a name="clean-up-resources"></a>Kaynakları temizleme

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Uç noktayı kod ile çağırabilirsiniz:

> [!div class="nextstepaction"]
> [Kod kullanarak bir LUIS uç noktasını çağırma](luis-get-started-cs-get-intent.md)
