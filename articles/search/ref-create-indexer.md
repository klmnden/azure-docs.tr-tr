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
ms.openlocfilehash: 595502ecf0a78491c73800e8077de65707c7a486
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="create-indexer-azure-search-service-rest-api-version2017-11-11-preview"></a>Dizin Oluşturucu yapın (Azure Search Hizmeti REST API sürümü 2017-11-11-Önizleme =)

Bu API başvuru belgeleri Önizleme özgü sürümüdür ekleme [bilişsel arama](cognitive-search-concept-intro.md) oluşturma dizin oluşturucu API öğelerine. İle [genel olarak kullanılabilir](https://docs.microsoft.com/rest/api/searchservice/create-indexer) sürümü, bir HTTP POST isteği kullanarak Azure Search hizmeti içinde yeni bir dizin oluşturucu oluşturabilirsiniz. 

```http
POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json  
    api-key: [admin key]  
```  
**Api anahtarını** bir yönetici anahtarı (aksine, bir sorgu anahtarı) olması gerekir. Kimlik doğrulaması bölümünde başvurmak [Azure Search'te güvenlik](search-security-overview.md) anahtarları hakkında daha fazla bilgi için. [Portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) hizmet URL'si ve anahtar özelliklerine istekte kullanılan alınacağı açıklanmaktadır.

Alternatif olarak, PUT kullanın ve URI üzerinde veri kaynağı adı belirtin. Veri kaynağı mevcut değilse oluşturulur.  

```http
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]  
```  
**API sürümü** gereklidir. Geçerli genellikle kullanılabilir sürümüdür `api-version=2017-11-11`, ancak Önizleme sürümü için bilişsel arama gerekir: `api-version=2017-11-11-Preview`.  Bkz: [API sürümleri Azure Search'te](search-api-versions.md) Ayrıntılar için.

Dizin oluşturucular oluşturma verileri platforma özgü konusunda rehberlik için başlayın [dizin oluşturucular genel bakış](search-indexer-overview.md), tam listesini içeren [ilgili makaleler](search-indexer-overview.md#next-steps).

> [!NOTE]  
>  Dizin oluşturucular izin verilen en fazla fiyatlandırma katmanı göre değişir. Ücretsiz hizmeti 3 adede kadar dizin oluşturucular sağlar. Standart hizmeti 50 dizin oluşturucular sağlar. Standart yüksek tanımı Hizmetleri dizin oluşturucular hiç desteklemez. Bkz: [hizmet sınırları](search-limits-quotas-capacity.md) Ayrıntılar için.    

## <a name="request"></a>İstek  

A [veri kaynağı](https://docs.microsoft.com/rest/api/searchservice/create-data-source), [dizin](https://docs.microsoft.com/rest/api/searchservice/create-index), ve [skillset](ref-create-skillset.md) parçası olan bir [dizin oluşturucu](search-indexer-overview.md) tanımı, ancak her olan kullanılabilir bir bağımsız bileşeni farklı birleşimleri. Örneğin, birden çok dizin oluşturucu veya birden çok dizin oluşturucu veya birden çok dizin oluşturucu için tek bir dizin yazma ile aynı dizini ile aynı veri kaynağını kullanabilirsiniz.

 İstek gövdesini bir dizin oluşturucu tanımıyla aşağıdaki bölümleri içerir.

+ [dataSourceName](#dataSourceName)
+ [targetIndexName](#targetIndexName)
+ [skillsetName](#skillset)
+ [schedule](#indexer-schedule)
+ [parametreler](#indexer-parameters)
+ [fieldMappings](#field-mappings)
+ [outputFieldMappings](#output-fieldmappings)

## <a name="request-syntax"></a>İstek sözdizimi

İstek yükünde yapılandırılması söz dizimi aşağıdaki gibidir. Örnek istek bu makalenin sonraki bölümlerinde sağlanır.  

```json
{   
    "name" : "Required for POST, optional for PUT. The name of the indexer",  
    "description" : "Optional. Anything you want, or null",  
    "dataSourceName" : "Required. The name of an existing data source",  
    "targetIndexName" : "Required. The name of an existing index",  
    "skillsetName" : "Required for cognitive search enrichment",
    "schedule" : { Optional, but immediately runs once if unspecified. See Indexing Schedule below. },  
    "parameters" : { Optional. See Indexing Parameters below. },  
    "fieldMappings" : { Optional. See fieldMappings below. },
    "outputFieldMappings" : { Required for enrichment pipelines. See outputFieldMappings below. },
    "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.
}  
```
<a name="dataSourceName"></a>

### <a name="datasourcename"></a>"dataSourceName"

A [veri kaynağı tanımını](https://docs.microsoft.com/rest/api/searchservice/create-data-source) genellikle bir dizin oluşturucu kaynak platform özelliklerini yararlanmak için kullanabileceğiniz özellikler içerir. Bu nedenle, dizin oluşturucu için geçirdiğiniz veri kaynağı Azure SQL veritabanı için Azure BLOB veya sorgu zaman aşımı filtreleme gibi içerik türü belirli özellikler ve parametreleri kullanılabilirliğini belirler. 

<a name="targetIndexName"></a>

### <a name="targetindexname"></a>"targetIndexName"

Bir [dizin şemasını](https://docs.microsoft.com/rest/api/searchservice/create-index) alanın nasıl kullanıldığını belirleyen aranabilir, filtrelenebilir, alınabilir ve diğer nitelikleri içeren alanlar koleksiyonu tanımlar. Dizin oluşturma sırasında dizin oluşturucu veri kaynağında gezinir, isteğe bağlı olarak belgeleri cracks bilgilerini ayıklar, JSON sonuçları serileştirir ve dizininiz için tanımlanan şemaya göre yükü dizinler.

<a name="skillset"></a>

### <a name="skillsetname"></a>"skillsetName"

[Bilişsel arama (Önizleme)](cognitive-search-concept-intro.md) doğal dil ve görüntü özelliklerini görüntüler ve diğerleri varlıklar, anahtar tümcecikleri, dil, bilgi ayıklamak için veri alımı sırasında uygulanan Azure Search'te işleme başvuruyor. İçerik için geçerli dönüşümler olan aracılığıyla *becerileri*, tek bir birleştirme [ *skillset*](ref-create-skillset.md), her dizin oluşturucu. Veri kaynakları ve dizinler'te olduğu gibi bir skillset bir dizin oluşturucu ekleyin bağımsız bileşendir. Diğer dizin oluşturucular ile skillset amaçlandırabilirsiniz, ancak her dizin oluşturucu aynı anda yalnızca bir skillset kullanabilirsiniz.
 
<a name="indexer-schedule"></a>

### <a name="schedule"></a>"zamanlama"  
Bir dizin oluşturucu, isteğe bağlı olarak bir zamanlama belirtebilirsiniz. İsteği gönderdiğinizde hemen olmadan zamanlama, dizin oluşturucu çalıştırır: bağlanma, gezinme ve veri kaynağı dizin oluşturma. Uzun süre çalışan dizin oluşturma işleri de dahil olmak üzere bazı senaryolar için zamanlamaları için kullanılan [işleme penceresi genişletmek](search-howto-reindex.md#scale-out-indexing) 24 saatlik maksimum ötesinde. Bir zamanlama varsa, dizin oluşturucu düzenli aralıklarla zamanlamaya göre çalışır. Zamanlayıcı yerleşik olarak bulunur; bir dış Zamanlayıcısı'nı kullanamazsınız. A **zamanlama** aşağıdaki özniteliklere sahiptir: 

-   **aralığı**: gerekli. Bir aralığı ya da dizin oluşturucu için süresi belirten bir süre değeri çalışır. İzin verilen en küçük aralık beş dakikadır; en uzun bir gündür. Bir XSD "dayTimeDuration" değeri biçimlendirilmelidir (sınırlı alt kümesi bir [ISO 8601 süre](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değeri). Bu desen: `"P[nD][T[nH][nM]]".` örnekler: `PT15M` her 15 dakikada için `PT2H` için her 2 saatte bir.  

-   **startTime**: isteğe bağlı. Dizin Oluşturucu çalıştıran başlattığınızda UTC datetime.  

<a name="indexer-parameters"></a>

### <a name="parameters"></a>"parametreleri"

Bir dizin oluşturucu, çalışma zamanı davranışlarını değiştirme yapılandırma parametreleri isteğe bağlı olarak alabilir. Dizin Oluşturucu isteği virgülle ayrılmış yapılandırma parametreleri. 

```json
    {
      "name" : "my-blob-indexer-for-cognitive-search",
      ... other indexer properties
      "parameters" : { "maxFailedItems" : "15", "batchSize" : "100", "configuration" : { "parsingMode" : "json", "indexedFileNameExtensions" : ".json, .jpg, .png", "imageAction" : "generateNormalizedImages", "dataToExtract" : "contentAndMetadata" } }
    }
```

#### <a name="general-parameters-for-all-indexers"></a>Tüm Dizin oluşturucuların için genel parametreler

| Parametre | Tür ve izin verilen değerler   | Kullanım  |
|-----------|------------|--------------------------|--------|
| `"batchSize"` | Tamsayı<br/>Varsayılan kaynak özgü (1000 Azure SQL Database ve Azure Cosmos DB, Azure Blob Storage için 10) | Veri kaynağından okumak ve dizinli öğe sayısı, performansı artırmak için tek bir toplu iş olarak belirtir. |
| `"maxFailedItems"` | Tamsayı<br/>Varsayılan değer 0'dır | Bir dizin oluşturucu Çalıştır bir hata olarak kabul edilmeden önce tolerans hataların sayısı. Dizin oluşturma işlemi durdurmak için herhangi bir hata istemiyorsanız, -1 olarak ayarlayın. Başarısız olan öğeler kullanma hakkında bilgi alabilir [dizin oluşturucu durumunu Al](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status).  |
| `"maxFailedItemsPerBatch"` | Tamsayı<br/>Varsayılan değer 0'dır | Bir dizin oluşturucu Çalıştır bir hata olarak kabul edilmeden önce her toplu işlemde tolerans hataların sayısı. Dizin oluşturma işlemi durdurmak için herhangi bir hata istemiyorsanız, -1 olarak ayarlayın. |

#### <a name="blob-configuration-parameters"></a>BLOB yapılandırma parametreleri

Bazı parametreler gibi belirli bir dizin oluşturucu özel [Azure blob dizini oluşturma](search-howto-indexing-azure-blob-storage.md).

| Parametre | Tür ve izin verilen değerler   | Kullanım  |
|-----------|---------------------------|--------|
| `"parsingMode"` | Dize<br/>`"text"`<br/>`"delimitedText"`<br/>`"json"`<br/>`"jsonArray"`  | İçin [Azure BLOB'ların](search-howto-indexing-azure-blob-storage.md), ayarlanmış `text` blob depolama düz metin dosyalarda dizin oluşturma performansı artırmak için. <br/>İçin [CSV BLOB'ları](search-howto-index-csv-blobs.md), ayarlanmış `delimitedText` BLOB'lar düz CSV dosyaları olduğunda. <br/>İçin [JSON BLOB'ları](search-howto-index-json-blobs.md), ayarlanmış `json` yapılandırılmış extract içerik ya da çok `jsonArray` Azure Search'te ayrı belgeler olarak bir dizi ayrı ayrı öğeler ayıklamak için (Önizleme). |
| `"excludedFileNameExtensions"` | Dize<br/>virgülle ayrılmış liste | İçin [Azure BLOB'ların](search-howto-indexing-azure-blob-storage.md), listedeki tüm dosya türlerini yoksay. Örneğin, hariç tuttuğunuz ".png, .png, .mp4" dizin oluşturma sırasında bu dosyalar atlanacak. | 
| `"indexedFileNameExtensions"` | Dize<br/>virgülle ayrılmış liste | İçin [Azure BLOB'ların](search-howto-indexing-azure-blob-storage.md), dosya uzantısı listede ise BLOB'lar seçer. Örneğin, belirli bir uygulama dizin odaklanmak dosyaları ".docx, .pptx, oldu.msg" özellikle bu dosya türlerini dahil etmek için. | 
| `"failOnUnsupportedContentType"` | true <br/>False (varsayılan) | İçin [Azure BLOB'ların](search-howto-indexing-azure-blob-storage.md), ayarlanmış `false` desteklenmeyen bir içerik türüyle karşılaştı ve tüm içerik türleri (dosya uzantıları) önceden tanımadığınız dizin oluşturma devam etmek istiyor. |
| `"failOnUnprocessableDocument"` | true <br/>False (varsayılan)| İçin [Azure BLOB'ların](search-howto-indexing-azure-blob-storage.md), ayarlanmış `false` bir belge, dizin oluşturma işlemi başarısız olursa, dizin oluşturmaya devam etmek istiyorsanız. |
| `"indexStorageMetadataOnlyForOversizedDocuments"` | true <br/>False (varsayılan)| İçin [Azure BLOB'ların](search-howto-indexing-azure-blob-storage.md), bu özelliği ayarlamak `true` hala işlemek için çok büyük blob içerik için depolama meta veri dizini oluşturmak için.  Büyük boyutlu BLOB'lar varsayılan hata olarak kabul edilir. Blob boyutu sınırları için bkz: [hizmet sınırları](search-limits-quotas-capacity.md). |
| `"delimitedTextHeaders"` | Dize<br/>virgülle ayrılmış liste| İçin [CSV BLOB'ları (Önizleme)](search-howto-index-csv-blobs.md), sütun üst bilgileri, hedef dizin alanlara kaynak alanlarını eşleme için yararlı virgülle ayrılmış listesini belirtir. |
| `"delimitedTextDelimiter"` | Dize<br/>Kullanıcı tanımlı | İçin [CSV BLOB'ları (Önizleme)](search-howto-index-csv-blobs.md), her satır, yeni bir belge başladığı CSV dosyaları için satır sonu sınırlayıcısı belirtir (örneğin, ' "|"`).  |
| `"firstLineContainsHeaders"` | TRUE (varsayılan) <br/>false | İçin [CSV BLOB'ları (Önizleme)](search-howto-index-csv-blobs.md), her bir blob ilk (boş olmayan) satırının üstbilgileri içerdiğini gösterir.|
| `"documentRoot"`  | Dize<br/>Kullanıcı tanımlı | İçin [JSON diziler (Önizleme)](search-howto-index-json-blobs.md#nested-json-arrays), yapılandırılmış veya yarı yapılandırılmış bir belge göz önüne alındığında, bu özelliği kullanan dizisine bir yolu belirtebilirsiniz. |
| `"dataToExtract"` | Dize<br/>`"storageMetadata"`<br/>`"allMetadata"`<br/>`"contentAndMetadata"` (varsayılan) | İçin [Azure BLOB'ların](search-howto-indexing-azure-blob-storage.md):<br/>Kümesine `"storageMetadata"` dizine yalnızca [standart blob özellikleri ve kullanıcı tanımlı meta veriler](../storage/blobs/storage-properties-metadata.md). <br/>Kümesine `"allMetadata"` Azure blob depolama alt sistemi tarafından sağlanan meta verileri ayıklamak için ve [içerik türü belirli meta veriler](search-howto-indexing-azure-blob-storage.md#ContentSpecificMetadata) (örneğin, meta verileri yalnızca .png dosyalarını benzersiz) dizini. <br/>Kümesine `"contentAndMetadata"` tüm meta veri ve metinsel içerik her bir blob ayıklayın. <br/><br/>İçin [görüntü çözümlemeleri bilişsel arama (Önizleme)](cognitive-search-concept-image-scenarios.md), `"imageAction"` ayarlanır `"generateNormalizedImages"`, `"dataToExtract"` ayar görüntü içeriği ayıklamak için hangi verilerin dizin oluşturucu söyler. Katıştırılmış Resim içerik uygulandığı bir. PDF veya başka bir uygulama veya .jpg ve Azure BLOB'ları içinde .png gibi görüntü dosyaları.  |
| `"imageAction"` |  Dize<br/>`"none"`<br/>`"generateNormalizedImages"` | İçin [Azure BLOB'ların](search-howto-indexing-azure-blob-storage.md), ayarlanmış`"none"` katıştırılmış görüntüler veya veri kümesi görüntü dosyaları yoksaymak için. Varsayılan değer budur. <br/><br/>İçin [bilişsel arama görüntü analizi](cognitive-search-concept-image-scenarios.md), ayarlanmış`"generateNormalizedImages"` (örneğin, "Durdur" trafiğinden Dur işareti word) görüntülerden metni ayıklayın ve içerik alanında parçası olarak eklemek için. Görüntü Çözümleme sırasında dizin oluşturucu normalleştirilmiş görüntüleri bir dizi belge çözme bir parçası olarak oluşturur ve oluşturulan bilgileri içerik alanın içine katıştırır. Bu eylemi gerektiren `"dataToExtract"` ayarlanır `"contentAndMetadata"`. Çıkış, boyuta sahip ve visual arama sonuçlarında görüntüleri dahil ettiğiniz zaman uyumlu işleme yükseltmek için Döndürülmüş Tekdüzen görüntüsündeki kaynaklanan ek işleme normalleştirilmiş bir görüntü başvurduğu (örneğin, aynı boyutta fotoğraf grafikteki görülenkontrol[JFK demo](https://github.com/Microsoft/AzureSearch_JFK_Files)). Kullandığınızda bu bilgiler için her görüntü oluşturulur |


#### <a name="other-configuration-parameters"></a>Diğer yapılandırma parametreleri

Aşağıdaki parametreler, Azure SQL veritabanına özgüdür.

| Parametre | Tür ve izin verilen değerler   | Kullanım  |
|-----------|---------------------------|--------|
| `"queryTimeout"` | Dize<br/>"ss: dd:"<br/>"00: 05:00"| İçin [Azure SQL veritabanı](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), ötesinde 5 dakikalık varsayılan zaman aşımı artırmak için bu parametreyi ayarlayın.|

<a name="field-mappings"></a>

### <a name="fieldmappings"></a>"fieldMappings"

Dizin Oluşturucu tanımları, Azure Search dizini hedef alana kaynak alan eşleme alan ilişkileri içerir. İçerik Aktarım doğrudan veya zenginleştirilmiş yolu uymayacağını bağlı olarak ilişkilendirmeleri iki tür vardır:

+ **fieldMappings** kaynak hedef alan adları eşleşmiyor veya bir işlev belirtmek istediğinizde uygulanan isteğe bağlıdır.
+ **outputFieldMappings** oluşturmakta olduğunuz kullanıyorsa gereklidir [iyileştirmesini ardışık düzen](cognitive-search-concept-intro.md). Bir iyileştirmesini ardışık düzeninde çıktı alanı iyileştirmesini işlemi sırasında tanımlanan bir yapıdır. Örneğin, çıktı alanı iyileştirmesini kaynak belgedeki iki ayrı alanlarından sırasında oluşturulan bir bileşik yapısı olabilir. 

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

<a name="output-fieldmappings"></a>

### <a name="outputfieldmappings"></a>"outputFieldMappings"

İçinde [bilişsel arama](cognitive-search-concept-intro.md) bir skillset bir dizin oluşturucu bağlıysa senaryoları eklemelisiniz `outputFieldMappings` herhangi bir çıktı dizini aranabilir bir alana içerik sağlayan bir iyileştirmesini adımın ilişkilendirilecek.

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

### <a name="field-mapping-functions"></a>Alan eşleme işlevleri

Alan eşlemelerini de kullanılabilir kaynak alan değerleri kullanarak dönüştürmek için *alan eşleme işlevleri*. Örneğin, bir belge anahtar alanı doldurmak için kullanılabilmesi için bir isteğe bağlı bir dize değeri base64 ile kodlanmış olabilir.

Ne zaman ve alan eşleme işlevlerinin nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [alan eşleme işlevleri](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions).

## <a name="request-examples"></a>İstek örnekleri  
 İlk örnek tarafından başvurulan tablodaki verileri kopyalayan bir dizin oluşturucu oluşturur `ordersds` veri kaynağı için `orders` , 1 Ocak 2015 tarihinde da UTC başlar ve saatte çalışır bir zamanlamaya göre dizin. Her dizin oluşturucu çağrıldığında her toplu iş içinde dizine en fazla 5 öğe başarısız olursa başarılı olur ve toplam listelenecek en fazla 10 öğe başarısız.  

```json
{
    "name" : "myindexer",  
    "description" : "a cool indexer",  
    "dataSourceName" : "ordersds",  
    "targetIndexName" : "orders",  
    "schedule" : { "interval" : "PT1H", "startTime" : "2018-01-01T00:00:00Z" },  
    "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5 }  
}
```

İkinci örnek bir skillset referansı belirttiği bir bilişsel arama işlemi gösterir ve [outputFieldMappings](#output-fieldmappings). [Skillsets](ref-create-skillset.md) üst düzey kaynakları, ayrı olarak tanımlanmış. Bu örnek bir dizin oluşturucu tanımında kısaltmasıdır [bilişsel arama Öğreticisi](cognitive-search-tutorial-blob.md).

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
    {
        "sourceFieldName" : "content",
        "targetFieldName" : "content"
    }
   ],
  "outputFieldMappings" : 
  [
    {
        "sourceFieldName" : "/document/organizations", 
        "targetFieldName" : "organizations"
    },
  ],
  "parameters":
  {
    "maxFailedItems":-1,
    "configuration": 
    {
    "dataToExtract": "contentAndMetadata",
    "imageAction": "generateNormalizedImages"
    }
  }
}
```

## <a name="response"></a>Yanıt  
 Başarılı bir istek için 201 oluşturuldu.  

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Bilişsel arama genel bakış](cognitive-search-concept-intro.md)
+ [Hızlı Başlangıç: Try bilişsel arama](cognitive-search-quickstart-blob.md)
+ [Alanları eşleme](cognitive-search-output-field-mapping.md)
