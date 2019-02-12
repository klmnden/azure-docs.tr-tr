---
title: Video Indexer Web sitesi, bir kişi modeli - Azure'ı özelleştirmek için kullanın
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer Web sitesiyle bir kişi model özelleştirme gösterilmektedir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: anzaman
ms.openlocfilehash: 29c32126a5bbe792dd6fe88ae189e09c511d2a22
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997076"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Video Indexer Web sitesiyle bir kişi modeli özelleştirme

Video Indexer, video içeriği için yüz algılama ve ünlü tanıma destekler. Ünlü tanıma özelliği, yaklaşık IMDB Wikipedia ve üst LinkedIn öğrenilenler gibi sık istenen bir veri kaynağına göre bir milyon yüzleri kapsar. Ünlü tanıma özelliği tarafından tanınmayan yüz algılandı; Ancak, sol adlandırılmamış. Video Indexer için videonuzu karşıya yükleyin ve geri sonuçlar alma sonra geri dönün ve değil tanındı yüzleri adlandırın. Bir ada sahip bir yüz etiket sonra adı ve yüz hesabınızın kişi modele eklenir. Video Indexer, ardından bu yüz gelecekteki videoları ve son videolar algılar.

Video Indexer Web sitesi, bu konuda açıklandığı bir video, algılanan yüzeylere düzenlemek için kullanabilirsiniz. Bölümünde anlatıldığı gibi API de kullanabilirsiniz [Özelleştir kişi modeli API'lerini kullanarak](customize-person-model-with-api.md).

## <a name="edit-a-face"></a>Bir yüzü Düzenle

> [!NOTE]
> Aynı ada, adı iki farklı yüzler, Video Indexer aynı kişi yüzleri görünümleri ve videonuzu yeniden sonra bunları uygun sonuç verir adları için benzersiz bir kişi modeller. Video Indexer'ın aynı yüz farklı birden çok defa geçmelerine algılanan görürseniz, bunları aynı adı verin ve videonuzu yeniden dizin oluşturma.
> Video Indexer, yeni bir adla bir ünlü tanınan bir yazıtipi güncelleştirebilirsiniz. Size yeni bir ad yerleşik ünlü tanıma öncelikli olur.

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.
2. Hesabınızdaki görüntülemek ve düzenlemek için istediğiniz bir video arayın.
3. Video yüz düzenlemek için şuraya gidin: **Insights** sekmesine ve pencerenin sağ üst köşedeki kalem simgesine tıklayın.

    ![Kişi model özelleştirme](./media/customize-face-model/customize-face-model.png)

4. Algılanan yüzeylere birini tıklatın ve "Bilinmeyen #X" (veya yüz için daha önce atanan adı) adlarını değiştirin.
5. Yeni adını yazdıktan sonra yeni adının yanındaki onay simgesine tıklayın. Bu yeni adını kaydedin ve tanımak ve bu diğer geçerli videolarınızı ve gelecekte, karşıya video yüz tüm oluşumlarını adlandırın. Diğer geçerli videolarınızı bir yüz tanıma, bir toplu işlem olarak etkili olması için biraz zaman alabilir. 

    ![Güncelleştirme Kaydet](./media/customize-face-model/save-update.png)

## <a name="delete-a-face"></a>Bir yüzü Sil

Algılanan bir yüz video silmek için Git **Insights** bölmesi ve bölmenin sağ üst köşedeki kalem simgesine tıklayın. Tıklayın **Sil** yüzü adı altında seçeneği. Bu video yüz algılanan kaldırır. Yüz tanıma hala göründükleri diğer videoları algılanır ancak dizine sonra bu videolardan yüzü silebilirsiniz. Ayrıca, yüz tanıma silmeden önce adlı, hesabınızın kişi modelinde var olmaya devam eder.

## <a name="next-steps"></a>Sonraki adımlar

[Kişi modeli API'lerini kullanarak özelleştirme](customize-person-model-with-api.md)
