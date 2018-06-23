---
title: Microsoft Çeviricisi metin API dilleri yöntemi | Microsoft Docs
description: Microsoft Çeviricisi metin API dilleri yöntemini kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 93c06218a560faf439f05903438d021b372ce257
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354155"
---
# <a name="text-api-30-languages"></a>Metin API 3.0: dilleri

Şu anda diğer metin API işlemleri tarafından desteklenen diller kümesini alır. 

## <a name="request-url"></a>İstek URL'si

Gönderme bir `GET` isteği:
```HTTP
https://api.cognitive.microsofttranslator.com/languages?api-version=3.0
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
    <td>scope</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Döndürülecek dil grubu tanımlama adlarının virgülle ayrılmış listesi. Grup adları izin verilir: `translation`, `transliteration` ve `dictionary`. Kapsam verilen sonra geçişine eşdeğer olduğu tüm grupları döndürülür `scope=translation,transliteration,dictionary`. Desteklenen diller hangi kümesi senaryonuz için uygun olduğuna karar vermek için açıklamasını görmek [yanıt nesnesi](#response-body).</td>
  </tr>
</table> 

İstek üstbilgilerini şunlardır:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Kabul dili</td>
    <td>*İsteğe bağlı istek üstbilgisi*.<br/>Kullanıcı arabirimi dizeleri için kullanılacak dili. Yanıt alanlarının adları, diller veya bölgelerin adlarını bazılarıdır. Bu adları döndürülen dil tanımlamak için bu parametreyi kullanın. Dili, doğru biçimlendirilmiş BCP 47 dil etiket sağlayarak belirtilir. Örneğin, değeri kullanmak `fr` değerini kullanın veya Fransızca adlarında isteği için `zh-Hant` isteği adlarına Geleneksel Çince.<br/>Adları hedef dil belirtilmediğinde veya yerelleştirme kullanılabilir olmadığında İngilizce dilinde sağlanır.
    </td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı istek üstbilgisi*.<br/>İstek benzersiz şekilde tanımlamak için bir istemci tarafından üretilen GUID.</td>
  </tr>
</table> 

Kimlik doğrulama dil kaynakları almak için gerekli değildir.

## <a name="response-body"></a>Yanıt gövdesi

Bir istemcinin kullandığı `scope` sorgu parametresi olarak ilgileniyor dilleri gruplarını tanımlamak için.

* `scope=translation` başka bir dilde bir dilden diğerine metne çevirmek için desteklenen diller sağlar;

* `scope=transliteration` bir dilde bir komut dosyasından başka bir komut dosyasına dönüştürmek için yetenekleri sağlar;

* `scope=dictionary` Dil çiftleri sağlayan `Dictionary` işlemleri veri döndürür.

Bir istemci adlarının virgülle ayrılmış bir listesini belirterek birkaç grupları aynı anda alabilir. Örneğin, `scope=translation,transliteration,dictionary` tüm gruplar için desteklenen diller döndürecektir.

Başarılı yanıt, istenen her grup için bir özelliği olan bir JSON nesnesidir:

```json
{
    "translation": {
        //... set of languages supported to translate text (scope=translation)
    },
    "transliteration": {
        //... set of languages supported to convert between scripts (scope=transliteration)
    },
    "dictionary": {
        //... set of languages supported for alternative translations and examples (scope=dictionary)
    }
}
```

Her bir özellik için değer aşağıdaki gibidir.

* `translation` Özelliği

  Değeri `translation` özelliktir (anahtar, değer) sözlüğü çiftleri. Her bir BCP 47 dil etiketi anahtardır. Bir anahtar için metin bulunabilir için çevrilmiş veya gelen çevrilen bir dil tanımlar. Anahtarıyla ilişkilendirilmiş değeri dili açıklayan özelliklere sahip bir JSON nesnesidir:

  * `name`: Aracılığıyla istenen yerel dilde görünen adını `Accept-Language` üstbilgi.

  * `nativeName`: Bu dil için yerel yerel dilde görünen adı.

  * `dir`: Olan yönlülüğü, `rtl` sağdan sola yazılan diller için veya `ltr` soldan sağa diller için.

  Bir örnek verilmiştir:
          
  ```json
  {
    "translation": {
      ...
      "fr": {
        "name": "French",
        "nativeName": "Français",
        "dir": "ltr"
      },
      ...
    }
  }
  ```

* `transliteration` Özelliği

  Değeri `transliteration` özelliktir (anahtar, değer) sözlüğü çiftleri. Her bir BCP 47 dil etiketi anahtardır. Bir anahtar için metin bir komut dosyasından başka bir komut dosyasına dönüştürülebilir bir dil tanımlar. Anahtarıyla ilişkilendirilmiş değeri, dil ve desteklenen betikleri açıklamak özelliklere sahip bir JSON nesnesidir:

  * `name`: Aracılığıyla istenen yerel dilde görünen adını `Accept-Language` üstbilgi.

  * `nativeName`: Bu dil için yerel yerel dilde görünen adı.

  * `scripts`: Dönüştürmek için komut dosyaları listesi. Her öğeye `scripts` listesi özellikleri vardır:

    * `code`: Komut tanımlayıcı kod.

    * `name`: Komut dosyası aracılığıyla istenen yerel görünen adını `Accept-Language` üstbilgi.

    * `nativeName`: Dil için yerel yerel dilde görünen adı.

    * `dir`: Olan yönlülüğü, `rtl` sağdan sola yazılan diller için veya `ltr` soldan sağa diller için.

    * `toScripts`: Metne dönüştürmek kullanılabilir komut dosyaları listesi. Her öğeye `toScripts` listesi olan özellikler `code`, `name`, `nativeName`, ve `dir` daha önce açıklandığı gibi.

  Bir örnek verilmiştir:

  ```json
  {
    "transliteration": {
      ...
      "ja": {
        "name": "Japanese",
        "nativeName": "日本語",
        "scripts": [
          {
            "code": "Jpan",
            "name": "Japanese",
            "nativeName": "日本語",
            "dir": "ltr",
            "toScripts": [
              {
                "code": "Latn",
                "name": "Latin",
                "nativeName": "ラテン語",
                "dir": "ltr"
              }
            ]
          },
          {
            "code": "Latn",
            "name": "Latin",
            "nativeName": "ラテン語",
            "dir": "ltr",
            "toScripts": [
              {
                "code": "Jpan",
                "name": "Japanese",
                "nativeName": "日本語",
                "dir": "ltr"
              }
            ]
          }
        ]
      },
      ...
    }
  }
  ```

* `dictionary` Özelliği

  Değeri `dictionary` özelliktir (anahtar, değer) sözlüğü çiftleri. Her bir BCP 47 dil etiketi anahtardır. Anahtar alternatif Çeviriler ve geri çevirileri kullanılabilir olduğu bir dil tanımlar. Değer kaynak dili ve kullanılabilir çevirileri hedef dillerle açıklayan bir JSON nesnesidir:

  * `name`: Aracılığıyla istenen yerel kaynak dil görünen adını `Accept-Language` üstbilgi.

  * `nativeName`: Bu dil için yerel yerel dilde görünen adı.

  * `dir`: Olan yönlülüğü, `rtl` sağdan sola yazılan diller için veya `ltr` soldan sağa diller için.

  * `translations`: Diğer çevirileri ve kaynak dille ifade edilen bir sorgu için örnekler dillerle listesi. Her öğeye `translations` listesi özellikleri vardır:

    * `name`: Aracılığıyla istenen yerel hedef dil görünen adını `Accept-Language` üstbilgi.

    * `nativeName`: Hedef dil için yerel yerel hedef dil görünen adı.

    * `dir`: Olan yönlülüğü, `rtl` sağdan sola yazılan diller için veya `ltr` soldan sağa diller için.
    
    * `code`: Hedef Dil tanımlama dil kodu.

  Bir örnek verilmiştir:

  ```json
  "es": {
    "name": "Spanish",
    "nativeName": "Español",
    "dir": "ltr",
    "translations": [
      {
        "name": "English",
        "nativeName": "English",
        "dir": "ltr",
        "code": "en"
      }
    ]
  },
  ```

API sürümü bir değişiklik olmadan yanıt nesnesi yapısını değiştirmez. Microsoft Translator sürekli olarak kendi Hizmetleri tarafından desteklenen dillerin listesini genişlettiğinden API aynı sürümü için kullanılabilir dillerin listesini zaman içinde değişebilir.

Desteklenen dillerin listesi sık değiştirmez. Ağ bant genişliğinden tasarruf ve yanıt hızını artırmak için bir istemci uygulaması Türkçe kaynaklar ve karşılık gelen varlık etiketi önbelleğe alma göz önünde bulundurmanız gerekir (`ETag`). Ardından, istemci uygulaması düzenli aralıklarla (örneğin, 24 saatte bir kez) için desteklenen diller son kümesini getirmek için hizmet sorgu. Geçerli geçirme `ETag` değeri bir `If-None-Match` üstbilgi alanı izin ver yanıt en iyi duruma getirme hizmeti. Kaynak değiştirilmemiş, hizmet durum kodu 304 bir boş yanıt gövdesi döndürür.

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>ETag</td>
    <td>Desteklenen diller istenen grupları için varlık etiketi geçerli değeri. İstemcinin sonraki istekleri daha verimli hale getirmek için gönderebilir `ETag` değeri bir `If-None-Match` üstbilgi alanı.
    </td>
  </tr>
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
    <td>304</td>
    <td>İstek üstbilgilerini tarafından belirtilen sürümden itibaren kaynak değiştirilmemiş `If-None-Match`.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerden biri eksik veya geçerli değil. İstek parametreleri denemeden önce düzeltin.</td>
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

Aşağıdaki örnekte, metin çevirisi için desteklenen dilleri alınacak gösterilmiştir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation"
```

---
