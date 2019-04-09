---
title: Söyleniş - konuşma Hizmetleri özelleştirme
titlesuffix: Azure Cognitive Services
description: Konuşma hizmeti telaffuz özelleştirmeyi öğrenin. Özel telaffuz ile fonetik formu ve görüntüleme bir sözcük veya terimi tanımlayabilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için kullanışlıdır. Başlamak için ihtiyacınız olan telaffuz dosya--basit .txt dosyası.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: f825cf8f381a7a2974b150a74a091412b24b09bc
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59005157"
---
# <a name="enable-custom-pronunciation"></a>Özel telaffuz etkinleştir

Özel telaffuz kullanılarak bir sözcük veya dönem (kısaltması) görüntü ve ses form tanımlayabilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için kullanışlıdır. Başlamak için ihtiyacınız olan telaffuz dosya--basit .txt dosyası.

Şekli aşağıda verilmiştir. Bir tek bir .txt dosyasına birden çok özel telaffuz girişi girebilirsiniz. Yapı aşağıdaki gibidir:

```
Display form <Tab> Spoken form <Newline>
```

Aşağıdaki tabloda bazı örnekler gösterilmektedir:

| Görüntüleme formu | Konuşulan formu |
|----------|-------|
| 3CPO | üç pea o bakın |
| L8R | geç olan |
| CNTK | c n t k|

## <a name="requirements-for-the-spoken-form"></a>Konuşulan formu için gereksinimler

Konuşulan form, içeri aktarma sırasında zorlayabilirsiniz küçük olmalıdır. Ayrıca veri alma denetimleri vermeniz gerekir. Sekme konuşulan form veya görüntüleme formu izin verilir. Ancak, orada daha görüntüleme formu karakter Yasak (örneğin, ~ ve ^).

Aşağıdaki görüntüde gösterildiği gibi her bir .txt dosyasına birden çok girişi sahip olabilir:

![Kısaltması telaffuz örnekleri](media/stt/custom-speech-pronunciation-file.png)

Görüntüleme formu fonetik dizisini konuşulan biçimidir. Harf, sözcükleri veya hece oluşur. Şu anda daha fazla rehberlik veya sizin söylenen formun formüle yardımcı olmak için standartları kümesini yoktur.

## <a name="supported-pronunciation-characters"></a>Desteklenen telaffuz karakter

Özel telaffuz şu anda İngilizce (en-US) ve Almanca (de-de) için desteklenir. Konuşulan biçiminde bir terim (özel telaffuz dosyası) ifade etmek için kullanabileceğiniz bir karakter kümesi aşağıdaki tabloda gösterilmiştir:

| Dil | Karakterler |
|---------- |----------|
| İngilizce (en-US) | a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |
| Almanca (de-de) | ä, ö, ü, ?, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |

> [!NOTE]
> Bir terimi ait görüntüleme formu (telaffuz dosyasında) aynı şekilde dil uyarlama kümesinde yazılması gerekir.

## <a name="requirements-for-the-display-form"></a>Görüntüleme formu için gereksinimleri

Görüntüleme formu yalnızca özel bir sözcük, bir kısaltma veya var olan sözcükler birleştiren bileşik sözcüklerin olabilir.

>[!NOTE]
>Bu özellik, ortak kelimeler yeniden oluşturun veya konuşulan formunu değiştirmek için kullanımı önerilmemektedir. Bu özelliği kullanmadan önce bazı olağan dışı bir sözcük (örneğin, kısaltmalar, teknik sözcüklerini veya yabancı kelimeler) yanlış transribed olup olmadığını, daha iyi olup olmadığını denetler. Varsa, bunları özel telaffuz dosyaya ekleyin. Dil modeli, sözcüklerin görüntüleme formu yalnızca ve her zaman kullanmalısınız.

## <a name="requirements-for-the-file-size"></a>Dosya boyutu gereksinimleri
Söyleniş girişler .txt dosyasının boyutu 1 megabayt (ücretsiz katmanı anahtarları için 1 KB) sınırlıdır. Genellikle, büyük miktarlarda verinin bu dosyayı karşıya yükleme gerekmez. Çoğu özel telaffuz dosya boyutu birkaç kilobayt (KB) etkilenebilir. Tüm yerel ayarlar için .txt dosyasının kodlama UTF-8 AĞACI olmalıdır. İngilizce yerel ayarı için ANSI de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturarak tanıma doğruluğunu artırmak bir [özel akustik model](how-to-customize-acoustic-models.md).
* Oluşturarak tanıma doğruluğunu artırmak bir [özel dil modeli](how-to-customize-language-model.md).
