---
title: Mevcut videolardan - Azure video öngörüleri oluşturma
titlesuffix: Azure Media Services
description: Bu konuda, mevcut video dosyalarından video içgörüleri oluşturma ve yayımlama açıklanmaktadır.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 4a65e88e3f94f64a56bde882b535030968ae354d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60560240"
---
# <a name="create-highlights-from-existing-videos"></a>Mevcut videolardan öne çıkan özellikleri oluşturma

Bu konuda, başka bir videodan video içgörüleri oluşturma ve yayımlama açıklanmaktadır.

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.
2. Video içgörülerinizi oluşturmak istediğiniz videoyu bulun.
3. **Oynat**’a basın.

    Sayfada videoya ilişkin özetlenmiş içgörüler gösterilir. 

    ![Insights](./media/video-indexer-create-new/video-indexer-summarized-insights.png)
3. **Düzenle** düğmesine basın.

    Bu sayfada videonun tam bir çözümlemesi gösterilir. Çözümleme bloklara ayrılır. Bloklara ayrılması, verileri daha kolayca incelemenize olanak tanır. Örneğin, blok ne zaman konuşmacının değiştiğine ya da uzun duraklamalar olduğuna bağlı olarak çözümlenebilir. Sadece istediğiniz satırları içeren kendi oynatma listenizi oluşturabilirsiniz. Kaynak videonun yalnızca belirli bölümlerini göstermek için konular/anahtar sözcükler, yaklaşımlar, kişiler veya konuşmacılara göre filtreleyebilirsiniz. Videonun yalnızca transkriptini veya OCR’sini görüntülemeyi seçebilirsiniz.    

    ![Insights](./media/video-indexer-create-new/video-indexer-create-new-playlist.png)
4. Oynatma listenizi oluşturun.

    Oynatma listenize satır eklemek veya listeden satır kaldırmak için **+**/**-** düğmelerine basın.
5. Oynatma listenizin önizlemesini görüntüleyin.

    Oynatma listesini oluşturmayı tamamladığınızda **Önizleme**’ye basın.
6. Oynatma listesini yayımlayın.

    Oynatma listesinin önizlemesini görüntüledikten sonra listeyi yayımlayabilirsiniz.

    Oynatma listesi yayımlandıktan sonra video içgörülerinizin listesine eklenir.


## <a name="next-steps"></a>Sonraki adımlar 

Yeni oynatma listesini oluşturduktan sonra işlemeye devam edebilirsiniz. Aşağıdaki konularda buna ilişkin bilgiler verilmektedir: 

- [Video Indexer REST API'si ile içerik işleme](video-indexer-use-apis.md)
- [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer’a genel bakış](video-indexer-overview.md) 
