---
title: Küçük resimleri - görüntü işleme oluşturuluyor
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak görüntüleri küçük resim oluşturma ile ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 9cb82b40d1fbec513b0219f26d1959fbd7f64570
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343971"
---
# <a name="generating-thumbnails"></a>Küçük resimler oluşturma

Bir küçük resim, bir tam boyutlu görüntüyü küçük bir gösterimidir. Telefonlar, tabletler ve bilgisayarlar gibi çeşitli cihazlar farklı bir kullanıcı deneyimi (UX) düzenleri ve küçük resim boyutları için bir gereksinim oluşturun. Bu görüntü işleme özelliği, akıllı kırpma kullanarak sorunu çözmeye yardımcı olur.

Görüntü karşıya yükledikten sonra görüntü işleme yüksek kaliteli bir küçük resim oluşturur ve ardından tanımlamak için resim içindeki nesneleri analiz eder *ilgi bölgesi* (ROI). İsteğe bağlı olarak yatırım getirisi gereksinimleri resmi kırpar. Oluşturulan küçük resmin en boy oranını ihtiyaçlarınıza uyum sağlamak için özgün görüntü, farklı bir en boy oranı kullanılarak sunulabilir.

Küçük resim algoritması gibi çalışır:

1. Görüntüden dikkat dağıtıcı öğeleri kaldırır ve bölgeyi ana nesnesini tanımlar.
2. İlgilendiğiniz tanımlanan bölgeye göre resmi kırpar.
3. En boy oranını hedef küçük resim boyutları uyacak şekilde değiştirir.

Oluşturulan küçük resim yaygın olarak hangi yükseklik, genişlik ve akıllı kırpma için belirttiğiniz bağlı olarak aşağıdaki görüntüde gösterildiği gibi farklılık gösterebilir.

![Küçük Resimler](./Images/thumbnail-demo.png)

## <a name="thumbnail-generation-examples"></a>Küçük resim oluşturma örnekleri

Aşağıdaki tabloda tipik küçük örnek görüntüleri için görüntü işleme tarafından oluşturulan gösterilmektedir. Küçük resimleri için belirtilen hedef yükseklik oluşturulan ve akıllı kırpma ile 50 piksel cinsinden genişliğini etkinleştirilir.

| Görüntü | Küçük resim |
|-------|-----------|
|![Dış Sıradağlar](./Images/mountain_vista.png) | ![Dış Mekanda Sıradağlar küçük resmi](./Images/mountain_vista_thumbnail.png) |
|![İşleme çiçek analiz edin](./Images/flower.png) | ![İşleme analiz çiçek küçük resmi](./Images/flower_thumbnail.png) |
|![Kadın tavan](./Images/woman_roof.png) | ![Kadın tavan küçük resmi](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri kategorilendirme](concept-categorizing-images.md).