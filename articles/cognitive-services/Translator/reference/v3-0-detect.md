---
title: Translator metin çevirisi API'si algılama yöntemi
titlesuffix: Azure Cognitive Services
description: Translator metin API'si algılama yöntemini kullanın.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: v-jansko
ms.openlocfilehash: ea8fe989dd0ef7026957153fb5c9836742d008dd
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65797504"
---
# <a name="translator-text-api-30-detect"></a>Translator metin çevirisi API'si 3.0: Detect

Bir metin parçası dilini tanımlar.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/detect?api-version=3.0
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
    <td>Content-Type</td>
    <td>*Gerekli istek üst bilgisi*.<br/>Akıştaki içerik türünü belirtir. Olası değerler şunlardır: `application/json`.</td>
  </tr>
  <tr>
    <td>İçerik uzunluğu</td>
    <td>*Gerekli istek üst bilgisi*.<br/>İstek gövdesi uzunluğu.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı*.<br/>İstek benzersiz olarak tanımlanabilmesi için bir istemci tarafından oluşturulan GUID. İzleme kimliği adlı bir sorgu parametresi kullanarak sorgu dizesinde eklerseniz bu başlığı atlayabilirsiniz Not `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İstek gövdesinde bir JSON dizisidir. Her dizi öğesi bir dize özelliği adlı bir JSON nesnesidir `Text`. Dil algılama değerine uygulanır `Text` özelliği. Örnek istek gövdesi aşağıdaki gibi görünür:

```json
[
    { "Text": "Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal." }
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 100 öğeleri olabilir.
* Bir dizi öğesinin metin değeri boşluk da dahil olmak üzere 10.000 karakterden uzun olamaz.
* İstekte bulunan tüm metin alanları dahil olmak üzere 50. 000'karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt için Giriş dizisinin her bir dizede tek bir sonuç ile bir JSON dizisidir. Sonuç nesnesi, aşağıdaki özellikleri içerir:

  * `language`: Algılanan dil kodu.

  * `score`: Sonuçta güvenle gösteren bir float değeri. Sıfır ile bir puan arasındadır ve düşük puanı düşük güven gösterir.

  * `isTranslationSupported`: Algılanan dilin metin çevirisi için desteklenen dilleri ise true olan bir Boole değeri.

  * `isTransliterationSupported`: Algılanan dilin alfabeye için desteklenen dilleri ise true olan bir Boole değeri.
  
  * `alternatives`: Diğer olası diller dizisi. Dizinin her öğesinin yukarıda listelenen aynı özelliklere sahip başka bir nesnedir: `language`, `score`, `isTranslationSupported` ve `isTransliterationSupported`.

JSON yanıtı örneği verilmiştir:

```json
[
  {
    "language": "de",
    "score": 0.92,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 0.23,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "sk",
        "score": 0.23,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>X-RequestId</td>
    <td>İstek tanımlamak için hizmeti tarafından oluşturulan değeri. Sorun giderme amacıyla kullanılır.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Yanıt durum kodları

Bir isteği döndüren olası HTTP durum kodları şunlardır: 

<table width="100%">
  <th width="20%">Durum Kodu</th>
  <th>Açıklama</th>
  <tr>
    <td>200</td>
    <td>Başarılı.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerden biri eksik veya geçerli değil. İstek parametreleri yeniden denemeden önce düzeltin.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>İsteğin kimliği doğrulanamadı. Kimlik bilgilerinin belirtilen ve geçerli olup olmadığını denetleyin.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>İstek yetkili değil. Ayrıntıları hata iletisine bakın. Bu, genellikle bir deneme aboneliği ile sağlanan tüm ücretsiz çevirileri kullanılmış olan olduğunu gösterir.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>İstemci istek sınırları aştığı için sunucu isteği reddetti.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Sorun devam ederse, raporu ile: tarih ve saat hatanın yanıt üst bilgisinden istek tanımlayıcısı `X-RequestId`ve istemci tanımlayıcısı istek üst bilgisinden `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Sorun devam ederse, raporu ile: tarih ve saat hatanın yanıt üst bilgisinden istek tanımlayıcısı `X-RequestId`ve istemci tanımlayıcısı istek üst bilgisinden `X-ClientTraceId`.</td>
  </tr>
</table> 

Bir hata oluşursa, isteği ayrıca JSON hata yanıtı döndürür. 3 haneli HTTP durum kodu için 3 basamaklı bir sayı daha da ardından 6 basamaklı bir sayı birleştirme kategorilere ayırma hatası hata kodudur. Genel hata kodları bulunabilir [v3 Translator Text API başvuru sayfası](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors). 

## <a name="examples"></a>Örnekler

Aşağıdaki örnekte, metin çevirisi için desteklenen dilleri alınacak gösterilmektedir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/detect?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'What language is this text written in?'}]"
```

---
