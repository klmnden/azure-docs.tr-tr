---
title: Konuşma Birleştirici işaretleme dili | Microsoft Docs
description: Konuşma Birleştirici biçimlendirme dili telaffuz ve okuma prosody denetlemek için kullanma.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: d955e7fd7805688ba103897c0d900c44f16514f8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354460"
---
# <a name="speech-synthesis-markup-language"></a>Konuşma Birleştirici işaretleme dili

Konuşma Birleştirici işaretleme dili (SSML) telaffuz denetlemek için bir yol sağlayan bir XML tabanlı biçimlendirme dilidir ve *prosody* metin okuma. (Prosody başvuruyor Ritim ve konuşma sıklık — şunları yapacaksınız varsa, müzik). Sözcükleri fonetik olarak belirtin, numaraları yorumlanması için ipuçları sağlayabilir, duraklatır, Denetim sıklık, birim ve oranı ve daha ekleme.

Daha fazla bilgi için bkz: [konuşma Birleştirici işaretleme dili (SSML) sürüm 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/) W3C adresindeki.

Aşağıdaki örnekler nasıl SSML ortak konuşma Birleştirici ihtiyaçları için kullanılacağını gösterir.

## <a name="add-a-break"></a>Bir sonu ekleme
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Konuşma hızını değiştirmek
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Söyleniş
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Toplu değiştirme
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

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
* [C# Konuşma tanıması bkz.](quickstart-csharp-windows.md)
