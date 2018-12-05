---
title: Azure Cosmos DB SQL API Python örnekleri
description: CRUD işlemleri gibi Azure Cosmos DB'deki yaygın görevler için Github'da Python örnekleri bulabilirsiniz.
keywords: python örnekleri
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: python
ms.topic: sample
ms.date: 03/14/2018
ms.author: sngun
ms.openlocfilehash: c7b4fc05fe0aff26f5a49d25345f519b15be590c
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879341"
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

Azure Cosmos DB kaynaklarında CRUD işlemlerini ve diğer yaygın işlemleri gerçekleştiren örnek çözümler, [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python) GitHub deposuna eklenmiştir. Bu makalede aşağıdakiler sunulmaktadır:

* Python örnek proje dosyalarının her birindeki görevlere bağlantılar. 
* İlgili API başvurusu içeriğine bağlantılar.

**Önkoşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [Visual Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, her ay size ücretli Azure hizmetleri için kullanabileceğiniz krediler verir.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Ayrıca [Python SDK](sql-api-sdk-python.md)'ya da ihtiyacınız vardır. 
   
   > [!NOTE]
   > Örnekler birbirinden bağımsızdır; kendi kendine ayarlanır ve sonra kendini temizler. Örneklerde [document_client.CreateCollection](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient#createcollection)'a birden çok çağrı yapılır. Bu her yapıldığında, aboneliğiniz bir saatlik kullanım için faturalandırılır. Azure Cosmos DB faturalandırması hakkında bilgi için bkz. [Azure Cosmos DB Fiyatlandırması](https://azure.microsoft.com/pricing/details/cosmos-db/).
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri
[DatabaseManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement) projesinin [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DatabaseManagement/Program.py) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir:

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı oluşturma](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) |[document_client.CreateDatabase](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#createdatabase) |
| [Kimliğe göre veritabanını okuma](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) |[document_client.ReadDatabase](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readdatabase) |
| [Hesabın veritabanlarını listeleme](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) |[document_client.ReadDatabases](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readdatabases) |
| [Veritabanı silme](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) |[document_client.DeleteDatabase](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#deletedatabase) |

## <a name="collection-examples"></a>Koleksiyon örnekleri
[CollectionManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement) projesinin [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir:

| Görev | API başvurusu |
| --- | --- |
| [Koleksiyon oluşturma](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) |[document_client.CreateCollection](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#createcollection) |
| [Veritabanındaki tüm koleksiyonların listesini okuma](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L209) |[document_client.ReadCollections](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readcollections) |
| [Kimliğe göre koleksiyon alma](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) |[document_client.ReadCollection](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readcollection) |
| [Koleksiyonun aktarım hızını değiştirme](https://github.com/Azure/azure-documentdb-python/blob/master/samples/CollectionManagement/Program.py#L168-L172) | [document_client.ReplaceOffer](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#replaceoffer)|
| [Koleksiyonu silme](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) |[document_client.DeleteCollection](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#deletecollection) |

## <a name="document-examples"></a>Belge örnekleri
[DocumentManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement) projesinin [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir:

| Görev | API başvurusu |
| --- | --- |
| [Belge oluşturma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[document_client.CreateDocument](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#createdocument) |
| [Belge koleksiyonu oluşturma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L55-L66) |[document_client.CreateDocuments](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python) |
| [Kimliğe göre belgeyi okuma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L69-L78) |[document_client.ReadDocument](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readdocument) |
| [Koleksiyondaki tüm belgeleri okuma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/DocumentManagement/Program.py#L81-L92) |[document_client.ReadDocuments](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#readdocuments) |

## <a name="indexing-examples"></a>Dizin örnekleri
[IndexManagement](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement) projesinin [Program.py](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir:

| Görev | API başvurusu |
| --- | --- |
| [Otomatik yerine el ile dizin oluşturmayı kullanma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L245-L246) |[IndexingPolicy.Automatic](https://docs.microsoft.com/python/api/pydocumentdb.documents?view=azure-python) |
| [Belirtilen belge yollarını dizinin dışında tutma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L294-L367) |[IndexingPolicy.ExcludedPaths](https://docs.microsoft.com/python/api/pydocumentdb.documents.indexingdirective?view=azure-python) |
| [Belgeyi dizinin dışında tutma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L204-L210) |[documents.IndexingDirective.Exclude](https://docs.microsoft.com/python/api/pydocumentdb.documents.indexingdirective?view=azure-python) |
| [Dizin modunu ayarlama](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L533) |[documents.IndexingMode](https://docs.microsoft.com/python/api/pydocumentdb.documents.indexingmode?view=azure-python) |
| [Dizelerde aralık dizinlerini kullanma](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L440-L456) |[IndexingPolicy.IncludedPaths](https://docs.microsoft.com/python/api/pydocumentdb.documents.indexingdirective?view=azure-python) |
| [Dizin dönüştürme gerçekleştirme](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L514-L559) |[document_client.ReplaceCollection](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#replacecollection) |

## <a name="query-examples"></a>Sorgu örnekleri
Örnek projelerde aşağıdaki sorgu görevlerinin nasıl gerçekleştirileceği de gösterilir:

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı için bir hesabı sorgulama](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) |[document_client.QueryDatabases](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#querydatabases) |
| [Belgeler için sorgu](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L152-L169) |[document_client.QueryDocuments](https://docs.microsoft.com/python/api/pydocumentdb.document_client.documentclient?view=azure-python#querydocuments) |
| [Karma dizin yolunda bir aralık tarama işlemini zorlama](https://github.com/Azure/azure-documentdb-python/blob/master/samples/IndexManagement/Program.py#L409-L415) |[http_constants.HttpHeaders.EnableScanInQuery](https://docs.microsoft.com/python/api/pydocumentdb.http_constants.httpheaders?view=azure-python#enablescaninquery) |
