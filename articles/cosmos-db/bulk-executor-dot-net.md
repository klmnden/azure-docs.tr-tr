---
title: Azure Cosmos DB'de toplu işlemleri gerçekleştirmek için BulkExecutor .NET kitaplığını kullanarak | Microsoft Docs
description: Toplu alma ve Azure Cosmos DB koleksiyonlarına belgeleri güncelleştirmek için Azure Cosmos veritabanı BulkExecutor .NET kitaplığını kullanın.
keywords: .NET toplu Yürütücü
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 608551090ce10e08ba517def644c72186a6f25e1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="using-bulkexecutor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Azure Cosmos DB'de toplu işlemleri gerçekleştirmek için BulkExecutor .NET kitaplığı kullanma

Bu öğretici, alabilir ve belgeleri Azure Cosmos DB koleksiyonlarına güncelleştirmek için Azure Cosmos veritabanı BulkExecutor .NET kitaplığı kullanma hakkında yönergeler sağlar. BulkExecutor kitaplığı nasıl, çok büyük verim ve depolama yararlanan yardımcı hakkında bilgi edinmek için bkz: [BulkExecutor kitaplığına genel bakış](bulk-executor-overview.md) makalesi. Bu öğretici, toplu bir Azure Cosmos DB koleksiyona rastgele oluşturulan belgeleri alır, örnek bir .NET uygulama size rehberlik yapar. İçeri aktardıktan sonra onu nasıl Toplu içe aktarılan verilerin üzerinde belirli belge alanları gerçekleştirmek için işlemler olarak düzeltme ekleri belirterek güncelleştirme gösterir.

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio yüklü 2017 yoksa kullanın karşıdan yükleyip [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Visual Studio Kurulumu sırasında Azure geliştirme etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun. 

* [Azure Cosmos DB’yi ücretsiz olarak](https://azure.microsoft.com/try/cosmosdb/) bir Azure aboneliği olmadan, ücretsiz ve herhangi bir taahhütte bulunmadan deneyebilirsiniz. Veya, kullanabileceğiniz [Azure Cosmos DB öykünücüsü](https://docs.microsoft.com/azure/cosmos-db/local-emulator) ile `https://localhost:8081` URI. Birincil anahtar sağlanan [istekleri kimlik doğrulaması](local-emulator.md#authenticating-requests).

* Açıklanan adımları kullanarak bir Azure Cosmos DB SQL API hesabı oluşturmak [veritabanı hesabı oluşturma](create-sql-api-dotnet.md#create-a-database-account) .NET hızlı başlangıç makalenin bölümüne. 

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi Github'dan bazı örnek .NET uygulamalarını yükleyerek kod ile çalışmak için şimdi geçin. Bu uygulamalar Azure Cosmos DB veri toplu işlemleri. Uygulamaları kopyalamak için bir komut istemi açın, onları kopyalayın ve aşağıdaki komutu çalıştırarak istediğiniz dizine gidin:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

Kopyalanan depo iki örnek "BulkImportSample" ve "BulkUpdateSample" içerir. Örnek uygulamalar herhangi birini açın, Azure Cosmos DB hesabınızın bağlantı dizeleriyle App.config dosyasında bağlantı dizelerini güncelleştirmek, çözümü oluşturun ve çalıştırın. 

"BulkImportSample" uygulama rastgele belgeler oluşturur ve bunları Azure Cosmos DB toplu alır. "BulkUpdateSample" uygulama toplu olarak belirli bir belge alanlarını gerçekleştirmek için işlemleri düzeltme ekleri belirterek alınan belgeleri güncelleştirir. Sonraki bölümlerde, her Bu örnek uygulama kodunda incelenecektir.

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Toplu veri Azure Cosmos DB Al

1. "BulkImportSample" klasörüne gidin ve "BulkImportSample.sln" dosyasını açın.  

2. Aşağıdaki kodda gösterildiği gibi Azure Cosmos veritabanı bağlantı dizelerini App.config dosyasından alınır:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   Toplu içeri Aktarıcı veritabanı adını, koleksiyon adı ve App.config dosyasında belirtilen işleme değerleri içeren yeni bir veritabanı ve koleksiyonu oluşturur. 

3. Sonraki DocumentClient nesne doğrudan TCP bağlantı modu ile başlatıldı:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. BulkExecutor nesnesi ile yüksek yeniden deneme değerlerinde bekleme süresi için başlatılır ve istekleri kısıtlanan. Ve bunlar Tıkanıklık denetimi BulkExecutor için yaşam süresi için geçirmek için 0 olarak sonra ayarlanır.  

   ```csharp
   // Set retry options high during initialization (default values).
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 30;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 9;

   IBulkExecutor bulkExecutor = new BulkExecutor(client, dataCollection);
   await bulkExecutor.InitializeAsync();

   // Set retries to 0 to pass complete control to bulk executor.
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 0;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 0;
   ```

5. Uygulama BulkImportAsync API çağırır. .NET kitaplığı API - seri hale getirilmiş JSON belgelerinin bir listesini kabul eden bir toplu iki aşırı almak ve diğer seri durumdan çıkarılmış POCO belgelerin listesini kabul eder sağlar. Tanımlarını aşırı yüklenmiş bu yöntemlerin her biri hakkında bilgi edinmek için bkz [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet).

   ```csharp
   BulkImportResponse bulkImportResponse = await bulkExecutor.BulkImportAsync(
     documents: documentsToImportInBatch,
     enableUpsert: true,
     disableAutomaticIdGeneration: true,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```
   **BulkImportAsync yöntemi aşağıdaki parametreleri kabul eder:**
   
   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |enableUpsert    |   Upsert belgelerin etkinleştirmek için bir bayrak. Bir belge, ile verilen kimliği zaten var, güncelleştirilir. Varsayılan olarak false değerine ayarlanır.      |
   |disableAutomaticIdGeneration    |    Kimliğini otomatik olarak oluşturulmasını devre dışı bırakmak için bayrak Varsayılan olarak ayarlanmış true.     |
   |maxConcurrencyPerPartitionKeyRange    | Eşzamanlılık bölüm anahtarı aralığının başına maksimum ölçüde, null ayarına 20 varsayılan değeri kullanılacak kitaplığı neden olur. |
   |maxInMemorySortingBatchSize     |  Maksimum belge sayısını çekilen API için geçirilen belge Numaralandırıcı gelen her aşamada çağırın.  Toplu içe aktarma önce bellek içi ön-işleme sıralama aşaması için ayar null min (documents.count, 1000000) varsayılan değeri kullanılacak kitaplığı neden olur.       |
   |CancellationToken    |    Toplu içeri düzgün biçimde çıkmak için iptal belirteci.     |

   **Toplu içe aktarma yanıt nesne tanımı** aşağıdaki öznitelikleri toplu içeri API çağrısının sonucunu içerir:

   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |NumberOfDocumentsImported (uzun)   |  Toplu için sağlanan belgeleri dışında başarıyla içeri aktarıldı belgeleri sayısı toplam API çağrısı içeri aktarın.       |
   |TotalRequestUnitsConsumed (çift)   |   Toplu tarafından kullanılan toplam istek birimleri (RU) API çağrısı içeri aktarın.      |
   |TotalTimeTaken (TimeSpan)    |   Toplu içeri aktarmayla API çağrısı yürütme tamamlamak için geçen toplam süre.      |
   |BadInputDocuments (liste<object>)   |     Toplu olarak başarıyla alınmadı bozuk biçimli belgeler listesini API çağrısı içeri aktarın. Kullanıcı döndürülen belgelerin düzeltip alma yeniden deneyin. Hatalı biçimlendirilmiş belgeleri kimliği değeri (null veya diğer veri türü geçersiz olarak kabul edilir) bir dize değil belgeleri içerir.    |

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Toplu güncelleştirme verilerini Azure Cosmos veritabanı

BulkUpdateAsync API'sini kullanarak, varolan belgeleri güncelleştirebilirsiniz. Bu örnekte, ad alanına yeni bir değere ayarlayın ve açıklama alanı varolan belgelerden kaldırma. Desteklenen alan tam kümesi için güncelleştirme işlemleri, başvurmak [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet). 

1. "BulkUpdateSample" klasörüne gidin ve "BulkUpdateSample.sln" dosyasını açın.  

2. Karşılık gelen alan güncelleştirme işlemlerinin yanı sıra güncelleştirme öğeleri tanımlar. Bu örnekte, ad alanı ve açıklama alanı tüm belgelerden kaldırma UnsetUpdateOperation güncelleştirmek için SetUpdateOperation kullanır. Ayrıca diğer artış gibi bir belge alan belirli bir değerle işlemleri, belirli değerleri bir dizi alanına anında veya belirli bir değer bir dizi alanından kaldırın. Toplu güncelleştirme API'sı tarafından sağlanan farklı yöntemleri hakkında bilgi edinmek için bkz [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet).

   ```csharp
   SetUpdateOperation<string> nameUpdate = new SetUpdateOperation<string>("Name", "UpdatedDoc");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   List<UpdateOperation> updateOperations = new List<UpdateOperation>();
   updateOperations.Add(nameUpdate);
   updateOperations.Add(descriptionUpdate);

   List<UpdateItem> updateItems = new List<UpdateItem>();
   for (int i = 0; i < 10; i++)
   {
    updateItems.Add(new UpdateItem(i.ToString(), i.ToString(), updateOperations));
   }
   ```

3. Uygulama BulkUpdateAsync API çağırır. BulkUpdateAsync yöntemi tanımı hakkında bilgi edinmek için bkz [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.ibulkexecutor.bulkupdateasync?view=azure-dotnet).  

   ```csharp
   BulkUpdateResponse bulkUpdateResponse = await bulkExecutor.BulkUpdateAsync(
     updateItems: updateItems,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```  
   **BulkUpdateAsync yöntemi aşağıdaki parametreleri kabul eder:**

   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |maxConcurrencyPerPartitionKeyRange    |   Eşzamanlılık bölüm anahtarı aralığının başına maksimum ölçüde, varsayılan değer 20 kullanılacak kitaplığı null ayarına neden olur.   |
   |maxInMemorySortingBatchSize    |    En fazla güncelleştirme öğe sayısı çekilen güncelleştirme öğeleri Numaralandırıcının her aşamadaki API çağrısı için toplu güncelleştirme önce bellek içi ön-işleme sıralama aşamada geçti, null ayarına min (updateItems.count, varsayılan değeri kullanılacak kitaplığı neden olur 1000000).     |
   | CancellationToken|Toplu güncelleştirme düzgün biçimde çıkmak için iptal belirteci. |

   **Toplu güncelleştirme yanıt nesne tanımı** aşağıdaki öznitelikleri toplu güncelleştirme API çağrısının sonucunu içerir:

   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |NumberOfDocumentsUpdated (uzun)    |   Toplu güncelleştirme API çağrısına sağlanan dosyalardan birkaçını başarıyla güncelleştirildi belgeleri toplam sayısı.      |
   |TotalRequestUnitsConsumed (çift)   |    Toplu güncelleştirme API çağrısı tarafından kullanılan toplam istek birimleri (RU).    |
   |TotalTimeTaken (TimeSpan)   | Toplu olarak geçen toplam süre yürütülmesinin tamamlanmasını API çağrısı güncelleştirin. |
    
## <a name="performance-tips"></a>Performans ipuçları 

BulkExecutor kitaplığı kullanırken daha iyi performans için aşağıdaki noktaları göz önünde bulundurun:

* En iyi performans için uygulamanızı Cosmos DB hesap yazma bölgeniz ile aynı bölgede olan bir Azure sanal makinesinden çalıştırın.  

* Belirli bir Cosmos DB koleksiyona karşılık gelen tek bir sanal makine içindeki tüm uygulama için tek bir BulkExecutor nesne örneği önerilir.  

* Bu yana bir tek toplu işlem API yürütme istemci makinenin CPU ve ağ GÇ büyük öbeğini kullanır. Birden çok görev oluşturma tarafından dahili olarak bu durum, yürütülen her toplu işlem API çağrıları, uygulama işlemi içinde birden çok eşzamanlı görev oluşturma kaçının. Tek bir sanal makine üzerinde çalışan tek toplu işlem API çağrısı tüm koleksiyonunuzun işleme tüketen kaydedemediği (varsa koleksiyonunuzun işleme > 1 milyon RU/s), kendi tercih eş zamanlı olarak toplu yürütmek için ayrı sanal makineler oluşturmak için işlemi API çağrıları.  

* InitializeAsync() hedef Cosmos DB koleksiyonu bölüm Haritası getirmek için bir BulkExecutor nesnesi örneği sonra çağrılan emin olun.  

* Uygulamanızın App.Config dosyasında olun **gcServer** daha iyi performans için etkin
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* Kitaplığı, bir günlük dosyasına veya konsol toplanan izlemeleri yayar. Her ikisi de etkinleştirmek için uygulamanızın App.Config aşağıdakileri ekleyin.

  ```xml
  <system.diagnostics>
    <trace autoflush="false" indentsize="4">
      <listeners>
        <add name="logListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="application.log" />
        <add name="consoleListener" type="System.Diagnostics.ConsoleTraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
```

## <a name="next-steps"></a>Sonraki adımlar
* Nuget paketi ayrıntıları hakkında bilgi edinin ve sürüm notları BulkExecutor .net kitaplığı için bkz:[BulkExecutor SDK ayrıntıları](sql-api-sdk-bulk-executor-dot-net.md). 
