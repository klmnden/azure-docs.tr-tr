---
title: Özel becerileri bilişsel arama - Azure Search için arabirim tanımı
description: Azure Search'te bilişsel arama işlem hattında özel yetenek web API'si için özel veri ayıklama arabirimi.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 1bf42e5f418f99f5e5327d790c1adffe2357b84e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65021929"
---
# <a name="how-to-add-a-custom-skill-to-a-cognitive-search-pipeline"></a>Bilişsel arama işlem hattı için özel bir yetenek ekleme

A [bilişsel arama dizini oluşturma ardışık düzeni](cognitive-search-concept-intro.md) gelen Azure Search'te birleştirilebilecek [önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) yanı [özel becerileri](cognitive-search-custom-skill-web-api.md) kişisel olarak oluşturduğunuz ve ekleyin işlem hattı. Bu makalede, bilişsel arama hattında eklenmesine izin veren bir arabirimi kullanıma sunan özel bir yetenek oluşturmayı öğrenin. 

Özel bir yetenek oluşturma dönüşümleri içeriğinize benzersiz eklemek için bir yol sağlar. Özel bir yetenek bağımsız olarak, ihtiyaç duyduğunuz her zenginleştirme adım uygulayarak yürütür. Örneğin, alana özgü özel varlıklar tanımlayan, işletme ve Finans sözleşmeleri ve belgeleri ayırt etmek için özel sınıflandırma modellerini Derleme veya ses dosyalarıyla ilgili içerik için daha ayrıntılı ulaşmak için bir konuşma tanıma becerisi ekleyin. Adım adım bir örnek için bkz: [örnek: özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md).

 Gerektiren özel bir yetenek zenginleştirme işlem hattının rest'e bağlamak için basit ve açık bir arabirim herhangi özel bir yetenek. Eklenmesi için tek gereksinim bir [beceri kümesi](cognitive-search-defining-skillset.md) girişleri kabul edin ve bir bütün olarak becerilerine içinde tüketilebilir şekillerde çıktıları yeteneğidir. Bu makalenin odak zenginleştirme işlem hattı gerektiren giriş ve çıkış biçimleri ' dir.

## <a name="web-api-custom-skill-interface"></a>Web API özel bir yetenek arabirimi

Özel Webapı beceri uç noktaları tarafından 30 ve ikinci bir pencere içinde yanıt döndürmeyin, varsayılan zaman aşımı. Dizinleme işlem hattına uyumludur ve bu pencerede bir yanıt alınmazsa, dizin oluşturma zaman aşımı hatasına neden olur.  Zaman aşımı parametresi ayarlanarak en fazla 90 saniye olarak zaman aşımı yapılandırmak mümkündür:

```json
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill has a 90 second timeout",
        "uri": "https://[your custom skill uri goes here]",
        "timeout": "PT90S",
```

Şu anda, özel bir yetenek ile etkileşim kurmak için yalnızca bir Web API arabirimi üzerinden mekanizmadır. Web API'si, bu bölümde açıklanan gereksinimleri ihtiyaçlarını karşılaması gerekir.

### <a name="1--web-api-input-format"></a>1.  Web API giriş biçimi

Web API'si, işlenmek üzere kayıtları dizisi kabul etmeniz gerekir. Her bir kaydın "özellik paketi" içermelidir. diğer bir deyişle Web API'niz için sağlanan giriş. 

Bir sözleşme metninin içinde belirtilen ilk tarihi tanımlayan basit bir enricher oluşturmak istediğinizi varsayalım. Bu örnekte, tek bir giriş yetenek kabul *contractText* sözleşme metin olarak. Yetenek tarihi sözleşmenin tek bir çıktı da vardır. Enricher daha ilginç hale getirmek için bu iade *contractDate* şeklinde çok parçalı bir karmaşık tür.

Web API'nizi giriş kayıtları toplu almaya hazır olmalıdır. Her üye *değerleri* dizi belirli bir kayıt girdisini temsil eder. Her kayıt, aşağıdaki öğelere sahip olması gereklidir:

+ A *RecordID* belirli bir kaydı için benzersiz tanımlayıcı üyesi. Sonuçlar, enricher geri döndüğünde, bu sağlamalısınız *RecordID* kendi giriş kaydı sonuçları eşleştirmek çağıranın izin vermek üzere.

+ A *veri* temelde her kayıt için giriş alanlarının bir paketi olan üye.

Daha fazla olacak şekilde somut, yukarıdaki örnekte, her Web API'nizi şuna istekleri beklemelisiniz:

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
Gerçekte, yüzlerce veya binlerce kayıt yerine yalnızca aşağıda gösterilen üç hizmetiniz çağrılmadığı.

### <a name="2-web-api-output-format"></a>2. Web API çıkış biçimi

Çıkış biçimi içeren bir kayıt kümesinde olan bir *RecordID*ve bir özellik paketi 

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

Söz konusu örnekte için yalnızca bir çıkış olsa da, birden fazla özelliğe çıkış. 

### <a name="errors-and-warning"></a>Hata ve uyarı

Önceki örnekte gösterildiği gibi hata ve uyarı iletilerini her kayıt için döndürebilir.

## <a name="consuming-custom-skills-from-skillset"></a>Beceri kümesi öğesinden özel becerileri kullanma

Bir Web API'sini enricher oluşturduğunuzda, isteğin bir parçası HTTP üst bilgileri ve parametrelerini açıklayan. Aşağıdaki kod parçacığında nasıl İstek parametreleri ve HTTP üstbilgileri beceri kümesi tanımının bir parçası tanımlanmış gösterir.

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

+ [Örnek: Özel bir yetenek Çevir metin API'si oluşturma](cognitive-search-create-custom-skill-example.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
