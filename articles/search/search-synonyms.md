---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Azure Search REST API'sini sunulan anlamlıları (Önizleme) özelliği, ön belgeleri."
services: search
documentationCenter: 
authors: mhko
manager: pablocas
editor: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/07/2016
ms.author: nateko
ms.openlocfilehash: 739a0ad77c68ea74ec25bc80c7539ac8b3f18201
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="synonyms-in-azure-search-preview"></a>Eş anlamlıları Azure Search'te (Önizleme)

Arama motorları eş anlamlı örtük olarak terimi aslında sağlamaya gerek olmadan kullanıcı bir sorgu kapsamını genişletmek eşdeğer terimler ilişkilendirin. Örneğin, "canine" ve "köpek yavrusu" terimi "köpek" ve eş ilişkileri "köpek" içeren tüm belgeleri verildiğinde, "köpek" veya "köpek yavrusu" sorgu kapsamında döner.

Azure Search'te sorgu zamanında eş genişletme yapılır. Var olan işlemler hiç kesintiye hizmetiyle eş eşlemeleri ekleyebilirsiniz. Ekleyebileceğiniz bir **synonymMaps** dizini yeniden oluşturmak zorunda kalmadan bir alan tanımı özelliğine. Daha fazla bilgi için bkz: [güncelleştirme dizin](https://docs.microsoft.com/rest/api/searchservice/update-index).

## <a name="feature-availability"></a>Özellik kullanılabilirliği

Eş anlamlıları özelliği şu anda önizlemede değil ve yalnızca en son Önizleme api sürümü desteklenir (API sürümü 2016-09-01-Önizleme =). Şu anda Azure portalı desteği yoktur. İstekte API sürümü belirtildiğinden, genel olarak kullanılabilir (GA) birleştirmek ve aynı uygulamada API'leri önizlemesini mümkündür. Üretim uygulamalarında kullanma önermiyoruz şekilde ancak API SLA ve Özellikler altında olmayan Önizleme, değişebilir.

## <a name="how-to-use-synonyms-in-azure-search"></a>Eş anlamlıları Azure search ile kullanma

Azure Search'te eş anlamlı destek tanımlayan ve hizmetinize karşıya eş eşlemeleri temel alır. Bu eşlemeleri bağımsız bir kaynak (örneğin, veri kaynaklarını veya dizin) oluşturur ve herhangi bir dizinde arama hizmetinizi aranabilir herhangi bir alana tarafından kullanılabilir.

Eş anlamlı eşler ve dizinleri bağımsız olarak korunur. Bir eş anlamlı harita tanımlayın ve hizmetinize karşıya sonra adlı yeni bir özellik ekleyerek bir alan eş anlamlı özelliğini etkinleştirebilirsiniz **synonymMaps** alan tanımında. Oluşturma, güncelleştirme ve bir eş anlamlı harita silme her zaman oluşturamaz, güncelleştirmek veya eş anlamlı harita bölümlerini artımlı olarak silin, yani bir bütün belge işlemdir. Tek giriş güncelleştirme, bir yeniden yükleme gerektirir.

Eş anlamlıları arama uygulamanıza ekleme iki adımlı bir işlemdir:

1.  Arama hizmetinizi API'leri aracılığıyla bir eş anlamlı eşlemesi ekleyin.  

2.  Aranabilir alana eş harita dizin tanımı'nda kullanmak üzere yapılandırın.

### <a name="synonymmaps-resource-apis"></a>SynonymMaps kaynak API'leri

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Eklemek veya bir eş anlamlı harita hizmetinizin altında güncelleştirmek, kullanarak gönderdiğiniz ya da yerleştirin.

Eş anlamlı maps hizmetine POST veya PUT aracılığıyla yüklenir. Her kural yeni satır karakteri ('\n') tarafından ayrılmış gerekir. Ücretsiz bir hizmettir eş eşlemesinde başına 5.000 kuralları ve diğer tüm SKU 10.000 kurallarında kadar tanımlayabilirsiniz. Her kural en fazla 20 uzantılarına sahip olabilir.

Bu Önizleme'de, eş anlamlı eşlemeleri, aşağıda açıklandığı Apache Solr biçiminde olmalıdır. Var olan bir eş anlamlı sözlük farklı bir biçimde varsa ve doğrudan kullanmak istiyorsanız, lütfen bize bilmeniz [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

Aşağıdaki örnekte olduğu gibi HTTP POST kullanılarak yeni bir eş anlamlı eşlemesi oluşturabilirsiniz:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Alternatif olarak, PUT kullanın ve URI üzerinde eş eşleme adı belirtin. Eş anlamlı eşlemesi yoksa, oluşturulur.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr eş biçimi

Solr biçimi eşdeğer ve açık eş eşlemeleri destekler. Eşleştirme kurallarına Apache Solr, bu belgede açıklanan açık kaynak eş filtre belirtimi için: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Aşağıda bir örnek için eşdeğer eş anlamlıları kuralıdır.
```
              USA, United States, United States of America
```

Yukarıdaki, bir arama sorgusu kuralla "ABD", "ABD" veya "ABD" veya "Amerika Birleşik Devletleri" genişletilir.

Açık eşleme bir okla gösterilen "= >". Belirtildiğinde, sol tarafındaki eşleşen bir arama sorgusu terimi bir dizi "= >" sağ tarafındaki alternatifleri ile değiştirilecek. Aşağıdaki kural verildiğinde, arama sorgularını "Washington", "Wash." veya "WA" tümü "WA" yazılacaktır. Açık eşleme yalnızca belirtilen yönde uygular ve "WA" "Washington" için sorgu bu durumda yeniden değil.
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Liste eş hizmetinizi altında eşler.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Bir eş anlamlı harita hizmetinizin altında alın.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Eş anlamlıları harita hizmetinizin altında silin.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a>Aranabilir alana eş harita dizin tanımı'nda kullanmak üzere yapılandırın.

Yeni bir alan özelliği **synonymMaps** aranabilir alan için kullanılacak bir eş anlamlı eşlemesi belirtmek için kullanılır. Eş anlamlı eşlemeleri hizmet düzeyi kaynaklar ve dizin hizmeti altındaki herhangi bir alan başvurduğu.

    POST https://[servicename].search.windows.net/indexes?api-version=2016-09-01-Preview
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

**synonymMaps** için 'Edm.String' veya 'Collection(Edm.String)' türünde aranabilir alanlar belirtilebilir.

> [!NOTE]
> Bu Önizleme'de, yalnızca her alan eşleme bir eş anlamlı olabilir. Birden çok eş eşlemesi kullanmak istiyorsanız, lütfen bize bilmeniz [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="impact-of-synonyms-on-other-search-features"></a>Eş anlamlıları diğer arama özellikleri üzerinde etkisi

Eş anlamlıları özelliği işleminin orijinal sorguyu veya işlecini ile anlamlıları ile yeniden yazar. Bu nedenle, isabet vurgulama ve profilleri Puanlama özgün terim ve eş anlamlıları eşdeğer olarak kabul eder.

Eş anlamlı özellik sorguları aramak için geçerlidir ve filtreleri veya modelleri için geçerli değildir. Benzer şekilde, öneriler yalnızca özgün terimini temel alır; Eş anlamlı sözcük eşleşmeleri yanıtta görünmez.

Eş anlamlı genişletmeleri joker arama terimleri için geçerli değildir; önek, benzer ve regex koşulları genişletilmiş değil.

## <a name="tips-for-building-a-synonym-map"></a>Bir eş anlamlı eşlemesi oluşturmak için ipuçları

- Kısa, iyi tasarlanmış eş harita olası eşleşmeler kapsamlı bir liste daha verimli olur. Aşırı büyük veya karmaşık sözlükler ayrıştırabilir ve birçok anlamlıları sorgu genişletir, sorgu gecikmesi etkileyen daha uzun sürer. Hangi koşulları kullanılabilir tahmin yerine gerçek koşulları aracılığıyla elde edebilirsiniz bir [arama trafiği analiz raporu](search-traffic-analytics.md).

- Hem Başlangıç hem de doğrulama çalışma gibi etkinleştirin ve ardından tam olarak hangi koşulları belirlemek için bu raporu eş eşleşmeden yararlanabilir ve eş haritanızı daha iyi sonuç üretme olduğunu doğrulama kullanmaya devam. Önceden tanımlanmış raporda döşeme "en yaygın arama sorguları" ve "sıfır sonuç arama sorguları" gerekli bilgileri sağlayacaktır.

- Arama uygulamanız için birden çok eş eşlemeleri (çok dilli müşteri tabanı uygulamanız destekliyorsa, örneğin, dil tarafından) oluşturabilirsiniz. Şu anda bir alana yalnızca bunlardan birini kullanabilirsiniz. Herhangi bir anda bir alanın synonymMaps özelliği güncelleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

- Varolan bir dizini geliştirme (üretim olmayan) ortamında varsa, eş anlamlıları eklenmesi profilleri Puanlama üzerinde etkisi de dahil olmak üzere, bir arama deneyimi nasıl değiştiğini görmek için isabet vurgulama küçük bir sözlük ve öneriler deneyin.

- [Arama trafiği analytics etkinleştirmek](search-traffic-analytics.md) ve hangi koşulları en kullanıldığını öğrenmek için önceden tanımlanmış Power BI raporu kullanabilir ve hangilerinin sıfır belgeleri döndürür. Bu ınsights ile çağrılmak, belgeleri dizininize çözümleme üretken sorguları için eş anlamlı sözcükleri dahil etmek için Sözlük gözden geçirin.
