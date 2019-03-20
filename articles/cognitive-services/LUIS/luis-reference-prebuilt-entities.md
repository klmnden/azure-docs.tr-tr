---
title: Tüm önceden oluşturulmuş varlıklar
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUIS), dahil edilen önceden oluşturulmuş varlıklar listesi içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/12/2019
ms.author: diberry
ms.openlocfilehash: fdb105fa5aa9baefc9e64b65c275f07db802daad
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58091707"
---
# <a name="entities-per-culture-in-your-luis-model"></a>Varlıkları aracılığıyla LUIS modelinize kültürün başına

Language Understanding (LUIS), önceden oluşturulmuş varlıklar sağlar. Uygulamanızda önceden oluşturulmuş bir varlık eklendiğinde LUIS uç nokta yanıtta ilgili varlık öngörü içerir. Tüm örnek konuşma de sahip bir varlık olarak etiketlenmiştir. Önceden oluşturulmuş varlıkların **olamaz** değiştirilemiyor. Önceden oluşturulmuş varlıklarla aksi belirtilmediği sürece tüm LUIS uygulaması yerel (kültür) kullanılabilir. Aşağıdaki tabloda her bir kültür için desteklenen önceden oluşturulmuş varlıklar gösterilmektedir.

|Kültür|Subcultures|
|--|--|
|Çince|[zh-CN](#chinese-entity-support)|
|Felemenkçe|[NL-NL](#dutch-entity-support)|
|Türkçe|[en-US (Amerika)](#english-american-entity-support)|
|Fransızca |[fr-CA (Kanada)](#french-canadian-entity-support), [fr-FR (Fransa)](#french-france-entity-support), |
|Almanca |[de-DE](#german-entity-support)|
|İtalyanca|[İt-IT](#italian-entity-support)|
|Japonca|[ja-JP](#japanese-entity-support)|
|Korece|[ko-KR](#korean-entity-support)|
|Portekizce|[pt-BR (Brezilya)](#portuguese-brazil-entity-support)|
|İspanyolca |[es-ES (İspanya)](#spanish-spain-entity-support), [es-MX (Meksika)](#spanish-mexico-entity-support)|
|Türkçe|[Türkçe](#turkish-entity-support)|

## <a name="chinese-entity-support"></a>Çince varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```zh-CN``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    -   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="dutch-entity-support"></a>Felemenkçe varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```nl-NL``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="english-american-entity-support"></a>İngilizce (Amerika) varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```en-US``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="french-france-entity-support"></a>Fransızca (Fransa) varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```fr-FR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="french-canadian-entity-support"></a>Fransızca (Kanada) varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```fr-CA``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="german-entity-support"></a>Alman varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```de-DE``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="italian-entity-support"></a>İtalyanca varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```it-IT``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="japanese-entity-support"></a>Japonca varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```ja-JP``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="korean-entity-support"></a>Korece varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```ko-KR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    -   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    -   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    -   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    -   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    -   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="portuguese-brazil-entity-support"></a>Portekizce (Brezilya) varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```pt-BR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="spanish-spain-entity-support"></a>İspanyolca (İspanya) varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```es-ES``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    ✔   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="spanish-mexico-entity-support"></a>İspanyolca (Meksika) varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```es-MX``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    -   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    -   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayı](luis-reference-prebuilt-number.md)   |    ✔   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    -   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    -   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

Notları bakın [önceden oluşturulmuş varlıklarla kullanım dışı](luis-reference-prebuilt-deprecated.md)

Anahtar cümlesi, Portekizce (Brezilya) - tüm subcultures içinde kullanılabilir değil ```pt-BR```.

## <a name="turkish-entity-support"></a>Türkçe varlık desteği

Aşağıdaki varlıkların desteklenir:

|Önceden oluşturulmuş varlık|```tr-tr``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    -   |
[Currency (para)](luis-reference-prebuilt-currency.md):<br>dolar<br>kesirli birimi (örn: kuruş)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birim<br>Alan<br>ağırlığı<br>bilgi (örn: bit/bayt)<br>uzunluk (örn: ölçüm)<br>Hız (örn: mil saat başına)  |    -   | 
[E-posta](luis-reference-prebuilt-email.md)   |    -   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[Anahtar cümlesi](luis-reference-prebuilt-keyphrase.md)   |    -   | 
[Sayı](luis-reference-prebuilt-number.md)   |    -   |  
[Sıra](luis-reference-prebuilt-ordinal.md)   |    -   |  
[Yüzdesi](luis-reference-prebuilt-percentage.md)   |    -   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefon numarası](luis-reference-prebuilt-phonenumber.md)   |    -   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    -   |

Notları bakın [önceden oluşturulmuş varlıklarla kullanım dışı](luis-reference-prebuilt-deprecated.md)

Anahtar cümlesi kullanılamıyor.


## <a name="contribute-to-prebuilt-entity-cultures"></a>Önceden oluşturulmuş varlık kültürler için katkıda bulunan
Önceden oluşturulmuş varlıklarla tanıyıcıları metin açık kaynak projenin geliştirilir. [Katkıda bulunan](https://github.com/Microsoft/Recognizers-Text) projeye. Bu proje, para birimi başına kültür örneklerini içerir. 

GeographyV2 ve PersonName tanıyıcıları metin projeye dahil değil. Bu önceden oluşturulmuş varlıklarla sorunları için lütfen açık bir [destek isteği](../../azure-supportability/how-to-create-azure-support-request.md). 

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [numarası](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), ve [para birimi](luis-reference-prebuilt-currency.md) varlıklar. 
