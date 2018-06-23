---
title: Microsoft Çeviricisi metin API sözlük arama yöntemi | Microsoft Docs
description: Microsoft Çeviricisi metin API sözlük arama yöntemini kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 31435fcfca61517bfc72d534e911a1dcadbee52b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354976"
---
# <a name="text-api-30-dictionary-lookup"></a>Metin API 3.0: Sözlük arama

Alternatif çevirileri bir sözcük ve deyimler az sayıda sağlar. Her çeviri bölümü-in-konuşma ve geri çevirileri listesini içeriyor. Geri çevirileri bir kullanıcı bağlamında çeviri anlamak etkinleştirin. [Sözlük örnek](.\v3-0-dictionary-examples.md) işlemin veren her çeviri çifti bkz: örnek kullanımları başka inceleyin.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen isteği Parametreler şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td>*Gerekli parametre*.<br/>İstemci tarafından istenilen API sürümü. Değeri olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>başlangıç</td>
    <td>*Gerekli parametre*.<br/>Giriş metni dilini belirtir. Kaynak dili biri olmalıdır [desteklenen diller](.\v3-0-languages.md) dahil `dictionary` kapsam.</td>
  </tr>
  <tr>
    <td>-</td>
    <td>*Gerekli parametre*.<br/>Çıktı metin dilini belirtir. Hedef Dil biri olmalıdır [desteklenen diller](.\v3-0-languages.md) dahil `dictionary` kapsam.</td>
  </tr>
</table>

İstek üstbilgilerini şunları içerir:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>_Bir yetkilendirme_<br/>_Üstbilgi_</td>
    <td>*Gerekli istek üstbilgisi*.<br/>Bkz: [kimlik doğrulama için kullanılabilir seçenekleri](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Gerekli istek üstbilgisi*.<br/>Yükü içerik türünü belirtir. Olası değerler şunlardır: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Gerekli istek üstbilgisi*.<br/>İstek gövdesini uzunluğu.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı*.<br/>İstek benzersiz şekilde tanımlamak için bir istemci tarafından üretilen GUID. İzleme kimliği adlı bir sorgu parametresini kullanarak sorgu dizesinde eklerseniz, bu başlığı atlayabilirsiniz `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İstek gövdesi bir JSON dizisidir. Her dizi öğesi adlı bir dize özelliği ile bir JSON nesnesidir `Text`, için arama terimi gösterir.

```json
[
    {"Text":"fly"}
]
```

Aşağıdaki sınırlamalar uygulanır:

* Dizi en fazla 10 öğeler bulunabilir.
* Bir dizi öğesinin metin değeri, boşluklar dahil olmak üzere 100 karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı yanıt Giriş dizisindeki her dize için bir sonuç içeren bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `normalizedSource`: Kaynak terim normalleştirilmiş biçiminde vermiş bir dize. Örneğin, "JOHN" isteğiyse normalleştirilmiş form "john" olacaktır. Bu alanın içeriği girdisi hale [arama örnekler](.\v3-0-dictionary-examples.md).
    
  * `displaySource`: Kaynak terim en iyi bir biçimde vermiş bir dizeyi son kullanıcı görüntü için uygundur. Örneğin, Giriş "JOHN" ise, her zamanki adının yazımını görüntüleme formu yansıtır: "John". 

  * `translations`: Kaynak terim için çeviriler bir listesi. Listedeki her öğeye, aşağıdaki özelliklere sahip bir nesnedir:

    * `normalizedTarget`: Bu terim normalleştirilmiş biçiminde hedef dilde vermiş bir dize. Bu değer için giriş olarak kullanılması gereken [arama örnekler](.\v3-0-dictionary-examples.md).

    * `displayTarget`: Hedef Dil ve form terimi en iyi vermiş bir dizeyi son kullanıcı görüntü için uygundur. Genellikle, bu yalnızca farklılık `normalizedTarget` büyük/küçük harf bakımından. Örneğin, bir uygun "Juan" yok gibi isim `normalizedTarget = "juan"` ve `displayTarget = "Juan"`.

    * `posTag`: Bu terim bir konuşma bölümü etiketi ile ilişkilendirme bir dize.

        | Etiket adı | Açıklama  |
        |----------|--------------|
        | AYAR      | Sıfatları   |
        | ADV      | Zarflar      |
        | CONJ     | Bağlaç |
        | DET      | Determiners  |
        | KALICI    | Fiiller        |
        | İSİM     | İsimleri        |
        | HAZIRLIĞI     | Edatlar |
        | PRON     | Zamirler     |
        | FİİL     | Fiiller        |
        | DİĞER    | Diğer        |

        Bu etiketler bölümü-in-İngilizce yan etiketleme ve ardından her kaynak/hedef çifti için en sık rastlanan etiketi getirmeden konuşma tarafından belirlenen bir uygulama Not. Bu nedenle kişiler sık İngilizce farklı bir konuşma bölümü etiketi İspanyolca Word'e Çevir, etiketler (İspanyolca word göre) yanlış olan bitebilir.

    * `confidence`: Bir değer 0,0 ile 1,0 "güvenirlik" temsil eden arasında (ya da belki daha doğru bir şekilde "olasılık eğitim verileri içinde"), çeviri çifti. Bir kaynak sözcük güvenirlik puanlarını toplamını olabilir veya 1.0 sum değil. 

    * `prefixWord`: Bir dizeyi vermiş Word'ün çeviri önek olarak görüntüler. Şu anda determiners gendered dillerde isimleri, gendered determiner budur. Örneğin, "mosca" dişi isim İspanyolca olduğundan önek İspanyolca Word "mosca" "la",'dir. Bu yalnızca çeviri ve kaynağında bağlıdır. Önek yoksa boş dize olacaktır.
    
    * `backTranslations`: "Geri çevirisinde" hedef bir listesi. Örneğin, hedef için çevirebilir sözcükler kaynağı. Listenin istenen kaynak sözcüğünü içeren garanti (örn., word kaynağı olan görünüyorsa "Çık" olduğundan, ardından "Çık" içinde olacağını garanti `backTranslations` listesi). Ancak, bu ilk konumda olması garanti edilmemiştir ve genellikle olmayacaktır. Her öğeye `backTranslations` listesi, aşağıdaki özellikleri tarafından tanımlanan bir nesnedir:

        * `normalizedText`: Normalleştirilmiş formun hedef çevrilmesi geri olan kaynak koşulunun vermiş bir dize. Bu değer için giriş olarak kullanılması gereken [arama örnekler](.\v3-0-dictionary-examples.md).        

        * `displayText`: Son kullanıcı görüntülenmek üzere geri çevirme hedef en iyi bir biçimde olduğu kaynak terim vermiş bir dizeyi uygundur.

        * `numExamples`: Bu çeviri çifti için kullanılabilir örneklerin sayısını temsil eden bir tam sayı. Gerçek örnekler alınan, ayrı çağrısıyla [arama örnekler](.\v3-0-dictionary-examples.md). Sayı çoğunlukla bir UX görüntüde kolaylaştırmak için tasarlanmıştır Örneğin, bir kullanıcı arabirimi örnekleri sayısı sıfırdan büyük olursa geri çevirme için bir köprü ekleyin ve hiçbir örnekler varsa geri çevirme düz metin olarak göster. Bir çağrı tarafından döndürülen örnekler gerçek sayısı Not [arama örnekler](.\v3-0-dictionary-examples.md) olabilir değerinden `numExamples`, ek filtreleme "bozuk" örnekler kaldırmak için anında uygulanabilir.
        
        * `frequencyCount`: Bu çeviri çifti veri sıklığını temsil eden bir tamsayı. Ana amacı, bu alan, en sık rastlanan koşulları ilk; bu nedenle geri çevirileri sıralamak için bir yol ile bir kullanıcı arabirimi sağlamaktır.

    > [!NOTE]
    > Aranan yukarı olan terim sözlükte yok yanıt 200 (Tamam) varsa, ancak `translations` listesi boş bir listedir.

## <a name="examples"></a>Örnekler

Bu örnek İspanyolca alternatif Çeviriler İngilizce koşulunun arama gösterilmektedir `fly` .

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly'}]"
```

---

(Daha anlaşılır olması için kısaltılmış) yanıt gövdesi aşağıdaki gibidir:

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

Bu örnek, aranan terimi için geçerli sözlük çifti yok ne olacağını gösterir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly123456'}]"
```

---

Terim sözlüğünde bulunamadı olduğundan, boş bir yanıt gövdesi içeriyor `translations` listesi.

```
[
    {
        "normalizedSource":"fly123456",
        "displaySource":"fly123456",
        "translations":[]
    }
]
```
