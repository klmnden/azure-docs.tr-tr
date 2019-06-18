---
title: Sorgu genişletme üzerinde bir arama dizininin - Azure Search için eş anlamlı sözcükler
description: Azure Search dizini bir arama sorgusuna kapsamını genişletmek için bir eş anlamlı eşlemi oluşturabilir. Kapsam, bir listede sağladığınız eşdeğer terimleri içerecek şekilde genişlettik.
author: brjohnstmsft
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
manager: jlembicz
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 567124f50745080da12178a458957a0f6c8266b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65024303"
---
# <a name="synonyms-in-azure-search"></a>Azure Search'te eş anlamlıları

Arama motorları anlamlı örtük olarak terimi aslında sağlamak zorunda kullanıcı bir sorgu kapsamını genişletin eşdeğer terimler ilişkilendirin. Örneğin, "canine" ve "sevimli köpek" terimi "köpek" ve eş anlamlı ilişkilendirmelerini "köpek" içeren tüm belgeleri verildiğinde, "köpek" veya "sevimli köpek" sorgu kapsamında denk gelir.

Azure Search'te eş anlamlı genişletme sorgu zamanında gerçekleştirilir. Eş anlamlı sözcük eşlemelerini hiçbir aksamasıyla mevcut işlemleri ile ilgili bir hizmete ekleyebilirsiniz. Ekleyebileceğiniz bir **synonymMaps** dizini yeniden oluşturmak zorunda kalmadan bir alan tanımı özelliği.

## <a name="create-synonyms"></a>Eş Anlamlılar oluşturma

Eş Anlamlılar oluşturmak için portalı desteği yoktur, ancak REST API'si veya .NET SDK'sını kullanabilirsiniz. REST ile çalışmaya başlamak için öneririz [Postman kullanarak](search-fiddler.md) ve bu API'yi kullanarak istekleri, oluşumunu: [Eş anlamlı haritaları](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map). İçin C# geliştiriciler, başlayabilirsiniz ile [Azure arama'yı kullanarak eş anlamlılar eklemek C# ](search-synonyms-tutorial-sdk.md).

İsteğe bağlı olarak kullanıyorsanız [müşteri tarafından yönetilen anahtarlar](search-security-manage-encryption-keys.md) Hizmet tarafı şifreleme bekleyen için bu koruma, eş anlamlı eşlemi içeriğini uygulayabilirsiniz.

## <a name="use-synonyms"></a>Eş anlamlıları kullanma

Azure Search'te eş anlamlı sözcük desteği, tanımladıktan sonra hizmetinize karşıya eş anlamlı eşlemeleri temel alır. Bu eşlemeler (dizinlere veya veri kaynakları için gibi) bağımsız bir kaynak oluşturur ve aranabilir alanların herhangi birinde arama hizmetinizdeki herhangi bir dizinde tarafından kullanılabilir.

Eş anlamlı eşler ve dizinleri birbirinden bağımsız olarak korunur. Bir eş anlamlı eşlemi tanımlayın ve hizmetinize karşıya sonra adlı yeni bir özellik ekleyerek bir alanı eş anlamlı özelliği etkinleştirebilirsiniz **synonymMaps** alan tanımında. Oluşturma, güncelleştirme ve silme bir eş anlamlı eşlemi her zaman oluşturamaz, güncelleştirme veya artımlı olarak eş anlamlı eşlemi bölümlerini silmek, yani bir bütün belge işlemdir. Hatta tek bir giriş güncelleştirilmesini, bir yeniden yükleme gerektirir.

Arama uygulamanız eş anlamlılar ekleme iki adımlı bir işlemdir:

1.  Aşağıdaki API'leri aracılığıyla, arama hizmetinize bir eş anlamlı eşlemi ekleyin.  

2.  Dizin tanımında eş anlamlı eşlemini kullanacak aranabilir bir alan yapılandırın.

### <a name="synonymmaps-resource-apis"></a>SynonymMaps kaynak API'leri

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Eklemek veya bir eş anlamlı eşlemi hizmetinizdeki güncelleştirme, gönderin veya yerleştirin.

Eş anlamlı sözcük eşlemelerini POST veya PUT hizmete yüklenir. Her kural yeni satır karakteri ('\n') tarafından ayrılmış olması gerekir. En fazla eş anlamlı eşleminde ücretsiz bir hizmet başına 5.000 kuralları ve diğer tüm SKU'lar 10.000 kuralları tanımlayabilirsiniz. Her kural, en fazla 20 genişletmeleri olabilir.

Eş anlamlı sözcük eşlemelerini, aşağıda açıklandığı Apache Solr biçiminde olmalıdır. Var olan bir eş anlamlı sözlük farklı bir biçime sahip ve doğrudan kullanmak istiyorsanız lütfen bize bilmeniz [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

Aşağıdaki örnekte olduğu gibi HTTP POST kullanan yeni bir eş anlamlı eşlemi oluşturabilirsiniz:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2019-05-06
    api-key: [admin key]

    {
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Alternatif olarak, PUT kullanıp URİ'SİNDE eş anlamlı eşlemi adı belirtin. Eş anlamlı eşlemi mevcut değilse oluşturulur.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

    {
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr eş anlamlı biçimi

Solr biçimi eşdeğer ve açık bir eş anlamlı sözcük eşlemelerini destekler. Eşleme kuralları, bu belgede açıklanan Apache Solr'ın açık kaynak eş anlamlı filtresi için belirtime: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Aşağıda bir örnek için eşdeğer eş anlamlılar kuralıdır.
```
USA, United States, United States of America
```

Yukarıdaki arama sorgusu kuralla "ABD", "ABD" veya "ABD" veya "ABD" genişletilir.

Eşleme açık bir okla gösterilen "= >". Belirtildiğinde, sol tarafında eşleşen bir arama sorgusu terim dizisi "= >" sağ taraftaki alternatifleri değiştirilecektir. Given the rule below, search queries "Washington", "Wash." veya "WA" tüm "WA" için yazılacaktır. Açık bir eşleme, yalnızca belirtilen yönde uygular ve "Washington" için "WA" sorgu bu durumda yeniden değil.
```
Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Liste eş anlamlı hizmetinizin altında eşler.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2019-05-06
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Bir eş anlamlı eşlemi hizmetinizdeki alın.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Bir eş anlamlılar harita hizmetinizdeki silin.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2019-05-06
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a>Dizin tanımında eş anlamlı eşlemini kullanacak aranabilir bir alan yapılandırın.

Yeni bir alan özelliği **synonymMaps** aranabilir bir alanı için kullanılacak bir eş anlamlı eşlemi belirtmek için kullanılabilir. Eş anlamlı sözcük eşlemelerini hizmet düzeyi kaynaklarıdır ve bir dizin hizmeti altındaki herhangi bir alan tarafından başvurulabilir.

    POST https://[servicename].search.windows.net/indexes?api-version=2019-05-06
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

**synonymMaps** 'Edm.String' veya 'Collection(Edm.String)' türündeki aranabilir alanları için belirtilebilir.

> [!NOTE]
> Yalnızca her alan eşleme bir eş anlamlı olabilir. Birden çok eş anlamlı eşlemesi kullanmak istiyorsanız, lütfen bize bilmeniz [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="impact-of-synonyms-on-other-search-features"></a>Eş Anlamlılar diğer arama özellikleri üzerinde etkisi

Eş Anlamlılar özelliğini OR işleci ile eş anlamlı sözcüklerle özgün sorguyu yeniden yazar. Bu nedenle, isabet vurgulama ve puanlama profilleri özgün terim ve eş anlamlılar eşdeğer olarak kabul edin.

Eş anlamlı özellik arama sorguları için geçerlidir ve filtreleri veya özellikleri için geçerli değildir. Benzer şekilde, öneriler, yalnızca özgün terimini dayanır; Eş anlamlı eşleşme yanıtta görünmez.

Eş anlamlı genişletmeleri joker arama terimi için geçerli değildir; ön ek, belirsiz ve normal ifade terimleri genişletilmiş değildir.

Eş anlamlı genişletme ve joker karakter, regex veya belirsiz aramalar geçerli tek bir sorgu yapmanız gerekiyorsa veya söz dizimi kullanılarak sorgular birleştirebilirsiniz. Örneğin, eş anlamlılar joker karakterler için Basit Sorgu söz dizimi ile birleştirmek için terimi olacaktır `<query> | <query>*`.

## <a name="tips-for-building-a-synonym-map"></a>Bir eş anlamlı eşlemi oluşturmaya yönelik ipuçları

- Bir kısa, iyi tasarlanmış bir eş anlamlı eşlemi, olası eşleşmeler kapsamlı bir liste daha verimli olur. Aşırı büyük veya karmaşık sözlükleri ayrıştırma ve birçok eş anlamlıları sorguyu genişletir, sorgu gecikme süresini etkileyen daha uzun sürer. Başlangıçtan koşulları kullanılabilir tahmin yerine gerçek koşulları aracılığıyla alabilirsiniz bir [arama trafiği analizi raporu](search-traffic-analytics.md).

- Hem Başlangıç hem de doğrulama çalışma gibi etkinleştirin ve ardından tam olarak hangi koşulları belirlemek için bu raporu eş anlamlı eşleşmeden yararlanır ve, eş anlamlı eşlemi daha iyi bir sonuç oluşturmayı, doğrulama kullanmaya devam edin. Önceden tanımlanmış raporun kutucukları "en yaygın arama sorguları" ve "sıfır sonuç arama sorguları" gerekli bilgileri sağlayacak.

- Arama uygulamanız için birden çok eş anlamlı sözcük eşlemelerini (çok dilli müşteri tabanı uygulamanız destekliyorsa, örneğin, dil tarafından) oluşturabilirsiniz. Şu anda bir alanı yalnızca bunlardan birini kullanabilirsiniz. Bir alanın synonymMaps özelliği herhangi bir zamanda güncelleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bir geliştirme (üretim olmayan) ortamında var olan bir dizin varsa, eş anlamlıların eklenmesi Puanlama profilleri üzerinde bir etkisi de dahil olmak üzere, arama deneyimini nasıl değiştiğini görmek için isabet vurgulama küçük bir sözlük ve öneriler ile denemeler yapın.

- [Arama trafiği analizi etkinleştirme](search-traffic-analytics.md) hangi koşulları en çok kullanıldığını öğrenmek için önceden tanımlanmış Power BI raporunu kullanın ve belge hangilerini döndürür. Bu Öngörüler ile kullanarak sözlük belgeleri dizininize çözümleme üretken sorgular için eş anlamlı sözcükler içerecek şekilde değiştirin.
