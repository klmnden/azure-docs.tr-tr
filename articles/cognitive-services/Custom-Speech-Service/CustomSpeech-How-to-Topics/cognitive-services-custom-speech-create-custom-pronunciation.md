---
title: Azure üzerinde özel konuşma hizmetiyle özel telaffuz kullanın | Microsoft Docs
description: Bilişsel hizmetler özel konuşma hizmetiyle dil modeli oluşturmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 11/23/2017
ms.author: panosper
ms.openlocfilehash: a74b69b84cc80809a25f18b580a18abb5721b8b1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351683"
---
# <a name="enable-custom-pronunciation"></a>Özel telaffuz etkinleştir
Özel telaffuz ses form ve bir sözcük veya terim görünümünü tanımlamak kullanıcıların sağlar. Ürün adlarını veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için yararlıdır. İhtiyacınız olan tek şey telaffuz dosyasının (Basit .txt dosyası).

İşte nasıl çalışır. Bir tek .txt dosyasına birden fazla özel telaffuz girişi girebilirsiniz. Yapısı aşağıdaki gibidir:

```
Display form <Tab> Spoken form <Newline>
```

*Örnekler*:

| Form Görüntüle | Konuşma formu |
|----------|-------|
| C3PO | üç pea o bakın |
| L8R | geç olan |
| CNTK | n Çay k bakın|

## <a name="requirements-for-the-spoken-form"></a>Konuşma biçimi için gereksinimler
Konuşma formun içeri aktarma sırasında zorlanabilir küçük olması gerekir. Ayrıca, verileri içeri Aktarıcı denetimlerinde sağlamanız gerekir. Konuşma form veya görüntü formun sekme izin verilir. Var. ancak, olabilir, daha fazla görüntü formun karakter Yasak (örneğin, ~ ve ^).

Her .txt dosyası birden çok girişi olabilir. Örneğin, aşağıdaki ekran görüntüsüne bakın:

![Not Defteri ekran kısaltma telaffuz birden çok girişi ile](../../../media/cognitive-services/custom-speech-service/custom-speech-pronunciation-file.png)

Konuşma form görüntüleme formu ses dizisidir. Harf, sözcük veya hece oluşur. Şu anda daha fazla kılavuzu veya konuşma formun formüle yardımcı olması için standartları kümesi yok. 

## <a name="supported-pronunciation-characters"></a>Desteklenen telaffuz karakterleri
Özel telaffuz şu anda İngilizce (en-US) ve Almanca (de-de) için desteklenir. Konuşma biçiminde bir terim (özel telaffuz dosyası) ifade etmek için kullanılan karakter kümesi aşağıdaki tabloda gösterilmiştir: 

| Dil | Karakterler |
|---------- |----------|
| İngilizce (en-US) | a, b, c, d, e, f, g, h, i j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |
| Almanca (de-de) | ä, ö, ü, ẞ, a, b, c, d, e, f, g, h, i j, k, l, o, p, q, r, s, t, u, v, w, x, y, z |

>[NOT] Terim 's görüntüleme formu (telaffuz dosyasındaki) aynı şekilde bir dil uyarlama veri kümesinde yazılması gerekir.

## <a name="requirements-for-the-display-form"></a>Görüntü biçimi için gereksinimler
Bir görüntü form yalnızca özel bir sözcük, terimi, kısaltma veya var olan sözcükler birleştiren bileşik sözcüklerin olabilir. Sık kullanılan sözcük için Söyleyiş da girebilirsiniz. 

>[!NOTE]
Sık kullanılan sözcük reformulate veya konuşma formu değiştirmek için bu özelliği kullanarak önermiyoruz. Olağan dışı bazı sözcükleri (örneğin, kısaltmalar, teknik sözcüklerini ve yabancı sözcükler) değil doğru çözülür varsa görmek için kod çözücü çalıştırmak iyidir. Varsa, bunları özel telaffuz dosyasına ekleyin. Dil modelinde, her zaman ve yalnızca bir word'ün görüntüleme formu kullanmalısınız. 

## <a name="requirements-for-the-file-size"></a>Dosya boyutu gereksinimleri
Söyleniş girişlerini içeren .txt dosyasının boyutu 1 MB ile sınırlıdır. Genellikle, büyük miktarlarda verinin bu dosya karşıya gerekmez. Çoğu özel telaffuz dosyaları yalnızca birkaç KB boyutunda olması muhtemeldir. Tüm yerel ayarlar için .txt dosya kodlamasını UTF-8 AĞACI olmalıdır. İngilizce yerel ayar için ANSI de kabul edilebilir.

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturarak tanıma doğruluğunu artırmak için [özel akustik modeli](cognitive-services-custom-speech-create-acoustic-model.md).
* [Özel bir konuşma metin uç noktası oluşturma](cognitive-services-custom-speech-create-endpoint.md), hangi uygulamadan kullanabilirsiniz.
