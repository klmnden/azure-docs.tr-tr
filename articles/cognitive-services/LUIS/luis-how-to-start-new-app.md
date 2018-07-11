---
title: LUIS ile yeni bir uygulama oluşturma | Microsoft Docs
description: Oluşturun ve Language Understanding (LUIS) sayfasındaki yönetin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/17/2018
ms.author: v-geberr
ms.openlocfilehash: 998a85720f5707fbf6ed4c5cfa3ed0dab5d1cc0e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932913"
---
# <a name="create-an-app"></a>Uygulama oluşturma
Yeni bir uygulama farklı şekillerde oluşturursunuz: 

* [Başlangıç](#create-new-app) boş bir uygulama ile ve hedefleri, konuşma ve varlıklar oluşturun.
* [Başlangıç](#create-new-app) boş bir uygulama ile ve bir [önceden oluşturulmuş etki alanı](luis-how-to-use-prebuilt-domains.md).
* [Bir LUIS uygulaması içeri](#import-new-app) zaten amacı, konuşma ve varlıkları içeren bir JSON dosyasından.

## <a name="what-is-an-app"></a>Uygulama nedir
Uygulamayı içeren [sürümleri](luis-how-to-manage-versions.md) modelinizi yanı sıra herhangi birini [ortak çalışanlar](luis-how-to-collaborate.md) uygulama için. Uygulamayı oluşturduğunuzda, kültür seçin ([dil](luis-supported-languages.md)) hangi **daha sonra değiştirilemez**. 

Varsayılan sürümü yeni bir uygulama, "0.1". 

Oluşturabilir ve uygulamalarınızı yönetme **uygulamalarım** sayfası. Bu sayfayı seçerek her zaman erişebilirsiniz **uygulamalarım** üst gezinti çubuğundaki [LUIS](luis-reference-regions.md) Web sitesi. 

[![](media/luis-create-new-app/apps-list.png "Uygulama listesinin ekran görüntüsü")](media/luis-create-new-app/apps-list.png#lightbox)

## <a name="create-new-app"></a>Yeni uygulama oluşturma

1. Üzerinde **uygulamalarım** sayfasında **yeni uygulama oluştur**.
2. İletişim kutusunda, "TravelAgent" uygulamanızı adlandırın.

    ![Yeni uygulama iletişim kutusu oluşturma](./media/luis-create-new-app/create-app.png)

3. Uygulama kültürü seçin (TravelAgent bir uygulama için İngilizce seçin) ve ardından **Bitti**. 

    >[!NOTE]
    >Kültür, uygulama oluşturulduktan sonra değiştirilemez. 

## <a name="import-new-app"></a>Yeni uygulama içeri aktarma
(En çok 50 karakter) adı, sürümü (en fazla 10 karakter) ve uygulama açıklamasını JSON dosyasında ayarlayabilirsiniz. Uygulama JSON dosyaları örnekleri kullanılabilir [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. Üzerinde **uygulamalarım** sayfasında **alma yeni uygulama**.
2. İçinde **alma yeni uygulama** iletişim kutusunda LUIS uygulaması tanımlayan JSON dosyasını seçin.

    ![Yeni bir uygulama iletişim kutusu içeri aktarma](./media/luis-create-new-app/import-app.png)

## <a name="export-app"></a>Uygulamayı dışarı aktarma
1. Üzerinde **uygulamalarım** sayfasında, üç noktayı seçin (***...*** ) uygulama satırının sonundaki.

    [![](media/luis-create-new-app/apps-list.png "Uygulama başına Eylemler açılır iletişim kutusunun ekran görüntüsü")](media/luis-create-new-app/three-dots.png#lightbox)

2. Seçin **dışarı aktarma uygulama** menüsünde. 

## <a name="rename-app"></a>Uygulamayı yeniden adlandırma

1. Üzerinde **uygulamalarım** sayfasında, üç noktayı seçin (***...*** ) uygulama satırının sonundaki. 
2. Seçin **Yeniden Adlandır** menüsünde.
3. Uygulama seçin ve yeni bir ad girin **Bitti**.

## <a name="delete-app"></a>Uygulamayı silme

> [!CAUTION]
> Uygulama için tüm çalışanlar ve sahibi siliyorsunuz. [Dışarı aktarma](#export-app) silinmeden önce uygulama. 

1. Üzerinde **uygulamalarım** sayfasında, üç noktayı seçin (***...*** ) uygulama satırının sonundaki. 
2. Seçin **Sil** menüsünde.
3. Seçin **Tamam** onay penceresinde.

## <a name="export-endpoint-logs"></a>Uç nokta günlükleri Dışarı Aktar
Sorgu günlükleri içeren UTC saati ve LUIS JSON yanıtı.

1. Üzerinde **uygulamalarım** sayfasında, üç noktayı seçin (***...*** ) uygulama satırının sonundaki. 
2. Seçin **uç nokta günlükleri dışarı aktar** menüsünde.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```

## <a name="next-steps"></a>Sonraki adımlar

Uygulamadaki ilk göreviniz [hedef ekleme](luis-how-to-add-intents.md).