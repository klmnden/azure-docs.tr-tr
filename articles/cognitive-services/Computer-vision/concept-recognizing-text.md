---
title: Metin, görüntü işleme yazdırılan/resimlerdeki el yazısı tanıma
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak görüntüleri metin yazdırılan ve el yazısı tanıma için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f7fd13b0b6df0b07543216e3c612520e528c1176
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998252"
---
# <a name="recognize-printed-and-handwritten-text"></a>Yazdırılmış ve el yazısı ile yazılan metinleri tanıma

Görüntü işleme, birkaç resimlerde görüntülenen yazdırılan veya el yazısı metinleri algılamanıza ve ayıklamanıza hizmetleri sağlar. Bu senaryolar not alma, tıbbi kayıtları, güvenlik ve bankacılık gibi birçok yararlı olur. Aşağıdaki üç bölüm ayrıntısı üç farklı metin tanıma API'leri, her farklı kullanım örnekleri için İyileştirildi.

## <a name="read-api"></a>Okuma API'si

Okuma API'si, sunduğumuz en son tanıma modelleri ile bir resimdeki metin içeriğini algılar ve bir makine tarafından okunabilir bir karakter akışı halinde tanımlanan metin dönüştürür. Çok fazla görsel gürültü ile görüntü ve metin ağırlıklı görüntüleri (örneğin, dijital olarak taranan belgeleri için) için optimize edilmiştir. Daha büyük belgelere bir sonuç döndürmek için birkaç dakika sürebilir çünkü zaman uyumsuz olarak yürütür.

Okuma işleminin çıktısını tanınan sözcükleri özgün satır gruplandırmalarına tutar. Her satırın sınırlama kutusu koordinatları ile gelir ve her sözcük satır içinde kendi koordinatları de içerir. Bir sözcük düşük güvenle tanınırsa bu bilgileri de aktarılır. Bkz: [okuma API başvuru belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) daha fazla bilgi için.

> [!NOTE]
> Bu özellik şu anda Önizleme aşamasındadır ve yalnızca İngilizce metinlerde kullanılabilir.

### <a name="image-requirements"></a>Görüntü gereksinimleri

Okuma API'si, aşağıdaki gereksinimleri karşılaması ve görüntüleri ile çalışır:

- Görüntü, JPEG, PNG, BMP, PDF veya TIFF biçiminde sunulmalıdır.
- Görüntü boyutları 50 x 50 ile 4200 x 4200 piksel arasında olmalıdır. PDF sayfaları 17 x 17 inç veya ondan küçük olmalıdır.
- Görüntü dosyası boyutu, 20 megabaytın (MB) olmalıdır.

### <a name="limitations"></a>Sınırlamalar

Ücretsiz katman abonelik kullanıyorsanız, okuma API yalnızca ilk iki PDF veya TIFF belge sayfaları işler. Ücretli bir aboneliğe, en fazla 200 sayfasını işler. Ayrıca API en fazla sayfa başına 300 satır algılar unutmayın.

## <a name="ocr-optical-character-recognition-api"></a>OCR (optik karakter tanıma) API'si

Görüntü işleme'nın optik karakter tanıma (OCR) API'si için okuma API benzer, ancak zaman uyumlu olarak yürütülür ve büyük dosyalar için optimize edilmemiş. Daha fazla dil ile çalışır ancak bir önceki tanıma modeli kullanır; bkz: [dil desteği](language-support.md#text-recognition) desteklenen dillerin tam bir listesi için.

Gerekirse, OCR yatay görüntü eksen hakkında derece cinsinden döngüsel uzaklığı döndürerek tanınan metin döndürmesini düzeltir. OCR, ayrıca aşağıdaki çizimde görüldüğü gibi her bir sözcüğün çerçeve koordinatları sağlar.

![Döndürülmüş bir görüntü ve metin okuma ve makaleyle](./Images/vision-overview-ocr.png)

Bkz: [OCR başvuru belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) daha fazla bilgi için.

### <a name="image-requirements"></a>Görüntü gereksinimleri

OCR API aşağıdaki gereksinimleri karşılaması ve görüntüleri üzerinde çalışır:

* Görüntü, JPEG, PNG, GIF veya BMP biçiminde sunulmalıdır.
* Girdi görüntüsünün boyutu 50 x 50 ile 4200 x 4200 piksel arasında olmalıdır.
* Görüntüdeki metnin birden fazla 90 derece yanı sıra en fazla 40 derece küçük bir açısını tarafından döndürülebilir.

### <a name="limitations"></a>Sınırlamalar

Metin baskın olduğu fotoğrafları üzerinde hatalı pozitif sonuçları kısmen tanınan sözcükleri gelebilir. Bazı fotoğraflar, özellikle fotoğraf olmadan herhangi bir metin üzerinde duyarlık görüntü türüne bağlı olarak değişebilir.

## <a name="recognize-text-api"></a>Metin çevirisi API'si tanıması

> [!NOTE]
> Metin tanıma API'si ile değiştiriliyor okuma API kullanım dışıdır. Okuma API'si, benzer özelliklere sahiptir ve PDF, TIFF ve çok sayfalı dosyalarını işlemek için güncelleştirilir.

Metin tanıma API'si, OCR için benzerdir, ancak zaman uyumsuz olarak yürütür ve güncelleştirilmiş tanıma modelleri kullanır. Bkz: [tanımak metin API başvuru belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) daha fazla bilgi için.

### <a name="image-requirements"></a>Görüntü gereksinimleri

Metin tanıma API'si, aşağıdaki gereksinimleri karşılaması ve görüntüleri ile birlikte çalışır:

- Görüntü, JPEG, PNG veya BMP biçiminde sunulmalıdır.
- Görüntü boyutları 50 x 50 ile 4200 x 4200 piksel arasında olmalıdır.
- Görüntü dosyasının boyutu küçüktür 4 megabayt (MB) olmalıdır.

## <a name="improve-results"></a>Sonuçları iyileştirin

Metin tanıma işlemlerini doğruluğunu görüntü kalitesini bağlıdır. Aşağıdaki faktörleri yanlış bir okuma neden olabilir:

* Bulanık görüntüler.
* El yazısı veya bitişik el yazısı metin.
* Artistik yazı tipi stilleri.
* Küçük metin boyutu.
* Karmaşık arka planlar, gölgeler ya da metnin üzerinde parlama veya perspektif bozukluk.
* Büyük boyutlu ya da eksik büyük harf sözcükleri başındaki.
* Alt simge, üst simge veya üstü çizili metin.

## <a name="next-steps"></a>Sonraki adımlar

İzleyin [yazdırılan metin (OCR) ayıklayın](./quickstarts/csharp-print-text.md) içinde basit bir metin tanıma uygulamak için Hızlı Başlangıç C# uygulama.
