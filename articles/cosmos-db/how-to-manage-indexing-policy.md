---
title: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
description: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: c27cee4842c0e65e1737f100a215cff82a0fd439
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54033103"
---
# <a name="manage-indexing-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizinleme yönetme

Azure Cosmos DB içinde bir kapsayıcı veya tüm öğeleri otomatik olarak dizinini isteyip istemediğinizi seçebilirsiniz. Varsayılan olarak, bir Azure Cosmos kapsayıcısındaki tüm öğeleri otomatik olarak dizine alınır, ancak otomatik dizin oluşturma devre dışı kapatabilirsiniz. Aracılığıyla dizin oluşturma devre dışı bırakıldığında öğelerine erişilebilir kendi kendine bağlantılar veya belge kimliği gibi öğesi Kimliğini kullanarak sorgular. Açıkça geçirerek dizini kullanmadan sonuçlar verecek isteyebilir **x-ms-documentdb-enable-tarama** üst bilgisinde REST API veya **EnableScanInQuery** seçeneği kullanarak istek. NET SDK.

Otomatik kapalı dizin oluşturma ile belirli öğeleri dizine seçmeli olarak yine de ekleyebilirsiniz. Buna karşılık, açık otomatik dizin oluşturma bırakın ve belirli öğelerini hariç tutmak seçime bağlı olarak seçin. Açık/kapalı yapılandırmaları dizin yararlı Sorgulanacak gereken öğeler bir alt kümesi olduğunda.  

Aktarım hızı ve istek birimlerinin orantılı tarafından dizin oluşturma ilkesini dahil kümesinde belirtilen dizine gereken değer sayısını yazın. Sorgu desenleri daha iyi bir anlayışa sahip, açıkça yazma aktarım hızını iyileştirmek için yollar ekleme/çıkarma kümesini seçebilirsiniz.

## <a name="manage-indexing-using-azure-portal"></a>Azure portalını kullanarak dizin yönetme

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Yeni bir Azure Cosmos hesabı oluşturun veya mevcut bir hesabı seçin.

3. Açık **Veri Gezgini** bölmesi.

4. Var olan bir kapsayıcı seçin, genişletin ve aşağıdaki değerleri değiştirin:

   * Açık **ölçek ve ayarlar** penceresi.
   * Değişiklik **indexingMode** gelen *tutarlı* için *hiçbiri* ya da belirli yollar dizin gelen içerme/dışlama.
   * Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

   ![Azure portalını kullanarak dizin yönetme](./media/how-to-manage-indexing/how-to-manage-indexing-portal.png)

## <a name="manage-indexing-using-azure-sdks"></a>Azure SDK'larını kullanarak dizin yönetme

### <a id="dotnet"></a>.NET SDK

Aşağıdaki örneği kullanarak bir öğeyi açıkça içerecek şekilde gösterilmektedir [SQL API .NET SDK'sı](sql-api-sdk-dotnet.md) ve [RequestOptions.IndexingDirective](/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective) özelliği.

```csharp
// To override the default behavior to exclude (or include) a document in indexing,
// use the RequestOptions.IndexingDirective property.
client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new { id = "myDocumentId", isRegistered = true },
    new RequestOptions { IndexingDirective = IndexingDirective.Include });
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de dizinleme hakkında daha fazla bilgi edinin:

* [Dizin oluşturma genel bakış](index-overview.md)
* [Dizin oluşturma ilkesi](index-policy.md)
* [Dizin türleri](index-types.md)
* [Dizin yolları](index-paths.md)
