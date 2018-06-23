---
title: HALUK önceden oluşturulmuş varlıklar başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makale, dil anlama (HALUK içinde) dahil edilen önceden oluşturulmuş varlıklar listesi içerir.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 7ce50e4c0be605e1700a2c18533cb087384f524c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316893"
---
# <a name="entities-per-culture"></a>Kültür başına varlıklar

Dil anlama (HALUK) önceden oluşturulmuş varlıklar sağlar. Önceden oluşturulmuş bir varlık uygulamanızda dahil edildiğinde HALUK uç nokta yanıtta karşılık gelen varlık öngörü içerir. Tüm örnek utterances de varlıkla etiketlenir. Önceden oluşturulmuş varlıkların davranışlarındaki **olamaz** değiştirilmesi. Önceden oluşturulmuş varlıklar aksi belirtilmedikçe, tüm HALUK uygulama yerel (kültürler) kullanılabilir. Aşağıdaki tabloda her kültür için desteklenen önceden oluşturulmuş varlıklar gösterir.

Önceden oluşturulmuş varlık   |   İngilizce (ABD)<br>```En-us```   |   Fransızca (Fransa)<br>```fr-FR```   |   İtalyanca (İtalya)<br>```it-IT```   |   İspanyolca (İspanya)<br>```es-ES```   |   Çince<br>```zh-CN```   |   Almanca <br>```de-DE```   |   Portekizce (Brezilya)<br>```pt-BR```   |   Japonca (Japonya)<br>```ja-JP```   |   Korece (Kore)<br>```ko-kr```   | Fransızca (Kanada)<br>```fr-CA```   |   İspanyolca (Meksika)<br>```es-MX```   |   Hollanda dili (Hollanda)<br>```nl-NL```   |
------|:------:|------|------|------|------|------|------|------|------|------|------|------|
[Geçerlilik süresi](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Para birimi](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>tarih<br>daterange<br>time<br>timerange   |    ✔   |   ✔   |   -   |   ✔   |    ✔   |   -   |   ✔   |   -   |   -   |   -   |   -   |   -   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>alan<br>Ağırlık<br>bilgi (örn: bit/bayt)<br>uzunluğu (örn: ölçer)<br>hızı (örn: saat başına mil)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>derece   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[URL](luis-reference-prebuilt-url.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |

Bkz: Notlar [önceden oluşturulmuş varlıklar kullanım dışı](luis-reference-prebuilt-deprecated.md)

## <a name="examples-of-prebuilt-entities-in-en-us-culture"></a>Önceden oluşturulmuş varlıklarda tr örnekleri-bize kültür
Aşağıdaki tabloda, örnek verilerle önceden oluşturulmuş varlıkları ve dönüş değerleri listelenmektedir.

Önceden oluşturulmuş varlık   |   Örnek utterance   |   JSON
------|------|------|
 ```builtin.age```   |   ```100 year old```   |```{ "type": "builtin.age", "entity": "100 year old" }```|  
 ```builtin.age```   |   ```19 years old```   |```{ "type": "builtin.age", "entity": "19 years old" }```|
 ```builtin.currency```     |   ```1000.00 US dollars```   |```{ "type": "builtin.currency", "entity": "1000.00 us dollars" }```
 ```builtin.currency```     |   ```$ 67.5 B```   |```{ "type": "builtin.currency", "entity": "$ 67.5" }```|
 ```builtin.datetimeV2``` | Bkz: [builtin.datetimeV2](luis-reference-prebuilt-datetimev2.md) | Bkz: [builtin.datetimeV2](luis-reference-prebuilt-datetimev2.md) |
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


## <a name="contribute-to-prebuilt-entity-cultures"></a>Önceden oluşturulmuş varlık kültürler katkıda bulunan
Önceden oluşturulmuş varlıklar tanıyıcıları metin açık kaynaklı proje geliştirilir. Lütfen [katkıda](https://github.com/Microsoft/Recognizers-Text) projeye. Bu proje kültür başına para birimi örnekleri içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [numarası](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), ve [para birimi](luis-reference-prebuilt-currency.md) varlıklar. 