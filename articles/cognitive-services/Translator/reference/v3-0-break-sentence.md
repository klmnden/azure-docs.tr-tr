---
title: Microsoft Çeviricisi metin API BreakSentence yöntemi | Microsoft Docs
description: Microsoft Çeviricisi metin API BreakSentence yöntemini kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 8ce6644d21b397ea0e7f2e71e3c3a5a96638eec5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354982"
---
# <a name="text-api-30-breaksentence"></a>Metin API 3.0: BreakSentence

Bir metin parçası cümle sınırlarında konumlandırma tanımlar.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen isteği Parametreler şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td>*Gerekli sorgu parametresi*.<br/>İstemci tarafından istenilen API sürümü. Değeri olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>Dil</td>
    <td>*İsteğe bağlı sorgu parametresi*.<br/>Giriş metni dilinin tanımlama dili etiketi. Bir kod belirtilmezse, otomatik dil algılama uygulanır.</td>
  </tr>
  <tr>
    <td>komut dosyası</td>
    <td>*İsteğe bağlı sorgu parametresi*.<br/>Komut dosyası etiketinin giriş metin tarafından kullanılan betik tanımlama. Bir komut dosyası belirtilmezse, varsayılan komut dosyası dili varsayılır.</td>
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
    <td>*İsteğe bağlı*.<br/>İstek benzersiz şekilde tanımlamak için bir istemci tarafından üretilen GUID. İzleme kimliği adlı bir sorgu parametresini kullanarak sorgu dizesinde eklerseniz, bu başlığı atlayabilirsiniz Not `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İstek gövdesi bir JSON dizisidir. Her dizi öğesi adlı bir dize özelliği ile bir JSON nesnesidir `Text`. Tümce sınırları değeri için hesaplanan `Text` özelliği. Metin bir parçası olan bir örnek istek gövde gibi görünür:

```json
[
    { "Text": "How are you? I am fine. What did you do today?" }
]
```

Aşağıdaki sınırlamalar uygulanır:

* Dizi en çok 100 öğeler bulunabilir.
* Bir dizi öğesinin metin değeri, boşluklar dahil olmak üzere 10.000 karakterden uzun olamaz.
* İstekte bulunan tüm metin, boşluklar dahil olmak üzere 50.000 karakterden uzun olamaz.
* Varsa `language` sorgu parametresi belirtilirse, ardından dizi öğelerin tümü aynı dilde olmalıdır. Aksi takdirde, dil otomatik algılama, her bir dizi öğesine bağımsız olarak uygulanır.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı yanıt Giriş dizisindeki her dize için bir sonuç içeren bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `sentLen`: Metin öğesi cümlelerde uzunlukları temsil eden tamsayılar bir dizi. Dizi uzunluğu cümleleri sayısı ve her tümcenin uzunluğu değerlerdir. 

  * `detectedLanguage`: Aşağıdaki özellikleri aracılığıyla algılanan dil açıklayan bir nesneyi:

     * `language`: Algılanan dil kodu.

     * `score`: Güvenilirlik sonucundaki gösteren bir float değeri. Sıfır ile bir puan arasındadır ve düşük puanı düşük güvenilirlik gösterir.
     
    Unutmayın `detectedLanguage` özelliği yalnızca sonuç nesnesinde mevcut dil otomatik algılamayı istendiğinde.

JSON yanıt örneğidir:

```json
[
  {
    "sentenceLengths": [ 13, 11, 22 ]
    "detectedLanguage": {
      "language": "en",
      "score": 401
    },
  }
]
```

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>X-RequestId</td>
    <td>İstek tanımlamak için hizmeti tarafından oluşturulan değer. Sorun giderme amacıyla kullanılır.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Yanıtı durum kodları

Bir istek döndürür olası HTTP durum kodları şunlardır: 

<table width="100%">
  <th width="20%">Durum Kodu</th>
  <th>Açıklama</th>
  <tr>
    <td>200</td>
    <td>Başarılı.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerden biri eksik veya geçerli değil. İstek parametreleri denemeden önce düzeltin.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>İsteğin kimliği doğrulanamadı. Kimlik bilgilerinin belirtilen ve geçerli olduğunu denetleyin.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>İstek yetkili değil. Ayrıntılar hata iletisine bakın. Bu, genellikle bir deneme aboneliği ile sağlanan tüm boş çevirileri kullanılmış olduğunu gösterir.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>Çağrıyı yapan çok sayıda istek gönderiyor.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Sorun devam ederse, kendisiyle Rapor: tarih ve saat hata, istek tanımlayıcısı yanıt başlığından `X-RequestId`ve istemci tanımlayıcısı istek üstbilgisinden `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Sorun devam ederse, kendisiyle Rapor: tarih ve saat hata, istek tanımlayıcısı yanıt başlığından `X-RequestId`ve istemci tanımlayıcısı istek üstbilgisinden `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, tek bir cümle için cümle sınırları elde gösterilmektedir. Tümcenin dilini hizmeti tarafından otomatik olarak algılanır.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'How are you? I am fine. What did you do today?'}]"
```

---

