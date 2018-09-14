---
title: Mevcut videolardan video öngörüleri oluşturmak için Azure Video Indexer'ı kullanma | Microsoft Docs
description: Bu konu oluşturma ve yayımlama hakkında bazı bir video dayalı içgörüler gösterilmektedir.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 09/09/2018
ms.author: juliako
ms.openlocfilehash: a713a85a3301b211f922268d6afc4d047bd12023
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45541709"
---
# <a name="create-highlights-from-existing-videos"></a>Mevcut videolardan öne çıkan özellikleri oluşturma

Bu konu oluşturma ve yayımlama hakkında bazı bir video dayalı içgörüler gösterilmektedir.

1. Gözat [Video Indexer](https://www.videoindexer.ai/) Web sitesine gidin ve oturum açma.
2. Video içgörülerinizi oluşturmak istediğiniz videoyu bulun.
3. Tuşuna **Play**.

    Sayfada özetlenen ınsights video gösterilir. 

    ![Insights](./media/video-indexer-create-new/video-indexer-summarized-insights.png)

3. Tuşuna **Düzenle** düğmesi.

    Bu sayfa bir video tam dökümünü gösterir. Dökümü bloklara ayrılır. Blokları verilerine geçin kolay hale getirmek için aşağıda verilmiştir. Örneğin, blok konuşmacıları değiştirin ya da uzun duraklama temel alınarak bölünmesine. İstediğiniz satırları içeren kendi çalma listesi oluşturabilirsiniz. Video kaynağı yalnızca belirli bölümlerini göstermek için filtre uygulayabilirsiniz konuları/anahtar sözcükler, yaklaşım, kişiler, konuşmacıları. Yalnızca videonun döküm veya OCR görüntülemek seçebilirsiniz.    

    ![Insights](./media/video-indexer-create-new/video-indexer-create-new-playlist.png)

4. Oynatma listeniz oluşturun.

    Ekleme veya/oynatma listeniz satırları kaldırmak için basın **+** / **-**.

5. Oynatma listeniz önizlemesini görüntüleyin.

    İşiniz bittiğinde çalma listesi oluşturma, basın **Önizleme**.
6. Çalma listesi yayımlayın.

    Çalma listesi Önizleme sonra bunu yayımlayabilirsiniz.

    Çalma listesi yayımladıktan sonra bu video içgörülerinizi listesine eklenir.


## <a name="next-steps"></a>Sonraki adımlar 

Yeni çalma listesi oluşturduktan sonra aşağıdaki konulardan birine açıklandığı gibi işleme geçebilirsiniz: 

- [Video Indexer REST API'si ile içerik işleme](video-indexer-use-apis.md)
- [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer genel bakış](video-indexer-overview.md) 
