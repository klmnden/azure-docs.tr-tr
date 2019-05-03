---
title: Özel bilişsel arama beceri - Azure Search
description: Web API'lerini çağırarak bilişsel arama becerilerini yeteneklerini genişletir
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seojan2018
ms.openlocfilehash: e5f7ee172563a81d45e3a35da2cfc7e8731de48d
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65023849"
---
# <a name="custom-web-api-skill"></a>Özel Web API'si beceri

**Özel Web API'si** beceri özel işlemler sağlayan bir Web API uç noktası çağırarak bilişsel arama genişletmenizi sağlar. Yerleşik yetenekler, benzer bir **özel Web API'si** yeteneğe sahip giriş ve çıkışları. Dizin Oluşturucu çalıştırıldığında girişleri bağlı olarak, Web API'niz bir JSON yükü alır ve bir başarı durum kodu ile birlikte bir yanıt olarak bir JSON yükü çıkarır. Yanıt, özel becerilerinizi tarafından belirtilen çıkışınızın beklenir. Başka bir yanıtı bir hata olarak kabul edilir ve hiçbir zenginleştirmelerinin gerçekleştirilir.

JSON yükü yapısını daha da aşağı bu belgede açıklanan.

> [!NOTE]
> Dizin Oluşturucu, Web API öğesinden geri döndürülen iki kez bazı standart HTTP durum kodları için yeniden deneyecek. Bu HTTP durum kodları şunlardır: 
> * `503 Service Unavailable`
> * `429 Too Many Requests`

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Custom.WebApiSkill

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| uri | Web API'sine URI'sini _JSON_ yükü gönderilir. Yalnızca **https** URI şeması izin verilir |
| HttpMethod | Yükü gönderilirken kullanılacak yöntem. İzin verilen yöntemler `PUT` veya `POST` |
| httpHeaders | Burada anahtarları üst bilgi adları ve değerleri temsil eden anahtar-değer çiftleri koleksiyonu yükün yanı sıra Web apı'nize gönderilen üstbilgi değerlerini temsil eder. Bu koleksiyonda yüklenmesini şu Yasak: `Accept`, `Accept-Charset`, `Accept-Encoding`, `Content-Length`, `Content-Type`, `Cookie`, `Host`, `TE`, `Upgrade`, `Via` |
| timeout | (İsteğe bağlı) Belirtilen zaman aşımı için http İstemcisi API çağrısı yapma gösterir. Bir XSD "dayTimeDuration" değeri biçimlendirilmelidir (sınırlı bir alt kümesini bir [ISO 8601 süre](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değeri). Örneğin, `PT60S` 60 saniye. Aksi durumda, küme, varsayılan değer 30 saniye olarak seçilir. Zaman aşımı, 90 saniyelik en fazla ve en az 1 saniye için ayarlanabilir. |
| batchSize | (İsteğe bağlı) "Veri kayıtları" kaç gösterir (bkz _JSON_ yükü yapısı aşağıdaki) gönderilecek API çağrısı başına. Aksi durumda kümesi, varsayılan 1000 olarak seçilir. Yapmanızı öneririz dizinleme aktarım hızı ve API üzerindeki yük arasında uygun bir denge sağlamak için bu parametreyi kullanın |

## <a name="skill-inputs"></a>Beceri girişleri

Bu yetenek için "önceden tanımlanmış" giriş vardır. Zaten girdi olarak bu beceri kullanıcının yürütme zamanında kullanılabilir olması bir veya daha fazla alan seçebilirsiniz ve _JSON_ Web API'ye gönderilen yükü farklı alanlara sahip olur.

## <a name="skill-outputs"></a>Beceri çıkışları

"Önceden tanımlanmış" çıkış için bu yeteneği vardır. Web API'nizi döndürür yanıt bağlı olarak, çıktı alanlarını ekleyebilirsiniz, böylece bunlar gelen çekilebilir _JSON_ yanıt.


## <a name="sample-definition"></a>Örnek tanımı

```json
  {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "A custom skill that can count the number of words or characters or lines in text",
        "uri": "https://contoso.count-things.com",
        "batchSize": 4,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/languageCode"
          },
          {
            "name": "countOf",
            "source": "/document/propertyToCount"
          }
        ],
        "outputs": [
          {
            "name": "count",
            "targetName": "countOfThings"
          }
        ]
      }
```
## <a name="sample-input-json-structure"></a>Örnek giriş JSON yapısı

Bu _JSON_ yapısı Web API'nize gönderilen yükünü temsil eder.
Ayrıca, bu kısıtlamaları her zaman izler:

* Üst düzey varlığı olarak adlandırılan `values` ve nesnelerin bir dizi olacaktır. Bu tür nesnelerin sayısı en fazla olur `batchSize`
* Her bir nesnenin `values` dizi olacaktır
    * A `recordId` olan özellik bir **benzersiz** kaydın tanımlamak için kullanılan dize.
    * A `data` olan özellik bir _JSON_ nesne. Alanlarını `data` özelliği "belirtilen adlarına" karşılık gelen `inputs` beceri tanımının bölümü. Bu alanların değer arasında `source` bu alanların (Bu belgede bir alan veya potansiyel olarak başka bir beceri olabilir)

```json
{
    "values": [
      {
        "recordId": "0",
        "data":
           {
             "text": "Este es un contrato en Inglés",
             "language": "es",
             "countOf": "words"
           }
      },
      {
        "recordId": "1",
        "data":
           {
             "text": "Hello world",
             "language": "en",
             "countOf": "characters"
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Hello world \r\n Hi World",
             "language": "en",
             "countOf": "lines"
           }
      },
      {
        "recordId": "3",
        "data":
           {
             "text": "Test",
             "language": "es",
             "countOf": null
           }
      }
    ]
}
```

## <a name="sample-output-json-structure"></a>Örnek çıktı JSON yapısı

"Çıktı" Web API'nizi döndürülen yanıtı karşılık gelir. Web API'sini yalnızca döndürmelidir bir _JSON_ Yükü (bakarak doğrulandı `Content-Type` yanıt üst bilgisi) ve aşağıdaki kısıtlamalar karşılaması gerekir:

* Adlı en üst düzey bir varlık olmalıdır `values` nesneleri içeren bir dizi olmalıdır.
* Dizideki nesne sayısını Web API'ye gönderilen nesne sayısı ile aynı olması gerekir.
* Her bir nesnenin sahip olmanız gerekir:
   * A `recordId` özelliği
   * A `data` alanların zenginleştirmelerinin olduğu bir nesne özelliğinin "adı" içinde eşleşen `output` ve değeri zenginleştirme olarak kabul edilir.
   * Bir `errors` özelliği, dizin oluşturucusu yürütme geçmişine eklenecek karşılaşılan hataları listeleyen bir dizi. Bu özellik gereklidir, ancak olabilir bir `null` değeri.
   * A `warnings` özelliği, dizin oluşturucusu yürütme geçmişine eklenecek karşılaştı uyarılarını listeleyen bir dizi. Bu özellik gereklidir, ancak olabilir bir `null` değeri.
* Nesneleri `values` dizi olması gerekmez nesneleri aynı sırada `values` isteği olarak Web API'ye gönderilen dizisi. Ancak, `recordId` bağıntı için herhangi bir yanıtı içeren kayıt için kullanılan bir `recordId` özgün istek Web API'sine bir parçası değildi atılacak.

```json
{
    "values": [
        {
            "recordId": "3",
            "data": {
            },
            "errors": [
              {
                "message" : "Cannot understand what needs to be counted"
              }
            ],
            "warnings": null
        },
        {
            "recordId": "2",
            "data": {
                "count": 2
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "0",
            "data": {
                "count": 6
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "1",
            "data": {
                "count": 11
            },
            "errors": null,
            "warnings": null
        },
    ]
}

```

## <a name="error-cases"></a>Hata durumları
Web API'nizi kullanılamıyor veya gönderme başarılı olmayan durum kodları dışarı aktarılan ek olarak aşağıdaki hatalı durumda olarak kabul edilir:

* Web API, bir başarı durum kodu döndürür, ancak yanıtını etkinleştirilmediğini gösteren `application/json` yanıt geçersiz olarak kabul edilir ve hiçbir zenginleştirmelerinin gerçekleştirilir.
* Varsa **geçersiz** (ile `recordId` özgün istek içinde değil veya yinelenen değerler) kayıtları yanıt `values` dizi, hiçbir zenginleştirme gerçekleştirilecek için **bu** kaydeder.

Web API kullanılamıyor ya da bir HTTP hatası döndürür durumlar için kolay bir hata HTTP hatası kullanılabilir tüm ayrıntılarını içeren dizin oluşturucusu yürütme geçmişine eklenir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Bilişsel arama için özel Yetenek Ekle](cognitive-search-custom-skill-interface.md)
+ [Metni Çevir API'sini kullanarak özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)