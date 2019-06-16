---
title: Translator metin API'si sözlük örnekleri yöntemi
titlesuffix: Azure Cognitive Services
description: Translator metin API'si sözlük örnekleri yöntemi kullanın.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-pawal
ms.openlocfilehash: 526fc3e51691773e27530a0fa775f4f07ee94acd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389766"
---
# <a name="translator-text-api-30-dictionary-examples"></a>Translator metin çevirisi API'si 3.0: Sözlük Örnekleri

Sözlük terimlerini bağlam içinde nasıl kullanılacağını gösteren örnekler sağlar. Bu işlem dağıtımınızla birlikte kullanılan [sözlük arama](./v3-0-dictionary-lookup.md).

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
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

İstek gövdesinde bir JSON dizisidir. Her dizi öğesi, aşağıdaki özelliklere sahip bir JSON nesnesidir:

  * `Text`: Terim araması belirten bir dize. Bu değeri olmalıdır bir `normalizedText` önceki geri çevirileri alanını [sözlük arama](./v3-0-dictionary-lookup.md) isteği. Değerini olabilir `normalizedSource` alan.

  * `Translation`: Daha önce döndürülen çevrilmiş metin belirten bir dize [sözlük arama](./v3-0-dictionary-lookup.md) işlemi. Bu değeri olmalıdır `normalizedTarget` alanındaki `translations` listesi [sözlük arama](./v3-0-dictionary-lookup.md) yanıt. Hizmet örnekleri için belirli kaynak-hedef sözcük çiftini döndürür.

Bir örnek verilmiştir:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 10 öğe olabilir.
* Bir dizi öğesinin metin değeri boşluklar dahil 100 karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt için Giriş dizisinin her bir dizede tek bir sonuç ile bir JSON dizisidir. Sonuç nesnesi, aşağıdaki özellikleri içerir:

  * `normalizedSource`: Kaynak terim normalleştirilmiş şeklinde veren bir dize. Genellikle, bu değeri olarak aynı olmalıdır `Text` isteğin gövdesindeki eşleşen liste dizinindeki alan.
    
  * `normalizedTarget`: Normalleştirilmiş formun hedef döneminin veren bir dize. Genellikle, bu değeri olarak aynı olmalıdır `Translation` isteğin gövdesindeki eşleşen liste dizinindeki alan.
  
  * `examples`: Örnekler (kaynak terim için hedef terim) listesini çifti. Listedeki her öğe, aşağıdaki özelliklere sahip bir nesnedir:

    * `sourcePrefix`: Dize birleştirme _önce_ değerini `sourceTerm` tam bir örnek oluşturmak için. Olmalıydı zaten var olduğundan, bir boşluk karakteri eklemeyin. Bu değer, boş bir dize olabilir.

    * `sourceTerm`: Gerçek terimi dizesi eşit aranabilir. Dize ile eklenen `sourcePrefix` ve `sourceSuffix` tam bir örnek oluşturmak için. Değeri, kullanıcı arabiriminde, örneğin kalın işaretlenebilir şekilde ayrılır.

    * `sourceSuffix`: Dize birleştirme _sonra_ değerini `sourceTerm` tam bir örnek oluşturmak için. Olmalıydı zaten var olduğundan, bir boşluk karakteri eklemeyin. Bu değer, boş bir dize olabilir.

    * `targetPrefix`: Benzer bir dize `sourcePrefix` ancak hedef.

    * `targetTerm`: Benzer bir dize `sourceTerm` ancak hedef.

    * `targetSuffix`: Benzer bir dize `sourceSuffix` ancak hedef.

    > [!NOTE]
    > Sözlükte örnek varsa, yanıt 200 (Tamam). ancak `examples` listesi boş bir listedir.

## <a name="examples"></a>Örnekler

Bu örnekte, İngilizce dönemi oluşan çifti için örnekleri aramak gösterilmektedir `fly` ve İspanyolca çevirisini `volar`.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

(Daha anlaşılır olması için kısaltılmıştır) yanıt gövdesi aşağıdaki gibidir:

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
