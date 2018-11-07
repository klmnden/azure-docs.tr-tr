---
title: Önceden oluşturulmuş varlıklar - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUIS), dahil edilen önceden oluşturulmuş varlıklar listesi içerir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: bc23f2e5d8304400802c74093d4d78a1fcd8cf22
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219503"
---
# <a name="entities-per-culture"></a>Kültür başına varlıklar

Language Understanding (LUIS), önceden oluşturulmuş varlıklar sağlar. Uygulamanızda önceden oluşturulmuş bir varlık eklendiğinde LUIS uç nokta yanıtta ilgili varlık öngörü içerir. Tüm örnek konuşma de sahip bir varlık olarak etiketlenmiştir. Önceden oluşturulmuş varlıkların **olamaz** değiştirilemiyor. Önceden oluşturulmuş varlıklarla aksi belirtilmediği sürece tüm LUIS uygulaması yerel (kültür) kullanılabilir. Aşağıdaki tabloda her bir kültür için desteklenen önceden oluşturulmuş varlıklar gösterilmektedir.

Önceden oluşturulmuş varlık   |   İngilizce (ABD)<br>```En-us```   |   Fransızca (Fransa)<br>```fr-FR```   |   İtalyanca (İtalya)<br>```it-IT```   |   İspanyolca (İspanya)<br>```es-ES```   |   Çince<br>```zh-CN```   |   Almanca <br>```de-DE```   |   Portekizce (Brezilya)<br>```pt-BR```   |   Japonca (Japonya)<br>```ja-JP```   |   Korece (Güney Kore)<br>```ko-kr```   | Fransızca (Kanada)<br>```fr-CA```   |   İspanyolca (Meksika)<br>```es-MX```   |   Hollanda dili (Hollanda)<br>```nl-NL```   |
------|:------:|------|------|------|------|------|------|------|------|------|------|------|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Para birimi](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   |   ✔   |   -   |   ✔   |    ✔   |   -   |   ✔   |   -   |   -   |   -   |   -   |   -   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   |   ✔   |   ✔   |   ✔   |   -   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   |    -   |    -   |    -   |    ✔   |    -   |    -   |    -   |   -   |   -   |   -   |   -   |
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[URL](luis-reference-prebuilt-url.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |

Notları bakın [önceden oluşturulmuş varlıklarla kullanım dışı](luis-reference-prebuilt-deprecated.md)

Anahtar cümlesi, Portekizce (Brezilya) - tüm subcultures içinde kullanılabilir değil ```pt-BR```.

## <a name="contribute-to-prebuilt-entity-cultures"></a>Önceden oluşturulmuş varlık kültürler için katkıda bulunan
Önceden oluşturulmuş varlıklarla tanıyıcıları metin açık kaynak projenin geliştirilir. [Katkıda bulunan](https://github.com/Microsoft/Recognizers-Text) projeye. Bu proje, para birimi başına kültür örneklerini içerir. 

GeographyV2 ve PersonName tanıyıcıları metin projeye dahil değil. Bu önceden oluşturulmuş varlıklarla sorunları için lütfen açık bir [destek isteği](../../azure-supportability/how-to-create-azure-support-request.md). 

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [numarası](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), ve [para birimi](luis-reference-prebuilt-currency.md) varlıklar. 
