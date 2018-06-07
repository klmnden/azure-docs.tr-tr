---
title: Azure Cosmos DB'de toplu işlemleri gerçekleştirmek için BulkExecutor Java kitaplığı kullanılarak | Microsoft Docs
description: Toplu alma ve Azure Cosmos DB koleksiyonlarına belgeleri güncelleştirmek için Azure Cosmos veritabanı BulkExecutor Java kitaplığını kullanın.
keywords: Java toplu Yürütücü
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 77135ec5f62636d1dd634361da345b00d98ad918
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34610251"
---
# <a name="use-bulkexecutor-java-library-to-perform-bulk-operations-on-azure-cosmos-db-data"></a>BulkExecutor Java kitaplığı Azure Cosmos DB veri toplu işlemleri gerçekleştirmek için kullanın

Bu öğretici, alma ve Azure Cosmos DB belgeleri güncelleştirmek için Azure Cosmos veritabanı toplu Yürütücü Java kitaplığı kullanma hakkında yönergeler sağlar. BulkExecutor kitaplığı nasıl, çok büyük verim ve depolama yararlanan yardımcı hakkında bilgi edinmek için bkz: [BulkExecutor kitaplığına genel bakış](bulk-executor-overview.md) makalesi. Bu öğreticide rastgele belgeleri oluşturan bir Java uygulaması oluşturma ve bunlar toplu bir Azure Cosmos DB koleksiyonuna alındı. İçeri aktardıktan sonra toplu bir belgenin bazı özellikler güncelleştirme. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.  

* [Azure Cosmos DB’yi ücretsiz olarak](https://azure.microsoft.com/try/cosmosdb/) bir Azure aboneliği olmadan, ücretsiz ve herhangi bir taahhütte bulunmadan deneyebilirsiniz. Veya, kullanabileceğiniz [Azure Cosmos DB öykünücüsü](https://docs.microsoft.com/azure/cosmos-db/local-emulator) ile `https://localhost:8081` URI. Birincil anahtar sağlanan [istekleri kimlik doğrulaması](local-emulator.md#authenticating-requests).  

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  
  - Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.  

  - JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.

* Bir [Maven](http://maven.apache.org/) ikili arşivi [indirin](http://maven.apache.org/download.cgi) ve [yükleyin](http://maven.apache.org/install.html)  
  
  - Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.

* Açıklanan adımları kullanarak bir Azure Cosmos DB SQL API hesabı oluşturmak [veritabanı hesabı oluşturma](create-sql-api-java.md#create-a-database-account) Java hızlı başlangıç makalenin bölümüne.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi Github'dan bir örnek Java uygulaması yükleyerek kod ile çalışmak için şimdi geçin. Bu uygulamayı Azure Cosmos DB verileri toplu işlemleri gerçekleştirir. Uygulama kopyalamak için bir komut istemi açın, uygulama kopyalayın ve aşağıdaki komutu çalıştırın istediğiniz dizine gidin:

```
 git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started.git 
```

Kopyalanan deposu iki örnekleri "BulkImport" ve "\azure-cosmosdb-bulkexecutor-java-getting-started\samples\bulkexecutor-sample\src\main\java\com\microsoft\azure\cosmosdb\bulkexecutor" klasörüyle "bulkupdate" içerir. "BulkImport" uygulama rastgele belgeler oluşturur ve bunları Azure Cosmos Veritabanına aktarır. "Bulkupdate" uygulama Azure Cosmos DB bazı belgelerde güncelleştirir. Sonraki bölümlerde, biz her Bu örnek uygulama kodunda inceleyin. 

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Toplu veri Azure Cosmos DB Al

1. Azure Cosmos veritabanı bağlantı dizeleri bağımsız değişken olarak okuma ve CmdLineConfiguration.java dosyasında tanımlanan değişkenler atandı.  

2. Sonraki DocumentClient nesnesi şu deyimleri kullanarak başlatılır:  

   ```java
   ConnectionPolicy connectionPolicy = new ConnectionPolicy();
   connectionPolicy.setMaxPoolSize(1000);
   DocumentClient client = new DocumentClient(
      HOST,
      MASTER_KEY, 
      connectionPolicy,
      ConsistencyLevel.Session)
   ```

3. DocumentBulkExecutor nesnesi ile yüksek yeniden deneme değerlerinde bekleme süresi için başlatılır ve istekleri kısıtlanan. Ve bunlar Tıkanıklık denetimi DocumentBulkExecutor için yaşam süresi için geçirmek için 0 olarak sonra ayarlanır.  

   ```java
   // Set client's retry options high for initialization
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(30);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(9);

   // Builder pattern
   Builder bulkExecutorBuilder = DocumentBulkExecutor.builder().from(
     client,
     DATABASE_NAME,
     COLLECTION_NAME,
     collection.getPartitionKey(),
     offerThroughput) // throughput you want to allocate for bulk import out of the collection's total throughput

   // Instantiate DocumentBulkExecutor
   DocumentBulkExecutor bulkExecutor = bulkExecutorBuilder.build()

   // Set retries to 0 to pass complete control to bulk executor
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(0);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(0);
```

4. Bir Azure Cosmos DB koleksiyonu içe toplu olarak rastgele belgeler oluşturur API importAll çağırın. Komut satırı yapılandırmaları CmdLineConfiguration.java dosya içinde yapılandırabilirsiniz.

   ```java
   BulkImportResponse bulkImportResponse = bulkExecutor.importAll(documents, false, true, null);
```
   JSON serileştirilmiş belgeleri topluluğu toplu içeri aktarma API'si kabul eder ve daha fazla ayrıntı için aşağıdaki söz dizimini varsa, bkz: [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor):

   ```java
   public BulkImportResponse importAll(
        Collection<String> documents,
        boolean isUpsert,
        boolean disableAutomaticIdGeneration,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;   
   ```

   İmportAll yöntemi aşağıdaki parametreleri kabul eder:
 
   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |isUpsert    |   Upsert belgelerin etkinleştirmek için bir bayrak. Bir belge, ile verilen kimliği zaten var, güncelleştirilir.  |
   |disableAutomaticIdGeneration     |   Kimliğini otomatik olarak oluşturulmasını devre dışı bırakmak için bayrak Varsayılan olarak ayarlanmış true.   |
   |maxConcurrencyPerPartitionRange    |  Eşzamanlılık bölüm anahtarı aralığının başına en fazla derecesi. Varsayılan değer 20'dir.  |

   **Toplu içe aktarma yanıt nesne tanımı** aşağıdaki get yöntemleri toplu içeri API çağrısının sonucunu içerir:

   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |int getNumberOfDocumentsImported()  |   Toplu için sağlanan belgeleri dışında başarıyla içeri aktarıldı belgeleri sayısı toplam API çağrısı içeri aktarın.      |
   |çift getTotalRequestUnitsConsumed()   |  Toplu tarafından kullanılan toplam istek birimleri (RU) API çağrısı içeri aktarın.       |
   |Süre getTotalTimeTaken()   |    Toplu içeri aktarmayla API çağrısı yürütme tamamlamak için geçen toplam süre.     |
   |Liste<Exception> getErrors() |  Bazı belgeleri toplu sağlanan toplu dışında API çağrısı eklenen için başarısız alırsanız hataların listesini alır.       |
   |Liste<Object> getBadInputDocuments()  |    Toplu olarak başarıyla alınmadı bozuk biçimli belgeler listesini API çağrısı içeri aktarın. Kullanıcı döndürülen belgelerin düzeltip alma yeniden deneyin. Hatalı biçimlendirilmiş belgeleri kimliği değeri (null veya diğer veri türü geçersiz olarak kabul edilir) bir dize değil belgeleri içerir.     |

5. Uygulama hazır alma toplu aldıktan sonra kaynak komut satırı aracını 'mvn temiz paketi' komutunu kullanarak oluşturun. Bu komut, hedef klasör jar dosyasını oluşturur:  

   ```java
   mvn clean package
   ```

6. Hedef bağımlılıklar üretildikten sonra aşağıdaki komutu kullanarak toplu içeri Aktarıcı uygulama çağırabilirsiniz:  

   ```java
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint *<Fill in your Azure Cosmos DB’s endpoint URI>*  -masterKey *<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkImportDb -collectionId bulkImportColl -operation import -shouldCreateCollection -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

   Toplu içeri Aktarıcı veritabanı adını, koleksiyon adı ve App.config dosyasında belirtilen işleme değerleri içeren yeni bir veritabanı ve koleksiyonu oluşturur. 

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Toplu güncelleştirme verilerini Azure Cosmos veritabanı

BulkUpdateAsync API'sini kullanarak, varolan belgeleri güncelleştirebilirsiniz. Bu örnekte, ad alanına yeni bir değere ayarlayın ve açıklama alanı varolan belgelerden kaldırma. Desteklenen alan tam kümesi için güncelleştirme işlemleri, bkz: [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor). 

1. Karşılık gelen alan güncelleştirme işlemlerinin yanı sıra güncelleştirme öğelerini tanımlar. Bu örnekte, ad alanı ve açıklama alanı tüm belgelerden kaldırma UnsetUpdateOperation güncelleştirmek için SetUpdateOperation kullanır. Ayrıca diğer artış gibi bir belge alan belirli bir değerle işlemleri, belirli değerleri bir dizi alanına anında veya belirli bir değer bir dizi alanından kaldırın. Toplu güncelleştirme API'sı tarafından sağlanan farklı yöntemleri hakkında bilgi edinmek için [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor).  

   ```java
   SetUpdateOperation<String> nameUpdate = new SetUpdateOperation<>("Name","UpdatedDocValue");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   ArrayList<UpdateOperationBase> updateOperations = new ArrayList<>();
   updateOperations.add(nameUpdate);
   updateOperations.add(descriptionUpdate);

   List<UpdateItem> updateItems = new ArrayList<>(cfg.getNumberOfDocumentsForEachCheckpoint());
   IntStream.range(0, cfg.getNumberOfDocumentsForEachCheckpoint()).mapToObj(j -> {                      
    return new UpdateItem(Long.toString(prefix + j), Long.toString(prefix + j), updateOperations);
    }).collect(Collectors.toCollection(() -> updateItems));
   ```

2. Bir Azure Cosmos DB koleksiyonuna alındı toplu olarak rastgele belgeler oluşturur API updateAll çağırın. Komut satırı yapılandırmaları CmdLineConfiguration.java dosyasına geçirilecek şekilde yapılandırabilirsiniz.

   ```java
   BulkUpdateResponse bulkUpdateResponse = bulkExecutor.updateAll(updateItems, null)
   ```

   Toplu güncelleştirme API'sı güncelleştirilmesi öğeleri koleksiyonu kabul eder. Her güncelleştirme öğesi kimliği ve bölüm anahtarı değerini tarafından tanımlanan bir belge üzerinde gerçekleştirilecek alan güncelleştirme işlemleri listesini belirtir. Daha fazla ayrıntı için bkz: [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor):

   ```java
   public BulkUpdateResponse updateAll(
        Collection<UpdateItem> updateItems,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;
   ```

   UpdateAll yöntemi aşağıdaki parametreleri kabul eder:

   |**Parametre** |**Açıklama** |
   |---------|---------|
   |maxConcurrencyPerPartitionRange   |  Eşzamanlılık bölüm anahtarı aralığının başına en fazla derecesi. Varsayılan değer 20'dir.  |
 
   **Toplu içe aktarma yanıt nesne tanımı** aşağıdaki get yöntemleri toplu içeri API çağrısının sonucunu içerir:

   |**Parametre** |**Açıklama**  |
   |---------|---------|
   |int getNumberOfDocumentsUpdated()  |   Toplu güncelleştirme API çağrısı için sağlanan belgeleri dışında başarıyla güncelleştirildi belgeleri toplam sayısı.      |
   |çift getTotalRequestUnitsConsumed() |  Toplu güncelleştirme API çağrısı tarafından kullanılan toplam istek birimleri (RU).       |
   |Süre getTotalTimeTaken()  |   Toplu olarak geçen toplam süre yürütülmesinin tamamlanmasını API çağrısı güncelleştirin.      |
   |Liste<Exception> getErrors()   |     Bazı belgeleri toplu dışında eklenen için toplu güncelleştirme API çağrısı başarısız için sağlandıysa hataların listesini alır.      |

3. Uygulama hazır güncelleştirme toplu aldıktan sonra kaynak komut satırı aracını 'mvn temiz paketi' komutunu kullanarak oluşturun. Bu komut, hedef klasör jar dosyasını oluşturur:  

   ```
   mvn clean package
   ```

4. Hedef bağımlılıklar üretildikten sonra aşağıdaki komutu kullanarak toplu güncelleştirme uygulaması çağırabilirsiniz:

   ```
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint **<Fill in your Azure Cosmos DB’s endpoint URI>* -masterKey **<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkUpdateDb -collectionId bulkUpdateColl -operation update -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

## <a name="performance-tips"></a>Performans ipuçları 

BulkExecutor kitaplığı kullanırken daha iyi performans için aşağıdaki noktaları göz önünde bulundurun:

* En iyi performans için bir Azure VM Cosmos DB hesap yazma bölgeniz ile aynı bölgede uygulamanızı çalıştırın.  
* Yüksek verimlilik elde için:  

   * Belgeleri çok sayıda işleme herhangi bellek sorunu önlemek için yeterince büyük bir sayıya JVM'ın öbek boyutunu ayarlayın. Yığın boyutu önerilen: en fazla (3GB, 3 * (tüm belgeleri toplu geçirilen Al API bir toplu işlemde) sizeof).  
   * Hangi nedeniyle daha yüksek verimlilik ile çok sayıda belgeleri toplu işlemleri gerçekleştirirken karşılaşırsınız birer önişlem yoktur. 10,000,000 belgeleri almak istiyorsanız, bu nedenle, 10 kez toplu içeri 10 belgeleri toplu üzerinde her 1.000.000 boyutunun çalışan toplu içeri 100 kez 100 belgeleri toplu üzerinde her boyutu 100.000 belgelerin çalıştırma daha tercih edilir.  

* Belirli bir Azure Cosmos DB koleksiyonuna karşılık gelen tek bir sanal makine içindeki tüm uygulama için tek bir DocumentBulkExecutor nesne örneği önerilir.  

* Bu yana bir tek toplu işlem API yürütme istemci makinenin CPU ve ağ GÇ büyük öbeğini kullanır. Birden çok görev oluşturma tarafından dahili olarak bu durum, yürütülen her toplu işlem API çağrıları, uygulama işlemi içinde birden çok eşzamanlı görev oluşturma kaçının. Tek bir sanal makine üzerinde çalışan tek toplu işlem API çağrısı tüm koleksiyonunuzun işleme tüketen kaydedemediği (varsa koleksiyonunuzun işleme > 1 milyon RU/s), eş zamanlı olarak toplu yürütmek için ayrı sanal makineler oluşturmak için tercih edilir işlemi API çağrıları.

    
## <a name="next-steps"></a>Sonraki adımlar
* Maven Paket ayrıntılarını hakkında bilgi edinin ve sürüm notları BulkExecutor Java kitaplığı için bkz:[BulkExecutor SDK ayrıntıları](sql-api-sdk-bulk-executor-java.md).


