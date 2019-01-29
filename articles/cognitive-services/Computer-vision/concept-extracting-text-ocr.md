---
title: Ayıklanan metin OCR - görüntü işleme
titleSuffix: Azure Cognitive Services
description: Optik karakter tanıma (OCR) görüntü işleme API'sini kullanarak metinle ayıklanması için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: b1fc2e18dd5fc8e995c2d997683a4c5e5c501087
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188975"
---
# <a name="extracting-text-with-optical-character-recognition"></a>Ayıklanan metin ile optik karakter tanıma

Görüntü işleme optik karakter tanıma (OCR) teknolojisi, bir resimdeki metin içeriğini algılar ve tanımlanan metin bir makine tarafından okunabilir bir karakter akışı halinde ayıklar. Sonucu, arama yapmak için veya tıbbi kayıtlar, güvenlik ve bankacılık gibi çok çeşitli amaçlarla kullanabilirsiniz. Dili otomatik olarak algılar. OCR, zaman tasarrufu sağlar ve kullanıcılara, metni yazmak yerine bunların fotoğrafını çekme olanağı tanıyarak kolaylık sunar.

OCR, 25 dili destekler. Bu diller şunlardır: Arapça, Çince, Geleneksel Çince, Basitleştirilmiş Çekçe, Danca, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Rumence, Rusça, Sırpça (Kiril ve Latin), Slovakya, İspanyolca, İsveççe ve Türkçe.

Gerekirse, OCR tanınan metnin yönünü yatay görüntü ekseninde derece olarak düzeltir. OCR aşağıdaki çizimde görüldüğü gibi her bir sözcüğün çerçeve koordinatları sağlar.

![Döndürülmüş görüntüyü gösteren diyagram ve okuma ve makaleyle metin](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>OCR gereksinimleri

Görüntü işleme, OCR aşağıdaki gereksinimleri karşılayan görüntüleri kullanarak metin ayıklayabilirsiniz:

* Sunulan görüntünün JPEG, PNG, GIF veya BMP biçiminde olması gerekir
* Girdi görüntüsünün boyutu 50 x 50 ile 4200 x 4200 piksel arasında olmalıdır


Girdi görüntüsünün 90 derece birden fazla ve en fazla 40 derece küçük bir açısını tarafından döndürülebilir.

## <a name="improving-ocr-accuracy"></a>OCR doğruluğunu artırma

Metin tanımanın doğruluğu görüntünün kalitesine bağlıdır. Aşağıdaki durumlar yanlış okumaya yol açabilir:

* Bulanık görüntüler.
* El yazısı veya bitişik el yazısı metin.
* Artistik yazı tipi stilleri.
* Küçük metin boyutu.
* Karmaşık arka planlar, gölgeler ya da metnin üzerinde parlama veya perspektif bozukluk.
* Sözcüklerin başındaki aşırı büyük veya eksik büyük harfler
* Alt simge, üst simge veya üstü çizili metin.

### <a name="ocr-limitations"></a>OCR sınırlamaları

Metin baskın olduğu fotoğrafları üzerinde hatalı pozitif sonuçları kısmen tanınan sözcükleri gelebilir. Bazı fotoğraflar, özellikle fotoğraf olmadan herhangi bir metin üzerinde duyarlık çok görüntü türüne bağlı olarak farklılık gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [yazdırılan ve el yazısı metinleri tanıma](concept-recognizing-text.md).
