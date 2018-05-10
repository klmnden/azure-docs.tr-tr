---
title: Bilişsel arama işlem hattı (Azure Search) içinde bir skillset oluşturma | Microsoft Docs
description: Doğal dil işleme, veri çıkarma tanımlayın veya Azure Search'te zenginleştirmek ve verileriniz için yapılandırılmış bilgi ayıklamak için görüntü analiz adımları kullanın.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 3ab35cfd8ce5cf54a68473736fe05b78d26850de
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-create-a-skillset-in-an-enrichment-pipeline"></a>İyileştirmesini ardışık düzeninde bir skillset oluşturma

Bilişsel arama ayıklar ve Azure Search'te aranabilir yapmak için veri aşağıdakilere zenginleştirir. Ayıklama ve iyileştirmesini adımları diyoruz *bilişsel becerileri*, içine birleşik bir *skillset* dizin oluşturma sırasında başvurulan. Bir skillset kullanabilirsiniz [becerileri önceden tanımlanmış](cognitive-search-predefined-skills.md) veya özel becerileri (bkz [örnek: özel bir yetenek oluşturmak](cognitive-search-create-custom-skill-example.md) daha fazla bilgi için).

Bu makalede, bir iyileştirmesini ardışık düzen için kullanmak istediğiniz yetenekler oluşturmayı öğrenin. Bir Azure Search bir skillset bağlı [dizin oluşturucu](search-indexer-overview.md). Bu makalede ele alınan ardışık düzen tasarımının bir parçası skillset oluşturma. 

> [!NOTE]
> Ardışık Düzen tasarım başka bir parçası ele bir dizin oluşturucu belirtme [sonraki adım](#next-step). Bir dizin oluşturucu tanımı skillset artı alan eşlemelerini hedef dizin çıktılarında girişleri bağlamak için kullanılan bir başvuru içeriyor.

Dikkat edilmesi gereken önemli noktalar:

+ Dizin Oluşturucu başına bir skillset yalnızca olabilir.
+ Bir skillset en az bir yetenek olması gerekir.
+ (Örneğin, bir görüntü analiz yetenek çeşitleri) aynı türde birden fazla yetenekleri oluşturabilirsiniz, ancak her yetenek aynı skillset içinde yalnızca bir kez kullanılabilir.

## <a name="begin-with-the-end-in-mind"></a>Son aklınızda başlayın

Önerilen ilk adım, ham verilerinizi ve bu verileri bir arama çözümde kullanmak istiyorsanız nasıl ayıklamak için hangi verilerin karar vermektir. Tüm iyileştirmesini ardışık çizim oluşturulması, gerekli adımları belirlemenize yardımcı olabilir.

Finansal analist yorumları kümesi işlemede ilgilendiğiniz varsayalım. Şirket adları ve açıklamaları genel düşünceleri ayıklamak istediğiniz her dosya için. Bing varlık arama hizmeti ne tür bir şirket içinde ilgilendiğini iş gibi şirket hakkında daha fazla bilgi bulmak için kullandığı özel bir enricher yazmak isteyebilirsiniz. Esas olarak, aşağıdaki gibi bilgileri ayıklamak istediğiniz her belge için dizine:

| Kayıt metin | Şirketler | Düşünceleri | Şirket açıklamaları |
|--------|-----|-----|-----|
|Örnek kayıt| ["Microsoft", "LinkedIn"] | 0.99 | ["Microsoft Corporation'ın bir Amerikan çokuluslu teknoloji şirketi...", "LinkedIn bir iş ve çalışma-odaklı sosyal ağ..."]

Aşağıdaki diyagramda bir kuramsal iyileştirmesini ardışık gösterilmektedir:

![Kuramsal iyileştirmesini ardışık düzen](media/cognitive-search-defining-skillset/sample-skillset.png "kuramsal iyileştirmesini ardışık düzen")


Bir kez istediğinizi ardışık düzeninde adımları sağlar skillset express Orta fikir sahip. İşlevsel olarak, Azure Search dizin oluşturucu tanımınızı yüklediğinizde skillset ifade edilir. Dizin karşıya yükleme hakkında daha fazla bilgi için bkz: [dizin oluşturucu belgelerine](https://docs.microsoft.com/rest/api/searchservice/create-indexer).


Çizimde *belge çözme* adım otomatik olarak gerçekleşir. Esas olarak, Azure Search iyi bilinen dosyaların nasıl açılacağını bilir ve oluşturan bir *içerik* her belgeden ayıklanan metin içeren alan. Yerleşik enrichers beyaz kutularıdır ve noktalı "Bing varlık arama" kutusuna oluşturmakta olduğunuz özel bir enricher temsil eder. Gösterildiği gibi skillset üç yetenekleri içerir.

## <a name="skillset-definition-in-rest"></a>REST Skillset tanımında

Bir skillset becerileri bir dizi tanımlanır. Her yetenek girdilerinden kaynağı ve üretilen çıkış adını tanımlar. Kullanarak [Skillset REST API'si oluşturma](ref-create-skillset.md), önceki diyagrama karşılık gelen bir skillset tanımlayabilirsiniz: 

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
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
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
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
          "Ocp-Apim-Subscription-Key": "foobar",
      },
      "context": "/document/content/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/content/organizations/*"
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

## <a name="create-a-skillset"></a>Bir skillset oluşturma

Bir skillset oluşturulurken, kendi kendine skillset belgeleme yapmak için bir açıklama girebilirsiniz. Açıklama isteğe bağlıdır, ancak bir skillset ne yaptığını izlemek için kullanışlıdır. Skillset açıklamaları izin verme, bir JSON belgesi olduğundan kullanmalısınız bir `description` bu öğesi için.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

Skillset sonraki parçada becerileri dizisidir. Her beceri iyileştirmesini temel türü olarak düşünebilirsiniz. Her yetenek bu iyileştirmesini ardışık düzeninde küçük bir görevi gerçekleştirir. Her bir giriş (ya da bir giriş kümesini) alır ve bazı çıktıları döndürür. Sonraki birkaç bölümlerde becerileri birlikte girdi ve çıktı başvuruları zincirleme önceden tanımlanmış ve özel becerileri belirtmek nasıl odaklanır. Girişleri kaynak verilerden veya başka bir yetenek gelebilir. Çıkış arama dizini alanına eşlenen veya bir aşağı akış yetenek girdi olarak kullanılır.

## <a name="add-predefined-skills"></a>Önceden tanımlanmış yetenekleri ekleme

Önceden tanımlanmış olan ilk yetenek bakalım [varlık tanıma yetenek adlı](cognitive-search-skill-named-entity-recognition.md):

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
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

* Önceden tanımlanmış her yetenek sahip `odata.type`, `input`, ve `output` özellikleri. Yetenek özgü özellikler için yetenek ilgili ek bilgiler sağlar. Varlık tanıma `categories` pretrained modeli tanıyabilmesi için varlık türleri sabit kümesi arasında bir varlıktır.

* Her yetenek olmalıdır bir ```"context"```. Bağlam işlemleri yer almakta düzeyi temsil eder. Yukarıdaki yetenek bağlam adlandırılmış varlık tanıma yetenek belge başına bir kez çağrılır yani belgenin tamamını ' dir. Çıkış düzeyde de oluşturulur. Daha belirgin olarak ```"organizations"``` bir üyesi olarak oluşturulan ```"/document"```. Bu bilgileri yeni oluşturulan başvurabilir aşağı akış yeteneklerini ```"/document/organizations"```.  Varsa ```"context"``` alan açıkça ayarlanmazsa, varsayılan bağlam belgedir.

* Yetenek "metin", bir kaynak giriş kümesiyle adlı bir girişte ```"/document/content"```. (Varlık tanıma adlı) yetenek çalıştırır *içerik* alanı standart bir alandır her belge, Azure blob Oluşturucu tarafından oluşturulmuş. 

* Adlı bir çıktı yetenek sahip ```"organizations"```. Çıkışları yalnızca işleme sırasında mevcut. Çıkış olarak bu çıkışı bir aşağı akış yetenek 's giriş zincir başvuru ```"/document/organizations"```.

* Değerini belirli bir belge için ```"/document/organizations"``` metinden ayıklanan kuruluşlar dizisidir. Örneğin:

  ```json
  ["Microsoft", "LinkedIn"]
  ```

Bazı durumlarda, bir dizinin her öğe ayrı olarak başvurmak için çağırın. Örneğin, her öğeye geçirmek istediğiniz varsayalım ```"/document/organizations"``` (örneğin, özel Bing varlık arama enricher) başka bir yetenek için ayrı olarak. Dizideki her öğe için bir yıldız işareti yolunu ekleyerek başvurabilir: ```"/document/organizations/*"``` 

İkinci yetenek düşünceleri ayıklama için ilk enricher olarak aynı düzeni izler. Sürdüğünü ```"/document/content"``` giriş olarak ve içerik her örneği için bir düşünceleri puan döndürür. Ayarlanmadı beri ```"context"``` açıkça alan, bir alt öğesi (mySentiment) çıktı sunulmuştur ```"/document"```.

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

Özel Bing varlık arama enricher yapısını geri çağırma:

```json
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "This skill calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar",
      }
      "context": "/document/content/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/content/organizations/*"
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

Bu tanım iyileştirmesini işleminin bir parçası bir web API'si çağıran özel bir yetenektir. Adlandırılmış varlık tanıma tarafından tanımlanan her kuruluş için bir web API açıklaması söz konusu kuruluşun bulmak için bu yetenek çağırır. Ne zaman orchestration çağrısı web API ve alınan bilgi akışını nasıl için dahili olarak iyileştirmesini altyapısı tarafından işlenir. Ancak, bu özel API çağırmak için gerekli başlatma (örneğin, URI, httpHeaders ve beklenen girişleri) JSON sağlanmalıdır. İyileştirmesini ardışık düzeni için özel web API'si oluşturma yönergeleri için bkz [özel bir arabirim tanımlamak nasıl](cognitive-search-custom-skill-interface.md).

"İçerik" alanı ayarlamak için bildirim ```"/document/content/organizations/*"``` iyileştirmesini adım anlamı yıldız işaretiyle adlı *her* altında kuruluş ```"/document/content/organizations"```. 

Çıkış, bu durumda şirket açıklaması oluşturulur tanımlanan her kuruluş için. Bir aşağı akış adımında (örneğin, anahtar tümcecik ayıklama) açıklamasında söz konusu olduğunda yol kullanacaksınız ```"/document/content/organizations/*/description"``` Bunu yapmak için. 

## <a name="enrichments-create-structure-out-of-unstructured-information"></a>Enrichments dışı yapılandırılmamış bilgilerinin yapısı oluşturun

Skillset yapılandırılmamış verileri yapılandırılmış bilgilerini oluşturur. Aşağıdaki örnek göz önünde bulundurun:

*"Kendi Dördüncü Çeyrek içinde Microsoft $1.1 milyar gelir geçen yıl satın sosyal ağ şirket LinkedIn oturum. Edinme, CRM LinkedIn özellikleriyle ve Office Özellikleri birleştirmek için Microsoft'a sağlar. Stockholders kadarki ilerlemesi Mutluluk duyuyoruz."*

Büyük olasılıkla sonucu oluşturulan yapısına aşağıdaki çizime benzer olacaktır:

![Örnek çıktı yapısı](media/cognitive-search-defining-skillset/enriched-doc.png "örnek çıktı yapısı")

Bu yapı iç geri çağırma. Bu grafik kodda gerçekte alınamıyor.

<a name="next-step"></a>
## <a name="next-steps"></a>Sonraki adımlar

İyileştirmesini ardışık düzen ve skillsets sahibiyseniz, devam [bir skillset ek açıklamalar başvurmak nasıl](cognitive-search-concept-annotations-syntax.md) veya [çıkışları dizin alanlarla eşlemek nasıl](cognitive-search-output-field-mapping.md). 