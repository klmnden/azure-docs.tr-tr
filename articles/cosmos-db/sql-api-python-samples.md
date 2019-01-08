---
title: Azure Cosmos DB SQL API Python örnekleri
description: Python örnekler, CRUD işlemleri dahil olmak üzere Azure Cosmos DB'de ortak görevler için Github'da bulabilirsiniz.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: sample
ms.date: 03/14/2018
ms.author: sngun
ms.openlocfilehash: 87c1157a81c20f3da0847909954b529c2b6bbcc0
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076476"
---
# <a name="azure-cosmos-db-python-examples"></a>Azure Cosmos DB Python örnekleri

> [!div class="op_single_selector"]
> * [.NET Örnekleri](sql-api-dotnet-samples.md)
> * [Java Örnekleri](sql-api-java-samples.md)
> * [Async Java Örnekleri](sql-api-async-java-samples.md)
> * [Node.js Örnekleri](sql-api-nodejs-samples.md)
> * [Python Örnekleri](sql-api-python-samples.md)
> * [Azure Kod Örneği Galerisi](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

CRUD işlemleri ve diğer yaygın işlemler Azure Cosmos DB kaynaklarını üzerinde örnek çözümler dahil edilecek [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python) GitHub deposu. Bu makalede aşağıdakiler sunulmaktadır:

* Python örnek proje dosyalarının her birindeki görevlere bağlantılar. 
* İlgili API başvurusu içeriğine bağlantılar.

**Önkoşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- Yapabilecekleriniz [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay krediler sunar.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Ayrıca [Python SDK](sql-api-sdk-python.md)'ya da ihtiyacınız vardır. 
   
   > [!NOTE]
   > Örnekler birbirinden bağımsızdır; kendi kendine ayarlanır ve sonra kendini temizler. Birden çok çağrı örnekleri sorun [CosmosClient.CreateContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createcontainer-database-link--collection--options-none-). Bu her yapıldığında, aboneliğiniz bir saatlik kullanım için faturalandırılır. Azure Cosmos DB faturalandırması hakkında bilgi için bkz. [Azure Cosmos DB Fiyatlandırması](https://azure.microsoft.com/pricing/details/cosmos-db/).
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri
[Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py) dosya [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement) proje, aşağıdaki görevlerin nasıl yapılacağını gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı oluşturma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L65-L76) |[CosmosClient.CreateDatabase](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createdatabase-database--options-none-) |
| [Kimliğe göre veritabanını okuma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L79-L96) |[CosmosClient.ReadDatabase](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readdatabase-database-link--options-none-) |
| [Hesabın veritabanlarını listeleme](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L99-L110) |[CosmosClient.ReadDatabases](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readdatabases-options-none-) |
| [Veritabanı silme](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L113-L126) |[CosmosClient.DeleteDatabase](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#deletedatabase-database-link--options-none-) |

## <a name="collection-examples"></a>Koleksiyon örnekleri
[Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py) dosya [CollectionManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement) proje, aşağıdaki görevlerin nasıl yapılacağını gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Koleksiyon oluşturma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L84-L135) |[CosmosClient.CreateContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createcontainer-database-link--collection--options-none-) |
| [Veritabanındaki tüm koleksiyonların listesini okuma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L210-L222) |[CosmosClient.ReadContainers](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readcontainers-database-link--options-none-) |
| [Kimliğe göre koleksiyon alma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L190-L208) |[CosmosClient.ReadContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readcontainer-collection-link--options-none-) |
| [Koleksiyonun aktarım hızını değiştirme](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L184-L188) | [CosmosClient.ReplaceOffer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#replaceoffer-offer-link--offer-)|
| [Koleksiyonu silme](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L224-L238) |[CosmosClient.DeleteContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#deletecontainer-collection-link--options-none-) |

## <a name="document-examples"></a>Belge örnekleri
[Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py) dosya [DocumentManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement) proje, aşağıdaki görevlerin nasıl yapılacağını gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Belge oluşturma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[CosmosClient.CreateItem](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createitem-database-or-container-link--document--options-none-) |
| [Belge koleksiyonu oluşturma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[CosmosClient.CreateItem](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#createitem-database-or-container-link--document--options-none-) |
| [Kimliğe göre belgeyi okuma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L69-L78) |[CosmosClient.ReadItem](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readitem-document-link--options-none-) |
| [Koleksiyondaki tüm belgeleri okuma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L81-L92) |[CosmosClient.ReadItems](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#readitems-collection-link--feed-options-none-) |
| [Koşullu ETag denetimiyle belgeyi değiştirme](https://github.com/Azure/azure-cosmos-python/blob/a21f6fb4bad3f59909ef43558b598f9fb476b7bc/test/crud_tests.py#L1216-L1218) | [CosmosClient.ReplaceItem](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#replaceitem-document-link--new-document--options-none-) |

## <a name="indexing-examples"></a>Dizin örnekleri
[Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py) dosya [IndexManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement) proje, aşağıdaki görevlerin nasıl yapılacağını gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Otomatik yerine el ile dizin oluşturmayı kullanma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L245-L246) |[IndexingPolicy.Automatic](https://docs.microsoft.com/python/api/pydocumentdb.documents?view=azure-python) |
| [Belirtilen belge yollarını dizinin dışında tutma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L294-L367) |[IndexingPolicy.ExcludedPaths](https://docs.microsoft.com/python/api/pydocumentdb.documents.indexingdirective?view=azure-python) |
| [Belgeyi dizinin dışında tutma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L204-L210) |[IndexingDirective.Exclude](/python/api/azure-cosmos/azure.cosmos.documents.indexingdirective#exclude) |
| [Dizin modunu ayarlama](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L533) |[IndexingMode](/python/api/azure-cosmos/azure.cosmos.documents.indexingmode) |
| [Dizelerde aralık dizinlerini kullanma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L440-L456) |[IndexingPolicy.IncludedPaths](https://docs.microsoft.com/python/api/pydocumentdb.documents.indexingdirective?view=azure-python) |
| [Dizin dönüştürme gerçekleştirme](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L514-L559) |[CosmosClient.ReplaceContainer](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#replacecontainer-collection-link--collection--options-none-) |

## <a name="query-examples"></a>Sorgu örnekleri
Örnek projelerine ayrıca aşağıdaki sorgu görevlerin nasıl yapılacağını göster:

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı için bir hesabı sorgulama](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py#L49-L62) |[CosmosClient.QueryDatabases](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#querydatabases-query--options-none-) |
| [Belgeler için sorgu](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L152-L169) |[CosmosClient.QueryItems](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient#queryitems-database-or-container-link--query--options-none--partition-key-none-) |
| [Karma dizin yolunda bir aralık tarama işlemini zorlama](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L409-L415) |[HttpHeaders.EnableScanInQuery](/python/api/azure-cosmos/azure.cosmos.http_constants.httpheaders#enablescaninquery) |

