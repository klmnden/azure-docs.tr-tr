---
title: "Azure Cosmos DB: SQL API'si için .NET örnekleri"
description: Bulma C# Azure Cosmos DB SQL CRUD işlemleri dahil olmak üzere API'sini kullanarak ortak görevleri için github'da .NET örnekleri.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 04/04/2019
ms.author: sngun
ms.openlocfilehash: e26252c57e6f482b619b2a496269f87be4889c83
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754428"
---
# <a name="azure-cosmos-db-net-examples-for-the-sql-api"></a>Azure Cosmos DB: SQL API'si için .NET örnekleri
> [!div class="op_single_selector"]
> * [.NET Örnekleri](sql-api-dotnet-samples.md)
> * [Java Örnekleri](sql-api-java-samples.md)
> * [Async Java Örnekleri](sql-api-async-java-samples.md)
> * [Node.js Örnekleri](sql-api-nodejs-samples.md)
> * [Python Örnekleri](sql-api-python-samples.md)
> * [Azure Kod Örneği Galerisi](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

[Azure-cosmos-dotnet-v2](https://github.com/Azure/azure-cosmos-dotnet-v2/tree/master/samples/code-samples) GitHub deposu CRUD ve Azure Cosmos DB kaynaklarını ortak diğer işlemleri gerçekleştirmek için en son .NET örnek çözümler sunar. Bu makalede aşağıdakiler sunulmaktadır:

* Örnek C# proje dosyalarının her birindeki görevlere bağlantılar. 
* İlgili API başvurusu içeriğine bağlantılar.

.NET SDK'sı sürüm 3.0 (Önizleme) kod örnekleri için en son örneklere bakın [azure-cosmos-dotnet-v3](https://github.com/Azure/azure-cosmos-dotnet-v3) GitHub deposu. 

## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2019 yüklü Azure geliştirme iş akışı ile
- İndirip kullanabilirsiniz **ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/). Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun. 

[Microsoft.Azure.DocumentDB NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) 

Bir Azure aboneliği veya ücretsiz Cosmos DB deneme hesabı
- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
  
- Yapabilecekleriniz [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay krediler sunar.
- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

> [!NOTE]
> Örnekleri kendi içinde bulunan ve ayarlama ve temizleme kendilerini birden çok çağrı kümesi [CreateDocumentCollectionAsync()](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync). Her bir saatlik kullanım koleksiyonunuzun performans katmanındaki aboneliğinizi düzenler. 
> 

## <a name="database-examples"></a>Veritabanı örnekleri
[RunDatabaseDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L72-L121) örnek yöntemi *DatabaseManagement* proje, aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos veritabanları hakkında bilgi edinmek için [veritabanları, kapsayıcılar ve öğeleri ile çalışma](databases-containers-items.md). 

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı oluşturma](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L90) |[DocumentClient.CreateDatabaseAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdatabaseasync) |
| [Veritabanı sorgulama](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L81) |[DocumentQueryable.CreateDatabaseQuery](/previous-versions/azure/dn850286(v=azure.100)) |
| [Kimliğe göre veritabanını okuma](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L102) |[DocumentClient.ReadDatabaseAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdatabaseasync) |
| [Tüm veritabanlarını okuma](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L108-L113) |[DocumentClient.ReadDatabaseFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdatabasefeedasync) |
| [Veritabanı silme](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L118) |[DocumentClient.DeleteDatabaseAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.deletedatabaseasync) |

## <a name="collection-examples"></a>Koleksiyon örnekleri
[RunCollectionDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/CollectionManagement/Program.cs#L96-L185) örnek yöntemi *CollectionManagement* proje, aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos Koleksiyonlar hakkında bilgi edinmek için [veritabanları, kapsayıcılar ve öğeleri ile çalışma](databases-containers-items.md). 

| Görev | API başvurusu |
| --- | --- |
| [Koleksiyon oluşturma](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L101) |[DocumentClient.CreateDocumentCollectionAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync) |
| [Bir koleksiyonun yapılandırılan performansını alma](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L198) |[DocumentQueryable.CreateOfferQuery](/previous-versions/azure/dn975114(v=azure.100)) |
| [Bir koleksiyonun yapılandırılan performansını değiştirme](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L207) |[DocumentClient.ReplaceOfferAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync) |
| [Kimliğe göre koleksiyon alma](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L153) |[DocumentClient.ReadDocumentCollectionAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync) |
| [Veritabanındaki tüm koleksiyonları okuma](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L162) |[DocumentClient.ReadDocumentCollectionFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentcollectionfeedasync) |
| [Koleksiyonu silme](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L175) |[DocumentClient.DeleteDocumentCollectionAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentcollectionasync) |

## <a name="document-examples"></a>Belge örnekleri
[RunDocumentsDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L97-L102) örnek yöntemi *DocumentManagement* proje, aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos belgeler hakkında bilgi edinmek için [veritabanları, kapsayıcılar ve öğeleri ile çalışma](databases-containers-items.md). 

| Görev | API başvurusu |
| --- | --- |
| [Belge oluşturma](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L198) |[DocumentClient.CreateDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync) |
| [Kimliğe göre belgeyi okuma](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L211) |[DocumentClient.ReadDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync) |
| [Koleksiyondaki tüm belgeleri okuma](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L227) |[DocumentClient.ReadDocumentFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync) |
| [Belgeler için sorgu](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L248-L251) |[DocumentClient.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [Belgeyi değiştirme](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L263) |[DocumentClient.ReplaceDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync) |
| [Belgeyi upsert etme](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L300) |[DocumentClient.UpsertDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync) |
| [Belgeyi silme](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L322) |[DocumentClient.DeleteDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync) |
| [.NET dinamik nesneleri ile çalışma](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L331-L380) |[DocumentClient.CreateDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync)<br>[DocumentClient.ReadDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync)<br>[DocumentClient.ReplaceDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync) |
| [Koşullu ETag denetimiyle belgeyi değiştirme](https://github.com/Azure/azure-documentdb-dotnet/blob/f2b11dec45a195ddeed333560ebba63055f5ed09/samples/code-samples/DocumentManagement/Program.cs#L398-L440) |[DocumentClient.AccessCondition](/dotnet/api/microsoft.azure.documents.client.accesscondition)<br>[Documents.Client.AccessConditionType](/dotnet/api/microsoft.azure.documents.client.accessconditiontype) |
| [Yalnızca belge değiştiğinde belgeyi okuma](https://github.com/Azure/azure-documentdb-dotnet/blob/f2b11dec45a195ddeed333560ebba63055f5ed09/samples/code-samples/DocumentManagement/Program.cs#L442-L470) |[DocumentClient.AccessCondition](/dotnet/api/microsoft.azure.documents.client.accesscondition)<br>[Documents.Client.AccessConditionType](/dotnet/api/microsoft.azure.documents.client.accessconditiontype) |

## <a name="indexing-examples"></a>Dizin oluşturma örnekleri
[RunIndexDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/ea8c977b9c2f37ddc2894911ec239907ab60e40a/samples/code-samples/IndexManagement/Program.cs#L89-L117) örnek yöntemi *IndexManagement* proje, aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos DB'de dizinleme hakkında bilgi edinmek için [dizin ilkeleri](index-policy.md), [dizin türleri](index-types.md), ve [dizin yolları](index-paths.md). 

| Görev | API başvurusu |
| --- | --- |
| [Belgeyi dizinin dışında tutma](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L125-L163) |[IndexingDirective.Exclude](/dotnet/api/microsoft.azure.documents.indexingdirective) |
| [Otomatik yerine el ile dizin oluşturmayı kullanma](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L171-L209) |[IndexingPolicy.Automatic](/dotnet/api/microsoft.azure.documents.indexingpolicy.automatic) |
| [Belirtilen belge yollarını dizinin dışında tutma](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L248-L297) |[IndexingPolicy.ExcludedPaths](/dotnet/api/microsoft.azure.documents.indexingpolicy.excludedpaths) |
| [Karma dizin yolunda bir aralık tarama işlemini zorlama](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L305-L340) |[FeedOptions.EnableScanInQuery](/dotnet/api/microsoft.azure.documents.client.feedoptions.enablescaninquery) |
| [Dizelerde aralık dizinlerini kullanma](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L342-L405) |[IndexingPolicy.IncludedPaths](/dotnet/api/microsoft.azure.documents.indexingpolicy.includedpaths)<br>[RangeIndex](/dotnet/api/microsoft.azure.documents.rangeindex) |
| [Dizin dönüştürme gerçekleştirme](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L407-L464) |[ReplaceDocumentCollectionAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentcollectionasync) |

## <a name="geospatial-examples"></a>Jeo-uzamsal örnekler
Jeo-uzamsal örnek dosyasında ([azure-documentdb-dotnet/samples/code-samples/Geospatial/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs)) aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilmektedir.  Aşağıdaki örneği çalıştırmadan önce GeoJSON ve Jeo-uzamsal veriler hakkında bilgi edinmek için [Jeo-uzamsal ve GeoJSON konum verileri](geospatial.md). 

| Görev | API başvurusu |
| --- | --- |
| [Yeni bir koleksiyonda jeo-uzamsal dizin oluşturmayı etkinleştirme](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L45-L63) |[IndexingPolicy](/dotnet/api/microsoft.azure.documents.indexingpolicy) <br> [IndexKind.Spatial](/dotnet/api/microsoft.azure.documents.indexkind) <br>[DataType.Point](/dotnet/api/microsoft.azure.documents.datatype) |
| [GeoJSON noktaları ile belgeleri ekleme](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L116-L126) |[DocumentClient.CreateDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync) </br> [DataType.Point](/dotnet/api/microsoft.azure.documents.datatype) |
| [Belirli bir uzaklık içinde noktaları bulma](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L152-L194) |[ST_DISTANCE](how-to-sql-query.md#spatial-functions) </br> [GeometryOperationExtensions.Distance](/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.distance) |
| [Poligon içindeki noktaları bulma](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L196-L221) |[ST_WITHIN](how-to-sql-query.md#spatial-functions) </br> [GeometryOperationExtensions.Within](/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.distance) </br>[Poligon](/dotnet/api/microsoft.azure.documents.spatial.polygon) |
| [Mevcut bir koleksiyonda jeo-uzamsal dizin oluşturmayı etkinleştirme](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L312-L336) |[DocumentClient.ReplaceDocumentCollectionAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentcollectionasync)<br>[DocumentCollection.IndexingPolicy](/dotnet/api/microsoft.azure.documents.documentcollection.indexingpolicy) |
| [Nokta ve poligon verilerini doğrulama](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L223-L265) |[ST_ISVALID](how-to-sql-query.md#spatial-functions)<br>[ST_ISVALIDDETAILED](how-to-sql-query.md#spatial-functions)<br>[GeometryOperationExtensions.IsValid](/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.isvalid)<br>[GeometryOperationExtensions.IsValidDetailed](/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.isvaliddetailed) |

## <a name="query-examples"></a>Sorgu örnekleri
Sorgu belgesi dosyası [azure-documentdb-dotnet/samples/code-samples/Queries/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs), her SQL sorgu dil bilgisi, LINQ sağlayıcısı, sorgu ve Lambda kullanarak aşağıdaki görevleri bunun nasıl yapılacağını göstermektedir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos DB'de SQL sorgu başvurusu hakkında bilgi edinmek için [SQL sorgu örnekleri için Azure Cosmos DB](how-to-sql-query.md). 

| Görev | API başvurusu |
| --- | --- |
| [Tüm belgeler için sorgu](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L122-L138) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [== kullanan eşitlik için sorgu](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L251-L268) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [!= ve NOT kullanan eşitsizlik için sorgu](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L270-L276) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [>, <, >=, <= gibi aralık işleçlerini kullanarak sorgulama](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L305-L325) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [Dizelere karşı aralık işleçlerini kullanarak sorgulama](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L337-L346) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [ORDER BY ile sorgu](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L370-L392) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [Toplama işlevleri ile sorgulama](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/samples/code-samples/Queries/Program.cs#L448-L496) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [alt belgelerle çalışma](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L394-L419) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [Belgeler arası Birleştirmeler ile sorgulama](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L421-L435) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [Dize, matematik ve dizi işleçleri ile sorgulama](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L527-L552) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [SqlQuerySpec kullanarak parametreli SQL ile sorgulama](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L140-L174) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100))<br>[SqlQuerySpec](/dotnet/api/microsoft.azure.documents.sqlqueryspec) |
| [Açık disk belleği ile sorgu](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L554-L576) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [Bölümlenmiş koleksiyonları paralel olarak sorgulama](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs#L664-L734) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |
| [ORDER BY ile bölümlenmiş koleksiyonları sorgulama](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs#L737-L810) |[DocumentQueryable.CreateDocumentQuery](/previous-versions/azure/dn850285(v=azure.100)) |

## <a name="change-feed-examples"></a>Akış örneklerini değiştirme 
Örnek, değişiklik akışı [azure-documentdb-dotnet/samples/code-samples/ChangeFeed/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ServerSideScripts/Program.cs), aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos DB'de akışı değiştirme hakkında bilgi edinmek için [okuma Azure Cosmos DB değişiklik akışı](read-change-feed.md) ve [değişiklik akışı İşlemci](change-feed-processor.md). 

| Görev | API başvurusu |
| --- | --- |
| [Değişiklik akışını okuma](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ChangeFeed/Program.cs#L132) |[DocumentClient.CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery) | 
| [Bölüm anahtarı aralıklarını okuma](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ChangeFeed/Program.cs#L118) |[DocumentClient.ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync) | 

Değişiklik akışı işlemci örnek [ChangeFeedMigrationTool](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedMigrationTool), verileri çoğaltmak için başka bir Cosmos DB koleksiyonu için değişiklik akışı işlemci kitaplığı kullanmayı gösterir.   

## <a name="server-side-programming-examples"></a>Sunucu tarafı programlama örnekleri
Sunucu tarafı programlama dosyasında ([azure-documentdb-dotnet/samples/code-samples/ServerSideScripts/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ServerSideScripts/Program.cs)) aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilmektedir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos DB'de sunucu tarafı programlama hakkında bilgi edinmek için [saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri](stored-procedures-triggers-udfs.md). 

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L112) |[DocumentClient.CreateStoredProcedureAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createstoredprocedureasync) |
| [Saklı yordamı yürütme](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L127) |[DocumentClient.ExecuteStoredProcedureAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.executestoredprocedureasync) |
| [Bir saklı yordam için belge akışını okuma](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L194) |[DocumentClient.ReadDocumentFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync) |
| [ORDER BY ile bir saklı yordam oluşturma](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L219) |[DocumentClient.CreateStoredProcedureAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createstoredprocedureasync) |
| [Tetikleme öncesi oluşturma](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L264) |[DocumentClient.CreateTriggerAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createtriggerasync) |
| [Tetikleme sonrası oluşturma](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L329) |[DocumentClient.CreateTriggerAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createtriggerasync) |
| [Bir kullanıcı tanımlı işlev (UDF) oluşturma](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L389) |[DocumentClient.CreateUserDefinedFunctionAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createuserdefinedfunctionasync) |

## <a name="user-management-examples"></a>Kullanıcı yönetim örnekleri
Kullanıcı Yönetimi dosyası [azure-documentdb-dotnet/samples/code-samples/UserManagement/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/UserManagement/Program.cs), aşağıdaki görevlerin nasıl yapılacağını gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Kullanıcı oluşturma](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L81) |[DocumentClient.CreateUserAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createuserasync) |
| [Koleksiyon veya belge üzerinde izinleri ayarlama](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L85) |[DocumentClient.CreatePermissionAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createpermissionasync) |
| [Bir kullanıcının izinlerinin listesini alma](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L218) |[DocumentClient.ReadUserAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readuserasync)<br>[DocumentClient.ReadPermissionFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpermissionfeedasync) |

