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
ms.date: 11/30/2018
ms.author: pafarley
ms.openlocfilehash: 7d914f394ecfcf02ed26f41cd8fe2ef799cf6103
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966747"
---
# <a name="generating-thumbnails"></a>Küçük resimler oluşturma

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

![Küçük Resimler](./Images/thumbnail-demo.png)

Aşağıdaki tabloda tipik küçük örnek görüntüleri için görüntü işleme tarafından oluşturulan gösterilmektedir. Küçük resimleri için belirtilen hedef yükseklik oluşturulan ve akıllı kırpma ile 50 piksel cinsinden genişliğini etkinleştirilir.

| Görüntü | Küçük resim |
|-------|-----------|
|![Dış Mekanda Dağ](./Images/mountain_vista.png) | ![Dış Mekanda Sıradağlar küçük resmi](./Images/mountain_vista_thumbnail.png) |
|![Görüntü Analizi Çiçek](./Images/flower.png) | ![İşleme analiz çiçek küçük resmi](./Images/flower_thumbnail.png) |
|![Çatıda Kadın](./Images/woman_roof.png) | ![Kadın tavan küçük resmi](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri kategorilendirme](concept-categorizing-images.md).