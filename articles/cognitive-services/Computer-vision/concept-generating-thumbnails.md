---
title: Küçük resimleri - görüntü işleme oluşturuluyor
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak görüntüleri küçük resim oluşturma ile ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/11/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 8bbc86f5c6fe0f30968a1ba5bd5fa28160ef6963
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60372895"
---
# <a name="generating-smart-cropped-thumbnails-with-computer-vision"></a>Akıllı kırpılmış küçük görüntü işleme ile oluşturma

Bir küçük resim görüntü azaltılmış boyutlu gösterimidir. Küçük resimler ve diğer veriler daha ekonomik, Düzen kullanımı kolay bir şekilde temsil etmek için kullanılır. Görüntü işleme API'si, sezgisel küçük resimleri için belirli bir görüntü oluşturmak için akıllı kırpma, görüntüyü yeniden boyutlandırma ile birlikte kullanır.

Görüntü işleme küçük resim oluşturma algoritması gibi çalışır:

1. Rahatsız edici öğeleri görüntüden kaldırın ve tanımlamak _ilgi_&mdash;ana nesneyi göründüğü görüntünün alanı.
1. Üzerinde tanımlanan temel görüntü kırpma _ilgi_.
1. Hedef küçük resim boyutlarına en boy oranını değiştirin.

## <a name="area-of-interest"></a>İlgi alanı

Bir görüntüyü karşıya yükleme, görüntü işleme API'si belirlemek için analiz *ilgi*. Bunu daha sonra bu bölge için resmi kırpar nasıl belirlemek için kullanabilirsiniz. Belirtilmişse, kırpma işlemi, ancak her zaman istenen en boy oranını eşleşir.

Ayrıca bu ham sınırlama kutusu koordinatları aynı alabilirsiniz *ilgi* çağırarak **areaOfInterest** API yerine. Ardından, istediğiniz ancak özgün resmin değiştirmek için bu bilgileri kullanabilirsiniz.

## <a name="examples"></a>Örnekler

Oluşturulan küçük resim yaygın olarak hangi yükseklik, genişlik ve akıllı kırpma için belirttiğiniz bağlı olarak aşağıdaki görüntüde gösterildiği gibi farklılık gösterebilir.

![Bir kırpma çeşitli yapılandırmalarda yanındaki Sıradağlar görüntüsü](./Images/thumbnail-demo.png)

Aşağıdaki tabloda tipik küçük örnek görüntüleri için görüntü işleme tarafından oluşturulan gösterilmektedir. Küçük resimleri için belirtilen hedef yükseklik oluşturulan ve akıllı kırpma ile 50 piksel cinsinden genişliğini etkinleştirilir.

| Image | Küçük resim |
|-------|-----------|
|![Bir kişinin siluet ile gün batımı en dış mekanda Sıradağlar](./Images/mountain_vista.png) | ![Dış Mekanda Sıradağlar küçük resim, bir kişinin siluet ile gün batımı](./Images/mountain_vista_thumbnail.png) |
|![Yeşil bir arka plan beyaz çiçek](./Images/flower.png) | ![İşleme analiz çiçek küçük resmi](./Images/flower_thumbnail.png) |
|![Bir grup oluşturma çatıyı üzerinde bir kadın](./Images/woman_roof.png) | ![küçük bir grup oluşturma çatıyı kadını resmi](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri kategorilendirme](concept-categorizing-images.md).
