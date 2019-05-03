---
title: İnsan etiketli döküm yönergeler - konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Özellikle sözcükleri silindi veya yanlış yerine, neden olduğu sorunları olan tanıma doğruluğunu artırmak için arıyorsanız İnsan etiketli döküm ses verilerinizi birlikte kullanmak isteyebilirsiniz. İnsan etiketli döküm nedir? Bunu yapmak kolaydır, bir ses dosyasının word sözcüklü, aynen döküm oldukları.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 7f0b467284872f3d936984741c6d092705008a5a
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025929"
---
# <a name="how-to-create-human-labeled-transcriptions"></a>İnsan etiketli döküm oluşturma

Özellikle sözcükleri silindi veya yanlış yerine, neden olduğu sorunları olan tanıma doğruluğunu artırmak için arıyorsanız İnsan etiketli döküm ses verilerinizi birlikte kullanmak isteyebilirsiniz. İnsan etiketli döküm nedir? Bunu yapmak kolaydır, bir ses dosyasının word sözcüklü, aynen döküm oldukları.

Döküm verilerinin büyük bir örnek tanımayı geliştirmek için gereklidir, döküm veri 10 ile 1000 saat arasında sağlama öneririz. Bu sayfada size yüksek kaliteli döküm oluşturmanıza yardımcı olmak için tasarlanan yönergeleri gözden geçirelim. Bu kılavuz, ABD İngilizcesi, Mandarin Çince ve Almanca için bölümleri olan bir yerel ayar tarafından ayrılmıştır.

## <a name="us-english-en-us"></a>ABD İngilizce (en-US)

İnsan etiketli döküm İngilizce ses yalnızca ASCII karakterler kullanarak düz metin olarak sağlanmalıdır. Latin-1 ya da Unicode noktalama karakterleri kullanmaktan kaçının. Bu karakterler, metin bir kelime işleme uygulaması veya web sayfalarından veri değiştirilene kopyalarken genellikle istemeden eklenir. Bu karakterler varsa, bunları uygun ASCII değiştirme ile güncelleştirdiğinizden emin olun.

İşte birkaç örnek:

| Önlemek için karakter | Değiştirme | Notlar |
|---------------------|--------------|-------|
| "Hello world" | "Hello world" | Uygun ASCII karakterleri ile açılış ve kapanış tırnak işareti yapıştırıyorsanız. |
| Can'ın gün | Can'ın gün | Kesme işareti uygun ASCII karakteri ile değiştirdi. |
| iyi — Hayır, harika! | en iyi--, harika! | Em dash iki kısa çizgi ile kullanıldı. |

### <a name="text-normalization-for-us-english"></a>ABD İngilizcesi için metin normalleştirme

Sözcük bir modeli eğitimindeki kullanılan tutarlı bir biçime metin normalleştirme dönüşümdür. Bazı normalleştirme kuralları metni otomatik olarak uygulanır, ancak, İnsan etiketli transkripsiyonu verilerinizi hazırlanırken bu yönergeleri kullanmanızı öneririz:

* Sözcük içindeki kısaltmalar dışarı yazın.
* Standart sayısal dizeler sözcük (örneğin, muhasebe koşulları) dışarı yazın.
* Alfabetik olmayan karakterler veya karma alfasayısal karakterler belirgin olarak transcribed.
* Sözcükler olarak telaffuz kısaltmaları (örneğin, "radar", "lazer", "RAM" veya "NATO") düzenlenebilir olmamalıdır.
* Bir boşluk ile ayrılan her bir harfle ayrı harf olarak telaffuz kısaltmalar çıkış yazma.

Döküm üzerinde gerçekleştirmeniz gereken normalleştirme bazı örnekleri aşağıda verilmiştir:

| Orijinal metni | Normalleştirme sonra gelen metin |
|---------------|--------------------------|
| Dr. Bruce başlığı | Hastanede Bruce başlığı |
| James Bond, 007 | James Bond çift oh yedi |
| Ke$ha | Kesha |
| 2 x 4 ne kadardır | Tarafından iki dört ne kadardır |
| 1-3'te gelen toplantı gider | Bir ila üç pm toplantı gider |
| My tansiyon O + türüdür | O pozitif My tansiyon türüdür |
| Su H20 olduğu | Su H 2 O olduğu |
| Van Halen tarafından OU812 Yürüt | O U Van Halen 8 1 2 Yürüt |
| BOM ile UTF-8 | BOM ile U T F 8 |

Aşağıdaki normalleştirme kuralları, otomatik döküm için uygulanır:

* Küçük harf kullanın.
* Tüm noktalama dışında sözcük içinde kesme kaldırın.
* Tutarlara gibi sözcükler ve konuşma form içine numaraları genişletin.

Normalleştirme transkripsiyonu üzerinde otomatik olarak gerçekleştirilen bazı örnekleri aşağıda verilmiştir:

| Orijinal metni | Normalleştirme sonra gelen metin |
|---------------|--------------------------|
| "Muhteşem inek!" söz konusu Batman. | Muhteşem inek batman dedi. |
| "Ne olacak?" söz konusu Batman'ın sidekick, bir kez deneme. | hangi söz konusu batman'ın sidekick bir kez deneme |
| Git - em alın! | go get em |
| Double-jointed istiyorum | Çift jointed istiyorum |
| 104 Elm Street | bir oh dört istasyon Sokak |
| 102.7 için ayarlama | bir oh ayarlama iki yedi |
| PI hakkında 3,14 olan | Pi yaklaşık üç noktası bir dört olduğu |
$3.14 maliyetleri| üç on dört maliyetleri |

## <a name="mandarin-chinese-zh-cn"></a>Mandarin Çince (zh-cn)

İnsan etiketli döküm Mandarin Çince ses için UTF-8 bayt sırası işaretçisi ile kodlanmış olmalıdır. Yarım genişlikteki noktalama karakterleri kullanmaktan kaçının. Bir sözcük program uygulamasında veri hazırlama veya web sayfalarından veri scrape şu karakterleri yanlışlıkla dahil edilebilir. Bu karakterler varsa, bunları uygun tam genişlikli değiştirme ile güncelleştirdiğinizden emin olun.

İşte birkaç örnek:

| Önlemek için karakter | Değiştirme | Notlar |
|---------------------|--------------|-------|
| "你好" | "你好" | Uygun karakterlerle açılış ve kapanış tırnak işareti yapıştırıyorsanız. |
| 需要什么帮助? | 需要什么帮助？ | Soru işareti uygun karakteri ile değiştirdi. |

### <a name="text-normalization-for-mandarin-chinese"></a>Mandarin Çince için metin normalleştirme

Sözcük bir modeli eğitimindeki kullanılan tutarlı bir biçime metin normalleştirme dönüşümdür. Bazı normalleştirme kuralları metni otomatik olarak uygulanır, ancak, İnsan etiketli transkripsiyonu verilerinizi hazırlanırken bu yönergeleri kullanmanızı öneririz:

* Sözcük içindeki kısaltmalar dışarı yazın.
* Sayısal dizeler sözlü biçimde dışarı yazın.

Döküm üzerinde gerçekleştirmeniz gereken normalleştirme bazı örnekleri aşağıda verilmiştir:

| Orijinal metni | Normalleştirme sonra gelen metin |
|---------------|--------------------------|
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

Aşağıdaki normalleştirme kuralları, otomatik döküm için uygulanır:

* Tüm noktalama işaretlerini kaldırın
* Konuşulan forma sayı genişletin
* Tam genişlikli harfleri yarım genişlik için Dönüştür
* Büyük harfler için tüm Türkçe kelimeler kullanarak

Normalleştirme transkripsiyonu üzerinde otomatik olarak gerçekleştirilen bazı örnekleri aşağıda verilmiştir:

| Orijinal metni | Normalleştirme sonra gelen metin |
|---------------|--------------------------|
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z |W F Y, Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

## <a name="german-de-de-and-other-languages"></a>Almanca (de-DE) ve diğer diller

İnsan etiketli döküm Almanca ses (ve diğer İngilizce olmayan veya Mandarin Çince dil), UTF-8 bayt sırası işaretçisi ile kodlanmış olmalıdır. Her ses dosyası için bir insan etiketli döküm sağlanmalıdır.

### <a name="text-normalization-for-german"></a>Almanca için metin normalleştirme

Sözcük bir modeli eğitimindeki kullanılan tutarlı bir biçime metin normalleştirme dönüşümdür. Bazı normalleştirme kuralları metni otomatik olarak uygulanır, ancak, İnsan etiketli transkripsiyonu verilerinizi hazırlanırken bu yönergeleri kullanmanızı öneririz:

*   Ondalık noktası olarak yazma ","ve".".
*   Yazma saat ayırıcısı olarak ":"ve"." (örneğin: 12:00 Uhr).
*   Kısaltmalar gibi "ca". Değiştirilen değildir. Tam konuşulan biçimini kullanmanızı öneririz.
*   Dört ana matematik işleçleri (+, -, \*, ve /) kaldırılır. Yazılı biçime ile değiştirmeden öneririz: "Ayrıca," "eksi," "kötü" ve "geteilt."
*   Karşılaştırma işleçleri kaldırılır (=, <, ve >). "Gleich ile" değiştirmeden öneririz "kleiner als," ve "grösser als."
*   3/4 gibi kesirler yazılı biçiminde yazın (örneğin: "drei viertel" 3/4 yerine).
*   Alt yazılı biçime "Euro." ile "€" simgesini değiştirme

Döküm üzerinde gerçekleştirmeniz gereken normalleştirme bazı örnekleri aşağıda verilmiştir:

| Orijinal metni | Kullanıcı normalleştirme sonra gelen metin | Sistem normalleştirme sonra gelen metin |
|---------------|-------------------------------|---------------------------------|
| ES ist 12.23 Uhr | ES ist 12:23 Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 2 + 3 - 4 | 2 ve 3 4 eksi | zwei plus drei minus vier |

Aşağıdaki normalleştirme kuralları, otomatik döküm için uygulanır:

* Tüm metni küçük harf kullanın.
* Çeşitli tırnak işaretleri dahil olmak üzere tüm noktalama işaretlerini kaldırın ("test", 'test', "test" ve «test» edilir).
* Özel karakterler bu kümesindeki satırları At: ¢ ¤ ¥ ¦ m © ª ¬® ° ± ² µ × ÿ Ø¬¬.
* Sayı dolar veya Euro miktarları dahil olmak üzere, konuşulan biçimine genişletin.
* Umlaut kabul yalnızca o için ve. Diğerleri "th" değiştirilir veya iptal edilecek.

Normalleştirme transkripsiyonu üzerinde otomatik olarak gerçekleştirilen bazı örnekleri aşağıda verilmiştir:

| Orijinal metni | Normalleştirme sonra gelen metin |
|---------------|--------------------------|
| Frankfurter halka | frankfurter halka |
| ¡Eine Frage! | eine frage |
| WIR, haben | WIR haben |

## <a name="next-steps"></a>Sonraki Adımlar

* [Hazırlama ve test, verileri](how-to-custom-speech-test-data.md)
* [Verilerinizi denetleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitin](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtma](how-to-custom-speech-deploy-model.md)
