---
title: Küçük resimler oluşturma
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Azure Bilişsel hizmetler görüntü işleme kullanarak görüntüleri küçük resim oluşturma ile ilgili kavramları.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 92de0c0a428c9010188131a768074234241ed604
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725499"
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