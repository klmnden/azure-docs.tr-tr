---
title: 10 dakikada ilk Language Understanding (LUIS) uygulamanızı oluşturma - Cognitive Services LUIS | Microsoft Docs
description: Bu hızlı başlangıçta ışıkları ve cihazları açıp kapatmak için önceden oluşturulmuş `HomeAutomation` etki alanını kullanan bir LUIS uygulaması oluşturacaksınız. Önceden oluşturulmuş olan bu etki alanı amaçlara, varlıklara ve örnek konuşmalara sahiptir. İşlemi tamamladığınızda bulut üzerinde çalışan bir LUIS uç noktasına sahip olacaksınız.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/22/2018
ms.author: diberry
ms.openlocfilehash: 457f23936dec0cf85e9aebbf3e54bba37c2f3ca3
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2018
ms.locfileid: "43772539"
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

HomeAutomation etki alanının amaçlarını incelemek için sol taraftaki gezinti bölmesinden **Amaçlar**'ı seçin. 

[![](media/luis-quickstart-new-app/home-automation-intents.png "Amaç adlarının vurgulanmış olduğu Amaçlar listesinin ekran görüntüsü")](media/luis-quickstart-new-app/home-automation-intents.png)

Her amaçta örnek konuşmalar bulunur.

> [!NOTE]
> **Hiçbiri**, tüm LUIS uygulamaları tarafından sağlanan bir amaçtır. Uygulamanızın sağladığı işlevleri karşılamayan konuşmaların işlenmesi için bunu seçersiniz. 

**HomeAutomation.TurnOff** amacını seçin. Amaçta varlıklarla etiketlenmiş olan konuşmaların bir listesini görebilirsiniz.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "HomeAutomation.TurnOff amacının ekran görüntüsü")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-your-app"></a>Uygulamanızı eğitme

Üst gezinti bölmesinde **Eğit**'i seçin.

[![](media/luis-quickstart-new-app/trained.png "Yeşil renkli başarı bildirimi ile HomeAutomation.TurnOff amacının ekran görüntüsü")](media/luis-quickstart-new-app/trained.png)

## <a name="test-your-app"></a>Uygulamanızı test etme
Uygulamanızı eğittikten sonra test edebilirsiniz. Üst gezinti bölmesinde **Test**'i seçin. Etkileşimli Test bölmesine "Turn off the lights" (Işıkları kapat) gibi bir test konuşması yazın. 

```
Turn off the lights
```

En yüksek puana sahip olan amacın test konuşması için beklediğiniz amaca karşılık geldiğinden emin olun.

Bu örnekte "Turn off the lights" (Işıkları kapat), olması gereken şekilde "HomeAutomation.TurnOff" için en yüksek puana sahip amaç olarak tanımlanır.

[![](media/luis-quickstart-new-app/test.png "Konuşmanın vurgulandığı Test paneli ekran görüntüsü")](media/luis-quickstart-new-app/test.png)


Test bölmesini daraltmak için yeniden **Test**'i seçin. 

## <a name="publish-your-app"></a>Uygulamanızı yayımlama
Üst gezinti bölmesinden **Yayımla**'yı seçin. 

[![](media/luis-quickstart-new-app/publish.png "Yayımla düğmesi vurgulanmış uygulamanın ekran görüntüsü")](media/luis-quickstart-new-app/publish.png)

Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

Üstteki yeşil bildirim çubuğu, uygulamanın başarıyla yayımlandığını gösterir.

[![](media/luis-quickstart-new-app/published.png "Başarıyla yayımlanmış olan uygulamanın ekran görüntüsü")](media/luis-quickstart-new-app/published.png)

Uygulama başarıyla yayımlandıktan sonra **Uygulamayı yayımla** sayfasında görüntülenen uç nokta URL'sini seçebilirsiniz.

[![](media/luis-quickstart-new-app/endpoint.png "Uç nokta URL'si vurgulanmış Yayımla sayfası ekran görüntüsü")](media/luis-quickstart-new-app/endpoint.png)

## <a name="use-your-app"></a>Uygulamanızı kullanma
Yayımlanan uç noktanızı oluşturulan URL'yi kullanarak tarayıcıda test edebilirsiniz. Bu URL'yi tarayıcınızda açın ve "&q" URL parametresini test sorgunuza göre ayarlayın. Örneğin URL'nin sonuna `turn off the living room light` ekleyin ve Enter tuşuna basın. Tarayıcıda HTTP uç noktanızın JSON yanıtı görüntülenir.


[![](media/luis-quickstart-new-app/turn-off-living-room.png "TurnOff amacını belirleyen JSON sonucunun gösterildiği tarayıcı ekran görüntüsü")](media/luis-quickstart-new-app/turn-off-living-room.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta (***...***) düğmesini ve sonra da **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uç noktayı kod ile çağırabilirsiniz:

> [!div class="nextstepaction"]
> [Kod kullanarak bir LUIS uç noktasını çağırma](luis-get-started-cs-get-intent.md)
