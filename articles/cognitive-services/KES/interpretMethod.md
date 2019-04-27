---
title: Yöntem - bilgi keşfetme hizmeti API'si yorumlama
titlesuffix: Azure Cognitive Services
description: Bilgi keşfetme hizmeti (KES içinde) API yorumlama yöntemi kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 88776e2f4167c950d60c0405dcf950b5173fb989
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814142"
---
# <a name="interpret-method"></a>Yöntem yorumlama

*Yorumlama* yöntemi doğal dildeki sorgu dizesini alır ve dil bilgisi ve dizin verileri temel alan kullanıcı hedefinin ınterpretations biçimlendirilmiş döndürür.  Her karakter ile kullanıcı tarafından girildiği gibi bir etkileşimli bir arama deneyimi sağlamak için bu yöntem çağrılabilir *tam* parametresi, otomatik tamamlama önerileri etkinleştirmek için 1 olarak ayarlayın.

## <a name="request"></a>İstek

`http://<host>/interpret?query=<query>[&<options>]`

Ad|Değer| Açıklama
----|----|----
sorgu    | Metin dizesi | Kullanıcı tarafından girilen sorgu.  Tam 1 olarak ayarlanırsa, sorgu oluşturma sorgu otomatik tamamlama önerileri için önek olarak yorumlanacaktır.        
Tamamlayın | 0 (varsayılan) veya 1 | 1 dilbilgisi ve dizin verileri temel alan otomatik tamamlama önerileri oluşturulan anlamına gelir.         
count    | Sayı (varsayılan = 10) | Yorum döndürülecek en fazla sayısı.         
uzaklık   | Sayı (varsayılan = 0) | Döndürülecek ilk yorumu dizini.  Örneğin, *sayısı = 2 & uzaklığı 0 =* ınterpretations 0 ve 1 döndürür. *sayısı 2 & uzaklığı = 2 =* ınterpretations 2 ve 3 döndürür.       
timeout  | Sayı (varsayılan = 1000) | Milisaniye cinsinden zaman aşımı. Zaman aşımı dolmadan bulunan ınterpretations döndürülür.

Kullanarak *sayısı* ve *uzaklığı* parametre sonuçları çok sayıda elde edilebilir artımlı olarak birden çok istek.

## <a name="response-json"></a>Yanıt (JSON)

JSONPath     | Açıklama
---------|---------
$.query |*Sorgu* istek parametresi.
$.interpretations   |0 veya daha fazla yolu dilbilgisi karşı giriş sorguyla eşleşen dizisi.
$.interpretations[\*].logprob   |Yorumu göreli günlük olasılığı (< = 0).  Yüksek değerler daha yüksektir.
$.interpretations[\*].parse |Sorgunun her bölümü nasıl yorumlanacağını gösterir XML dizesi.
$.interpretations [\*] .rules |1 veya daha fazla kural yorumlama sırasında çağrılan dilbilgisi içinde tanımlanan dizisi.
$.interpretations[\*].rules[\*].name    |kuralın adı.
$.interpretations[\*].rules[\*].output  |Kural anlam çıkışı.
$.interpretations[\*].rules[\*].output.type |Anlam çıkış veri türü.
$.interpretations[\*].rules[\*].output.value|Anlam çıktı değeri.  
$.aborted | İstek zaman aşımına uğrarsa true.

### <a name="parse-xml"></a>XML Ayrıştırma

Ayrıştırma XML (tamamlanmış) sorgu nasıl dilbilgisi kuralları ve dizin öznitelikleri karşı eşleşen hakkında bilgi içeren bir açıklama ekler.  Akademik yayınlar etki alanından bir örnek aşağıda verilmiştir:

```xml
<rule name="#GetPapers">
  papers by 
  <attr name="academic#Author.Name" canonical="heungyeung shum">harry shum</attr>
  <rule name="#GetPaperYear">
    written in
    <attr name="academic#Year">2000</attr>
  </rule>
</rule>
```

`<rule>` Öğesi tarafından belirtilen kural eşleşen sorgu aralığında sınırlandırır kendi `name` özniteliği.  Ayrıştırma içerdiğinde kuralı başvuruları dilbilgisi içinde iç içe.

`<attr>` Öğesi tarafından belirtilen dizin öznitelikleri eşleşen sorgu aralığında sınırlandırır kendi `name` özniteliği.  Giriş sorgusu içindeki bir eş anlamlı eşleşme içerdiğinde `canonical` eş anlamlı dizinden eşleşen kurallı değer özniteliği içerir.

## <a name="example"></a>Örnek

Akademik yayınlar örnekte aşağıdaki isteği öneki sorgu "jaime tarafından incelemeler" 2 otomatik tamamlama önerileri kadar döndürür:

`http://<host>/interpret?query=papers by jaime&complete=1&count=2`

İlk iki yanıt içerir ("sayısı 2 =") "jaime tarafından incelemeler" kısmi sorguyu tamamlamak en olası ınterpretations: "raporlar tarafından jaime teevan" ve "raporlar tarafından jaime yeşil".  İstek belirtildiği için yalnızca dikkate yerine hizmet oluşturulan sorgu tamamlamaları Yazar "jaime" için eşleşme tam "Tam = 1". Canonical "j l yeşil" değeri "jamie green" eş anlamlı eşleşen ayrıştırma gösterildiği gibi unutmayın.


```json
{
  "query": "papers by jaime",
  "interpretations": [
    {
      "logprob": -5.615,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\">jaime teevan</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='jaime teevan')"
          }
        }
      ]
    },
    {
      "logprob": -5.849,
      "parse": "<rule name=\"#GetPapers\">papers by <attr name=\"academic#Author.Name\" canonical=\"j l green\">jaime green</attr></rule>",
      "rules": [
        {
          "name": "#GetPapers",
          "output": {
            "type": "query",
            "value": "Composite(Author.Name=='j l green')"
          }
        }
      ]
    }
  ]
}
```  

Anlam çıktı türünü "sorgu, bu örnekte olduğu gibi" olduğunda eşleşen nesneleri geçirilerek alınabilir *output.value* için [ *değerlendirmek* ](evaluateMethod.md) API aracılığıyla*expr* parametresi.

`http://<host>/evaluate?expr=Composite(AA.AuN=='jaime teevan')`
  
