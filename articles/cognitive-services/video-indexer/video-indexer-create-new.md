---
title: Mevcut videolardan video öngörüleri oluşturmak için Azure Video Indexer'ı kullanma | Microsoft Docs
description: Bu konu oluşturma ve yayımlama hakkında bazı bir video dayalı içgörüler gösterilmektedir.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 161a47f72a0f8038a515c09f25ec2a8e8a520547
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39390849"
---
# <a name="create-highlights-from-existing-videos"></a>Mevcut videolardan öne çıkan özellikleri oluşturma

Bu konu oluşturma ve yayımlama hakkında bazı bir video dayalı içgörüler gösterilmektedir.

1. Oturum açın, [Video Indexer](https://api-portal.videoindexer.ai/) hesabı.
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

- [Video Indexer REST API ile işlem içerik](video-indexer-use-apis.md)
- [Uygulamanıza Visual pencere öğeleri](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer genel bakış](video-indexer-overview.md) 
