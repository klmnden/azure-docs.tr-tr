---
title: Konuşma sentezi biçimlendirme dili - konuşma Hizmetleri
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
ms.openlocfilehash: f721c91c828b5c7c8497c07bb0cfe79646daf0f8
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868846"
---
# <a name="speech-synthesis-markup-language"></a>Konuşma sentezi biçimlendirme dili

Konuşma sentezi işaretleme dili (SSML'yi) söylenişi denetlemek için bir yol sağlayan bir XML-tabanlı işaretleme dilidir ve *prosody* metin okuma. Konuşma aralığını ve Ritim prosody ifade eder —, müzik, gerçekleştirir. Sözcükleri fonetik olarak belirtin, sayıları yorumlanması için ipuçları sağlar, duraklatır, Denetim aralık, birim ve oranı ve daha ekleyin. Daha fazla bilgi için [konuşma sentezi işaretleme dili (SSML'yi) sürüm 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/).

Desteklenen diller, yerel ayarlar ve sesler (sinir ve standart) tam listesi için bkz: [dil desteği](language-support.md#text-to-speech).

Aşağıdaki bölümlerde, yaygın konuşma sentezi görevler örnekleri sağlar.

>[!IMPORTANT]
> Şu anda prosody etiketleme yalnızca standart ses için kullanılabilir.

## <a name="add-a-break"></a>Bir Sonu Ekle
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Konuşma hızını değiştirme
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Söylenişi
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Birimi Değiştir
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Aralık değiştirme
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Değişiklik aralık dağılımı
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="use-multiple-voices"></a>Birden çok kişilerden daha fazlasını kullanın
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Good morning!
</voice>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Good morning to you too Jessa!
</voice> </speak>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Dil desteği: sesleri, yerel ayarlar, dilleri](language-support.md)
