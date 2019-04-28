---
title: Yöntem - akademik bilgi API'si yorumlama
titlesuffix: Azure Cognitive Services
description: Kullanıcı sorgu dizeleri üzerinde Academic Graph verileri ve Microsoft Bilişsel hizmetler akademik dilbilgisi göre biçimlendirilmiş ınterpretations döndürülecek yorumlama yöntemi kullanın.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: b679f1da0ada3e61fca79cdb985a43dc445877ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61338461"
---
# <a name="interpret-method"></a>Yöntem yorumlama

**Yorumlama** REST API, sorgu dizesi (yani, uygulamanızın kullanıcı tarafından girilen bir sorgu) son kullanıcı alır ve yorumlar, kullanıcının amacını üzerinde Academic Graph veriler ve akademik dilbilgisi göre biçimlendirilmiş döndürür.

Etkileşimli bir deneyim sağlamak üzere bu yöntemi tekrar tekrar sonrasında kullanıcı tarafından girilen her karakter çağırabilirsiniz. Bu durumda, ayarlamalısınız **tam** otomatik tamamlama önerileri etkinleştirmek için 1 için'parametre. Uygulamanızı otomatik tamamlama gereksinimi yoksa, ayarlamalısınız **tam** parametresini 0.

**REST uç noktası:**

    https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?

## <a name="request-parameters"></a>İstek Parametreleri

Ad     | Değer | Gerekli mi?  | Açıklama
---------|---------|---------|---------
**Sorgu**    | Metin dizesi | Evet | Kullanıcı tarafından girilen sorgu.  Tam 1 olarak ayarlanırsa, sorgu oluşturma sorgu otomatik tamamlama önerileri için önek olarak yorumlanacaktır.        
**Model**    | Metin dizesi | Hayır  | Sorgulamak istediğiniz modelin adı.  Değer şu anda, varsayılan olarak *son*.        
**Tamamlayın** | 0 veya 1 | Hayır<br>Varsayılan: 0  | 1, otomatik tamamlama önerileri, dil bilgisi ve graf verilerine dayalı olarak oluşturulan anlamına gelir.         
**count**    | Sayı | Hayır<br>Varsayılan: 10 | Yorum döndürülecek en fazla sayısı.         
**uzaklık**   | Sayı | Hayır<br>Varsayılan: 0  | Döndürülecek ilk yorumu dizini. Örneğin, *sayısı = 2 & uzaklığı 0 =* ınterpretations 0 ve 1 döndürür. *sayısı 2 & uzaklığı = 2 =* ınterpretations 2 ve 3 döndürür.       
**zaman aşımı**  | Sayı | Hayır<br>Varsayılan: 1000 | Milisaniye cinsinden zaman aşımı. Zaman aşımı dolmadan bulunan ınterpretations döndürülür.

<br>
  
## <a name="response-json"></a>Yanıt (JSON)

Ad     | Açıklama
---------|---------
**Sorgu** |*Sorgu* istek parametresi.
**Yorumlar** |Kullanıcı girişi dilbilgisi karşı eşleşen 0 veya daha farklı yolları dizisi.
**[x] ınterpretations .logprob**  |Yorumu göreli doğal logaritmayı olasılık. Daha büyük değerler daha yüksektir.
**[x] ınterpretations .parse**  |Sorgunun her bölümü nasıl yorumlanacağını gösteren bir XML dizesi.
**[x] ınterpretations .rules**  |Yorumlama sırasında çağrılan dilbilgisi içinde tanımlanan 1 veya daha fazla kuralları dizisi. Akademik bilgi API'si için her zaman olmayacaktır 1 kuralı.
**[x] [y] .rules .name yorumlar**  |kuralın adı.
**[x] [y] .rules .output yorumlar**  |Kural çıktısı.
**[x] [y] .rules.output.type yorumlar** |Kural çıktısı veri türü.  Akademik bilgi API'si için bu her zaman "sorgu" olacaktır.
**[x] [y] .rules.output.value yorumlar**  |Kural çıktısı. Akademik bilgi API'si için değerlendir ve calchistogram yöntemlere geçirilen sorgu ifadesi dize budur.
**İptal edildi** | İstek zaman aşımına uğrarsa true.

<br>

#### <a name="example"></a>Örnek:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime&complete=1&count=2
 ```
<br>İlk iki yanıtı aşağıda içerir (parametre nedeniyle *sayısı = 2*) kısmi kullanıcı girişini tamamlayın büyük olasılıkla ınterpretations *incelemeler jaime tarafından*: *jaime teevan tarafından incelemeler*  ve *incelemeler jaime yeşil*.  Yazar için eşleşme yalnızca dikkate yerine hizmet oluşturulan sorgu tamamlamaları tam *jaime* istek belirttiğinden *tam = 1*. Unutmayın kurallı değer *j l yeşil* eş anlamlı eşleşen *Can'ın yeşil*, ayrıştırma gösterildiği gibi.


```JSON
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -12.728,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#AA.AuN\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(AA.AuN=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -12.774,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#AA.AuN\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(AA.AuN=='j l green')"
          }
        }
      ]
    }
  ]
}
```  
<br>Bir yorumu için varlık sonuçları almak için kullanın *output.value* gelen **yorumlama** API ve de **değerlendirmek** API aracılığıyla *ifade*  parametresi. Bu örnekte, ilk yorumu için sorgu aşağıdaki gibidir: 
```
evaluate?expr=Composite(AA.AuN=='jaime teevan')
```
 
