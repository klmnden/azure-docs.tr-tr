---
title: Yazdırılan, el yazısı metni - görüntü işleme tanıma
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak görüntüleri metin yazdırılan ve el yazısı tanıma için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 48ce15a11c3e3282535420f3e1bb1915276d70f5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313186"
---
# <a name="recognizing-printed-and-handwritten-text"></a>Yazdırılmış ve el yazısı ile yazılan metinleri tanıma

Görüntü işleme, algılayın ve farklı yüzey ve arka planlar, giriş, posterler, kartvizit, harf ve beyaz tahtalar gibi çeşitli nesne görüntülerdeki yazdırılan veya el yazısı metinleri ayıklamak.

Metin tanıma özelliği çok benzer [optik karakter tanıma (OCR)](concept-extracting-text-ocr.md), ancak zaman uyumsuz olarak yürütür ve kullandığı OCR aksine tanıma modelleri güncelleştirildi.

> [!NOTE]
> Bu teknoloji şu an Önizleme aşamasındadır ve yalnızca İngilizce metinlerde kullanılabilir.

## <a name="text-recognition-requirements"></a>Metin tanıma gereksinimleri

Görüntü işleme, aşağıdaki gereksinimleri karşılaması görüntüdeki basılı ve el yazısı metni tanıyabilmesi:

- Görüntü JPEG, PNG veya BMP biçiminde sunulması
- Görüntünün dosya boyutunun 4 megabayt (MB) değerini aşmaması gerekir
- Görüntü boyutları 50 x 50 ile 4200 x 4200 piksel arasında olmalıdır

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [metin başvuru belgeleri tanımak](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) daha fazla bilgi için.