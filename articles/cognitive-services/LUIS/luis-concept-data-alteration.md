---
title: HALUK - Azure veri değişikliğinin kavramlarını anlama | Microsoft Docs
description: Tahminleri dil anlama (HALUK) önce verileri nasıl değiştirilebilir öğrenin
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/26/2018
ms.author: v-geberr
ms.openlocfilehash: 4fb1a5542bb56bd853984e66198ebfbd189451f8
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266874"
---
# <a name="data-alterations"></a>Veri değişiklikleri
HALUK öncesinde veya sırasında tahmin utterance işlemek için yöntemler sağlar. 

## <a name="correct-spelling-errors-in-utterance"></a>Utterance doğru yazım hataları
HALUK kullanan [Bing yazım denetleme API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) utterance yazım hatalarını gidermek için. HALUK hizmetle ilişkili anahtarı gerekir. Anahtar oluşturun, sonra sorgu dizesi parametresi olarak anahtarı ekleyin [endpoint](https://aka.ms/luis-endpoint-apis). 

Ayrıca, yazım hatalarını düzeltebilirsiniz **Test** tarafından panel [anahtarı girme](interactive-test.md#view-bing-spell-check-corrections-in-test-panel). Anahtar Test paneli tarayıcıda oturum değişkeni olarak tutulur. Anahtar düzeltildi yazım istediğiniz her tarayıcı oturumunda Test Masası'na ekleyin. 

Test panelinde ve uç nokta sayısı doğru anahtar kullanımını [anahtar kullanımı](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) kota. HALUK metin uzunluğu için sınırları Bing yazım denetimi uygular. 

Uç nokta yazım düzeltmeleri çalışmak için iki params gerektirir:

|Param|Değer|
|--|--|
|`spellCheck`|boole|
|`bing-spell-check-subscription-key`|[Bing yazım denetleme API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) abonelik anahtarı|

Zaman [Bing yazım denetleme API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) bir hata, özgün utterance ve düzeltilmiş utterance döndürülür tahminleri yanı sıra uç noktasından algılar.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Önceden oluşturulmuş datetimeV2 varlık saat dilimini değiştirme
HALUK uygulamayı önceden oluşturulmuş datetimeV2 varlık kullanırken bir tarih saat değeri tahmin yanıtta döndürülebilir. Saat dilimi isteğin döndürmek için doğru datetime belirlemek için kullanılır. İstek bir bot veya başka bir merkezi uygulamayı HALUK için alma önce geliyorsa HALUK kullanan saat dilimi düzeltin. 

### <a name="endpoint-querystring-parameter"></a>Son nokta sorgu dizesi parametresi
Kullanıcının saat dilimi olarak ekleyerek saat dilimi düzeltilinceye [endpoint](https://aka.ms/luis-endpoint-apis) kullanarak `timezoneOffset` param. Değeri `timezoneOffset` saati değiştirmek için dakika cinsinden pozitif veya negatif sayı olmalıdır.  

|Param|Değer|
|--|--|
|`timezoneOffset`|dakika cinsinden pozitif veya negatif sayı|

### <a name="daylight-savings-example"></a>Gün ışığından yararlanma örneği
Günışığından yararlanma saatine ayarlamak için döndürülen önceden oluşturulmuş datetimeV2 gerekiyorsa, kullanması gereken `timezoneOffset` sorgu dizesi parametresi ile bir değer dakika cinsinden +/- [endpoint](https://aka.ms/luis-endpoint-apis) sorgu.

60 dakika ekleyin: 

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/Apps/{appId}?q=Turn üzerinde ışık? **timezoneOffset = 60**& ayrıntılı {boolean} = & Yazım denetimi {boolean} = & hazırlama {boolean} = & bing-yazım-onay-subscription-key = {dize} & oturum {boolean} =

60 dakika kaldırın: 

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/Apps/{appId}?q=Turn üzerinde ışık? **timezoneOffset = 60**& ayrıntılı {boolean} = & Yazım denetimi {boolean} = & hazırlama {boolean} = & bing-yazım-onay-subscription-key = {dize} & oturum {boolean} =

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C# kodu timezoneOffset doğru değerini belirler
Aşağıdaki C# kod kullanır [Timezoneınfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1) sınıfının [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples) doğru belirlemek amacıyla yöntemi `timezoneOffset` sistem zamanına bağlıdır:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu öğretici ile doğru yazım](luis-tutorial-bing-spellcheck.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions