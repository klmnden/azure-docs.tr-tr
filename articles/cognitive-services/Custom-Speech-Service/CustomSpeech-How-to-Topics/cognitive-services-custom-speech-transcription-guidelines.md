---
title: Azure üzerinde özel konuşma hizmetindeki transcription yönergeleri | Microsoft Docs
description: Bilişsel hizmetler özel konuşma hizmet için verilerinizi hazırlama hakkında bilgi edinin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 2785a35ac7583ac3d9503cb721d10078d86aa365
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351832"
---
# <a name="transcription-guidelines"></a>Transcription yönergeleri
Metin verilerinizi kullanım ve dil modeli özelleştirmesi için en iyi kullanımı sağlamak için aşağıdaki transcription yönergeleri gelmelidir. Bu dile özgü yönergelerdir.

## <a name="text-normalization"></a>Metin normalleştirme

Kullanım veya dil modeli özelleştirmesinde en iyi kullanımı için metin verileri, başka bir deyişle, sistem tarafından standart, anlaşılır forma okunabilir dönüştürülmüş normalleştirilmiş gerekir. Bu bölümde, verileri içe aktarıldığında özel konuşma hizmeti tarafından gerçekleştirilen metin normalleştirme ve kullanıcı veri içeri aktarma öncesinde gerçekleştirmelisiniz metin normalleştirme açıklanmaktadır.

## <a name="inverse-text-normalization"></a>Ters metin normalleştirme

Biçimlendirilmiş metin, yani büyük/küçük harf ve noktalama işaretleri, "ham" biçimlendirilmemiş metin geri dönüştürme işlemi ters metin normalleştirme (ITN) adı verilir. ITN Microsoft Bilişsel Hizmetleri konuşma API tarafından döndürülen sonuçları gerçekleştirilir. Özel konuşma hizmet kullanılarak dağıtılan özel bir uç nokta aynı ITN Microsoft Bilişsel Hizmetleri konuşma API kullanır. Ancak, özel dil modeli tarafından sunulan yeni terimler tanıma sonuçlarında biçimlendirilmemiş şekilde bu hizmete özel ITN şu anda desteklemiyor.

## <a name="transcription-guidelines-for-en-us"></a>En-us transcription yönergeleri

Bu hizmete yüklenen metin veriler düz metin halinde yalnızca yazdırılabilir ASCII kullanılarak yazılan karakter kümesi. Her satır dosyanın yalnızca tek bir utterance için metin içermelidir.

Unicode noktalama karakterleri kullanımını önlemek önemlidir. Bu yanlışlıkla Word'de program işleme veya web sayfalarından veri değiştirilene verileri hazırlama durum meydana gelebilir. Bu karakterleri uygun ASCII değişimler ile değiştirin. Örneğin:

| Unicode önlemek için | ASCII değiştirme |
|----- | ----- |
| "Hello world" (açın ve çift tırnak işareti Kapat) | "Hello world" (çift tırnak) |
| Can'ın gün (sağ tek tırnak işareti) | Can'ın gün (kesme) |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Özel konuşma hizmeti tarafından gerçekleştirilen metin normalleştirme

Bu hizmet, bir dil veri kümesi veya akustik bir veri kümesi için transcriptions olarak içe aktarılan verilerin üzerinde aşağıdaki metin normalleştirme gerçekleştirir. Bu içerir

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

### <a name="text-normalization-required-by-users"></a>Metin normalleştirme kullanıcılar tarafından gerekli

Verilerinizi en iyi kullanımı sağlamak için aşağıdaki normalleştirme kurallarını içe aktarmadan önce verilerinize uygulanmalıdır.

*   Kısaltmalar sözcükleri Konuşma form yansıtacak şekilde yazılıp
*   Standart sayısal dizeleri sözcükleri yazılıp
*   Alfabetik olmayan karakterler veya karma alfasayısal karakterler içeren sözcükleri belirgin olarak transcribed
*   Ortak kısaltmalar nokta veya boşluklardan olmadan tek bir varlık olarak harfler arasında bırakılabilir, ancak diğer tüm kısaltmalar ayrı harflerle tek boşlukla ayırarak her harfle yazılmalıdır

İşte bazı örnekler

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| 14 NE 3 Dr. | on dört Kuzeydoğu yönüne üçüncü sürücü |
| Dr. Strangelove | Doctor Strangelove |
| Ahmet bono 007 | Ahmet bağlamak çift yedi |
| Ke$ ha | Kesha |
| 2 x 4 uzunluğu nedir | Tarafından iki dört uzunluğu nedir |
| 1-3 pm gelen toplantı gider | Bir ile üç pm toplantı gider |
| My kan O + türüdür | O pozitif My kan türüdür |
| Su H20 olduğu | Su H 2 O olduğu |
| OU812 Van Halen tarafından Yürüt | O U 8 1 2 Van Halen tarafından Yürüt |

## <a name="transcription-guidelines-for-zh-cn"></a>Zh-CN transcription yönergeleri

Özel konuşma hizmete yüklenen metin veriler kullanması gereken **(ürün reçetesi dahil) UTF-8 kodlaması**. Her satır dosyanın yalnızca tek bir utterance için metin içermelidir.

Yarı genişlikli noktalama karakterleri kullanımını önlemek önemlidir. Bu yanlışlıkla Word'de program işleme veya web sayfalarından veri değiştirilene verileri hazırlama durum meydana gelebilir. Bu karakterleri uygun tam genişlikli değişimler ile değiştirin. Örneğin:

| Unicode önlemek için | ASCII değiştirme |
|----- | ----- |
| "你好" (açma ve kapama çift tırnak) | "你好" (tırnak) |
| 需要什么帮助? (soru işareti) | 需要什么帮助? |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Özel konuşma hizmeti tarafından gerçekleştirilen metin normalleştirme

Bu konuşma hizmeti, bir dil veri kümesi veya akustik bir veri kümesi için transcriptions olarak içe aktarılan verilerin üzerinde aşağıdaki metin normalleştirme gerçekleştirir. Bu içerir

*   Tüm noktalama kaldırma
*   Konuşma forma sayıların genişletme
*   Tam genişlikli harfleri yarı genişlikli harfe Dönüştür.
*   Üst büyük/küçük harf tüm İngilizce sözcükler

İşte bazı örnekler:

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-required-by-users"></a>Metin normalleştirme kullanıcılar tarafından gerekli

Verilerinizi en iyi kullanımı sağlamak için aşağıdaki normalleştirme kuralları verileriniz için uygulanması gereken _önceki_ almadan için.

*   Kısaltmalar sözcükleri Konuşma form yansıtacak şekilde yazılıp
*   Bu hizmet tüm sayısal miktarları kapsamaz. Sayısal dizeler konuşulan formunda yazamadı daha güvenilirdir

İşte bazı örnekler

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="transcription-guidelines-for-de-de"></a>De-DE transcription yönergeleri

Özel konuşma hizmete yüklenen metin veriler yalnızca kullanması gereken **(ürün reçetesi dahil) UTF-8 kodlaması**. Her satır dosyanın yalnızca tek bir utterance için metin içermelidir.

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Özel konuşma hizmeti tarafından gerçekleştirilen metin normalleştirme

Bu hizmet, bir dil veri kümesi veya akustik bir veri kümesi için transcriptions olarak içe aktarılan verilerin üzerinde aşağıdaki metin normalleştirme gerçekleştirir. Bu içerir

*   Küçük büyük/küçük harf tüm metni
*   İngilizce veya Almanca tırnak ("test", 'test', "test" veya «test» Tamam) dahil olmak üzere tüm noktalama kaldırma
*   Herhangi bir özel karakter dahil içeren satırı At: ^ ¢ £ ¤ ¥ ¦ m © ª ¬® ° ± ² µ × ÿ Ø¬¬
*   Dolar veya avro tutarlar dahil olmak üzere word forma sayıların genişletme
*   Biz yalnızca umlaut kabul etmek için o u; Başkalarının "th" tarafından değiştirilen veya kaldırılacak atıldı

İşte bazı örnekler

| Özgün metin | Normalleştirme sonra |
|----- | ----- |
| Frankfurter halkası | frankfurter halkası |
| "Hallo, Mama!" sagt dökme Tochter. | hallo mama sagt dökme tochter |
| ¡Eine Frage! | eine frage |
| WIR, haben | WIR haben |
| DAS macht $10 | DAS macht zehn dolar |


### <a name="text-normalization-required-by-users"></a>Metin normalleştirme kullanıcılar tarafından gerekli

Verilerinizi en iyi kullanımı sağlamak için aşağıdaki normalleştirme kurallarını içe aktarmadan önce verilerinize uygulanmalıdır.

*   Ondalık ayırıcının olmalıdır ve değil. Örneğin, %2,3 ve % 2.3'değil
*   Zaman ayırıcı saat ve dakika arasında olmalıdır: ve., örn., 12:00 Uhr
*   Kısaltmalar gibi 'ca.', 'bzw.' değiştirilmez. Doğru telaffuz olması için tam formun kullanılması önerilir.
*   Beş ana matematik işleçleri kaldırılır: +, -, \*, /.
 Değişmez değer formlarına tarafından artı ve eksi mal, geteilt değiştirmek için öneririz.
*   Aynı durum geçerlidir (=, <>,) karşılaştırıcıları için - gleich, kleiner als, grösser als
*   3/4 gibi kesir word formu ¾ yerine ' drei viertel' kullanın.
*   Word formu "Euro" € sembolü Değiştir


İşte bazı örnekler

| Özgün metin | Kullanıcının normalleştirme sonra | Sistem normalleştirme sonra
|--------  | ----- | -------- |
| ES ist 12.23Uhr | ES ist 12:23Uhr | ES ist zwölf uhr drei çizili zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als vier |
| 2 + 3-4 | 2 ve 3 4 eksi | zwei artı drei vier eksi|
| DAS macht 12€ | DAS macht 12 Euro | DAS macht zwölf Euro |



## <a name="next-steps"></a>Sonraki adımlar
* [Özel bir konuşma metin uç noktası kullanma](cognitive-services-custom-speech-create-endpoint.md)
* İle doğruluğunu artırmak için [özel akustik modeli](cognitive-services-custom-speech-create-acoustic-model.md)
* İle doğruluğunu artırmak bir [özel dil modeli](cognitive-services-custom-speech-create-language-model.md)
