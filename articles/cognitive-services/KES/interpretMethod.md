---
title: Bilgi Bankası araştırması hizmeti API'si yönteminde yorumlama | Microsoft Docs
description: İçinde bilgi araştırması hizmet (KES) API Bilişsel Hizmetleri'ndeki yorumlama yöntemi kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: ef68d98dacf393abf8d030b9312217ea380947d2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351713"
---
# <a name="interpret-method"></a>Yöntem yorumlama
*Yorumlama* yöntemi doğal dil sorgu dizesini alır ve biçimlendirilmiş dilbilgisi ve dizin verileri temel alan kullanıcı hedefinin yorumlar döndürür.  Her karakteri ile kullanıcı tarafından girilen gibi bir etkileşimli arama deneyimi sağlamak için bu yöntemi çağrılabilir *tam* parametre otomatik tamamlama önerilerini etkinleştirmek için 1 olarak ayarlayın.

## <a name="request"></a>İstek
`http://<host>/interpret?query=<query>[&<options>]`

Ad|Değer| Açıklama
----|----|----
sorgu    | Metin dizesi | Kullanıcı tarafından girilen sorgu.  Tam 1 olarak ayarlanırsa, sorgu oluşturma sorgu otomatik tamamlama önerileri için bir önek olarak yorumlanacak.        
tamamlayın | 0 (varsayılan) veya 1 | Otomatik Tamamlama öneriler, dilbilgisi ve Dizin verilerine dayalı olarak oluşturulan 1 anlamına gelir.         
count    | Sayı (varsayılan = 10) | Döndürülecek yorumlar maksimum sayısı.         
uzaklık   | Sayı (varsayılan = 0) | Döndürülecek ilk yorumlama dizini.  Örneğin, *sayısı = 2 & uzaklığı 0 =* yorumlar 0 ve 1 döndürür. *Count = 2 & uzaklık = 2* yorumlar 2 ve 3 döndürür.       
timeout  | Sayı (varsayılan = 1000) | Milisaniye cinsinden zaman aşımı. Yalnızca zaman aşımı dolmadan bulunan yorumlar döndürülür.

Kullanarak *sayısı* ve *uzaklık* parametreleri, çok sayıda sonuç elde edilebilir artımlı olarak birden çok istek.

## <a name="response-json"></a>Yanıt (JSON)
JSONPath     | Açıklama
---------|---------
$.query |*Sorgu* istek parametresi.
$.interpretations   |0 veya daha fazla yolunu dilbilgisi karşı giriş sorguyla eşleşen dizisi.
$.interpretations [\*] .logprob   |Göreli günlük olasılık yorumu (< = 0).  Daha yüksek değerleri daha yüksektir.
$.interpretations [\*] .parse |Her bölümü nasıl yorumlanacağını gösterir XML dizesi.
$.interpretations [\*] .rules |1 veya daha fazla kural yorumlama sırasında çağrılan dilbilgisi tanımlanan dizisi.
$.interpretations [\*] .rules [\*] .ad    |Kural adı.
$.interpretations [\*] .rules [\*] .output  |Kural anlamsal çıkışı.
$.interpretations [\*] .rules [\*]. output.type |Anlam çıktı veri türü.
$.interpretations [\*] .rules [\*]. output.value|Anlam çıktı değeri.  
$.aborted | İstek zaman aşımına uğrarsa true.

### <a name="parse-xml"></a>XML Ayrıştırma
Ayrıştırma (tamamlanmış) sorguyu nasıl dilbilgisi kurallarında ve dizin öznitelikleri karşı eşleşen hakkında bilgi ile XML açıklama ekler.  Akademik yayınlar etki alanından bir örnek aşağıda verilmiştir:

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

`<rule>` Öğesi tarafından belirtilen kural eşleşen sorgu aralığında sınırlandırır kendi `name` özniteliği.  Ayrıştırma dilbilgisi kuralı başvurularında gerektirdiğinde iç içe.

`<attr>` Öğesi tarafından belirtilen dizin özniteliği eşleşen sorgu aralığında sınırlandırır kendi `name` özniteliği.  Eşleşme eşanlamlısı giriş sorgusunda gerektirdiğinde `canonical` özniteliği dizinden eş eşleşen kurallı değer içerir.

## <a name="example"></a>Örnek
Akademik yayınlar örnekte aşağıdaki isteği öneki sorgu "jaime tarafından yazıları" 2 otomatik tamamlama önerileri kadar döndürür:

`http://<host>/interpret?query=papers by jaime&complete=1&count=2`

İki üst yanıtı içerir ("count = 2") kısmi sorgu "jaime tarafından yazıları" tamamlamak olasılıkla yorumlar: "yazıları tarafından jaime teevan" ve "incelemeleri tarafından jaime yeşil".  İstek belirtilmediğinden yalnızca dikkate yerine oluşturulan hizmet sorgu tamamlamalar "jaime" yazar için eşleşen tam "Tam = 1". "Jamie yeşil" eş anlamlısı eşleşen kurallı değeri "j l yeşil" ayrıştırma belirtildiği gibi unutmayın.


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

Anlam çıktı türünü "query, bu örnekte olduğu gibi" olduğunda eşleşen nesneleri geçirerek alınabilir *output.value* için [ *değerlendirmek* ](evaluateMethod.md) API üzerinden*expr* parametresi.

`http://<host>/evaluate?expr=Composite(AA.AuN=='jaime teevan')`
  
