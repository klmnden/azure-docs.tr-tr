---
title: Yeni bir uygulama ile HALUK oluşturma | Microsoft Docs
description: Oluşturun ve dil anlama (HALUK) Web uygulamalarınızı yönetin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/17/2018
ms.author: v-geberr
ms.openlocfilehash: 75edd39346995cdef72bb1e1fcb9eaff53d29702
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355756"
---
# <a name="create-an-app"></a>Bir uygulama oluşturun
Farklı şekillerde yeni bir uygulama oluşturun: 

* [Başlat](#create-new-app) boş bir uygulama ile ve amacı, utterances ve varlıklar oluşturun.
* [Başlat](#create-new-app) boş bir uygulama ile ve ekleme bir [önceden oluşturulmuş bir etki alanı](luis-how-to-use-prebuilt-domains.md).
* [HALUK uygulama alma](#import-new-app) zaten hedefleri, utterances ve varlıkları içeren bir JSON dosyasından.

## <a name="what-is-an-app"></a>Uygulaması nedir
Uygulamasını içeren [sürümleri](luis-how-to-manage-versions.md) modelinizi yanı sıra herhangi birini [ortak](luis-how-to-collaborate.md) uygulaması. Uygulama oluşturduğunuzda, kültür seçin ([dil](luis-supported-languages.md)) hangi **daha sonra değiştirilemez**. 

Varsayılan sürümü yeni bir uygulama, "0,1.". 

Oluşturma ve üzerinde uygulamalarınızı yönetmek **My uygulamaları** sayfası. Bu sayfayı seçerek her zaman erişebilirsiniz **uygulamalarım** üst gezinti çubuğundaki [HALUK](luis-reference-regions.md) Web sitesi. 

[![](media/luis-create-new-app/apps-list.png "Uygulamaların listesini ekran görüntüsü")](media/luis-create-new-app/apps-list.png#lightbox)

## <a name="create-new-app"></a>Yeni uygulama oluşturma

1. Üzerinde **My uygulamaları** sayfasında, **yeni uygulama oluştur**.
2. İletişim kutusunda "TravelAgent" uygulamanızı adlandırın.

    ![Yeni uygulama iletişim kutusu oluşturma](./media/luis-create-new-app/create-app.png)

3. Uygulama kültür seçin (TravelAgent uygulama için İngilizce seçin) ve ardından **Bitti**. 

    >[!NOTE]
    >Kültür uygulama oluşturulduktan sonra değiştirilemez. 

## <a name="import-new-app"></a>Yeni uygulama alma
JSON dosyasının adını (en çok 50 karakter), sürüm (en çok 10 karakter) ve bir uygulama açıklaması ayarlayabilirsiniz. Uygulama JSON dosyaları örnekleri kullanılabilir [HALUK-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. Üzerinde **My uygulamaları** sayfasında, **alma yeni uygulama**.
2. İçinde **alma yeni uygulama** iletişim kutusunda, HALUK uygulama tanımlama JSON dosyasını seçin.

    ![Yeni bir uygulama iletişim kutusu içeri aktarma](./media/luis-create-new-app/import-app.png)

## <a name="export-app"></a>Uygulama dışarı aktarma
1. Üzerinde **My uygulamaları** sayfasında, uygulama satırın sonundaki üç noktaya (...) seçin.

    [![](media/luis-create-new-app/apps-list.png "Uygulama başına eylemlerin açılır iletişim kutusunun ekran görüntüsü")](media/luis-create-new-app/three-dots.png#lightbox)

2. Seçin **verme uygulama** menüsünde. 

## <a name="rename-app"></a>Uygulama yeniden adlandırma

1. Üzerinde **My uygulamaları** sayfasında, uygulama satırın sonundaki üç noktaya (...) seçin. 
2. Seçin **yeniden adlandırma** menüsünde.
3. Yeni adını seçin ve uygulama girin **Bitti**.

## <a name="delete-app"></a>Uygulamayı silme

> [!CAUTION]
> Uygulama için tüm ortak çalışanlar ve sahibi siliyorsunuz. [Dışarı aktarma](#export-app) silmeden önce uygulama. 

1. Üzerinde **My uygulamaları** sayfasında, uygulama satırın sonundaki üç noktaya (...) seçin. 
2. Seçin **silmek** menüsünde.
3. Seçin **Tamam** onay penceresinde.

## <a name="export-endpoint-logs"></a>Uç nokta günlükleri dışarı aktarma
Sorgu günlükleri içeren UTC saati ve HALUK JSON yanıt.

1. Üzerinde **My uygulamaları** sayfasında, uygulama satırın sonundaki üç noktaya (...) seçin. 
2. Seçin **verme uç nokta günlükleri** menüsünde.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```

## <a name="next-steps"></a>Sonraki adımlar

İlk göreviniz uygulama [hedefleri Ekle](luis-how-to-add-intents.md).