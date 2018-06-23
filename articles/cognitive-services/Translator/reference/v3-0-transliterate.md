---
title: Microsoft Çeviricisi metin API Transliterate yöntemi | Microsoft Docs
description: Microsoft Çeviricisi metin API Transliterate yöntemini kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: fdd6fa9236f0c02685198b6de3228c444993dad6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354970"
---
# <a name="text-api-30-transliterate"></a>Metin API 3.0: Transliterate

Bir dilde bir komut dosyasından başka bir komut dosyasına dönüştürür.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
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
    <td>Dil</td>
    <td>*Gerekli parametre*.<br/>Bir komut dosyasından diğerine Dönüştürülecek metin dilini belirtir. Olası dilleri içinde listelenen `transliteration` kapsam elde hizmet için sorgulayarak kendi [desteklenen diller](.\v3-0-languages.md).</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Gerekli parametre*.<br/>Giriş metni tarafından kullanılan komut dosyası belirtir. Arama [desteklenen diller](.\v3-0-languages.md) kullanarak `transliteration` seçilen dil için giriş komut dosyaları bulmak için kapsam.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Gerekli parametre*.<br/>Çıkış betiğini belirtir. Arama [desteklenen diller](.\v3-0-languages.md) kullanarak `transliteration` çıkış betikleri giriş dili seçili bileşimi için kullanılabilir ve giriş komut dosyasını bulmak için kapsam.</td>
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

İstek gövdesi bir JSON dizisidir. Her dizi öğesi adlı bir dize özelliği ile bir JSON nesnesidir `Text`, dönüştürülecek dizeyi temsil eder.

```json
[
    {"Text":"こんにちは"},
    {"Text":"さようなら"}
]
```

Aşağıdaki sınırlamalar uygulanır:

* Dizi en fazla 10 öğeler bulunabilir.
* Bir dizi öğesinin metin değeri, boşluklar dahil olmak üzere 1.000 karakterden uzun olamaz.
* İstekte bulunan tüm metin, boşluklar dahil olmak üzere 5.000 karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı yanıt, giriş dizideki her öğe için bir sonuç içeren bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `text`: Giriş dizesi çıkış komut dosyasına dönüştürme sonucu bir dize.
  
  * `script`: Çıktıda kullanılan komut dosyası belirtme bir dize.

JSON yanıt örneğidir:

```json
[
    {"text":"konnnichiha","script":"Latn"},
    {"text":"sayounara","script":"Latn"}
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

Aşağıdaki örnekte, iki Japonca dizelerini Japonca Romanized dönüştürmek gösterilmiştir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

Bu örnek istek için JSON yükü:

```
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Unicode karakterler desteklemeyen bir komut satırı penceresinde cUrl kullanıyorsanız, aşağıdaki JSON yükü alın ve adlı bir dosyaya Kaydet `request.txt`. Dosyayı kaydettiğinizden emin olun `UTF-8` kodlama.

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```

---
