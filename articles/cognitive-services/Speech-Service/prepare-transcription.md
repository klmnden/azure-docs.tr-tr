---
title: Konuşma Hizmetleri modellerin eğitimi transkripsiyonu yönergeleri
titleSuffix: Azure Cognitive Services
description: Akustik ve dil modellerini ve sesler konuşma Hizmetleri özelleştirmek için metin hazırlamayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: panosper
ms.openlocfilehash: 0d7508ed9cf1807fa05c57a1d60c804af7d2244f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61461451"
---
# <a name="transcription-guidelines-for-using-the-speech-service"></a>Konuşma hizmeti kullanarak transkripsiyonu yönergeleri

Özelleştirme için **Konuşmayı metne dönüştürme** veya **metin okuma**, konuşma yanı sıra metin sağlamanız gerekir. Metnin her satır için tek bir utterance karşılık gelir. Metni konuşma mümkün olduğunca eşleşmelidir. Metin adlı bir *döküm*, belirli bir biçimde oluşturmanız gerekir.

Konuşma Hizmetleri metin tutarlı tutmak için giriş Normalleştir.

Bu makalede her iki türdeki normalizations açıklanır. Yönergeler, çeşitli diller için biraz farklılık gösterir.

## <a name="us-english-en-us"></a>ABD İngilizce (en-us)

Metin veri her satıra bir utterance yalnızca ASCII karakter kümesi kullanılarak düz metin olarak yazılması gerekir.

Genişletilmiş (Latin-1) veya Unicode noktalama karakterleri kullanmaktan kaçının. Bir sözcük program uygulamasında veri hazırlama ya da veri sayfalarından scrape şu karakterleri yanlışlıkla dahil edilebilir. Karakterleri uygun ASCII değişimler ile değiştirin. Örneğin:

| Önlemek için karakter | Değiştirme |
|----- | ----- |
| "(Açma ve kapama çift tırnak işareti) Merhaba Dünya" | "Hello world" (çift tırnak) |
| Can'ın gün (sağ tek tırnak işareti) | Can'ın gün (kesme işareti) |
| iyi — Hayır, harika! (em dash) | en iyi--, harika! (kısa) |

### <a name="text-normalization-rules-for-english"></a>İngilizce için metin normalleştirme kuralları

Konuşma Hizmetleri aşağıdaki normalleştirme kurallarını izleyin:

* Küçük harfler kullanarak tüm metni
* Tüm noktalama dışında sözcük iç kesme kaldırılıyor
* Sayı konuşulan forma tutarlara dahil olmak üzere genişletme

İşte bazı örnekler:

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| "Muhteşem inek!" söz konusu Batman. | Muhteşem inek batman dedi. |
| "Ne olacak?" söz konusu Batman'ın sidekick, bir kez deneme. | hangi söz konusu batman'ın sidekick bir kez deneme |
| Git - em alın! | go get em |
| Double-jointed istiyorum | Çift jointed istiyorum |
| 104 Elm Street | bir oh dört istasyon Sokak |
| 102.7 için ayarlama | bir oh ayarlama iki yedi |
| PI hakkında 3,14 olan | Pi yaklaşık üç noktası bir dört olduğu |
| $3.14 maliyetleri | üç on dört maliyetleri |

Aşağıdaki normalleştirme, metin dökümleri için geçerlidir:

* Kısaltmalar sözcük yazılır.
* Standart sayısal dizeler (örneğin, bazı tarih veya hesap formlar) sözcük yazılır.
* -Alfabetik olmayan karakterler veya karma alfasayısal karakterler ile sözcükleri belirgin olarak transcribed.
* Sözcükler olarak tanınmıyor telaffuz kısaltmalar bırakın (örneğin, "radar" "Lazer," "RAM" veya "NATO").
* Boşluklarla ayırarak harf ile ayrı olarak telaffuz kısaltmalar harflerini yazın (örneğin, "IBM" "CPU," "FBI," "TBD" veya "NaN").

İşte bazı örnekler:

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| 14 NE 3rd Dr. | on dört northeast üçüncü sürücü |
| Dr. Bruce başlığı | Hastanede Bruce başlığı |
| James Bond, 007 | James Bond çift oh yedi |
| Ke$ha | Kesha |
| 2 x 4 ne kadardır | Tarafından iki dört ne kadardır |
| 1-3'te gelen toplantı gider | Bir ila üç pm toplantı gider |
| My tansiyon O + türüdür | O pozitif My tansiyon türüdür |
| Su H20 olduğu | Su H 2 O olduğu |
| Van Halen tarafından OU812 Yürüt | O U Van Halen 8 1 2 Yürüt |
| BOM ile UTF-8 | BOM ile U T F 8 |

## <a name="chinese-zh-cn"></a>Çince (zh-cn)

Özel Konuşma hizmetlerine yüklenen metin verileri, bir bayt sırası işareti ile UTF-8 kodlamasını kullanmanız gerekir. Dosya her satırda bir utterance yazılması gerekir.

Yarım genişlikteki noktalama karakterleri kullanmaktan kaçının. Bir sözcük program uygulamasında veri hazırlama ya da veri sayfalarından scrape şu karakterleri yanlışlıkla dahil edilebilir. Bunları uygun tam genişlikli değişimler ile değiştirin. Örneğin:

| Önlemek için karakter | Değiştirme |
|----- | ----- |
| "(Açma ve kapama çift tırnak işareti) 你好" | "你好" (çift tırnak) |
| 需要什么帮助? (soru işareti) | 需要什么帮助？ |

### <a name="text-normalization-rules-for-chinese"></a>Çince için metin normalleştirme kuralları

Konuşma Hizmetleri aşağıdaki normalleştirme kurallarını izleyin:

* Tüm noktalama işaretleri kaldırılıyor
* Konuşulan forma sayı genişletme
* Tam genişlikli harf yarım genişlikteki harfe dönüştürme
* Büyük harfler için tüm Türkçe kelimeler kullanarak

İşte bazı örnekler:

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y, Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

Metninizi içeri aktarmadan önce aşağıdaki normalleştirme uygulayabilir:

* (Olduğu gibi konuşulan formu) sözcüklerdeki kısaltmalar yazılır.
* Sayısal dizeler sözlü biçimde dışarı yazın.

İşte bazı örnekler:

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Diğer diller

Metin veri tabanına **Konuşmayı metne dönüştürme** hizmeti bir bayt sırası işareti ile UTF-8 kodlamasını kullanmanız gerekir. Dosya her satırda bir utterance yazılması gerekir.

> [!NOTE]
> Aşağıdaki örnekler, Almanca kullanın. Ancak, ABD İngilizcesi veya Çince olmayan tüm diller için yönergeleri uygulayın.

### <a name="text-normalization-rules-for-german"></a>Almanca için metin normalleştirme kuralları

Konuşma Hizmetleri aşağıdaki normalleştirme kurallarını izleyin:

* Küçük harfler kullanarak tüm metni
* Çeşitli tırnak işaretleri dahil olmak üzere tüm noktalama kaldırma ("test", 'test', "test" ve «test» edilir)
* Satır kümesi ¢ herhangi bir özel karakterin ile atılıyor ¤ ¥ ¦ m © ª ¬® ° ± ² µ × ÿ Ø¬¬
* Genişleyen sayılara dolar veya Euro miktarları dahil olmak üzere, sözcük formu
* Kabul umlaut o yalnızca için ve; diğerleri "th" değiştirilir veya iptal edilecek

İşte bazı örnekler:

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| Frankfurter halka | frankfurter halka |
| ¡Eine Frage! | eine frage |
| WIR, haben | WIR haben |

Metninizi içeri aktarmadan önce aşağıdaki normalleştirme uygulayabilir:

* Ondalık basamak olmalıdır ","ve".".
* Saat ayırıcısı saat ve dakika arasında olmalıdır. ":"ve"." (örneğin, 12:00 Uhr).
* Kısaltmalar gibi "ca". Değiştirilen değildir. Tam biçimini kullanmanızı öneririz.
* Dört ana matematik işleçleri (+, -, \*, ve /) kaldırılır. Değişmez değer formlarına değiştirmeden öneririz: "Ayrıca," "eksi," "kötü" ve "geteilt."
* Karşılaştırma işleçleri için aynı kural uygular (=, <, ve >). "Gleich ile" değiştirmeden öneririz "kleiner als," ve "grösser als."
* 3/4 gibi kesirler sözcük formu (örneğin, "drei viertel" ¾ yerine) kullanın.
* € Sembol sözcük formu "Euro." ile değiştirin.

İşte bazı örnekler:

| Orijinal metni | Kullanıcının normalleştirme sonra | Sistem normalleştirme sonra |
|--------  | ----- | -------- |
| ES ist 12.23 Uhr | ES ist 12:23 Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 2 + 3 - 4 | 2 ve 3 4 eksi | zwei plus drei minus vier|

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Tanıma Hizmetleri deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# dilinde konuşma tanıma](quickstart-csharp-dotnet-windows.md)
