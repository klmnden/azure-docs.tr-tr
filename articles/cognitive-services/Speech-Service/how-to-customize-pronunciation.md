---
title: Azure Bilişsel hizmetler konuşma hizmeti
description: Bilişsel hizmetler konuşma hizmeti ile telaffuz özelleştirmeyi öğrenin.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/02/2018
ms.author: panosper
ms.openlocfilehash: c7c06fc2f33baa7357fd5f945414daf2bc6e4858
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284947"
---
# <a name="enable-custom-pronunciation"></a>Özel telaffuz etkinleştir
Özel Söyleniş kullanıcıların fonetik formu ve görüntüleme bir sözcük veya terimi tanımlamasına olanak sağlar. Ürün adları veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için kullanışlıdır. İhtiyacınız olan telaffuz dosyası (Basit .txt dosyası).

Şekli aşağıda verilmiştir. Bir tek bir .txt dosyasına birden çok özel telaffuz girişi girebilirsiniz. Yapı aşağıdaki gibidir:

```
Display form <Tab> Spoken form <Newline>
```

*Örnekler*:

| Görüntüleme formu | Konuşulan formu |
|----------|-------|
| C3PO | üç pea o bakın |
| L8R | geç olan |
| CNTK | n Çay k bakın|

## <a name="requirements-for-the-spoken-form"></a>Konuşulan formu için gereksinimler
İçeri aktarma sırasında zorlanabilir konuşulan formun küçük olmalıdır. Ayrıca, veri alma denetimleri sağlamanız gerekir. Sekme konuşulan form veya görüntüleme formu izin verilir. Var. ancak, olabilir, daha fazla karakter görüntüleme formu Yasak (örneğin, ~ ve ^).

Her bir .txt dosyasına birden çok girişi olabilir. Örneğin, aşağıdaki ekran görüntüsüne bakın:

![Not Defteri ekran harflendirme söylenişi için birden çok girişi ile](media/stt/custom-speech-pronunciation-file.png)

Görüntüleme formu fonetik dizisini konuşulan biçimidir. Harf, sözcükleri veya hece oluşur. Şu anda daha fazla rehberlik veya sizin söylenen formun formüle yardımcı olmak için standartları kümesini yoktur. 

## <a name="supported-pronunciation-characters"></a>Desteklenen telaffuz karakter
Özel telaffuz şu anda İngilizce (en-US) ve Almanca (de-de) için desteklenir. Konuşulan biçiminde bir terim (özel telaffuz dosyası) ifade etmek için kullanılan karakter kümesi aşağıdaki tabloda gösterilmiştir: 

| Dil | Karakterler |
|---------- |----------|
| İngilizce (en-US) | a, b, c, d, e, f, g, h, i, j, k, m, o, p, q, r, s, t, u, v, w, x, y, z |
| Almanca (de-de) | ä, ö, ü, ẞ, a, b, c, d, e, f, g, h, i, j, k, m, o, p, q, r, s, t, u, v, w, x, y, z |

> [!NOTE]
> Bir terimi ait görüntüleme formu (telaffuz dosyasında) aynı şekilde dil uyarlama veri kümesinde yazılması gerekir.

## <a name="requirements-for-the-display-form"></a>Görüntüleme formu için gereksinimleri
Görüntüleme formu yalnızca özel bir sözcük, terim, kısaltması veya var olan sözcükler birleştiren bileşik sözcüklerin olabilir. Ayrıca, ortak kelimeler için Söyleyiş girebilirsiniz. 

>[!NOTE]
>Ortak kelimeler yeniden oluşturun veya konuşulan formunu değiştirmek için bu özelliği kullanarak önermiyoruz. Bazı olağan dışı bir sözcük (örneğin, kısaltmalar, teknik sözcüklerini ve yabancı kelimeler) değil doğru çözülür olmadığını görmek için kod çözücü çalıştırmak daha iyidir. Varsa, bunları özel telaffuz dosyaya ekleyin. Dil modeli, sözcüklerin görüntüleme formu yalnızca ve her zaman kullanmalısınız. 

## <a name="requirements-for-the-file-size"></a>Dosya boyutu gereksinimleri
Söyleniş girişlerini içeren .txt dosyasının boyutu 1 MB ile sınırlıdır. Genellikle, bu dosya büyük miktarlarda veri yüklemek gerekmez. Çoğu özel telaffuz dosya boyutu birkaç KB'leri etkilenebilir. Tüm yerel ayarlar için .txt dosyasının kodlama UTF-8 AĞACI olmalıdır. İngilizce yerel ayarı için ANSI de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturarak tanıma doğruluğunu artırmak bir [özel akustik model](how-to-customize-acoustic-models.md)
* Oluşturarak tanıma doğruluğunu artırmak bir [özel dil modeli](how-to-customize-language-model.md)
