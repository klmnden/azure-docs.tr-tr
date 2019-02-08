---
title: Azure'da özel konuşma hizmeti ile özel telaffuz kullanma | Microsoft Docs
description: Bilişsel hizmetler, özel konuşma hizmeti ile bir dil modeli oluşturmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 11/23/2017
ms.author: nitinme
ms.openlocfilehash: 4387307a516b3ebb39b777b3a1fd32e7608c4a82
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867775"
---
# <a name="enable-custom-pronunciation"></a>Özel telaffuz etkinleştir

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

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

![Not Defteri ekran harflendirme söylenişi için birden çok girişi ile](../../../media/cognitive-services/custom-speech-service/custom-speech-pronunciation-file.png)

Görüntüleme formu fonetik dizisini konuşulan biçimidir. Harf, sözcükleri veya hece oluşur. Şu anda daha fazla rehberlik veya sizin söylenen formun formüle yardımcı olmak için standartları kümesini yoktur. 

## <a name="supported-pronunciation-characters"></a>Desteklenen telaffuz karakter
Özel telaffuz şu anda İngilizce (en-US) ve Almanca (de-de) için desteklenir. Konuşulan biçiminde bir terim (özel telaffuz dosyası) ifade etmek için kullanılan karakter kümesi aşağıdaki tabloda gösterilmiştir: 

| Dil | Karakterler |
|---------- |----------|
| İngilizce (en-US) | a, b, c, d, e, f, g, h, i, j, k, m, o, p, q, r, s, t, u, v, w, x, y, z |
| Almanca (de-de) | ä, ö, ü, ẞ, a, b, c, d, e, f, g, h, i, j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |

>[NOT] Bir terimi ait görüntüleme formu (telaffuz dosyasında) aynı şekilde dil uyarlama veri kümesinde yazılması gerekir.

## <a name="requirements-for-the-display-form"></a>Görüntüleme formu için gereksinimleri
Görüntüleme formu yalnızca özel bir sözcük, terim, kısaltması veya var olan sözcükler birleştiren bileşik sözcüklerin olabilir. Ayrıca, ortak kelimeler için Söyleyiş girebilirsiniz. 

>[!NOTE]
Ortak kelimeler yeniden oluşturun veya konuşulan formunu değiştirmek için bu özelliği kullanarak önermiyoruz. Bazı olağan dışı bir sözcük (örneğin, kısaltmalar, teknik sözcüklerini ve yabancı kelimeler) değil doğru çözülür olmadığını görmek için kod çözücü çalıştırmak daha iyidir. Varsa, bunları özel telaffuz dosyaya ekleyin. Dil modeli, sözcüklerin görüntüleme formu yalnızca ve her zaman kullanmalısınız. 

## <a name="requirements-for-the-file-size"></a>Dosya boyutu gereksinimleri
Söyleniş girişlerini içeren .txt dosyasının boyutu 1 MB ile sınırlıdır. Genellikle, bu dosya büyük miktarlarda veri yüklemek gerekmez. Çoğu özel telaffuz dosya boyutu birkaç KB'leri etkilenebilir. Tüm yerel ayarlar için .txt dosyasının kodlama UTF-8 AĞACI olmalıdır. İngilizce yerel ayarı için ANSI de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturarak tanıma doğruluğunu artırmak, [özel akustik model](cognitive-services-custom-speech-create-acoustic-model.md).
* [Özel Konuşmayı metne uç nokta oluşturma](cognitive-services-custom-speech-create-endpoint.md), bir uygulamadan kullanabilirsiniz.
