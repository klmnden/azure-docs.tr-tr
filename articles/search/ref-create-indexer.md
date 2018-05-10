---
title: Dizin Oluşturucu yapın (Azure Search Hizmeti REST API sürümü 2017-11-11-Önizleme =)
description: Önizleme API dizin oluşturucular iyileştirmesini çıktı Azure Search dizini alanında eşleştirmek için kullanılan outputFieldMapping parametreleri içerecek şekilde genişletilmiştir.
services: search
manager: pablocas
author: luiscabrer
ms.author: luisca
ms.date: 05/01/2018
ms.prod: azure
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: language-reference
ms.openlocfilehash: d47b14342caf0312e052584d20f1a9c86ca29cad
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-indexer-azure-search-service-rest-api-version2017-11-11-preview"></a>Dizin Oluşturucu yapın (Azure Search Hizmeti REST API sürümü 2017-11-11-Önizleme =)

Bu API Başvurusu dizin oluşturma için bilişsel arama yenilikleri kapsayan belgeler, Önizleme özgü sürümüdür.

İle [genel olarak kullanılabilir](https://docs.microsoft.com/rest/api/searchservice/create-indexer) sürümü, bir HTTP POST isteği kullanarak Azure Search hizmeti içinde yeni bir dizin oluşturucu oluşturabilirsiniz. 

```http
POST https://[service name].search.windows.net/indexers?api-version=[api-version]  
    Content-Type: application/json  
    api-key: [admin key]  
```  
**Api anahtarını** bir yönetici anahtarı (aksine, bir sorgu anahtarı) olması gerekir. Kimlik doğrulaması bölümünde başvurmak [Azure Search'te güvenlik](search-security-overview.md) anahtarları hakkında daha fazla bilgi için. [Portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) hizmet URL'si ve anahtar özelliklerine istekte kullanılan alınacağı açıklanmaktadır.

Alternatif olarak, PUT kullanın ve URI üzerinde veri kaynağı adı belirtin. Veri kaynağı mevcut değilse oluşturulur.  

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]  
```  
**API sürümü** gereklidir. Geçerli sürüm `2016-09-01`. Bkz: [API sürümleri Azure Search'te](search-api-versions.md) Ayrıntılar için.

Dizin oluşturucular oluşturma verileri platforma özgü konusunda rehberlik için başlayın [dizin oluşturucular genel bakış](search-indexer-overview.md), tam listesini içeren [ilgili makaleler](search-indexer-overview.md#next-steps).

> [!NOTE]  
>  Dizin oluşturucular izin verilen en fazla fiyatlandırma katmanı göre değişir. Ücretsiz hizmeti 3 adede kadar dizin oluşturucular sağlar. Standart hizmeti 50 dizin oluşturucular sağlar. Bkz: [hizmet sınırları](search-limits-quotas-capacity.md) Ayrıntılar için.    

## <a name="request"></a>İstek  
 İstek gövdesini veri kaynağı ve dizin oluşturma, hem de isteğe bağlı dizin oluşturma zamanlama ve parametreler için hedef dizin belirten bir dizin oluşturucu tanımını içerir.  

 İstek yükünde yapılandırılması söz dizimi aşağıdaki gibidir. Örnek istek, bu konunun ilerleyen bölümlerinde sağlanır.  

```json
{   
    "name" : "Required for POST, optional for PUT. The name of the indexer",  
    "description" : "Optional. Anything you want, or null",  
    "dataSourceName" : "Required. The name of an existing data source",  
    "targetIndexName" : "Required. The name of an existing index",  
    "schedule" : { Optional. See Indexing Schedule below. },  
    "parameters" : { Optional. See Indexing Parameters below. },  
    "fieldMappings" : { Optional. See fieldMappings below. },
    "outputFieldMappings" : { Required for enrichment pipelines. See outputFieldMappings below. },
    "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.
}  
```

### <a name="indexer-schedule"></a>Dizin Oluşturucu zamanlaması  
Bir dizin oluşturucu, isteğe bağlı olarak bir zamanlama belirtebilirsiniz. Bir zamanlama varsa, dizin oluşturucu düzenli aralıklarla zamanlamaya göre çalışır. Zamanlayıcı yerleşiktir; bir dış Zamanlayıcısı'nı kullanamazsınız. **Zamanlama** aşağıdaki özniteliklere sahiptir: 

-   **aralığı**: gerekli. Bir aralığı ya da dizin oluşturucu için süresi belirten bir süre değeri çalışır. İzin verilen en küçük aralık 5 dakikadır; en uzun bir gündür. Bir XSD "dayTimeDuration" değeri biçimlendirilmelidir (sınırlı alt kümesi bir [ISO 8601 süre](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değeri). Bu desen: `"P[nD][T[nH][nM]]".` örnekler: `PT15M` her 15 dakikada için `PT2H` için her 2 saatte bir.  

-   **startTime**: isteğe bağlı. Dizin Oluşturucu çalıştıran başlattığınızda UTC datetime.  

### <a name="indexer-parameters"></a>Dizin Oluşturucu parametreleri  
 Bir dizin oluşturucu davranışını etkileyen çeşitli parametreler isteğe bağlı olarak belirtebilirsiniz. Aşağıda listelenen tüm parametreler isteğe bağlıdır.  

-   **maxFailedItems**: dizin oluşturucunun çalıştırılması hata olarak kabul edilmeden önce dizine eklenemeyecek olan öğe sayısı. Varsayılan 0'dır. Başarısız olan öğeler hakkında bilgi tarafından döndürülen [dizin oluşturucu durumunu Al &#40;Azure Search Hizmeti REST API'si&#41; ](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) işlemi.  

-   **maxFailedItemsPerBatch**: dizin oluşturucunun çalıştırılması hata olarak kabul edilmeden önce her bir toplu iş içinde dizine eklenemeyecek olan öğe sayısı. Varsayılan 0'dır.  

-   **batchSize:** performansı iyileştirmek için tek bir toplu iş olarak veri kaynağından okumak ve dizinli öğe sayısını belirtir. Varsayılan veri kaynağı türüne bağlıdır: Azure SQL ve Azure Cosmos DB 1000 ve Azure Blob Storage için 10 olan.

### <a name="field-mapping-parameters"></a>Alan eşleme parametreleri

Dizin Oluşturucu tanımları, Azure Search dizini hedef alana kaynak alan eşleme alan ilişkileri içerir. İçerik Aktarım doğrudan veya zenginleştirilmiş yolu uymayacağını bağlı olarak ilişkilendirmeleri iki tür vardır:

+ **fieldMappings** kaynak hedef alan adları eşleşmiyor veya bir işlev belirtmek istediğinizde uygulanan isteğe bağlıdır.
+ **outputFieldMappings** oluşturmakta olduğunuz kullanıyorsa gereklidir [iyileştirmesini ardışık düzen](cognitive-search-concept-intro.md). Bir iyileştirmesini ardışık düzeninde çıktı alanı iyileştirmesini işlemi sırasında tanımlanan bir yapıdır. Örneğin, çıktı alanı iyileştirmesini kaynak belgedeki iki ayrı alanlarından sırasında oluşturulan bir bileşik yapısı olabilir. 

#### <a name="fieldmappings"></a>fieldMappings

Aşağıdaki örnekte, bir alan içeren bir kaynak tablo göz önünde bulundurun `_id`. Azure arama alan yeniden adlandırılması gerekir böylece bir alt çizgi ile başlayan bir alan adı izin vermez. Bu yapılabilir kullanarak `fieldMappings` şekilde dizin oluşturucu özelliği:

```json
"fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
```

Birden çok alan eşlemelerini belirtebilirsiniz:

```json
"fieldMappings" : [
    { "sourceFieldName" : "_id", "targetFieldName" : "id" },
    { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" }
]
```

Hem kaynak hem de hedef alan adları büyük/küçük harfe duyarsızdır.

Alan eşlemelerini nerede yararlı senaryoları hakkında bilgi edinmek için [arama dizin oluşturucu alan eşlemelerini](https://docs.microsoft.com/azure/search/search-indexer-field-mappings).

#### <a name="outputfieldmappings"></a>outputFieldMappings

Bir dizin oluşturucu için bir skillset bağlıysa bilişsel arama senaryolarda eklemelisiniz `outputFieldMappings` herhangi bir çıktı dizini aranabilir bir alana içerik sağlayan bir iyileştirmesini adımın ilişkilendirilecek.

```json
  "outputFieldMappings" : [
        {
          "sourceFieldName" : "/document/organizations", 
          "targetFieldName" : "organizations"
        },
        {
          "sourceFieldName" : "/document/pages/*/keyPhrases/*", 
          "targetFieldName" : "keyphrases"
        },
        {
            "sourceFieldName": "/document/languageCode",
            "targetFieldName": "language",
            "mappingFunction": null
        }      
   ],
```

<a name="FieldMappingFunctions"></a>

#### <a name="field-mapping-functions"></a>Alan eşleme işlevleri

Alan eşlemelerini de kullanılabilir kaynak alan değerleri kullanarak dönüştürmek için *alan eşleme işlevleri*. Örneğin, bir belge anahtar alanı doldurmak için kullanılabilmesi için bir isteğe bağlı bir dize değeri base64 ile kodlanmış olabilir.

Ne zaman ve alan eşleme işlevlerinin nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [alan eşleme işlevleri](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions).

### <a name="request-body-examples"></a>İstek gövdesi örnekleri  
 Aşağıdaki örnek tarafından başvurulan tablodaki verileri kopyalayan bir dizin oluşturucu oluşturur `ordersds` veri kaynağı için `orders` , 1 Ocak 2015 tarihinde da UTC başlar ve saatte çalışır bir zamanlamaya göre dizin. Her dizin oluşturucu çağrıldığında her toplu iş içinde dizine en fazla 5 öğe başarısız olursa başarılı olur ve toplam listelenecek en fazla 10 öğe başarısız.  

```json
{
    "name" : "myindexer",  
    "description" : "a cool indexer",  
    "dataSourceName" : "ordersds",  
    "targetIndexName" : "orders",  
    "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },  
    "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }  
}
```

## <a name="response"></a>Yanıt  
 Başarılı bir istek için 201 oluşturuldu.  

## <a name="see-also"></a>Ayrıca bkz.

+ [Bilişsel arama genel bakış](cognitive-search-concept-intro.md)
+ [Hızlı Başlangıç: Try bilişsel arama](cognitive-search-quickstart-blob.md)
+ [Alanları eşleme](cognitive-search-output-field-mapping.md)
