---
title: Ayıklanan metin OCR - görüntü işleme
titleSuffix: Azure Cognitive Services
description: Optik karakter tanıma (OCR) görüntü işleme API'sini kullanarak metinle ayıklanması için ilgili kavramları.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: af88063a68cef4953da8357596ce64e4510e8d36
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985465"
---
# <a name="extracting-text-with-ocr"></a>OCR ile metin ayıklama

Görüntü işleme optik karakter tanıma (OCR) teknolojisi, bir resimdeki metin içeriğini algılar ve tanımlanan metin bir makine tarafından okunabilir bir karakter akışı halinde ayıklar. Sonuç, arama ve tıbbi kayıtları, güvenlik ve bankacılığa gibi çeşitli amaçlar için kullanabilirsiniz. Dili otomatik olarak algılar. OCR, zaman tasarrufu sağlar ve kullanıcılara metin yerine metnin fotoğrafını çekme olanağı tanıyarak kolaylık sunar.

OCR, 25 dilleri destekler. Bu diller: Arapça, Basitleştirilmiş Çince, Geleneksel Çince, Çekçe, Danca, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Rumence, Rusça, Sırpça (Kiril ve Latin) Slovakça, İspanyolca, İsveççe ve Türkçe.

Gerekirse, OCR tanınan metnin yatay görüntü ekseni etrafında derece döndürme düzeltir. OCR aşağıdaki çizimde görüldüğü gibi her bir sözcüğün çerçeve koordinatları sağlar.

![OCR genel bakış](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>OCR gereksinimleri

Görüntü işleme, OCR aşağıdaki gereksinimleri karşılayan görüntüleri kullanarak metin ayıklayabilirsiniz:

* Sunulan görüntünün JPEG, PNG, GIF veya BMP biçiminde olması gerekir
* Girdi görüntüsünün boyutu, 40 x 40 3200 x 3200 piksel arasında olmalıdır.
* Görüntü 10 megapiksel büyük olamaz

Girdi görüntüsünün 90 derece birden fazla ve en fazla 40 derece küçük bir açısını tarafından döndürülebilir.

## <a name="improving-ocr-accuracy"></a>OCR doğruluğunu artırma

Metin tanıma doğruluğunu görüntü kalitesini üzerinde bağlıdır. Yanlış bir okuma tarafından aşağıdaki durumlarda oluşabilir:

* Bulanık görüntüler.
* El yazısı veya el yazısı metin.
* Artistik yazı tipi stili.
* Küçük metin boyutu.
* Karmaşık bir arka plan, gölgeler veya içinde metin veya perspektif bozulma önleyici.
* Sözcükleri başındaki eksik veya büyük boyutlu büyük harf
* Alt simge, üst simge ya da üstü çizili metin.

### <a name="ocr-limitations"></a>OCR sınırlamaları

Metin baskın olduğu fotoğrafları üzerinde hatalı pozitif sonuçları kısmen tanınan sözcükleri gelebilir. Bazı fotoğraflar, özellikle fotoğraf olmadan herhangi bir metin üzerinde duyarlık çok görüntü türüne bağlı olarak farklılık gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [yazdırılan ve el yazısı metinleri tanıma](concept-recognizing-text.md).