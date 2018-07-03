---
title: Eğitim konuşma transkripsiyonu yönergeleri | Microsoft Docs
description: Akustik özelleştirmek için metin ve dil modellerini ve ses tiplerini konuşma hizmeti için hazırlamayı öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: PanosPeriorellis
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/01/2018
ms.author: panosper
ms.openlocfilehash: acca6f4cd2f4e7b452f5a70457d3d034e4d4aa7e
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345194"
---
# <a name="transcription-guidelines-for-using-speech-service"></a>Konuşma hizmeti kullanarak transkripsiyonu yönergeleri

Özelleştirme için **Konuşmayı metne dönüştürme** veya **metin okuma**, konuşma yanı sıra metin sağlamanız gerekir. Metnin her satır için tek bir utterance karşılık gelir. Metni konuşma gibi tam olarak eşleşmelidir mümkün olduğunca. Metin adlı bir *döküm*, belirli bir biçimde oluşturmanız gerekir.

Konuşma hizmeti, girdi metin tutarlı tutmak normalleştirir. 

Bu makalede her iki türdeki normalizations açıklanır. Yönergeler, çeşitli diller için biraz farklılık gösterir.

## <a name="us-english-en-us"></a>ABD İngilizce (en-US)

Metin veri her satıra bir utterance yalnızca ASCII karakter kümesi kullanılarak düz metin olarak yazılması gerekir.

Genişletilmiş (Latin-1) veya Unicode noktalama karakterleri kullanmaktan kaçının. Bu karakterler yanlışlıkla bir sözcük program veya web sayfalarından veri değiştirilene verileri hazırlanırken dahil edilebilir. Bu karakterler uygun ASCII değişimler ile değiştirin. Örneğin:

| Önlemek için karakter | Değiştirme |
|----- | ----- |
| "Hello world" (açın ve çift tırnak kapatma) | "Hello world" (çift tırnak) |
| Can'ın gün (sağ tek tırnak işareti) | Can'ın gün (kesme işareti) |
| iyi — Hayır, harika! (em dash) | en iyi--, harika! (kısa) |

### <a name="text-normalization-rules-for-english"></a>İngilizce için metin normalleştirme kuralları

Konuşma hizmeti aşağıdaki normalleştirme kurallarını taşır.

*   Küçük büyük/küçük harf tüm metni
*   Tüm noktalama dışında sözcük iç kesme kaldırılıyor
*   Tutarlara dahil olmak üzere, konuşulan forma sayı genişletme

Bazı örnekleri aşağıda verilmiştir

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| "Muhteşem inek!" söz konusu Batman. | Muhteşem inek batman dedi. |
| "Ne olacak?" söz konusu Batman'ın sidekick, bir kez deneme. | hangi söz konusu batman'ın sidekick bir kez deneme |
| Git - em alın! | go get em |
| Double-jointed istiyorum | Çift jointed istiyorum |
| 104 istasyon Sokak | bir oh dört istasyon Sokak |
| 102.7 için ayarlama | bir oh ayarlama iki yedi |
| PI hakkında 3,14 olan | Pi yaklaşık üç noktası bir dört olduğu |
| $3.14 maliyetleri | üç on dört maliyetleri |

Aşağıdaki normalleştirme, metin dökümleri için geçerlidir.

*   Kısaltmalar sözcük yazılır
*   Standart sayısal dizeler (örneğin, bazı tarih veya hesap formlar) sözcükleri yazılır
*   -Alfabetik olmayan karakterler veya karma alfasayısal karakterler ile sözcükleri belirgin olarak transcribed
*   Sözcükler olarak tanınmıyor telaffuz kısaltmalar bırakın. Örneğin, radar Lazer, RAM, NATO.
*   Boşluklarla ayırarak harflerle ayrı harf olarak telaffuz kısaltmalar yazın. Örneğin, IBM, CPU, FBI, TBD, NaN. 

İşte bazı örnekler:

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| 14 NE 3 Dr. | on dört northeast üçüncü sürücü |
| Dr. Bruce başlığı | Hastanede Bruce başlığı |
| James Bond, 007 | James Bond çift oh yedi |
| Ke$ ha | Kesha |
| 2 x 4 ne kadardır | Tarafından iki dört ne kadardır |
| 1-3'te gelen toplantı gider | Bir ila üç pm toplantı gider |
| My tansiyon O + türüdür | O pozitif My tansiyon türüdür |
| Su H20 olduğu | Su H 2 O olduğu |
| Van Halen tarafından OU812 Yürüt | O U Van Halen 8 1 2 Yürüt |
| BOM ile UTF-8 | BOM ile U T F 8 |

## <a name="chinese-zh-cn"></a>Çince (zh-CN)

Metin veri özel konuşma hizmeti için karşıya yüklenen bayt sırası işareti ile UTF-8 kodlamasını kullanmanız gerekir. Dosya her satırda bir utterance yazılması gerekir.

Yarım genişlikteki noktalama karakterleri kullanmaktan kaçının. Bu karakterler yanlışlıkla bir sözcük program veya web sayfalarından veri değiştirilene verileri hazırlanırken dahil edilebilir. Bunları uygun tam genişlikli değişimler ile değiştirin. Örneğin:

| Önlemek için karakter | Değiştirme |
|----- | ----- |
| "你好" (açma ve kapama çift tırnak) | "你好" (çift tırnak) |
| 需要什么帮助? (soru işareti) | 需要什么帮助? |

### <a name="text-normalization-rules-for-chinese"></a>Çince için metin normalleştirme kuralları

Konuşma hizmeti aşağıdaki normalleştirme kurallarını taşır.

*   Tüm noktalama işaretleri kaldırılıyor
*   Konuşulan forma sayı genişletme
*   Tam genişlikli harf yarım genişlikteki harfe dönüştürme
*   Üst büyük/küçük harf tüm sözcükleri, İngilizce

Bazı örnekler aşağıda verilmiştir.

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y, Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

Aşağıdaki normalleştirme, içeri aktarmadan önce metne uygulayın.

*   Kısaltmalar (olduğu gibi konuşulan formu) sözcüklerdeki yazılır
*   Sayısal dizeler sözlü biçimde dışarı yazın.

Bazı örnekler aşağıda verilmiştir.

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Diğer diller

Metin veri tabanına **Konuşmayı metne dönüştürme** hizmet UTF-8 kodlamasını bayt sırası işareti ile kullanmanız gerekir. Dosya her satırda bir utterance yazılması gerekir.

> [!NOTE]
> Bu örnekler, Almanca kullanın. Ancak, ABD İngilizcesi veya Çince olmayan tüm diller için bu yönergeleri uygulayın.

### <a name="text-normalization-rules-for-german"></a>Almanca için metin normalleştirme kuralları

Konuşma hizmeti aşağıdaki normalleştirme kurallarını taşır.

*   Küçük büyük/küçük harf tüm metni
*   Çeşitli tırnak işaretleri dahil olmak üzere tüm noktalama işaretleri kaldırma ("test", 'test', "test" veya «test» Tamam)
*   Satır kümesi ¢ herhangi bir özel karakterin ile atılıyor ¤ ¥ ¦ m © ª ¬® ° ± ² µ × ÿ Ø¬¬
*   Dolar veya avro miktarları dahil olmak üzere word forma sayı genişletme
*   Umlaut o yalnızca için kabul edilir; u diğerleri "th" değiştirilir veya iptal edilecek

Bazı örnekleri aşağıda verilmiştir

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| Frankfurter halka | frankfurter halka |
| ¡Eine Frage! | eine frage |
| WIR, haben | WIR haben |

Aşağıdaki normalleştirme, içeri aktarmadan önce metne uygulayın.

*   Ondalık nokta olmalıdır ","ve"."
*   Zaman ayırıcı saat ve dakika arasında olmalıdır. ":"ve".": 12:00 Uhr
*   Kısaltmalar gibi 'ca.' Değiştirilen değildir. Tam biçimini kullanmanızı öneririz.
*   Beş ana matematik işleçleri kaldırılır: +, -, \*, /. Değişmez değer formlarına değiştirmeden öneririz: artı eksi, mal, geteilt.
*   Aynı durum geçerlidir (=, <>,) Karşılaştırma işleçleri - gleich, kleiner als, grösser als
*   3/4 gibi kesirler sözcük formu (örneğin, 'drei viertel' ¾ yerine) kullanın
*   € Sembol sözcük formu "Euro" ile değiştirin.

Bazı örnekler aşağıda verilmiştir.

| Orijinal metni | Kullanıcının normalleştirme sonra | Sistem normalleştirme sonra
|--------  | ----- | -------- |
| ES ist 12.23Uhr | ES ist 12:23Uhr | ES ist zwölf uhr drei çizili zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf ||
| 2 + 3-4 | 2 ve 3 4 eksi | zwei artı eksi vier drei|

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
- [C# ' de Konuşma tanıma](quickstart-csharp-windows.md)
