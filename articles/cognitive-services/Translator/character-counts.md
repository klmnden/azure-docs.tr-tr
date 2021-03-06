---
title: Karakter sayısı - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Nasıl Translator Text API karakter sayar.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cfd5823009b66b6b525c7add1fb56953d3c1a507
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445257"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Translator Text API karakter nasıl sayılır?

Translator metin çevirisi API'si giriş metnin her Unicode kod noktasını bir karakter olarak sayılır. İsteğin bir tek API çağrısı için birden çok dil çevirme içinde yapıldığını olsa bile her bir dilin metin çevirisi ayrı bir çeviri sayılır. Yanıt uzunluğu önemli değildir.

Ne sayıları şöyledir:

* Translator metin çevirisi API'si isteğinin gövdesinde geçirilen metin
   * `Text` Çeviri Transliterate ve sözlük arama yöntemleri kullanılırken
   * `Text` ve `Translation` sözlük örnekleri yöntemi kullanırken
* Tüm biçimlendirme: HTML, XML etiketleri, vb. metin alanı istek gövdesi içinde. Derleme isteği için kullanılan JSON gösterimi (örneğin "metni:") yok sayılır.
* Tek bir harfi
* Noktalama işaretleri
* Bir boşluk, sekme, biçimlendirme ve herhangi bir türden boşluk karakteri
* Unicode olarak tanımlanan her kod noktası
* Aynı metni önceden çevrilmiş olsa bile yinelenen bir çeviri

Çince ve Japonca Kanji gibi kavramyazıların seçilmesini göre betikler, Translator Text API hala ideogram başına bir karakter Unicode kod noktaları, kaç sayılır. Özel durum: Unicode sayısı iki karakter olarak temsilciler.

İstek, sözcük, bayt veya cümleler karakter sayısına ilgisiz sayısıdır.

Algılama ve BreakSentence yöntemlerine yapılan çağrıda karakter tüketimini sayılmaz. Ancak, algılama ve BreakSentence yöntemlere yapılan çağrılar sayılan diğer işlevleri kullanımını kabul edilebilir bir oranda olduğunu bekliyoruz. Yaptığınız Algıla veya BreakSentence çağrılarının sayısı 100 kez Microsoft sayılan başka yöntemlerle sayısını aşarsa algılama ve BreakSentence yöntemlerinin kullanımını kısıtlamak için hakkını saklı tutar.


Karakter sayıları hakkında daha fazla bilgi yer [Microsoft Translator SSS](https://www.microsoft.com/en-us/translator/faq.aspx).
