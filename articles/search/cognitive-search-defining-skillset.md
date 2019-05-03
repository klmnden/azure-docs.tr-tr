---
title: Bilişsel arama hattında - Azure Search bir beceri kümesi oluşturma
description: Veri ayıklama, doğal dil işleme, tanımlama veya görüntü analizi adımları zenginleştiren ve yapılandırılmış bilgiler verileriniz için ayıklamak için Azure arama'yı kullanın.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 9eedf0be6089764c8111ae81d558f7e65af0a66d
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021780"
---
# <a name="how-to-create-a-skillset-in-an-enrichment-pipeline"></a>Bir zenginleştirme işlem hattı, bir beceri kümesi oluşturma

Bilişsel arama ayıklar ve Azure Search aranabilir hale getirmek için veri zenginleştirir. Ayıklama ve zenginleştirme adımları diyoruz *bilişsel beceriler*, içine birleşik bir *beceri kümesi* dizin oluşturma sırasında başvurulan. Bir beceri kümesi kullanabilirsiniz [yerleşik yetenekler](cognitive-search-predefined-skills.md) veya özel becerileri (bkz [örnek: özel bir yetenek oluşturmak](cognitive-search-create-custom-skill-example.md) daha fazla bilgi için).

Bu makalede, kullanmak istediğiniz yetenekler için bir zenginleştirme işlem hattı oluşturma konusunda bilgi edinin. Bir beceri kümesi için bir Azure Search bağlı [dizin oluşturucu](search-indexer-overview.md). Bu makalede ele alınan işlem hattı tasarımının bir parçası becerilerine kendisini oluşturmak. 

> [!NOTE]
> Başka bir işlem hattı tasarım parçası ele bir dizin oluşturucu belirten [sonraki adıma](#next-step). Bir dizin oluşturucu tanımı beceri kümesi yanı sıra alan eşlemelerini hedef dizinde çıktılarına girişleri bağlamak için kullanılan bir başvuru içerir.

Dikkat edilmesi gereken önemli noktalar:

+ Yalnızca, dizin oluşturucu başına bir beceri kümesi olabilir.
+ Bir beceri kümesi en az bir yetenek olmalıdır.
+ (Örneğin, bir görüntü analizi beceri çeşitleri) aynı türde birden fazla becerileri oluşturabilirsiniz.

## <a name="begin-with-the-end-in-mind"></a>Son aklınızda başlayın

Önerilen bir ilk adım, ham verilerinizi ve bu verileri bir arama çözümü kullanmak istediğiniz nasıl ayıklamak için hangi veri karar vermektir. İşlem hattının tamamını zenginleştirme çizim oluşturma, gerekli adımları belirlemenize yardımcı olabilir.

Finansal analist yorumları kümesi işlenirken ilgilendiğiniz varsayalım. Şirket adları ve açıklamaları genel yaklaşım ayıklamak istediğiniz her dosya için. Ne tür bir şirket olarak görevlendirildikten iş gibi bir şirket hakkında ek bilgi için Bing varlık arama hizmetini kullanan özel bir enricher yazmak isteyebilirsiniz. Aslında, aşağıdaki gibi bilgileri ayıklamak istediğiniz her belge için dizini:

| kaydı-metin | Şirketler | yaklaşım | Şirket açıklamaları |
|--------|-----|-----|-----|
|Örnek-record| ["Microsoft", "LinkedIn"] | 0.99 | ["Microsoft Corporation'ın bir Amerikan çok uluslu teknoloji şirketi...", "LinkedIn bir iş ve çalışma-odaklı sosyal ağ..."]

Aşağıdaki diyagramda bir kuramsal zenginleştirme işlem hattı gösterilmektedir:

![Bir kuramsal zenginleştirme işlem hattı](media/cognitive-search-defining-skillset/sample-skillset.png "kuramsal zenginleştirme işlem hattı")


İstediğiniz işlem hattında adımları sağlayan becerilerine ifade edebilirsiniz adil fikir olduğunda. Azure Search, dizin oluşturucu tanımı karşıya yüklediğinizde bu işlev, becerilerine ifade edilir. Dizin karşıya yükleme hakkında daha fazla bilgi için bkz. [dizin oluşturucu belgeleri](https://docs.microsoft.com/rest/api/searchservice/create-indexer).


Diyagramdaki *belge çözme* adımı otomatik olarak gerçekleşir. Esas olarak, Azure Search iyi bilinen dosyaların nasıl açılacağını bilir ve oluşturan bir *içeriği* her belge ayıklanan metinleri içeren alan. Yerleşik enrichers beyaz kutularıdır ve noktalı "Bing varlık arama" kutusuna oluşturmakta olduğunuz özel bir enricher temsil eder. Gösterildiği gibi üç becerileri beceri kümesi içerir.

## <a name="skillset-definition-in-rest"></a>Beceri kümesi tanımında REST

Bir beceri kümesi becerileri bir dizi olarak tanımlanır. Her yetenek girdilerinden kaynağını ve üretilen çıkış adını tanımlar. Kullanarak [beceri kümesi REST API'si oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset), önceki diyagrama karşılık gelen bir beceri kümesi tanımlayabilirsiniz: 

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

```json
{
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "Calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar"
      },
      "context": "/document/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
  ]
}
```

## <a name="create-a-skillset"></a>Beceri kümesi oluşturma

Bir beceri kümesi oluştururken, kendi kendine beceri kümesi tanım yapmak için bir açıklama girebilirsiniz. Bir açıklama isteğe bağlıdır, ancak bir beceri kümesi ne yaptığını izlemek için kullanışlıdır. Beceri kümesi açıklamalar izin vermez, bir JSON belgesi olduğundan kullanmalısınız bir `description` bu öğe.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

Sonraki parçada becerilerine becerileri dizisidir. Her beceri zenginleştirme, basit bir tür düşünebilirsiniz. Her yetenek bu zenginleştirme işlem hattı, küçük bir görev gerçekleştirir. Her bir giriş (ya da bir dizi bir giriş) alır ve bazı çıktıları döndürür. Sonraki birkaç bölümlerde becerileri girdi ve çıktı başvuruları birbirine zincirleme yerleşik ve özel becerileri belirlemek nasıl odaklanır. Giriş veri kaynağı veya başka bir beceri gelebilir. Çıkış bir arama dizini bir alana eşlenmiş veya bir aşağı akış becerisi girdi olarak kullanılır.

## <a name="add-built-in-skills"></a>Yerleşik yetenekler Ekle

Yerleşik olan ilk beceri göz atalım [varlık tanıma beceri](cognitive-search-skill-entity-recognition.md):

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    }
```

* Her bir yerleşik yetenek sahip `odata.type`, `input`, ve `output` özellikleri. Beceri özgü özellikler yetenek için ilgili ek bilgiler sağlar. Varlık tanıma, `categories` sabit pretrained modeli tanıyabilmesi varlık türleri kümesi arasında bir varlıktır.

* Her yetenek olmalıdır bir ```"context"```. Bağlamı operations yer almakta düzeyi temsil eder. Yukarıdaki yetenek varlık tanıma beceri belge başına bir kez çağrılır, yani tüm belgeyi bağlamıdır. Çıktı, o seviyede da oluşturulur. Daha açık belirtmek gerekirse ```"organizations"``` üyesi olarak oluşturulan ```"/document"```. Bu yeni bilgi olarak oluşturulan başvurabilir aşağı akış yeteneklerinizi ```"/document/organizations"```.  Varsa ```"context"``` alan açıkça ayarlanmamışsa, varsayılan bağlamı belgesidir.

* "Metin", bir kaynak giriş kümesiyle adlı bir giriş yeteneğe sahip ```"/document/content"```. Beceri (varlık tanıma) çalıştığını *içeriği* standart bir alandır her belge alanının Azure blob dizin oluşturucu tarafından oluşturulmuş. 

* Adlı bir çıktı yeteneğe sahip ```"organizations"```. Çıkış işleme sırasında mevcut. Çıktı olarak bu çıkışı bir aşağı akış becerisi 's giriş zincir başvurusu ```"/document/organizations"```.

* Değerini belirli bir belge için ```"/document/organizations"``` metin ayıklandı kuruluşların dizisidir. Örneğin:

  ```json
  ["Microsoft", "LinkedIn"]
  ```

Bazı durumlarda, bir dizideki her öğe ayrı olarak başvurmak için çağırın. Örneğin, her öğeye geçirmek istediğiniz varsayalım ```"/document/organizations"``` (örneğin, özel Bing varlık arama enricher) başka bir yetenek için ayrı olarak. Yol için bir yıldız işareti ekleyerek dizinin her öğesine başvurabilir: ```"/document/organizations/*"``` 

İkinci yetenek yaklaşım ayıklama için ilk enricher olarak aynı deseni izler. Sürdüğünü ```"/document/content"``` giriş ve bir yaklaşım puanını içerik her örneği için döndürür. Ayarlanmamış olduğundan ```"context"``` açıkça alan, ' % s'çıkış (mySentiment) artık bir alt öğesidir ```"/document"```.

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
```

## <a name="add-a-custom-skill"></a>Özel bir yetenek Ekle

Özel Bing varlık arama enricher yapısını Hatırla:

```json
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "This skill calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar"
      }
      "context": "/document/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
```

Bu tanımı bir [özel bir yetenek](cognitive-search-custom-skill-web-api.md) zenginleştirme işleminin bir parçası bir web API'sini çağırır. Bu yetenek, varlık tanıma tarafından tanımlanan her kuruluş için bir web API'si, kuruluş açıklamasını bulmak için çağırır. Düzenlenmesi ne zaman web API'sini çağırın ve alınan bilgi akışını nasıl zenginleştirme motoru tarafından dahili olarak işlenir. Ancak, bu özel API'yi çağırmak için gerekli başlatma (örneğin, URI, httpHeaders ve beklenen girişleri) JSON biçiminde sağlanmalıdır. Zenginleştirme işlem hattı için özel web API'si oluşturma yönergeleri için bkz [özel arabirim tanımlama](cognitive-search-custom-skill-interface.md).

"Bağlam" alanı ayarlandığına dikkat edin ```"/document/organizations/*"``` yıldız işaretiyle zenginleştirme adım anlamı çağrılır *her* kuruluş altında ```"/document/organizations"```. 

Çıktı, bu durumda şirket açıklaması, oluşturulan tanımlanan her kuruluş için. Bir aşağı akış adımda (örneğin, anahtar ifade ayıklama) bir açıklama söz konusu olduğunda, yol kullanacağınız ```"/document/organizations/*/description"``` Bunu yapmak için. 

## <a name="add-structure"></a>Yapı ekleme

Yapılandırılmamış verileri dışında yapılandırılmış bilgiler beceri kümesi oluşturur. Aşağıdaki örnek göz önünde bulundurun:

*"Dördüncü üç ay içinde Microsoft $1.1 milyar geliri LinkedIn, geçen yıl satın sosyal ağ şirket bir günlüğe kaydedilir. Alım LinkedIn yetenekleri, CRM ve Office Özellikleri birleştirmek Microsoft sağlar. Stockholders şimdiye ilerleme durumunu heyecan duyuyoruz."*

Büyük olasılıkla bir sonucu oluşturulan yapı aşağıdaki çizime benzer olacaktır:

![Örnek çıktı yapısını](media/cognitive-search-defining-skillset/enriched-doc.png "örnek çıktı yapısı")

Şimdiye kadar bu yapı, yalnızca dahili, yalnızca bellek ve kullanılan yalnızca Azure Search dizinlerini olmuştur. Bir Bilgi Bankası deposunun eklenmesi, arama dışında kullanmak biçimlendirilmiş zenginleştirmelerinin kaydetmek için bir yol sunar.

## <a name="add-a-knowledge-store"></a>Bilgi Bankası deposu ekleme

[Bilgi Bankası Store](knowledge-store-concept-intro.md) zenginleştirilmiş belgenizi kaydetmek için Azure Search'te bir önizleme özelliğidir. Oluşturduğunuz bir Bilgi Bankası store bir Azure depolama hesabı tarafından desteklenen zenginleştirilmiş verilerinizi burada gölünüzdeki depodur. 

Bilgi deposunu tanımı için bir beceri kümesi eklenir. Bir işlem kılavuzu için bkz. [bilgi store ile çalışmaya başlama konusunda](knowledge-store-howto.md).

```json
"knowledgeStore": {
  "storageConnectionString": "<an Azure storage connection string>",
  "projections" : [
    {
      "tables": [ ]
    },
    {
      "objects": [
        {
          "storageContainer": "containername",
          "source": "/document/EnrichedShape/",
          "key": "/document/Id"
        }
      ]
    }
  ]
}
```

Hiyerarşik ilişkileri korunur veya JSON belgeleri olarak blob depolama, tablo olarak zenginleştirilmiş belgeleri kaydetmek seçebilirsiniz. Çıkış herhangi birinden standartlarındaki şu yeteneklerinizi yansıtma için giriş olarak kaynağı. Belirli bir proje verilerini istiyorsanız şekli, güncelleştirilmiş [shaper beceri](cognitive-search-skill-shaper.md) kullanabilmeniz için karmaşık türler artık modelleyebilirsiniz. 

<a name="next-step"></a>

## <a name="next-steps"></a>Sonraki adımlar

Uzmanlık becerileri ve zenginleştirme işlem hattı ile ilgili bilgi sahibi olduğunuz, devam [bir beceri kümesi açıklamalarda başvurmak nasıl](cognitive-search-concept-annotations-syntax.md) veya [çıkışları dizin alanlarına eşleme nasıl](cognitive-search-output-field-mapping.md). 
