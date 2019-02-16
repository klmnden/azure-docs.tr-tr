---
title: Ayıklanan metin ile optik karakter tanıma (OCR) - görüntü işleme
titleSuffix: Azure Cognitive Services
description: Optik karakter tanıma (OCR) görüntü işleme API'sini kullanarak metinle ayıklanması için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: deb73eb9fdd6879a5fbe1fed820bf92b2d627b65
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310449"
---
# <a name="extract-text-with-optical-character-recognition"></a>Optik karakter tanıma ile metni Ayıkla

Görüntü işleme'nın optik karakter tanıma (OCR) özelliği, bir resimdeki metin içeriğini algılar ve bir makine tarafından okunabilir bir karakter akışı halinde tanımlanan metin dönüştürür. Sonuç, arama, tıbbi kayıtları, güvenlik ve bankacılık gibi birçok amaç için kullanabilirsiniz. 

OCR, 25 dilleri destekler: Arapça, Çince, Geleneksel Çince, Basitleştirilmiş Çekçe, Danca, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Rumence, Rusça, Sırpça (Kiril ve Latin), Slovakya, İspanyolca, İsveççe ve Türkçe. OCR, otomatik olarak algılanan metnin dilini algılar.

Gerekirse, OCR yatay görüntü eksen hakkında derece cinsinden döngüsel uzaklığı döndürerek tanınan metin döndürmesini düzeltir. OCR, aşağıdaki çizimde görüldüğü gibi her bir sözcüğün çerçeve koordinatları de sağlar.

![Döndürülmüş görüntüyü gösteren diyagram ve okuma ve makaleyle metin](./Images/vision-overview-ocr.png)

## <a name="image-requirements"></a>Görüntü gereksinimleri

Görüntü işleme, OCR aşağıdaki gereksinimleri karşılayan görüntüleri kullanarak metin ayıklayabilirsiniz:

* Sunulan görüntünün JPEG, PNG, GIF veya BMP biçiminde olması gerekir
* Girdi görüntüsünün boyutu 50 x 50 ile 4200 x 4200 piksel arasında olmalıdır
* Görüntüdeki metnin birden fazla 90 derece yanı sıra en fazla 40 derece küçük bir açısını tarafından döndürülebilir.

## <a name="improving-ocr-accuracy"></a>OCR doğruluğunu artırma

Metin tanımanın doğruluğu görüntünün kalitesine bağlıdır. Yanlış bir okuma aşağıdakilerden kaynaklanabilir:

* Bulanık görüntüler.
* El yazısı veya bitişik el yazısı metin.
* Artistik yazı tipi stilleri.
* Küçük metin boyutu.
* Karmaşık arka planlar, gölgeler ya da metnin üzerinde parlama veya perspektif bozukluk.
* Sözcüklerin başındaki aşırı büyük veya eksik büyük harfler
* Alt simge, üst simge veya üstü çizili metin.

### <a name="ocr-limitations"></a>OCR sınırlamaları

Metin baskın olduğu görüntülerde, hatalı pozitif sonuçları kısmen tanınan sözcükleri gelebilir. Özellikle fotoğraf herhangi bir metin olmadan bazı görüntülerindeki duyarlık çok görüntü türüne bağlı olarak farklılık gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [OCR başvuru belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) daha fazla bilgi için.
