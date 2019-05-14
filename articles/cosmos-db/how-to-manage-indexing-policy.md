---
title: Azure Cosmos DB'de dizinleme ilkeleri yönetme
description: Azure Cosmos DB'de dizinleme ilkeleri yönetmeyi öğrenin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/06/2019
ms.author: thweiss
ms.openlocfilehash: 0b47ffd77ee23f997bb7de2ea41f83c2854cba72
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550899"
---
# <a name="manage-indexing-policies-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizinleme ilkeleri yönetme

Azure Cosmos DB'de aşağıdaki veriler dizinlendikten [dizinleme ilkeleri](index-policy.md) her kapsayıcı için tanımlanır. Varsayılan dizinleme ilkesinin yeni oluşturulan kapsayıcılar için aralığı için herhangi bir dize veya sayı ve uzamsal dizinleri herhangi bir GeoJSON nesne türü noktası zorlar. Bu ilkeyi geçersiz kılınabilir:

- Azure portalından
- Azure CLI kullanma
- Sdk'lardan birini kullanarak

Bir [dizin oluşturma ilkesi güncelleştirme](index-policy.md#modifying-the-indexing-policy) bir dizin dönüşümü tetikler. Bu dönüşüm ilerlemesini de Sdk'lardan izlenebilir.

> [!NOTE]
> SDK ve portalı yükseltme işleminin bir parçası olarak, biz size yeni kapsayıcılar için piyasaya sunuluyor yeni bir dizin düzeni ile hizalamak için dizin ilke dönüştürüyoruz. Bu yeni düzen ile tüm basit veri türleri, tam duyarlıklı (-1) aralığı olarak dizinlenir. Bu nedenle, dizin türü ve duyarlık kullanıcıya artık gösterilmez. Gelecekte, kullanıcıların yalnızca yolları includedPaths bölümüne ekleyin ve indexKinds ve duyarlık yoksaymak gerekir. Bu değişiklik, performans üzerinde hiçbir etkisi yoktur ve aynı sözdizimini kullanarak dizin oluşturma ilkesini güncelleştirmeye devam edebilirsiniz. Dizin ilkesini güncelleştirmek için mevcut belgelerimizde tüm örnekleri kullanmaya devam edebilirsiniz.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure Cosmos kapsayıcıları, dizin oluşturma ilkesini Azure portalında doğrudan düzenlemenize olanak sağlayan bir JSON belgesi olarak depolar.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Yeni bir Azure Cosmos hesabı oluşturun veya mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. Tıklayarak **ölçek ve ayarlar**.

1. Dizin oluşturma ilkesi JSON belgesini değiştirme (örnekler [aşağıda](#indexing-policy-examples))

1. Tıklayın **Kaydet** işiniz bittiğinde.

![Azure portalını kullanarak dizin yönetme](./media/how-to-manage-indexing-policy/indexing-policy-portal.png)

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

[Az cosmosdb koleksiyonu güncelleştirme](/cli/azure/cosmosdb/collection#az-cosmosdb-collection-update) komutu Azure clı'dan bir kapsayıcının dizin oluşturma ilkesi JSON tanımı değiştirmenizi sağlar:

```azurecli-interactive
az cosmosdb collection update \
    --resource-group-name $resourceGroupName \
    --name $accountName \
    --db-name $databaseName \
    --collection-name $containerName \
    --indexing-policy "{\"indexingMode\": \"consistent\", \"includedPaths\": [{ \"path\": \"/*\", \"indexes\": [{ \"dataType\": \"String\", \"kind\": \"Range\" }] }], \"excludedPaths\": [{ \"path\": \"/headquarters/employees/?\" } ]}"
```

## <a name="use-the-net-sdk-v2"></a>.NET SDK'sı V2 kullanın

`DocumentCollection` Nesnesinden [.NET SDK'sı v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) (bkz [Bu hızlı başlangıçta](create-sql-api-dotnet.md) kullanımı ile ilgili) sunan bir `IndexingPolicy` değiştirmenize olanak sağlayan bir özellik `IndexingMode` eklemek veya kaldırmak `IncludedPaths` ve `ExcludedPaths`.

```csharp
// retrieve the container's details
ResourceResponse<DocumentCollection> containerResponse = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"));
// set the indexing mode to Consistent
containerResponse.Resource.IndexingPolicy.IndexingMode = IndexingMode.Consistent;
// add an excluded path
containerResponse.Resource.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/headquarters/employees/?" });
// update the container with our changes
await client.ReplaceDocumentCollectionAsync(containerResponse.Resource);
```

Geçişi dizin dönüştürme ilerleme durumunu izlemek için bir `RequestOptions` ayarlar nesnesi `PopulateQuotaInfo` özelliğini `true`.

```csharp
// retrieve the container's details
ResourceResponse<DocumentCollection> container = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("database", "container"), new RequestOptions { PopulateQuotaInfo = true });
// retrieve the index transformation progress from the result
long indexTransformationProgress = container.IndexTransformationProgress;
```

## <a name="use-the-java-sdk"></a>Java kullanma SDK'sı

`DocumentCollection` Nesnesinden [Java SDK'sı](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) (bkz [Bu hızlı başlangıçta](create-sql-api-java.md) kullanımı ile ilgili) sunan `getIndexingPolicy()` ve `setIndexingPolicy()` yöntemleri. `IndexingPolicy` Dizin oluşturma modunu değiştirme ve ekleme sağlar, işleme nesne veya kaldırma dahil ve yolları hariç.

```java
// retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), null);
containerResponse.subscribe(result -> {
    DocumentCollection container = result.getResource();
    IndexingPolicy indexingPolicy = container.getIndexingPolicy();
    // set the indexing mode to Consistent
    indexingPolicy.setIndexingMode(IndexingMode.Consistent);
    Collection<ExcludedPath> excludedPaths = new ArrayList<>();
    ExcludedPath excludedPath = new ExcludedPath();
    excludedPath.setPath("/*");
    // add an excluded path
    excludedPaths.add(excludedPath);
    indexingPolicy.setExcludedPaths(excludedPaths);
    // update the container with our changes
    client.replaceCollection(container, null);
});
```

Bir kapsayıcı dizini dönüştürme ilerleme durumunu izlemek için başarılı bir `RequestOptions` doldurulması için kota bilgisi istekleri nesnesi daha sonra değerini almak `x-ms-documentdb-collection-index-transformation-progress` yanıt üst bilgisi.

```java
// set the RequestOptions object
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPopulateQuotaInfo(true);
// retrieve the container's details
Observable<ResourceResponse<DocumentCollection>> containerResponse = client.readCollection(String.format("/dbs/%s/colls/%s", "database", "container"), requestOptions);
containerResponse.subscribe(result -> {
    // retrieve the index transformation progress from the response headers
    String indexTransformationProgress = result.getResponseHeaders().get("x-ms-documentdb-collection-index-transformation-progress");
});
```

## <a name="use-the-nodejs-sdk"></a>Node.js SDK'sını kullanma

`ContainerDefinition` Alanından arabirim [Node.js SDK'sı](https://www.npmjs.com/package/@azure/cosmos) (bkz [Bu hızlı başlangıçta](create-sql-api-nodejs.md) kullanımı ile ilgili) sunan bir `indexingPolicy` değiştirmenize olanak sağlayan özellik `indexingMode` eklemek veya kaldırmak `includedPaths` ve `excludedPaths`.

```javascript
// retrieve the container's details
const containerResponse = await client.database('database').container('container').read();
// set the indexing mode to Consistent
containerResponse.body.indexingPolicy.indexingMode = "consistent";
// add an excluded path
containerResponse.body.indexingPolicy.excludedPaths.push({ path: '/headquarters/employees/?' });
// update the container with our changes
const replaceResponse = await client.database('database').container('container').replace(containerResponse.body);
```

Bir kapsayıcı dizini dönüştürme ilerleme durumunu izlemek için başarılı bir `RequestOptions` ayarlar nesnesi `populateQuotaInfo` özelliğini `true`, değerinden daha sonra almak `x-ms-documentdb-collection-index-transformation-progress` yanıt üst bilgisi.

```javascript
// retrieve the container's details
const containerResponse = await client.database('database').container('container').read({
    populateQuotaInfo: true
});
// retrieve the index transformation progress from the response headers
const indexTransformationProgress = replaceResponse.headers['x-ms-documentdb-collection-index-transformation-progress'];
```

## <a name="use-the-python-sdk"></a>Python SDK'sını kullanma

Kullanırken [Python SDK'sı](https://pypi.org/project/azure-cosmos/) (bkz [Bu hızlı başlangıçta](create-sql-api-python.md) kullanımı ile ilgili), kapsayıcı yapılandırması bir sözlük olarak yönetilir. Bu sözlükten dizin oluşturma ilkesini ve tüm öznitelikleri erişmek mümkündür.

```python
containerPath = 'dbs/database/colls/collection'
# retrieve the container's details
container = client.ReadContainer(containerPath)
# set the indexing mode to Consistent
container['indexingPolicy']['indexingMode'] = 'consistent'
# add an excluded path
container['indexingPolicy']['excludedPaths'] = [{"path" : "/headquarters/employees/?"}]
# update the container with our changes
response = client.ReplaceContainer(containerPath, container)
```

## <a name="indexing-policy-examples"></a>Dizin oluşturma ilkesi örnekleri

Dizin oluşturma ilkeleri, Azure portalında nasıl açığa kendi JSON biçiminde gösterilen bazı örnekleri aşağıda verilmiştir. Aynı parametrelere, Azure CLI veya herhangi bir SDK ayarlanabilir.

### <a name="opt-out-policy-to-selectively-exclude-some-property-paths"></a>Seçime bağlı olarak bazı özellik yolları hariç tutmak için ilke çevirme
```
    {
        "indexingPolicy": "consistent",
        "includedPaths": [
            {
                "path": "/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    },
                    {
                        "kind": "Spatial",
                        "dataType": "Point"
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/path/to/single/excluded/property/?"
            },
            {
                "path": "/path/to/root/of/multiple/excluded/properties/*"
            }
        ]
    }
```

### <a name="opt-in-policy-to-selectively-include-some-property-paths"></a>Bazı özellik yolları seçerek eklenecek kabul etme İlkesi
```
    {
        "indexingPolicy": "consistent",
        "includedPaths": [
            {
                "path": "/path/to/included/property/?",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    }
                ]
            },
            {
                "path": "/path/to/root/of/multiple/included/properties/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    }
                ]
            }
        ],
        "excludedPaths": [
            {
                "path": "/*"
            }
        ]
    }
```

Not: Genel olarak kullanılması önerilir bir **çevirme** dizin oluşturma ilkesi, Azure Cosmos DB proaktif bir şekilde izin vermek için dizin modelinize eklediğiniz herhangi bir yeni özelliği.

### <a name="using-a-spatial-index-on-a-specific-property-path-only"></a>Belirli özellik yolda yalnızca bir uzamsal dizin kullanma
```
    {
        "indexingPolicy": "consistent",
        "includedPaths": [
            {
                "path": "/*",
                "indexes": [
                    {
                        "kind": "Range",
                        "dataType": "Number"
                    },
                    {
                        "kind": "Range",
                        "dataType": "String"
                    }
                ]
            },
            {
                "path": "/path/to/geojson/property/?",
                "indexes": [
                    {
                        "kind": "Spatial",
                        "dataType": "Point"
                    }
                ]
            }
        ],
        "excludedPaths": []
    }
```

### <a name="excluding-all-property-paths-but-keeping-indexing-active"></a>Tüm özellik yolları hariç, ancak dizin oluşturma etkin tutma

Bu ilke durumlarda kullanılabilir olduğu [özelliği için-yaşam süresi (TTL)](time-to-live.md) etkin, ancak hiçbir ikincil (saf bir anahtar-değer deposu olarak Azure Cosmos DB kullanmak üzere) dizin gereklidir.
```
    {
        "indexingMode": "consistent",
        "includedPaths": [],
        "excludedPaths": [{
            "path": "/*"
        }]
    }
```

### <a name="no-indexing"></a>Dizin oluşturma
```
    {
        "indexingPolicy": "none"
    }
```

## <a name="composite-indexing-policy-examples"></a>Bileşik dizin oluşturma ilkesi örnekleri

İçerir veya dışlar yolları tek tek özellikler için ek olarak, bir bileşik dizin de belirtebilirsiniz. Sahip bir sorgu gerçekleştirmek istiyorsanız bir `ORDER BY` yan tümcesi birden çok özellik için bir [bileşik dizin](index-policy.md#composite-indexes) bu özellikleri gereklidir.

### <a name="composite-index-defined-for-name-asc-age-desc"></a>Bileşik dizin (asc adı, yaşı desc) için tanımlanan:
```
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

Bu bir bileşik dizin aşağıdaki iki sorguları desteklemek olanağına sahip olacaktır:

Sorgu #1:
```sql
    SELECT *
    FROM c
    ORDER BY name asc, age desc    
```

Sorgu #2:
```sql
    SELECT *
    FROM c
    ORDER BY name desc, age asc
```

### <a name="composite-index-defined-for-name-asc-age-asc-and-name-asc-age-desc"></a>Bileşik dizin (asc adı, yaşı asc) için tanımlanan ve (asc adı, yaşı desc):

Birden çok farklı Bileşik dizinler aynı dizin oluşturma ilkesini içinde tanımlayabilirsiniz. 
```
    {  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"ascending"
                }
            ]
            [  
                {  
                    "path":"/name",
                    "order":"ascending"
                },
                {  
                    "path":"/age",
                    "order":"descending"
                }
            ]
        ]
    }
```

### <a name="composite-index-defined-for-name-asc-age-asc"></a>Bileşik dizin (asc adı, yaşı asc) için tanımlanan:

Sırayı belirtmek isteğe bağlıdır. Belirtilmezse, artan sırada.
```
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[  

        ],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/name",
                },
                {  
                    "path":"/age",
                }
            ]
        ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de dizinleme hakkında daha fazla bilgi edinin:

- [Dizin oluşturma genel bakış](index-overview.md)
- [Dizin oluşturma ilkesi](index-policy.md)
