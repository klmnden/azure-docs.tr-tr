---
title: Özel konuşma hizmeti azure'da transkripsiyonu yönergeleri | Microsoft Docs
description: Özel konuşma hizmeti Bilişsel hizmetler için veri hazırlamayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: nitinme
ms.openlocfilehash: 68abce649b1c16870d98fc9f837e5dae756f63ef
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878553"
---
# <a name="transcription-guidelines"></a>Transkripsiyon yönergeleri

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Akustik ve dil modeli özelleştirme için metin verilerinizi en iyi kullanımı sağlamak için aşağıdaki transkripsiyonu yönergeleri takip edilmelidir. Dile özgü bu yönergelerdir.

## <a name="text-normalization"></a>Metin normalleştirme

Akustik veya dil modeli özelleştirme, en iyi kullanım için metin veri, yani standart, belirsiz forma okunabilir sistem tarafından dönüştürülür normalleştirilmesine gerekir. Bu bölümde, veriler içeri aktarıldığında, özel konuşma hizmeti tarafından gerçekleştirilen metin normalleştirme ve kullanıcı veri içeri aktarma önce gerçekleştirmeniz gereken metin normalleştirme açıklanmaktadır.

## <a name="inverse-text-normalization"></a>Ters metin normalleştirme

"Ham" biçimlendirilmemiş metin büyük/küçük harf ve noktalama işaretleri, yani ile biçimlendirilmiş metin geri dönüştürme işleminin ters metin normalleştirme (edemezsiniz) adı verilir. Microsoft Bilişsel hizmetler konuşma tanıma API'si tarafından döndürülen sonuç edemezsiniz gerçekleştirilir. Özel konuşma hizmeti kullanılarak dağıtılan özel bir uç nokta, Microsoft Bilişsel hizmetler konuşma tanıma API'si aynı edemezsiniz kullanır. Ancak, özel dil modeli tarafından sunulan yeni terimler tanıma sonuçları biçimlendirilmemiş için bu hizmet özel edemezsiniz, şu anda desteklemiyor.

## <a name="transcription-guidelines-for-en-us"></a>En-US için döküm yönergeleri

Bu hizmete yüklenen metin verileri yalnızca yazdırılabilir ASCII kullanarak düz metin olarak yazılması gerektiğini karakter kümesi. Her satır dosyanın yalnızca tek bir utterance metni içermelidir.

Unicode noktalama işaretleri kullanımını önlemek önemlidir. Bu, bir sözcükteki program işleme veya veri web sayfalarından değiştirilene verileri hazırlama, yanlışlıkla oluşabilir. Bu karakterler uygun ASCII değişimler ile değiştirin. Örneğin:

| Unicode önlemek için | ASCII değiştirme |
|----- | ----- |
| "Hello world" (açın ve çift tırnak kapatma) | "Hello world" (çift tırnak) |
| Can'ın gün (sağ tek tırnak işareti) | Can'ın gün (kesme işareti) |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Özel konuşma hizmeti tarafından gerçekleştirilen metin normalleştirme

Bu hizmet, bir dil veri kümesi veya döküm akustik bir veri kümesi için içe aktarılan verilerin üzerinde aşağıdaki metin normalleştirme gerçekleştirir. Bu içerir

*   Küçük büyük/küçük harf tüm metni
*   Tüm noktalama dışında sözcük iç kesme kaldırılıyor
*   Tutarlara dahil olmak üzere, konuşulan forma sayı genişletme

Bazı örnekleri aşağıda verilmiştir

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| Kahve kahve | Kahve kahve |
| "Muhteşem inek!" söz konusu Batman. | Muhteşem inek batman dedi. |
| "Ne olacak?" söz konusu Batman'ın sidekick, bir kez deneme. | hangi söz konusu batman'ın sidekick bir kez deneme |
| Git - em alın! | go get em |
| Double-jointed istiyorum | i çift jointed |
| 104 Main Street | bir oh dört ana Sokak |
| 102.7 için ayarlama | bir oh iki noktası yedi ayarlama |
| PI hakkında 3,14 olan | Pi yaklaşık üç noktası bir dört olduğu |
| $3.14 maliyetleri | üç on dört maliyetleri |

### <a name="text-normalization-required-by-users"></a>Metin normalleştirme kullanıcılar tarafından gerekli

Verilerinizi en iyi kullanımı sağlamak için aşağıdaki normalleştirme kuralları, içeri aktarmadan önce verilerinize uygulanmalıdır.

*   Kısaltmalar sözcükleri konuşulan form yansıtacak şekilde yazılmalıdır
*   Standart sayısal dizeler sözcük yazılır
*   -Alfabetik olmayan karakterler veya karma alfasayısal karakterler ile sözcükleri belirgin olarak transcribed
*   Genel anlamda kısaltmalardan nokta veya boşluk olmadan tek bir varlık olarak arasındaki harfleri bırakılabilir, ancak diğer tüm kısaltmaları ayrı harflerle tek bir boşluk ile ayrılan her bir harfle yazılmalıdır

Bazı örnekleri aşağıda verilmiştir

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| 14 NE 3rd Dr. | on dört northeast üçüncü sürücü |
| Dr. Strangelove | Doktor Strangelove |
| James Bond 007 | James bağlamak çift oh yedi |
| Ke$ha | Kesha |
| 2 x 4 ne kadardır | Tarafından iki dört ne kadardır |
| 1-3'te gelen toplantı gider | Bir ila üç pm toplantı gider |
| My tansiyon O + türüdür | O pozitif My tansiyon türüdür |
| Su H20 olduğu | Su H 2 O olduğu |
| Van Halen tarafından OU812 Yürüt | O U Van Halen 8 1 2 Yürüt |

## <a name="transcription-guidelines-for-zh-cn"></a>Döküm yönergeleri zh-CN

Özel konuşma hizmeti için karşıya bir metin veri kullanması gereken **(BOM dahil) UTF-8 kodlaması**. Her satır dosyanın yalnızca tek bir utterance metni içermelidir.

Yarım genişlikteki noktalama işaretleri kullanımını önlemek önemlidir. Bu, bir sözcükteki program işleme veya veri web sayfalarından değiştirilene verileri hazırlama, yanlışlıkla oluşabilir. Bu karakterler uygun tam genişlikli değişimler ile değiştirin. Örneğin:

| Unicode önlemek için | ASCII değiştirme |
|----- | ----- |
| "你好" (açma ve kapama çift tırnak) | "你好" (çift tırnak) |
| 需要什么帮助? (soru işareti) | 需要什么帮助？ |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Özel konuşma hizmeti tarafından gerçekleştirilen metin normalleştirme

Bu konuşma hizmeti, bir dil veri kümesi veya döküm akustik bir veri kümesi için içe aktarılan verilerin üzerinde aşağıdaki metin normalleştirme gerçekleştirir. Bu içerir

*   Tüm noktalama işaretleri kaldırılıyor
*   Konuşulan forma sayı genişletme
*   Tam genişlikli yarım genişlikteki harfe Dönüştür.
*   Üst büyük/küçük harf tüm sözcükleri, İngilizce

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

### <a name="text-normalization-required-by-users"></a>Metin normalleştirme kullanıcılar tarafından gerekli

Verilerinizi en iyi kullanımı sağlamak için aşağıdaki normalleştirme kuralları verileriniz için uygulanması gereken _önceki_ almadan için.

*   Kısaltmalar sözcükleri konuşulan form yansıtacak şekilde yazılmalıdır
*   Bu hizmet, tüm sayısal miktarları ele alınmamıştır. Sözlü biçimde sayısal dizeler yazamadı daha güvenilirdir

Bazı örnekleri aşağıda verilmiştir

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="transcription-guidelines-for-de-de"></a>De-DE transkripsiyonu yönergeleri

Özel konuşma hizmeti için karşıya metin verileri yalnızca kullanması gereken **(BOM dahil) UTF-8 kodlaması**. Her satır dosyanın yalnızca tek bir utterance metni içermelidir.

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Özel konuşma hizmeti tarafından gerçekleştirilen metin normalleştirme

Bu hizmet, bir dil veri kümesi veya döküm akustik bir veri kümesi için içe aktarılan verilerin üzerinde aşağıdaki metin normalleştirme gerçekleştirir. Bu içerir

*   Küçük büyük/küçük harf tüm metni
*   Tüm noktalama işaretleri ("test", 'test', "test" veya «test» Tamam) İngilizce veya Almanca tırnak işaretleri ile birlikte kaldırılıyor
*   Dahil dilediğiniz özel karakter içeren herhangi bir satırı At: ^ ¢ £ ¤ ¥ ¦ m © ª ¬® ° ± ² µ × ÿ Ø¬¬
*   Dolar veya avro miktarları dahil olmak üzere word forma sayı genişletme
*   Biz yalnızca umlaut kabul etmek için o u; diğerleri "th" ile değiştirilmiştir veya kaldırılacak atıldı

Bazı örnekleri aşağıda verilmiştir

| Orijinal metni | Normalleştirme sonra |
|----- | ----- |
| Frankfurter halka | frankfurter halka |
| "Hallo, Mama!" sagt zar Tochter. | hallo mama sagt zar tochter |
| ¡Eine Frage! | eine frage |
| WIR, haben | WIR haben |
| DAS macht 10 ABD Doları | DAS macht zehn ABD Doları |


### <a name="text-normalization-required-by-users"></a>Metin normalleştirme kullanıcılar tarafından gerekli

Verilerinizi en iyi kullanımı sağlamak için aşağıdaki normalleştirme kuralları, içeri aktarmadan önce verilerinize uygulanmalıdır.

*   Ondalık nokta olmalıdır ve değil. Örneğin, %2,3 ve % 2.3'değil
*   Zaman ayırıcı saat ve dakika arasında olmalıdır: ve değil., örneğin, 12:00 Uhr
*   Kısaltmalar gibi 'ca.', 'bzw.' değiştirilmez. Doğru söylenişi için tam formun kullanılması önerilir.
*   Beş ana matematik işleçleri kaldırılır: +, -, \*, /.
 Değişmez değer formlarına tarafından artı eksi mal, geteilt değiştirmek için öneririz.
*   Aynı durum geçerlidir (=, <>,) karşılaştırıcılar için - gleich, kleiner als, grösser als
*   Kesir 3/4 gibi sözcük formu 'drei viertel' ¾ yerine kullanın
*   € Sembol sözcük formu "Euro" ile değiştirin.


Bazı örnekleri aşağıda verilmiştir

| Orijinal metni | Kullanıcının normalleştirme sonra | Sistem normalleştirme sonra
|--------  | ----- | -------- |
| ES ist 12.23Uhr | ES ist 12:23Uhr | es ist zwölf uhr drei und zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als vier |
| 2 + 3 - 4 | 2 ve 3 4 eksi | zwei plus drei minus vier|
| DAS macht 12€ | DAS macht 12 Euro | das macht zwölf euros |



## <a name="next-steps"></a>Sonraki adımlar
* [Özel bir konuşmayı metne uç noktası kullanma](cognitive-services-custom-speech-create-endpoint.md)
* İle doğruluğunu artırmak, [özel akustik model](cognitive-services-custom-speech-create-acoustic-model.md)
* İle doğruluğunu artırmak bir [özel dil modeli](cognitive-services-custom-speech-create-language-model.md)
