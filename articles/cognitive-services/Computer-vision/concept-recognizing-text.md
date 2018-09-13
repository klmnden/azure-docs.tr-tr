---
title: Yazdırılan ve el yazısı metinleri tanıma
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Azure Bilişsel hizmetler görüntü işleme kullanarak görüntüleri metin yazdırılan ve el yazısı tanıma için ilgili kavramları.
services: cognitive-services
author: deken
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: bdfef668eddc712d2637dcf53a01f193a3204880
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44725483"
---
# <a name="recognizing-printed-and-handwritten-text"></a>Yazdırılan ve el yazısı metinleri tanıma

Görüntü işleme, algılayın ve farklı yüzey ve arka planlar, giriş, posterler, kartvizit, harf ve beyaz tahtalar gibi çeşitli nesne görüntülerdeki yazdırılan veya el yazısı metinleri ayıklamak.

Metin tanıma, zaman ve çaba harcamanızı sağlar. Metin görüntüsü almak yerine onu fotoğrafını daha verimli olabilir. Metin tanıma, notları dijitalleştirerek mümkün kılar. Bu digitization hızla ve kolayca arama olanak tanır. Tüm bunların yanı sıra kağıt karmaşasını da azaltır.

## <a name="text-recognition-requirements"></a>Metin tanıma gereksinimleri

Görüntü işleme, aşağıdaki gereksinimleri karşılaması görüntüdeki basılı ve el yazısı metni tanıyabilmesi:

- Görüntü JPEG, PNG veya BMP biçiminde sunulması
- Görüntünün dosya boyutunun 4 megabayt (MB) değerini aşmaması gerekir
- Görüntü boyutları 40 x 40 3200 x 3200 piksel arasında olmalıdır

> [!NOTE]
> Bu teknoloji şu an Önizleme aşamasındadır ve yalnızca İngilizce metinlerde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [OCR metinle ayıklama](concept-extracting-text-ocr.md).