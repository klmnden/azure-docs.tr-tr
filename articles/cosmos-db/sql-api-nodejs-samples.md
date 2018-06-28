---
title: Azure Cosmos DB için Node.js örnekleri | Microsoft Docs
description: CRUD işlemleri gibi Azure Cosmos DB'deki yaygın görevler için Github'da Node.js örnekleri bulabilirsiniz.
keywords: node.js örnekleri
services: cosmos-db
author: moderakh
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: sample
ms.date: 05/23/2017
ms.author: moderakh
ms.openlocfilehash: a4bda45f9a620d70c78b7b4142cd5f152bd5436d
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960398"
---
# <a name="azure-cosmos-db-nodejs-examples"></a>Azure Cosmos DB Node.js örnekleri
> [!div class="op_single_selector"]
> * [.NET Örnekleri](sql-api-dotnet-samples.md)
> * [Java Örnekleri](sql-api-java-samples.md)
> * [Async Java Örnekleri](sql-api-async-java-samples.md)
> * [Node.js Örnekleri](sql-api-nodejs-samples.md)
> * [Python Örnekleri](sql-api-python-samples.md)
> * [Azure Kod Örneği Galerisi](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Azure Cosmos DB kaynaklarında CRUD işlemlerini ve diğer yaygın işlemleri gerçekleştiren örnek çözümler, [azure-documentdb-nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) GitHub deposuna eklenmiştir. Bu makalede aşağıdakiler sunulmaktadır:

* Node.js örnek proje dosyalarının her birindeki görevlere bağlantılar.
* İlgili API başvurusu içeriğine bağlantılar.

**Önkoşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [Visual Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, her ay size ücretli Azure hizmetleri için kullanabileceğiniz krediler verir.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Ayrıca [Node.js SDK'sı](sql-api-sdk-node.md) da gerekir.
   
   > [!NOTE]
   > Örnekler birbirinden bağımsızdır, kendi kendine ayarlanır ve sonra kendini temizler. Böyle olunca, örneklerde [DocumentClient.createCollection](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createcollection-databaselink--body--options--callback-
)'a birden çok çağrı yapılır. Bu her yapıldığında, oluşturulan koleksiyonun performans katmanı başına aboneliğiniz 1 saatlik kullanım için faturalandırılır.
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri
[DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) projesinin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir.

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) |[DocumentClient.createDatabase](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createdatabase-body--options--callback-) |
| [Veritabanı için bir hesabı sorgulama](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) |[DocumentClient.queryDatabases](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#querydatabases-query--options-) |
| [Kimliğe göre veritabanını okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) |[DocumentClient.readDatabase](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdatabase-databaselink--options--callback-) |
| [Hesabın veritabanlarını listeleme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) |[DocumentClient.readDatabases](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdatabases-options-) |
| [Veritabanı silme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) |[DocumentClient.deleteDatabase](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#deletedatabase-databaselink--options--callback-) |

## <a name="collection-examples"></a>Koleksiyon örnekleri
[CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) projesinin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir.

| Görev | API başvurusu |
| --- | --- |
| [Koleksiyon oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) |[DocumentClient.createCollection](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createcollection-databaselink--body--options--callback-) |
| [Veritabanındaki tüm koleksiyonların listesini okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) |[DocumentClient.readCollections](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readcollections-databaselink--options-) |
| [_self'e göre koleksiyon alma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) |[DocumentClient.readCollection](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readcollection-collectionlink--options--callback-) |
| [Kimliğe göre koleksiyon alma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) |[DocumentClient.readCollection](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readcollection-collectionlink--options--callback-) |
| [Koleksiyonun performans katmanını alma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) |[DocumentQueryable.queryOffers](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#queryoffers-query--options-) |
| [Koleksiyonun performans katmanını değiştirme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) |[DocumentClient.replaceOffer](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#replaceoffer-offerlink--offer--callback-) |
| [Koleksiyonu silme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) |[DocumentClient.deleteCollection](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#deletecollection-collectionlink--options--callback-) |

## <a name="document-examples"></a>Belge örnekleri
[DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) projesinin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir.

| Görev | API başvurusu |
| --- | --- |
| [Belgeleri oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) |[DocumentClient.createDocument](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createdocument-documentsfeedordatabaselink--body--options--callback-) |
| [Koleksiyon için belge akışını okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) |[DocumentClient.readDocument](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdocument-documentlink--options--callback-) |
| [Kimliğe göre belgeyi okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) |[DocumentClient.readDocument](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdocument-documentlink--options--callback-) |
| [Yalnızca belge değiştiğinde belgeyi okuma](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) |[DocumentClient.readDocument](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdocument-documentlink--options--callback-)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.client.requestoptions.accesscondition?view=azure-dotnet#Microsoft_Azure_Documents_Client_RequestOptions_AccessCondition) |
| [Belgeler için sorgu](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) |[DocumentClient.queryDocuments](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#querydocuments-documentsfeedordatabaselink--query--options-) |
| [Belgeyi değiştirme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |[DocumentClient.replaceDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#replacedocument-documentlink--document--options--callback-) |
| [Koşullu ETag denetimiyle belgeyi değiştirme](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |[DocumentClient.replaceDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#replacedocument-documentlink--document--options--callback-)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.client.requestoptions.accesscondition?view=azure-dotnet#Microsoft_Azure_Documents_Client_RequestOptions_AccessCondition) |
| [Belgeyi silme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) |[DocumentClient.deleteDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#deletedocument-documentlink--options--callback-) |

## <a name="indexing-examples"></a>Dizin örnekleri
[IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) projesinin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir.

| Görev | API başvurusu |
| --- | --- |
| [Varsayılan dizinle bir koleksiyon oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L657-L701) |[DocumentClient.createCollection](https://docs.microsoft.com/en-us/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createcollection-databaselink--body--options--callback-) |
| [Belirli bir belgeyi el ile dizine alma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) |[RequestOptions.indexingDirective: 'include'](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective?view=azure-dotnet#Microsoft_Azure_Documents_Client_RequestOptions_IndexingDirective) |
| [Belirli bir belgeyi el ile dizinin dışında bırakma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) |[RequestOptions.indexingDirective: 'exclude'](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective?view=azure-dotnet#Microsoft_Azure_Documents_Client_RequestOptions_IndexingDirective) |
| [Toplu içeri aktarma için gecikmeli dizine alma kullanma veya büyük koleksiyonları okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) |[IndexingMode.Lazy](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.indexingmode?view=azure-dotnet) |
| [Dizin oluştururken belgenin belirli yollarını ekleme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) |[IndexingPolicy.IncludedPaths](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.indexingpolicy.includedpaths?view=azure-dotnet#Microsoft_Azure_Documents_IndexingPolicy_IncludedPaths) |
| [Belirli yolları dizinin dışında tutma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) |[IndexingPolicy.ExcludedPath](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.indexingpolicy.excludedpaths?view=azure-dotnet#Microsoft_Azure_Documents_IndexingPolicy_ExcludedPaths) |
| [Aralık işlemi sırasında bir dize yolunda taramaya izin verme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347) |[FeedOptions.EnableScanInQuery](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.client.feedoptions.enablescaninquery?view=azure-dotnet#Microsoft_Azure_Documents_Client_FeedOptions_EnableScanInQuery) |
| [Dize yolunda bir aralık dizini oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) |[IndexKind.Range](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.indexkind?view=azure-dotnet#Microsoft_Azure_Documents_IndexKind_Range), [IndexingPolicy](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.documents.indexingpolicy?view=azure-dotnet), [DocumentClient.queryDocument](https://docs.microsoft.com/en-us/javascript/api/documentdb/documentclient?view=azure-node-latest) |
| [Varsayılan indexPolicy ile koleksiyon oluşturma, ardından bunu çevrimiçi güncelleştirme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) |[DocumentClient.createCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createcollection-databaselink--body--options--callback-)<br> [DocumentClient.replaceCollection#replaceCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#replacecollection-collectionlink--collection--options--callback-) |

Dizin oluşturma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB dizin oluşturma ilkeleri](indexing-policies.md).

## <a name="server-side-programming-examples"></a>Sunucu tarafı programlama örnekleri
[ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) projesinin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir.

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) |[DocumentClient.createStoredProcedure](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createstoredprocedure-collectionlink--sproc--options--callback-) |
| [Saklı yordamı yürütme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) |[DocumentClient.executeStoredProcedure](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#executestoredprocedure-sproclink--params--options--callback-) |

Sunucu tarafı programlaması hakkında daha fazla bilgi için bkz. [Azure Cosmos DB sunucu tarafı programlaması: Saklı yordamlar, veritabanı tetikleyicileri ve UDF'leri](programming.md).

## <a name="partitioning-examples"></a>Bölümleme örnekleri
[Partitioning](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) projesinin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir.

| Görev | API başvurusu |
| --- | --- |
| [HashPartitionResolver kullanma](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) |[HashPartitionResolver](https://docs.microsoft.com/javascript/api/documentdb/HashPartitionResolver?view=azure-node-latest) |

Azure Cosmos DB'de bölümleme verileri hakkında daha fazla bilgi için bkz. [Azure Cosmos DB'de verileri bölümleme ve ölçeklendirme](partition-data.md).

