---
title: Translator metin çevirisi API'si dilleri yöntemi
titlesuffix: Azure Cognitive Services
description: Translator metin API'si dilleri yöntemi kullanın.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: v-jansko
ms.openlocfilehash: 6e0342d876db424454526637322d67d55c0432a8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65797291"
---
# <a name="translator-text-api-30-languages"></a>Translator metin çevirisi API'si 3.0: Languages

Translator metin çevirisi API'si, diğer işlemler tarafından şu anda desteklenen diller kümesini alır. 

## <a name="request-url"></a>İstek URL'si

Gönderme bir `GET` isteği:
```HTTP
https://api.cognitive.microsofttranslator.com/languages?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek Parametreler şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td><em>Gerekli parametre</em>.<br/>İstemci tarafından istenen API sürümü. Değer olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>kapsam</td>
    <td>*İsteğe bağlı parametre*.<br/>Döndürülecek dil grubunu tanımlama adlarının virgülle ayrılmış listesi. Grup adları izin verilir: `translation`, `transliteration` ve `dictionary`. Kapsam verilen sonra geçişine eşdeğer olduğu tüm grupları döndürülür `scope=translation,transliteration,dictionary`. Desteklenen diller hangi kümesini senaryonuz için uygun olduğuna karar vermek için açıklamasını görmek [yanıt nesnesi](#response-body).</td>
  </tr>
</table> 

İstek üstbilgilerini şunlardır:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Kabul dil</td>
    <td>*İsteğe bağlı bir istek üst bilgisi*.<br/>Kullanıcı arabirimi dizelerinde kullanılacak dil. Yanıt alanları dil adını veya bölge adlarını bazılarıdır. Bu adlar döndürülen dil tanımlamak için bu parametreyi kullanın. Dil, doğru biçimlendirilmiş BCP 47 dil etiketi sağlayarak belirtilir. Örneğin, değerini kullanın `fr` Fransızca adlarında istek veya değeri kullanmak için `zh-Hant` isteği adlarına Geleneksel Çince.<br/>Hedef Dil belirtilmediğinde veya yerelleştirme mevcut olmadığında, adları İngilizce dilinde sağlanır.
    </td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı bir istek üst bilgisi*.<br/>İstek benzersiz olarak tanımlanabilmesi için bir istemci tarafından oluşturulan GUID.</td>
  </tr>
</table> 

Dil kaynakları almak için kimlik doğrulaması gerekli değildir.

## <a name="response-body"></a>Yanıt gövdesi

Bir istemcinin kullandığı `scope` sorgu parametresi, ilgileniyor dilleri gruplarını tanımlamak için.

* `scope=translation` bir dilden metni başka bir dile çevirmek için desteklenen diller sağlar.

* `scope=transliteration` bir dilde bir komut dosyasından başka bir komut dosyasına dönüştürmek için özellikleri sağlar.

* `scope=dictionary` Dil çiftleri sağlayan `Dictionary` operations dönüş verileri.

Bir istemci birden fazla grup adlarının virgülle ayrılmış bir listesini belirterek aynı anda alabilir. Örneğin, `scope=translation,transliteration,dictionary` tüm gruplar için desteklenen diller döndürür.

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

Her bir özellik değeri aşağıdaki gibidir.

* `translation` Özelliği

  Değerini `translation` özelliktir (anahtar, değer) sözlüğü çiftleri. Her bir BCP 47 dil etiketi anahtardır. Bir anahtar için metin için çevrilmiş veya yapabilirsiniz den çevrilen bir dil tanımlar. Anahtarıyla ilişkilendirilmiş değeri, dil tanımlayan özelliklere sahip bir JSON nesnesidir:

  * `name`: Görünen adı aracılığıyla istenen yerel dilde `Accept-Language` başlığı.

  * `nativeName`: Bu dil için dil yerel ayardaki görünen adı.

  * `dir`: Olan yönlülüğü `rtl` sağdan sola diller için veya `ltr` sağdan sola diller için.

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

  Değerini `transliteration` özelliktir (anahtar, değer) sözlüğü çiftleri. Her bir BCP 47 dil etiketi anahtardır. Bir anahtar için metin bir komut dosyasından başka bir betiğe dönüştürülebilir bir dil tanımlar. Anahtarıyla ilişkilendirilmiş değeri, dil ve desteklenen, komut dosyaları tanımlayan özelliklere sahip bir JSON nesnesidir:

  * `name`: Görünen adı aracılığıyla istenen yerel dilde `Accept-Language` başlığı.

  * `nativeName`: Bu dil için dil yerel ayardaki görünen adı.

  * `scripts`: Dönüştürmek için komut listesi. Her öğeyi `scripts` listesi özelliklere sahiptir:

    * `code`: Komut tanımlayıcı kod.

    * `name`: Komut dosyası aracılığıyla istenen yerel ayarında görünen adını `Accept-Language` başlığı.

    * `nativeName`: Dil adını dilini yerel ayarında görüntüler.

    * `dir`: Olan yönlülüğü `rtl` sağdan sola diller için veya `ltr` sağdan sola diller için.

    * `toScripts`: Metne dönüştürmek kullanılabilir komut listesi. Her öğeyi `toScripts` listesi olan özellikleri `code`, `name`, `nativeName`, ve `dir` daha önce açıklandığı gibi.

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

  Değerini `dictionary` özelliktir (anahtar, değer) sözlüğü çiftleri. Her bir BCP 47 dil etiketi anahtardır. Anahtar alternatif çevirileri ve arka çevirileri kullanılabilir olduğu bir dil tanımlar. Kaynak dili ve kullanılabilir çevirileri hedef dillerle açıklayan bir JSON nesnesi değerdir:

  * `name`: Kaynak dili aracılığıyla istenen yerel ayarında görünen adını `Accept-Language` başlığı.

  * `nativeName`: Bu dil için dil yerel ayardaki görünen adı.

  * `dir`: Olan yönlülüğü `rtl` sağdan sola diller için veya `ltr` sağdan sola diller için.

  * `translations`: Diğer çevirileri ve örnekler için kaynak dilinde ifade sorgu dillerin listesi. Her öğeyi `translations` listesi özelliklere sahiptir:

    * `name`: Görünen adı aracılığıyla istenen yerel hedef dilde `Accept-Language` başlığı.

    * `nativeName`: Hedef dil adı, hedef dil için yerel ayarında görüntüler.

    * `dir`: Olan yönlülüğü `rtl` sağdan sola diller için veya `ltr` sağdan sola diller için.
    
    * `code`: Hedef Dil tanımlayan dil kodu.

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

Yanıt nesnesini yapısını API sürümünde bir değişiklik yapmadan değiştirmez. Microsoft Translator, hizmetleri tarafından desteklenen dillerin listesi sürekli olarak genişlettiğinden için aynı API sürümü, kullanılabilir diller listesini zaman içinde değişebilir.

Desteklenen dillerin listesini sık değiştirmez. Ağ bant genişliğinden tasarruf ve yanıt hızını artırmak için bir istemci uygulaması dil kaynakları ve ilgili varlık etiketi önbelleğe alma özelliğini dikkate almanız gerekir (`ETag`). Ardından, istemci uygulaması düzenli aralıklarla (örneğin, 24 saatte bir kez) için desteklenen diller son kümesini getirmek için hizmetini sorgulama. Geçerli geçirme `ETag` değerini bir `If-None-Match` üstbilgi alanı yanıt en iyi duruma getirme hizmeti sağlayacaktır. Kaynak değiştirilmiş değil, hizmet 304 durum kodu ve boş yanıt gövdesi döndürür.

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>ETag</td>
    <td>Varlık etiketi istenen gruplarını desteklenen diller için geçerli değeri. İstemcinin sonraki istekleri daha verimli hale getirmek için gönderebilir `ETag` değerini bir `If-None-Match` üstbilgi alanı.
    </td>
  </tr>
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
    <td>304</td>
    <td>Kaynak istek üst bilgileri tarafından belirtilen sürümden bu yana değiştirilmedi `If-None-Match`.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerden biri eksik veya geçerli değil. İstek parametreleri yeniden denemeden önce düzeltin.</td>
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
curl "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation"
```

---
