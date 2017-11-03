---
title: "Azure Cosmos DB node.js Örnekleri | Microsoft Docs"
description: "Node.js örnekler github'da Azure Cosmos veritabanı CRUD işlemleri dahil olmak üzere, ortak görevler için bulun."
keywords: "node.js Örnekleri"
services: cosmos-db
author: moderakh
manager: jhubbard
editor: monicar
documentationcenter: nodejs
ms.assetid: d87d97be-47a5-4928-8d46-a541fbb33213
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: moderakh
ms.openlocfilehash: 751ad19e72c7d705639227bd6a44dafab78e41d8
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="azure-cosmos-db-nodejs-examples"></a>Azure Cosmos DB Node.js Örnekleri
> [!div class="op_single_selector"]
> * [.NET örnekleri](documentdb-dotnet-samples.md)
> * [Node.js Örnekleri](documentdb-nodejs-samples.md)
> * [Python örnekleri](documentdb-python-samples.md)
> * [Azure Kod örnek Galerisi](https://azure.microsoft.com/documentation/samples/?service=documentdb&ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
> 
> 

CRUD işlemleri ve Azure Cosmos DB kaynaklardaki ortak diğer işlemleri gerçekleştirmek örnek çözümleri dahil edilmiştir [azure documentdb nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) GitHub depo. Bu makalede aşağıdakiler sunulmaktadır:

* Her Node.js örnek proje dosyalarının görevlere bağlantılar.
* Bağlantılar ilgili API'ye içerik başvuru.

**Önkoşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- Yapabilecekleriniz [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): bilgisayarınızı Visual Studio abonelik size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Ayrıca gerekir [Node.js SDK'sı](documentdb-sdk-node.md).
   
   > [!NOTE]
   > Her örnek kendi içinde bulunan, kendisini ayarlayan ve kendisini sonra temizler. Bu nedenle, örnekler için birden fazla çağrı sorun [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection). Bu, aboneliğinizin yapılır her zaman 1 saat performans katmanı oluşturulan koleksiyonunun başına kullanım için faturalandırılır.
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri
[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) dosyasının [DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) proje, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.

| Görev | API başvurusu |
| --- | --- |
| [Bir veritabanı oluşturun](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) |[DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase) |
| [Sorgu bir veritabanı için bir hesap](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) |[DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases) |
| [Bir veritabanı kimliğiyle okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) |[DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase) |
| [Bir hesap için veritabanlarını Listele](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) |[DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabases) |
| [Veritabanı silme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) |[DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase) |

## <a name="collection-examples"></a>Koleksiyon örnekleri
[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) dosyasının [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) proje, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.

| Görev | API başvurusu |
| --- | --- |
| [Koleksiyon oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) |[DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection) |
| [Veritabanındaki tüm koleksiyonlar listesini okur](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) |[DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections) |
| [Bir koleksiyon tarafından _self Al](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) |[DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection) |
| [Bir koleksiyon kimliğine göre alma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) |[DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection) |
| [Bir koleksiyonun performans katmanı Al](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) |[DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers) |
| [Bir koleksiyonun performans katmanı değiştirme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) |[DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer) |
| [Bir koleksiyonu silin](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) |[DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection) |

## <a name="document-examples"></a>Belge örnekleri
[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) dosyasının [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) proje, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.

| Görev | API başvurusu |
| --- | --- |
| [Belgeleri oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) |[DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument) |
| [Bir koleksiyon için akış belgeyi okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) |[DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument) |
| [Kimliğe göre bir belgeyi okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) |[DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument) |
| [Yalnızca belge değiştiyse belgeyi okuma](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) |[DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions) |
| [Belgeler için sorgu](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) |[DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments) |
| [Bir belgeyi değiştirme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |[DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument) |
| [Koşullu ETag denetimi ile belgeyi değiştirme](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |[DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions) |
| [Bir belgeyi silme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) |[DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument) |

## <a name="indexing-examples"></a>Dizin oluşturma örnekleri
[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) dosyasının [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) proje, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.

| Görev | API başvurusu |
| --- | --- |
| [Varsayılan dizin ile bir koleksiyon oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L657-L701) |[DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection) |
| [El ile belirli bir belge dizini](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) |[RequestOptions.indexingDirective: 'include'](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions) |
| [El ile belirli bir belgeyi dizinden dışlayın](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) |[RequestOptions.indexingDirective: 'Dışla'](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions) |
| [Yavaş toplu alma için dizin oluşturma kullanır veya ağır koleksiyonları okuma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) |[IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode) |
| [Dizin oluşturma bir belgenin belirli yollar içerir](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) |[IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy) |
| [Dizin oluşturma gelen belirli yollar Dışla](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) |[IndexingPolicy.ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy) |
| [Bir tarama bir aralık işlemi sırasında dize yola izin ver](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347) |[FeedOptions.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions) |
| [Dize yola aralığı dizini oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) |[IndexKind.Range](http://azure.github.io/azure-documentdb-node/global.html#IndexKind), [IndexingPolicy](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy), [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument) |
| [Varsayılan indexPolicy bir koleksiyon oluşturun, sonra bu çevrimiçi güncelleştirme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) |[DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html) |

Dizin oluşturma hakkında daha fazla bilgi için bkz: [Azure Cosmos ilkeleri dizin DB](indexing-policies.md).

## <a name="server-side-programming-examples"></a>Sunucu tarafı programlama örnekleri
[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) dosyasının [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) proje, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) |[DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure) |
| [Bir saklı yordamı yürütme](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) |[DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure) |

Sunucu tarafı programlama hakkında daha fazla bilgi için bkz: [Azure Cosmos DB sunucu tarafı programlama: saklı yordamlar, veritabanı tetikleyiciler ve UDF'lerin](programming.md).

## <a name="partitioning-examples"></a>Bölümleme örnekleri
[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) dosyasının [bölümleme](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) proje, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.

| Görev | API başvurusu |
| --- | --- |
| [Bir HashPartitionResolver kullanın](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) |[HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html) |

Azure Cosmos veritabanı veri bölümlendirme hakkında daha fazla bilgi için bkz: [Azure Cosmos DB bölüm ve ölçek verileri](partition-data.md).

