---
title: Özel nitelik (Azure Search) bilişsel arama ardışık düzeninde tanımı arabirim | Microsoft Docs
description: Azure Search'te bilişsel arama düzenindeki web API özel yetenek için özel veri ayıklama arabirimi.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 8f21a56982189aa13745f27f0fae49310ae55aa0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640328"
---
# <a name="how-to-add-a-custom-skill-to-a-cognitive-search-pipeline"></a>Bilişsel arama ardışık düzene özel bir yetenek ekleme

Bu makalede, özel bir yetenek bilişsel arama ardışık düzene eklemeyi öğrenin. A [bilişsel arama dizini oluşturma ardışık düzeni](cognitive-search-concept-intro.md) Azure Search'te gelen birleştirilebilecek [becerileri önceden tanımlanmış](cognitive-search-predefined-skills.md) ve kişisel oluşturmak ve ardışık düzene ekleyen özel yetenekleri.

Özel bir yetenek oluşturma dönüşümleri içeriğinize benzersiz eklemek için bir yol sağlar. Özel bir yetenek ihtiyaç duyduğunuz ne olursa olsun iyileştirmesini adımı uygulamadan bağımsız olarak yürütür. Örneğin, alana özgü özel varlıklarını tanımlama, iş ve finansal sözleşmeleri ve belgeleri ayırt etmek için özel sınıflandırma modelleri oluşturabilir veya ilgili içerik için ses dosyalarına daha derin ulaşmak için bir konuşma tanıma yetenek ekleyin. Adım adım bir örnek için bkz: [örnek: özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md).

 Hangi özel yetenek, gereksinim duyduğunuz iyileştirmesini ardışık düzen geri kalanı için özel bir yetenek bağlamak için basit ve açık bir arabirim yok. Tek gereksinim eklenmek üzere bir [skillset](cognitive-search-defining-skillset.md) girişleri kabul edin ve bir bütün olarak skillset içinde tüketilebilir yolları çıktılarında yayma yeteneğidir. Bu makaleyi odağını iyileştirmesini ardışık düzen gerektiren giriş ve çıkış biçimleri ' dir.

## <a name="web-api-custom-skill-interface"></a>Web API özel nitelik arabirimi

Şu anda, özel bir yetenek ile etkileşim için yalnızca bir Web API arabirimi üzerinden mekanizmadır. Web API ihtiyaçlarını Bu bölümde açıklanan gereksinimleri karşılaması gerekir.

### <a name="1--web-api-input-format"></a>1.  Web API giriş biçimi

Web API kayıtları işlenmek üzere bir dizi kabul etmeniz gerekir. Her kayıt "özellik paketi" içermesi gereken diğer bir deyişle, Web API için sağlanan girdi. 

Bir sözleşme metinde belirtilen ilk tarihi tanımlayan basit bir enricher oluşturmak istediğinizi varsayalım. Bu örnekte, tek bir giriş yetenek kabul *contractText* sözleşme metin olarak. Yetenek sözleşmenin tarihi olan tek bir çıktı da sahiptir. Enricher daha ilginç hale getirmek için bu iade *contractDate* çok parçalı bir karmaşık tür şeklinde.

Web API giriş kayıtları toplu almaya hazır olması gerekir. Her bir üyesi *değerleri* dizisini belirli bir kayıt için giriş temsil eder. Her bir kaydı aşağıdaki öğeleri olması gerekir:

+ A *RecordID* belirli bir kaydı için benzersiz tanıtıcıdır üye. Sonuçları, enricher geri döndüğünde, bu sağlamalısınız *RecordID* kendi giriş kayıt sonuçları eşleşecek şekilde çağıran izin vermek üzere.

+ A *veri* temelde her kayıt için girdi alanlarının bir paketi olan üye,.

Daha fazla olması, yukarıdaki örnek başına somut Web API şuna istekleri beklemesi gerekir:

```json
{
    "values": [
      {
        "recordId": "a1",
        "data":
           {
             "contractText": 
                "This is a contract that was issues on November 3, 2017 and that involves... "
           }
      },
      {
        "recordId": "b5",
        "data":
           {
             "contractText": 
                "In the City of Seattle, WA on February 5, 2018 there was a decision made..."
           }
      },
      {
        "recordId": "c3",
        "data":
           {
             "contractText": null
           }
      }
    ]
}
```
Gerçekte, yüzlerce veya binlerce kayıt yalnızca aşağıda gösterilen üç yerine Hizmetinizi çağrılmadığı.

### <a name="2-web-api-output-format"></a>2. Web API çıktı biçimi

Çıktı biçimi içeren bir kayıt kümesinde olan bir *RecordID*ve bir özellik paketi 

```json
{
  "values": 
  [
      {
        "recordId": "b5",
        "data" : 
        {
            "contractDate":  { "day" : 5, "month": 2, "year" : 2018 }
        }
      },
      {
        "recordId": "a1",
        "data" : {
            "contractDate": { "day" : 3, "month": 11, "year" : 2017 }                    
        }
      },
      {
        "recordId": "c3",
        "data" : 
        {
        },
        "errors": [ { "message": "contractText field required "}   ],  
        "warnings": [ {"message": "Date not found" }  ]
      }
    ]
}
```

Söz konusu örnekte için yalnızca bir çıktı olsa da, birden fazla özellik çıktı. 

### <a name="errors-and-warning"></a>Hata ve uyarı

Önceki örnekte gösterildiği gibi hata ve uyarı iletileri her kayıt için döndürebilir.

## <a name="consuming-custom-skills-from-skillset"></a>Skillset gelen özel becerileri kullanma

Bir Web API enricher oluşturduğunuzda, isteğin bir parçası HTTP üstbilgilerine ve parametreleri tanımlayabilirsiniz. Aşağıdaki kod parçacığında nasıl İstek parametreleri ve HTTP üstbilgileri skillset tanımının bir parçası tanımlanmış gösterir.

```json
{
    "skills": [
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill calls an Azure function, which in turn calls TA sentiment",
        "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/DateExtractor?language=en",
        "context": "/document",
        "httpHeaders": {
            "DateExtractor-Api-Key": "foo"
        },
        "inputs": [
          {
            "name": "contractText",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "contractDate",
            "targetName": "date"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

+ [Örnek: özel bir yetenek Çevir metin API'si oluşturma](cognitive-search-create-custom-skill-example.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
+ [Skillset (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
