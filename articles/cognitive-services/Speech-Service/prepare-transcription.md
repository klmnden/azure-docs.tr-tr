---
title: Konuşma eğitim transcription yönergeleri | Microsoft Docs
description: Akustik özelleştirmek için metin ve dil modelleri ve ses yazı tiplerini konuşma hizmeti hazırlamanızı öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 93ab7c81a773f692b2b970bb1901d82b7aceb5a2
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356154"
---
# <a name="transcription-guidelines-for-using-speech-service"></a>Konuşma hizmetini kullanarak transcription yönergeleri

Özelleştirmek için **metin konuşma** veya **metin okuma**, konuşma birlikte metin sağlamanız gerekir. Metnin her satır için tek bir utterance karşılık gelir. Metin olarak adlandırılan bir *dökümü*, ve belirli bir biçimde oluşturmanız gerekir.

Konuşma hizmeti metninizi tutarlı hale getirmek için bazı normalizations sizin için gerçekleştirir. Metin için eğitim gönderilmeden önce diğer normalleştirme görevleri gerçekleştirilmesi gerekir. 

Bu makalede her iki tür normalizations açıklanmaktadır. Yönergeleri, çeşitli diller için biraz farklılık gösterir.

## <a name="us-english-en-us"></a>Türkçe (tr-tr)

Bu hizmete yüklenen metin veriler yazılması düz metin olarak kullanarak yalnızca ASCII karakter kümesi. Her satır dosyanın tek utterance için metin içermelidir.

Genişletilmiş (Latin-1) veya Unicode noktalama karakterleri kullanımını önlemek önemlidir. Bu karakterleri yanlışlıkla bir sözcük işlem programı veya web sayfalarından veri değiştirilene verileri hazırlanırken eklenebilir. Bu karakterleri uygun ASCII değişimler ile değiştirin. Örneğin:

| Önlemek için karakter | Değiştirme |
|----- | ----- |
| "Hello world" (açın ve çift tırnak işareti Kapat) | "Hello world" (çift tırnak) |
| Can'ın gün (sağ tek tırnak işareti) | Can'ın gün (kesme) |
| iyi — Hayır, harika! (em tire) | en iyi--, harika! (tire) |

### <a name="text-normalization-performed-by-the-service"></a>Hizmet tarafından gerçekleştirilen metin normalleştirme

Konuşma hizmeti aşağıdaki metin normalleştirme metin dökümleri üzerinde gerçekleştirir.

*   Küçük büyük/küçük harf tüm metni
*   Word iç kesme dışındaki tüm noktalama kaldırma
*   Konuşma forma dolar tutarlar dahil olmak üzere sayıların genişletme

İşte bazı örnekler

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| Starbucks kahve | starbucks kahve |
| "Muhteşem inek!" konusu Batman. | Muhteşem inek batman söyledi |
| "Ne?" konusu Batman'ın sidekick, bir kez deneme. | hangi konusu batman'ın sidekick deneme |
| Git get - em! | Git get em |
| Double-jointed ben | i çift jointed |
| 104 ana Sokak | bir dört ana Sokak |
| 102.7 için ayarlama | birine iki nokta yedi ayarlama |
| Pi hakkında 3,14 olan | Pi yaklaşık üç noktası bir dört olduğu |
| $3,14 maliyetleri | üç on dört maliyetleri |

### <a name="text-normalization-you-must-perform"></a>Metin normalleştirme gerçekleştirmeniz gerekir

Aşağıdaki normalleştirme, metin dökümleri uygulayın.

*   Kısaltmalar sözcükleri Konuşma form yansıtacak şekilde yazılıp
*   Standart sayısal dizeleri (örneğin, bazı tarih veya hesap formlar) sözcükleri yazılıp
*   Alfabetik olmayan karakterler veya karma alfasayısal karakterler içeren sözcükleri belirgin olarak transcribed
*   Sözcük olarak dokunulmadan belirgin kısaltmalar bırakın. Örneğin, radar, lazer, RAM, NATO ve Mr.
*   Boşluklarla ayırarak harflerle ayrı harf olarak belirgin kısaltmalar yazma. Örneğin, IBM, CPU, FBI, henüz Belirlenmedi, NaN. 

İşte bazı örnekler:

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| 14 NE 3 Dr. | on dört Kuzeydoğu yönüne üçüncü sürücü |
| Dr. Strangelove | Doctor Strangelove |
| Ahmet bono 007 | Ahmet bono çift yedi |
| Ke$ ha | Kesha |
| 2 x 4 uzunluğu nedir | Tarafından iki dört uzunluğu nedir |
| 1-3 pm gelen toplantı gider | Bir ile üç pm toplantı gider |
| My kan O + türüdür | O pozitif My kan türüdür |
| Su H20 olduğu | Su H 2 O olduğu |
| OU812 Van Halen tarafından Yürüt | O U 8 1 2 Van Halen tarafından Yürüt |
| UTF-8 ile AĞACI | U T F 8 AĞACI ile |

## <a name="chinese-zh-cn"></a>Çince (zh-CN)

Özel konuşma hizmete karşıya metin veri bayt sırası işareti ile UTF-8 kodlamasını kullanmanız gerekir. Her satır dosyanın tek utterance için metin içermelidir.

Yarı genişlikli noktalama karakterleri kullanımını önlemek önemlidir. Bu karakterleri yanlışlıkla bir sözcük işlem programı veya web sayfalarından veri değiştirilene verileri hazırlanırken eklenebilir. Bunları uygun tam genişlikli değişimler ile değiştirin. Örneğin:

| Önlemek için karakter | Değiştirme |
|----- | ----- |
| "你好" (açma ve kapama çift tırnak) | "你好" (tırnak) |
| 需要什么帮助? (soru işareti) | 需要什么帮助? |

### <a name="text-normalization-performed-by-the-service"></a>Hizmet tarafından gerçekleştirilen metin normalleştirme

Konuşma hizmeti aşağıdaki metin normalleştirme metin dökümleri üzerinde gerçekleştirir.

*   Tüm noktalama kaldırma
*   Konuşma forma numaraları genişletme
*   Tam genişlikli harf yarı genişlikli harf dönüştürme
*   Üst büyük/küçük harf tüm İngilizce sözcükler

Bazı örnekler aşağıda verilmiştir.

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-you-must-perform"></a>Metin normalleştirme gerçekleştirmeniz gerekir

Aşağıdaki normalleştirme almadan önce metninizi için geçerlidir.

*   Kısaltmalar sözcükleri Konuşma form yansıtacak şekilde yazılıp
*   Bu hizmet tüm sayısal miktarları kapsamaz. Konuşma form sayısal dizeleri çıkışı yazma daha güvenilirdir.

Bazı örnekler aşağıda verilmiştir.

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Diğer diller

Metin veri tabanına **metin konuşma** hizmeti ile bayt sırası işaret UTF-8 kodlamasını kullanmanız gerekir. Her satır dosyanın tek utterance için metin içermelidir.

> [!NOTE]
> Bu örnekler Almanca kullanın. Ancak, bu yönergeler, ABD İngilizcesi veya Çince olmayan tüm diller için geçerlidir.

### <a name="text-normalization-performed-by-the-service"></a>Hizmet tarafından gerçekleştirilen metin normalleştirme

Konuşma hizmeti aşağıdaki metin normalleştirme metin dökümleri üzerinde gerçekleştirir.

*   Küçük büyük/küçük harf tüm metni
*   Çeşitli türlerde tırnak işaretleri dahil olmak üzere tüm noktalama kaldırma ("test", 'test', "test" veya «test» Tamam)
*   Özel karakter kümesinden içeren herhangi bir satır atılıyor ^ ¢ £ ¤ ¥ ¦ m © ª ¬® ° ± ² µ × ÿ Ø¬¬
*   Dolar veya avro tutarlar dahil olmak üzere word forma sayıların genişletme
*   Umlaut o için yalnızca kabul u; Başkalarının "tarafından th" değiştirilecek veya iptal

İşte bazı örnekler

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| Frankfurter halkası | frankfurter halkası |
| "Hallo, Mama!" sagt dökme Tochter. | hallo mama sagt dökme tochter |
| ¡Eine Frage! | eine frage |
| WIR, haben | WIR haben |
| DAS macht $10 | DAS macht zehn dolar |

### <a name="text-normalization-you-must-perform"></a>Metin normalleştirme gerçekleştirmeniz gerekir

Aşağıdaki normalleştirme almadan önce metninizi için geçerlidir.

*   Ondalık ayırıcının olmalıdır ","ve".": %2,3 ve % 2.3'değil
*   Zaman ayırıcı saat ve dakika arasında olmalıdır ":"ve".": 12:00 Uhr
*   Kısaltmalar gibi 'ca.', 'bzw.' değiştirilmez. Tam biçimini kullanmanızı öneririz.
*   Beş ana matematik işleçleri kaldırılır: +, -, \*, /. Değişmez değer formlarına ile değiştirerek öneririz: artı ve eksi mal, geteilt.
*   Aynı durum geçerlidir (=, <>,) Karşılaştırma işleçleri - gleich, kleiner als, grösser als
*   3/4 gibi kesirler word biçiminde (örneğin, 'drei viertel' ¾ yerine) kullanın.
*   Word formu "Euro" € sembolü Değiştir

Bazı örnekler aşağıda verilmiştir.

| Özgün metin | Kullanıcının normalleştirme sonra | Sistem normalleştirme sonra
|--------  | ----- | -------- |
| ES ist 12.23Uhr | ES ist 12:23Uhr | ES ist zwölf uhr drei çizili zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als vier |
| 2 + 3-4 | 2 ve 3 4 eksi | zwei artı drei vier eksi|
| DAS macht 12€ | DAS macht 12 Euro | DAS macht zwölf Euro |

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
- [C# Konuşma tanıması](quickstart-csharp-windows.md)
