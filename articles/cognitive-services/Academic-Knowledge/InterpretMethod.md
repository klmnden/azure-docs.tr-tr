---
title: Akademik bilgi API'si yönteminde yorumlama | Microsoft Docs
description: Kullanıcı sorgu dizeleri akademik grafik verilerini ve Microsoft Bilişsel Hizmetleri'ndeki akademik dilbilgisi göre biçimlendirilmiş yorumlar döndürmek için yorumlama yöntemi kullanın.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a46c792f14fabf6562666d1067ef880bd505741f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351449"
---
# <a name="interpret-method"></a>Yöntem yorumlama

**Yorumlama** REST API sorgu dizesi (yani, uygulamanızın kullanıcı tarafından girilen bir sorgu) son kullanıcı alır ve Yorumlar kullanıcı hedefinin akademik grafik verileri ve akademik dilbilgisi göre biçimlendirilmiş döndürür.

Etkileşimli bir deneyim sağlamak üzere bu yöntemi sürekli olarak kullanıcı tarafından girilen her karakterinden sonraki çağırabilirsiniz. Bu durumda, ayarlamalısınız **tam** parametre otomatik tamamlama önerilerini etkinleştirmek için 1. Uygulamanızın otomatik tamamlama gereksinimi yoksa ayarlamalısınız **tam** 0 parametresi.

**REST uç noktası:**

    https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?

## <a name="request-parameters"></a>İstek Parametreleri

Ad     | Değer | Gerekli mi?  | Açıklama
---------|---------|---------|---------
**Sorgu**    | Metin dizesi | Evet | Kullanıcı tarafından girilen sorgu.  Tam 1 olarak ayarlanırsa, sorgu oluşturma sorgu otomatik tamamlama önerileri için bir önek olarak yorumlanacak.        
**modeli**    | Metin dizesi | Hayır  | Sorgu istediğiniz modelin adı.  Değer şu anda varsayılan olarak *son*.        
**tamamlayın** | 0 veya 1 | Hayır<br>Varsayılan: 0  | 1 dilbilgisi ve grafik verileri temel alan otomatik tamamlama önerileri oluşturulan anlamına gelir.         
**sayısı**    | Sayı | Hayır<br>Varsayılan: 10 | Döndürülecek yorumlar maksimum sayısı.         
**uzaklık**   | Sayı | Hayır<br>Varsayılan: 0  | Döndürülecek ilk yorumlama dizini. Örneğin, *sayısı = 2 & uzaklığı 0 =* yorumlar 0 ve 1 döndürür. *Count = 2 & uzaklık = 2* yorumlar 2 ve 3 döndürür.       
**zaman aşımı**  | Sayı | Hayır<br>Varsayılan: 1000 | Milisaniye cinsinden zaman aşımı. Yalnızca zaman aşımı dolmadan bulunan yorumlar döndürülür.
<br>
  
## <a name="response-json"></a>Yanıt (JSON)
Ad     | Açıklama
---------|---------
**Sorgu** |*Sorgu* istek parametresi.
**Yorumlar** |Kullanıcı girişi dilbilgisi karşı eşleşen 0 veya daha fazla farklı yolları dizisi.
**Yorumlar [x] .logprob**  |Yorumu göreli doğal günlük olasılık. Daha büyük değerler daha yüksektir.
**Yorumlar [x] .parse**  |Her bölümü nasıl yorumlanacağını gösterir XML dizesi.
**Yorumlar [x] .rules**  |Yorumlama sırasında çağrıldı dilbilgisi içinde tanımlanan 1 veya daha fazla kural dizisi. Akademik bilgi API'si, her zaman olacaktır 1 kuralı.
**[x] .rules [y] .ad yorumlar**  |Kural adı.
**[x] .rules [y] .output yorumlar**  |Kuralın çıktısı.
**[x] .rules [y].output.type yorumlar** |Kuralın çıktısı veri türü.  Akademik bilgi API'si, bu her zaman "sorgu" olacaktır.
**[x] .rules [y].output.value yorumlar**  |Kuralın çıktısı. Akademik bilgi API'si için değerlendir ve calchistogram yöntemlere iletilen bir sorgu ifadesi dize budur.
**iptal edildi** | İstek zaman aşımına uğrarsa true.

<br>
#### <a name="example"></a>Örnek:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime&complete=1&count=2
 ```
<br>İki üst yanıt içerir (parametre nedeniyle *sayısı = 2*) kısmi kullanıcı girişini tamamlamak olasılıkla yorumlar *jaime tarafından yazıları*: *jaime teevan tarafından yazıları*  ve *jaime yeşil yazıları*.  Yalnızca dikkate yerine oluşturulan hizmet sorgu tamamlamalar yazar için eşleşen tam *jaime* istek belirtilmediğinden *tam = 1*. Unutmayın kurallı değer *j l yeşil* eş eşleşen *CAN yeşil*, ayrıştırma belirtildiği şekilde.


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
<br>Bir yorumu için varlık sonuçları almak için kullanın *output.value* gelen **yorumlama** API ve, içine geçirin **değerlendirmek** API üzerinden *ifade*  parametresi. Bu örnekte, ilk yorumlama için sorgu aşağıdaki gibidir: 
```
evaluate?expr=Composite(AA.AuN=='jaime teevan')
```
 