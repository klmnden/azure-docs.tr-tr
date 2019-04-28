---
title: Konuşma sentezi biçimlendirme dili (SSML'yi) - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Söyleniş ve metin okuma, prosody denetlemek için konuşma sentezi biçimlendirme dili kullanma.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 57fc7e699d88dbe777750e3acdb7f96794b66fc0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61460295"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Konuşma Sentezi Biçimlendirme Dili (SSML)

Konuşma sentezi işaretleme dili (SSML'yi) söylenişi denetlemek için bir yol sağlayan bir XML-tabanlı işaretleme dilidir ve *prosody* metin okuma. Konuşma aralığını ve Ritim prosody ifade eder —, müzik, gerçekleştirir. Sözcükleri fonetik olarak belirtin, sayıları yorumlanması için ipuçları sağlar, duraklatır, Denetim aralık, birim ve oranı ve daha ekleyin. Daha fazla bilgi için [konuşma sentezi işaretleme dili (SSML'yi) sürüm 1.0](https://www.w3.org/TR/2009/REC-speech-synthesis-20090303/).

Desteklenen diller, yerel ayarlar ve sesler (sinir ve standart) tam listesi için bkz: [dil desteği](language-support.md#text-to-speech).

Aşağıdaki bölümlerde, yaygın konuşma sentezi görevler örnekleri sağlar.

## <a name="add-a-break"></a>Bir Sonu Ekle
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Konuşma hızını değiştirme

Word veya cümle düzeyinde standart sesleri oranı Konuşmayı uygulanabilir. Konuşma hızını yalnızca sinir sesleri cümle düzeyinde uygulanabilir ise.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Söylenişi
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Birimi Değiştir

Birim değişiklikleri standart sesleri word veya cümle düzeyinde uygulanabilir. Birim değişiklikleri yalnızca sinir sesleri cümle düzeyinde uygulanabilir ise.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Aralık değiştirme

Aralık değişiklikleri standart sesleri word veya cümle düzeyinde uygulanabilir. Aralık değişiklikler yalnızca sinir sesleri cümle düzeyinde uygulanabilir ise.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Değişiklik aralık dağılımı

> [!IMPORTANT]
> Aralık contour değişiklikleri sinir sesleri ile desteklenmez.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="use-multiple-voices"></a>Birden çok kişilerden daha fazlasını kullanın
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Good morning!
</voice>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Good morning to you too Jessa!
</voice> </speak>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Dil desteği: sesleri, yerel ayarlar, dilleri](language-support.md)
