---
title: LUIS önceden oluşturulmuş varlıklar başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makale, Language Understanding (LUIS), dahil edilen önceden oluşturulmuş varlıklar listesi içerir.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/11/2018
ms.author: diberry
ms.openlocfilehash: 731ac279b4b0c162809d8e0638b9337924859b3d
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238838"
---
# <a name="entities-per-culture"></a>Kültür başına varlıklar

Language Understanding (LUIS), önceden oluşturulmuş varlıklar sağlar. Uygulamanızda önceden oluşturulmuş bir varlık eklendiğinde LUIS uç nokta yanıtta ilgili varlık öngörü içerir. Tüm örnek konuşma de sahip bir varlık olarak etiketlenmiştir. Önceden oluşturulmuş varlıkların **olamaz** değiştirilemiyor. Önceden oluşturulmuş varlıklarla aksi belirtilmediği sürece tüm LUIS uygulaması yerel (kültür) kullanılabilir. Aşağıdaki tabloda her bir kültür için desteklenen önceden oluşturulmuş varlıklar gösterilmektedir.

Önceden oluşturulmuş varlık   |   İngilizce (ABD)<br>```En-us```   |   Fransızca (Fransa)<br>```fr-FR```   |   İtalyanca (İtalya)<br>```it-IT```   |   İspanyolca (İspanya)<br>```es-ES```   |   Çince<br>```zh-CN```   |   Almanca <br>```de-DE```   |   Portekizce (Brezilya)<br>```pt-BR```   |   Japonca (Japonya)<br>```ja-JP```   |   Korece (Kore)<br>```ko-kr```   | Fransızca (Kanada)<br>```fr-CA```   |   İspanyolca (Meksika)<br>```es-MX```   |   Hollanda dili (Hollanda)<br>```nl-NL```   |
------|:------:|------|------|------|------|------|------|------|------|------|------|------|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Para birimi](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>tarih<br>daterange<br>time<br>timerange   |    ✔   |   ✔   |   -   |   ✔   |    ✔   |   -   |   ✔   |   -   |   -   |   -   |   -   |   -   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   |   ✔   |   ✔   |   ✔   |   -   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[URL](luis-reference-prebuilt-url.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |

Notları bakın [önceden oluşturulmuş varlıklarla kullanım dışı](luis-reference-prebuilt-deprecated.md)

Anahtar cümlesi, Portekizce (Brezilya) - alt kültürde tüm kullanılabilir değil ```pt-BR```.

<!--
## Examples of prebuilt entities in en-us culture
The following table lists prebuilt entities with example data and the return values.

Prebuilt entity   |   Example utterance   |   JSON
------|------|------|
 ```builtin.age```   |   ```100 year old```   |```{ "type": "builtin.age", "entity": "100 year old" }```|  
 ```builtin.age```   |   ```19 years old```   |```{ "type": "builtin.age", "entity": "19 years old" }```|
 ```builtin.currency```     |   ```1000.00 US dollars```   |```{ "type": "builtin.currency", "entity": "1000.00 us dollars" }```
 ```builtin.currency```     |   ```$ 67.5 B```   |```{ "type": "builtin.currency", "entity": "$ 67.5" }```|
 ```builtin.datetimeV2``` | See [builtin.datetimeV2](luis-reference-prebuilt-datetimev2.md) | See [builtin.datetimeV2](luis-reference-prebuilt-datetimev2.md) |
 ```builtin.dimension```     |   ```2 miles```   |```{ "type": "builtin.dimension", "entity": "2 miles" }```|
 ```builtin.dimension```     |  ```650 square kilometers```   |```{ "type": "builtin.dimension", "entity": "650 square kilometers" }```|
 ```builtin.email```     |  ```patti.owens@microsoft.com```   |```{ "type": "builtin.email", "entity": "patti.owens@microsoft.com" }```|
 ```builtin.number```     |  ```ten```   |``` { "type": "builtin.number", "entity": "ten" } ```|
 ```builtin.number```     |   ```3.1415```   |```  { "type": "builtin.number", "entity": "3 . 1415" }``` |
 ```builtin.ordinal```     |   ```first```   |```{ "type": "builtin.ordinal", "entity": "first" }``` |
 ```builtin.ordinal```     |   ```10th```   | ```{ "type": "builtin.ordinal", "entity": "10th" }``` |  
 ```builtin.percentage```   |   ```The stock price increase by 7 $ this year```   |```{ "type": "builtin.percentage", "entity": "7 %" }```|
 ```builtin.phonenumber```   |   ```my mobile is 00 44 161 1234567```   |```{ "type": "builtin.phonenumber", "entity": "00 44 161 1234567" }```|
 ```builtin.temperature```     |   ```10 degrees celsius```   | ```{ "type": "builtin.temperature", "entity": "10 degrees celcius" }```|   
 ```builtin.temperature```     |   ```78 F```   |```{ "type": "builtin.temperature", "entity": "78 f" }```|
 ```builtin.url```     |   ```http://www.luis.ai is a great cognitive service```   |```{ "type": "builtin.url", "entity": "http://www.luis.ai" }```|
-->

## <a name="contribute-to-prebuilt-entity-cultures"></a>Önceden oluşturulmuş varlık kültürler için katkıda bulunan
Önceden oluşturulmuş varlıklarla tanıyıcıları metin açık kaynak projenin geliştirilir. Lütfen [katkıda](https://github.com/Microsoft/Recognizers-Text) projeye. Bu proje, para birimi başına kültür örneklerini içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [numarası](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), ve [para birimi](luis-reference-prebuilt-currency.md) varlıklar. 