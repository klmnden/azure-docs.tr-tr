---
title: Microsoft Çeviricisi metin API sözlük örnekleri yöntemi | Microsoft Docs
description: Microsoft Çeviricisi metin API sözlük örnekleri yöntemini kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9960f3be42090edaec1df935d70e4c1a0d25b691
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354977"
---
# <a name="text-api-30-dictionary-examples"></a>Metin API 3.0: Sözlük örnekleri

Sözlük terimlerini bağlamda nasıl kullanıldığını gösteren örnekler sağlar. Bu işlem dağıtımınızla birlikte kullanılan [sözlük arama](.\v3-0-dictionary-lookup.md).

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
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

İstek gövdesi bir JSON dizisidir. Her dizi öğesi, aşağıdaki özelliklere sahip bir JSON nesnesidir:

  * `Text`: Arama terimi belirten bir dize. Bu değeri olmalıdır bir `normalizedText` önceki arka çevirileri alanının [sözlük arama](.\v3-0-dictionary-lookup.md) isteği. Değerini olabilir `normalizedSource` alan.

  * `Translation`: Daha önce tarafından döndürülen çevrilmiş metni belirten bir dize [sözlük arama](.\v3-0-dictionary-lookup.md) işlemi. Bu değerden olmalıdır `normalizedTarget` alanındaki `translations` listesi [sözlük arama](.\v3-0-dictionary-lookup.md) yanıt. Hizmet örnekleri belirli kaynak hedef sözcük çifti için döndürür.

Bir örnek verilmiştir:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Aşağıdaki sınırlamalar uygulanır:

* Dizi en fazla 10 öğeler bulunabilir.
* Bir dizi öğesinin metin değeri, boşluklar dahil olmak üzere 100 karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı yanıt Giriş dizisindeki her dize için bir sonuç içeren bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `normalizedSource`: Kaynak terim normalleştirilmiş biçiminde vermiş bir dize. Genellikle, bu değeri için özdeş olmalıdır `Text` istek gövdesinde eşleşen liste dizinindeki alan.
    
  * `normalizedTarget`: Normalleştirilmiş formun hedef koşulunun vermiş bir dize. Genellikle, bu değeri için özdeş olmalıdır `Translation` istek gövdesinde eşleşen liste dizinindeki alan.
  
  * `examples`: Örnekler (kaynak terim, hedef terim) için listesi çifti. Listedeki her öğeye, aşağıdaki özelliklere sahip bir nesnedir:

    * `sourcePrefix`: Birleştirmek için dize _önce_ değerini `sourceTerm` tam bir örnek oluşturmak için. Olması gerekirken, zaten var olduğundan, bir boşluk karakteri eklemeyin. Bu değer boş bir dize olabilir.

    * `sourceTerm`: Gerçek terim bir dizeyi eşit Aranan. Dize ile eklenen `sourcePrefix` ve `sourceSuffix` tam bir örnek oluşturmak için. Değeri, kullanıcı arabiriminde, örn., kalın işaretlenebilir için ayrılmış.

    * `sourceSuffix`: Birleştirmek için dize _sonra_ değerini `sourceTerm` tam bir örnek oluşturmak için. Olması gerekirken, zaten var olduğundan, bir boşluk karakteri eklemeyin. Bu değer boş bir dize olabilir.

    * `targetPrefix`: Benzer bir dize `sourcePrefix` ancak hedefi için.

    * `targetTerm`: Benzer bir dize `sourceTerm` ancak hedefi için.

    * `targetSuffix`: Benzer bir dize `sourceSuffix` ancak hedefi için.

    > [!NOTE]
    > Sözlükteki hiçbir örnek varsa, yanıt 200 (Tamam) olan ancak `examples` listesi boş bir listedir.

## <a name="examples"></a>Örnekler

Bu örnek örnekler İngilizce şartını oluşan çifti için arama gösterilmektedir `fly` ve kendi İspanyolca çevirisi `volar`.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

(Daha anlaşılır olması için kısaltılmış) yanıt gövdesi aşağıdaki gibidir:

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```
