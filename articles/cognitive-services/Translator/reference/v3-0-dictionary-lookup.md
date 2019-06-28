---
title: Translator metin API'si sözlük arama yöntemi
titlesuffix: Azure Cognitive Services
description: Translator metin API'si sözlük arama yöntemi kullanın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: swmachan
ms.openlocfilehash: 8cc37c9edd5679158f8454e00bd27a53e4491004
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357707"
---
# <a name="translator-text-api-30-dictionary-lookup"></a>Translator metin çevirisi API'si 3.0: Sözlük Arama

Bir sözcük ve az sayıda deyimler için alternatif çevirileri sağlar. Her çeviri arka çevirileri listesini ve bir-konuşma bölümü vardır. Arka-çevirileri bir kullanıcı bağlamında çeviri anlamak etkinleştirin. [Sözlük örnek](./v3-0-dictionary-examples.md) işlemi her çeviri çifti bakın örnek kullanımları daha fazla detaya sağlar.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek Parametreler şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td>*Gerekli parametre*.<br/>İstemci tarafından istenen API sürümü. Değer olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>from</td>
    <td>*Gerekli parametre*.<br/>Giriş metninin dilini belirtir. Kaynak dili olmalıdır [desteklenen diller](./v3-0-languages.md) dahil `dictionary` kapsam.</td>
  </tr>
  <tr>
    <td>-</td>
    <td>*Gerekli parametre*.<br/>Çıkış metnini dilini belirtir. Hedef Dil olmalıdır [desteklenen diller](./v3-0-languages.md) dahil `dictionary` kapsam.</td>
  </tr>
</table>

İstek üst bilgileri ekleyin:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Kimlik doğrulaması üstbilgi</td>
    <td><em>Gerekli istek üst bilgisi</em>.<br/>Bkz: <a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">kimlik doğrulaması için kullanılabilir seçenekler</a>.</td>
  </tr>
  <tr>
    <td>İçerik türü</td>
    <td>*Gerekli istek üst bilgisi*.<br/>Akıştaki içerik türünü belirtir. Olası değerler şunlardır: `application/json`.</td>
  </tr>
  <tr>
    <td>İçerik uzunluğu</td>
    <td>*Gerekli istek üst bilgisi*.<br/>İstek gövdesi uzunluğu.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı*.<br/>İstek benzersiz olarak tanımlanabilmesi için bir istemci tarafından oluşturulan GUID. İzleme kimliği adlı bir sorgu parametresi kullanarak sorgu dizesinde eklerseniz bu başlığı atlayabilirsiniz `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İstek gövdesinde bir JSON dizisidir. Her dizi öğesi bir dize özelliği adlı bir JSON nesnesidir `Text`, aranan terimi gösterir.

```json
[
    {"Text":"fly"}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 10 öğe olabilir.
* Bir dizi öğesinin metin değeri boşluklar dahil 100 karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt için Giriş dizisinin her bir dizede tek bir sonuç ile bir JSON dizisidir. Sonuç nesnesi, aşağıdaki özellikleri içerir:

  * `normalizedSource`: Kaynak terim normalleştirilmiş şeklinde veren bir dize. Örneğin, istek "JOHN" ise, normalleştirilmiş form "john" olacaktır. Bu alanın içeriğini giriş olur [arama örnekler](./v3-0-dictionary-examples.md).
    
  * `displaySource`: Kaynak ifadesi bir formda en iyi veren bir dize, son kullanıcı görüntüsü için uygundur. Örneğin, Giriş "JOHN" ise, her zamanki adının yazımını görüntüleme formu yansıtır: "John". 

  * `translations`: Çeviriler kaynak terim listesi. Listedeki her öğe, aşağıdaki özelliklere sahip bir nesnedir:

    * `normalizedTarget`: Bu terim normalleştirilmiş biçiminin hedef dilde veren bir dize. Bu değer, giriş olarak kullanılmalıdır [arama örnekler](./v3-0-dictionary-examples.md).

    * `displayTarget`: Bir dize ifadesi hedef dil ve bir formda en iyi vererek son kullanıcı görüntüsü için uygundur. Genellikle, bu yalnızca farklılık `normalizedTarget` büyük/küçük harf bakımından. Örneğin, "Juan" olacaktır gibi özel isim `normalizedTarget = "juan"` ve `displayTarget = "Juan"`.

    * `posTag`: Bu terim bir konuşma bölümü etiketle ilişkilendirmek için bir dize.

        | Etiket adı | Açıklama  |
        |----------|--------------|
        | AYAR      | Sıfat   |
        | ADV      | Zarflar      |
        | CONJ     | Bağlaçlar |
        | DET      | Determiners  |
        | KALICI    | Verbs        |
        | İSİM     | Adlar        |
        | HAZIRLIĞI     | Edat |
        | PRON     | Zamirler     |
        | VERB     | Verbs        |
        | DİĞER    | Diğer        |

        Bir uygulama Not Bu etiketleri-İngilizce yan etiketleme ve ardından her kaynak/hedef çifti için en sık rastlanan etiketi alma konuşma bölümü olarak belirlenmiştir. Bu nedenle kişiler için farklı bir konuşma bölümü etiketi İngilizce, İspanyolca bir kelime sık Çevir, etiketleri (İspanyolca bir kelime göre) yanlış olan sonlandırabiliriz.

    * `confidence`: Değeri 0.0 ile 1.0 "olasılık" temsil eden (veya belki de daha doğru bir şekilde "olasılık eğitim veri") Bu çeviri çifti. Bir kaynak sözcük için güven puanlarını toplamı olabilir veya 1.0 toplamı değildir. 

    * `prefixWord`: Çeviri ön eki olarak görüntülenecek word veren bir dize. Şu anda determiners gendered dillerde isimleri, gendered determiner budur. Örneğin, "mosca" İspanyolca dişi bir isim olduğundan "on", İspanyolca bir kelime "mosca" ön ekidir. Bu yalnızca çeviri ve kaynağında bağlıdır. Önek yok ise, boş bir dize olacaktır.
    
    * `backTranslations`: "Geri çevirileri" hedef listesi. Örneğin, hedef için çevirebilir sözcükleri kaynağı. İstenen kaynak sözcüğünü içeren liste sağlanır (örneğin, word kaynağı olan görünüyorsa "Çık" olan ve ardından içinde "Çık" olacağı garanti edilir `backTranslations` listesi). Ancak, bunu ilk konumda olmasını garanti edilmez ve genellikle olmayacaktır. Her öğeyi `backTranslations` liste, şu özellikler tarafından açıklanan bir nesnedir:

        * `normalizedText`: Geri çevirme hedef kaynak terimi normalleştirilmiş şeklinde veren bir dize. Bu değer, giriş olarak kullanılmalıdır [arama örnekler](./v3-0-dictionary-examples.md).        

        * `displayText`: Geri çevirme hedefinin bir formda en iyi kaynak terimi veren bir dize, son kullanıcı görüntüsü için uygundur.

        * `numExamples`: Bu çeviri çifti için kullanılabilir örneklerin sayısını temsil eden bir tamsayı. Gerçek örnekler alınan, ayrı bir çağrıyla [arama örnekler](./v3-0-dictionary-examples.md). Sayı çoğunlukla bir UX görüntüde kolaylaştırmak için tasarlanmıştır Örneğin, bir kullanıcı arabirimi örnekleri sayısı sıfırdan büyük olması durumunda, geri çevirme için köprü ekleme ve örnek varsa, geri çevirme düz metin olarak göster. Bir çağrı tarafından döndürülen örnek gerçek sayısı Not [arama örnekler](./v3-0-dictionary-examples.md) olabilir küçüktür `numExamples`, ek bir filtreleme "kötü" örnekler kaldırmak için çalışma sırasında uygulanabilir.
        
        * `frequencyCount`: Bu çeviri çifti veri sıklığını temsil eden bir tamsayı. Ana amacı, bu alan, bir kullanıcı arabirimi arka çevirileri en sık kullanılan terimler ilk olacak şekilde sıralamak için bir yol sağlamaktır.

    > [!NOTE]
    > Aranan yukarı olan terim sözlüğünde yoksa yanıt 200 (Tamam). ancak `translations` listesi boş bir listedir.

## <a name="examples"></a>Örnekler

Bu örnekte, İspanyolca İngilizce dönemi alternatif çevirileri aramak gösterilmektedir `fly` .

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly'}]"
```

---

(Daha anlaşılır olması için kısaltılmıştır) yanıt gövdesi aşağıdaki gibidir:

```
[
    {
        "normalizedSource":"fly",
        "displaySource":"fly",
        "translations":[
            {
                "normalizedTarget":"volar",
                "displayTarget":"volar",
                "posTag":"VERB",
                "confidence":0.4081,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":4637},
                    {"normalizedText":"flying","displayText":"flying","numExamples":15,"frequencyCount":1365},
                    {"normalizedText":"blow","displayText":"blow","numExamples":15,"frequencyCount":503},
                    {"normalizedText":"flight","displayText":"flight","numExamples":15,"frequencyCount":135}
                ]
            },
            {
                "normalizedTarget":"mosca",
                "displayTarget":"mosca",
                "posTag":"NOUN",
                "confidence":0.2668,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":1697},
                    {"normalizedText":"flyweight","displayText":"flyweight","numExamples":0,"frequencyCount":48},
                    {"normalizedText":"flies","displayText":"flies","numExamples":9,"frequencyCount":34}
                ]
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```

Bu örnek, aranan terim için geçerli bir sözlük çifti olmadığında ne olacağını gösterir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly123456'}]"
```

---

Terim sözlükte bulunamadı olduğundan, boş bir yanıt gövdesini içerir `translations` listesi.

```
[
    {
        "normalizedSource":"fly123456",
        "displaySource":"fly123456",
        "translations":[]
    }
]
```
