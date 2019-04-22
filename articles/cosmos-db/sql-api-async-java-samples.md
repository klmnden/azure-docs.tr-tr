---
title: "Azure Cosmos DB: SQL API'si için zaman uyumsuz Java örnekleri"
description: CRUD işlemleri gibi Azure Cosmos DB SQL API'sinin kullanıldığı yaygın görevler için GitHub'da Zaman Uyumsuz Java örnekleri bulabilirsiniz.
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: sample
ms.date: 06/18/2018
ms.author: sngun
ms.openlocfilehash: 34551dcc64e6add2baa1ec94067037c98de67abe
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58917949"
---
# <a name="azure-cosmos-db-async-java-examples-for-the-sql-api"></a>Azure Cosmos DB: SQL API'si için zaman uyumsuz Java örnekleri

> [!div class="op_single_selector"]
> * [.NET Örnekleri](sql-api-dotnet-samples.md)
> * [Java Örnekleri](sql-api-java-samples.md)
> * [Async Java Örnekleri](sql-api-async-java-samples.md)
> * [Node.js Örnekleri](sql-api-nodejs-samples.md)
> * [Python Örnekleri](sql-api-python-samples.md)
> * [Azure Kod Örneği Galerisi](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Azure Cosmos DB kaynaklarında CRUD işlemlerini ve diğer yaygın işlemleri gerçekleştiren en son örnek uygulamalar, [azure-cosmosdb-java](https://github.com/Azure/azure-cosmosdb-java) GitHub deposuna eklenmiştir. Bu makalede aşağıdakiler sunulmaktadır:

* Örnek Zaman Uyumsuz Java proje dosyalarının her birindeki görevlere bağlantılar. 
* İlgili API başvurusu içeriğine bağlantılar.

**Önkoşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- Yapabilecekleriniz [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay krediler sunar.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Bu örnek uygulamayı çalıştırmanız için aşağıdakiler gereklidir:

* Java Development Kit 8
* Microsoft Azure Cosmos DB Java SDK’sı

Projenizde kullanmak üzere en son Microsoft Azure Cosmos DB Java SDK'sı ikili dosyalarını almak için isteğe bağlı olarak Maven de kullanabilirsiniz. [Buradan](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) en son sürümü seçebilirsiniz. Maven tüm gerekli bağımlılıkları otomatik olarak ekler. Aksi takdirde, pom.xml dosyasında listelenen bağımlılıkları doğrudan indirebilir ve derleme yolunuza ekleyebilirsiniz.

```bash
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-cosmosdb</artifactId>
    <version>${LATEST}</version>
</dependency>
```

**Örnek uygulamaları çalıştırma**

Örnek depoyu kopyalama:
```bash
$ git clone https://github.com/Azure/azure-cosmosdb-java.git

$ cd azure-cosmosdb-java
```

Örnekleri Eclipse kullanarak çalıştırabileceğiniz gibi, Maven kullanıp komut satırından da çalıştırabilirsiniz.

Eclipse'ten çalıştırmak için:
* Eclipse'te ana üst proje pom.xml dosyasını yükleyin; bunun azure-cosmosdb-examples dosyasını otomatik olarak yüklemesi gerekir.
* Örnekleri çalıştırmak için geçerli bir Azure Cosmos DB Uç Noktası gerekir. Uç noktalar `src/test/java/com/microsoft/azure/cosmosdb/rx/examples/TestConfigurations.java` konumundan okunur.
* Uç nokta kimlik bilgilerinizi Eclipse JUnit Run Config'de VM Arguments olarak geçirebilir veya TestConfigurations.java dosyasına koyabilirsiniz.
    ```bash
    -DACCOUNT_HOST=<Fill your Azure Cosmos DB account name> -DACCOUNT_KEY=<Fill your Azure Cosmos DB primary key>
    ```
* Şimdi örnekleri Eclipse'de JUnit testleri olarak çalıştırabilirsiniz.

Komut satırından çalıştırmak için:
* Örnekleri çalıştırmanın diğer yolu Maven kullanmaktır:
* Maven'i çalıştırın ve Azure Cosmos DB Uç Nokta kimlik bilgilerinizi geçirin:
    ```bash
    mvn test -DACCOUNT_HOST=<Fill your Azure Cosmos DB account name> -DACCOUNT_KEY=<Fill your Azure Cosmos DB primary key>
    ```

   > [!NOTE]
   > Örnekler birbirinden bağımsızdır; kendi kendine ayarlanır ve sonra kendini temizler. Örneklerde [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createcollection)'a birden çok çağrı yapılır. Bu her yapıldığında, oluşturulan koleksiyonun performans katmanına karşılık aboneliğiniz 1 saatlik kullanım için faturalandırılır. 
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri
[DatabaseCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java) dosyasını aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos veritabanları hakkında bilgi edinmek için [veritabanları, kapsayıcıları ve öğeleri ile çalışma](databases-containers-items.md) kavramsal makale. 

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L115-L132) | [AsyncDocumentClient.createDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createdatabase) |
| [Zaten var olan bir veritabanı oluşturulamadı](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L189-L204) | |
| [Veritabanını oluşturma ve okuma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L228-L249) | [AsyncDocumentClient.readDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.readdatabase) |
| [Veritabanı oluşturma ve silme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L255-L276) | [AsyncDocumentClient.deleteDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.deletedatabase) |
| [Veritabanını oluşturma ve sorgulama](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L282-L312) | [AsyncDocumentClient.queryDatabases](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.querydatabases) |

## <a name="collection-examples"></a>Koleksiyon örnekleri
[CollectionCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java) dosyasını aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos Koleksiyonlar hakkında bilgi edinmek için [veritabanları, kapsayıcıları ve öğeleri ile çalışma](databases-containers-items.md) kavramsal makale. 

| Görev | API başvurusu |
| --- | --- |
| [Tek bir bölüm koleksiyonu oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L134-L153) | [AsyncDocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createcollection) |
| [Özel çok bölümlü bir koleksiyon oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L164-L184) | [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.documentcollection)<br>[PartitionKeyDefinition](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.partitionkeydefinition)<br>[RequestOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.requestoptions) |
| [Zaten var olan bir koleksiyon oluşturulamadı](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L241-L256) | |
| [Koleksiyon oluşturma ve okuma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L281-L304) | [AsyncDocumentClient.readCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.readcollection) |
| [Koleksiyon oluşturma ve silme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L310-L333) | [AsyncDocumentClient.deleteCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.deletecollection) |
| [Koleksiyon oluşturma ve sorgulama](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L339-L372) | [AsyncDocumentClient.queryCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.querycollections) |

## <a name="document-examples"></a>Belge örnekleri
[DocumentCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java) dosyasını aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos belgeler hakkında bilgi edinmek için [veritabanları, kapsayıcıları ve öğeleri ile çalışma](databases-containers-items.md) kavramsal makale. 

| Görev | API başvurusu |
| --- | --- |
| [Belge oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L137-L156) | [AsyncDocumentClient.createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createdocument) |
| [Programlanabilir belge tanımıyla belge oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L213-L242) | [Belge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.document)<br>[Resource.setId](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.resource.setid) |
| [Belge oluşturma ve toplam RU maliyetini bulma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L248-L280) | [ResourceResponse.getRequestCharge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.resourceresponse.getrequestcharge) |
| [Zaten var olan bir belge oluşturulamadı](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L316-L336) | |
| [Belge oluşturma ve değiştirme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L342-L365) | [AsyncDocumentClient.replaceDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.replacedocument) |
| [Belge oluşturma ve upsert etme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L371-L393) | [AsyncDocumentClient.upsertDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.upsertdocument) |
| [Belge oluşturma ve silme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L399-L431) | [AsyncDocumentClient.deleteDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.deletedocument) |
| [Belge oluşturma ve okuma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L437-L458) | [AsyncDocumentClient.readDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.readdocument) |

## <a name="indexing-examples"></a>Dizin örnekleri
[CollectionCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java) dosyasını aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.  Aşağıdaki örneği çalıştırmadan önce Azure Cosmos DB'de dizinleme hakkında bilgi edinmek için [dizinleme ilkeleri](index-policy.md), [türlerini dizinleme](index-types.md), ve [dizin yolları](index-paths.md) kavramsal makaleler. 

| Görev | API başvurusu |
| --- | --- |
| [Dizin oluşturma ve dizin oluşturma ilkesini ayarlama](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L394-L410) | [Dizin](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.index)<br>[IndexingPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.indexingpolicy) |

Dizin oluşturma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB dizin oluşturma ilkeleri](index-policy.md).

## <a name="query-examples"></a>Sorgu örnekleri
[DocumentQuerySamples](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java) dosyasını aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos DB'de SQL sorgu başvurusu hakkında bilgi edinmek için bkz. [SQL sorgu örnekleri](how-to-sql-query.md) kavramsal makale. 

| Görev | API başvurusu |
| --- | --- |
| [Basit bir belge sorgusu gerçekleştirme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L154-L190) | [AsyncDocumentClient.queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.querydocuments) |
| [Basit bir belge sorgusu gerçekleştirme ve Toplam RU maliyetini öğrenme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L249-L268) | [FeedResponse.getRequestCharge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.feedresponse.getrequestcharge) |
| [Basit bir belge sorgusu gerçekleştirme, bir sayfa okuma ve döndürülen gözlemlenebilir öğe aboneliğinden çıkma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L274-L312) | |
| [Basit bir belge sorgusu gerçekleştirme ve sonuçları filtreleme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L318-L368) | |
| [Bir sipariş tarafından belge bölümler arası sorgu gerçekleştirin](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L410-L457) | [FeedOptions.setEnableCrossPartitionQuery](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.feedoptions.setenablecrosspartitionquery) |

Sorgu yazma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB içinde SQL sorgusu](how-to-sql-query.md).

## <a name="offer-examples"></a>Teklif örnekleri
[OfferCRUDAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java) dosyası aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir:

| Görev | API başvurusu |
| --- | --- |
| [Koleksiyon oluşturma ve aktarım hızını ayarlama](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L106-L113) | [AsyncDocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createcollection)<br>[RequestOptions.setOfferThroughput](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.requestoptions.setofferthroughput) |
| [İlişkili teklifi bulmak için koleksiyonu okuma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L118-L130) | [Offer.getContent](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.offer.getContent)<br>[DocumentClient.queryOffers](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.queryoffers) |
| [Bir koleksiyonun teklifini değiştirerek aktarım hızını güncelleştirme](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L101-L153) | [AsyncDocumentClient.replaceOffer](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.replaceoffer) |

## <a name="stored-procedure-examples"></a>Saklı yordam örnekleri
[StoredProcedureAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java) dosyasını aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos DB'de sunucu tarafı programlama hakkında bilgi edinmek için bkz. [saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri](stored-procedures-triggers-udfs.md) kavramsal makale. 

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma ve çalıştırma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L108-L155) | [StoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.storedprocedure)<br>[DocumentClient.createStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.createstoredprocedure)<br>[AsyncDocumentClient.executeStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient.executestoredprocedure) |
| [Saklı yordamı bağımsız değişkenlerle oluşturma ve çalıştırma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L161-L195) | |
| [Saklı yordamı bir nesne bağımsız değişkeniyle oluşturma ve çalıştırma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L201-L241) | |

## <a name="unique-key"></a>Benzersiz Anahtar
[UniqueIndexAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/UniqueIndexAsyncAPITest.java) dosyasını aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örneği çalıştırmadan önce Azure Cosmos DB'de benzersiz anahtarlar hakkında bilgi edinmek için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md) kavramsal makale. 

| Görev | API başvurusu |
| --- | --- |
| [Benzersiz bir anahtarla bir koleksiyon oluşturma](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/UniqueIndexAsyncAPITest.java#L65-L97) | [UniqueKey](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.uniquekey)<br>[UniqueKeyPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.uniquekeypolicy)<br>[DocumentCollection.setUniqueKeyPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.documentcollection.setuniquekeypolicy) |
