---
title: Toplu olarak içeri aktarma ve güncelleştirme işlemlerinin Azure Cosmos DB'de toplu Yürütücü .NET kitaplığı kullanma
description: Toplu içeri aktarma ve Azure Cosmos DB belgeleri toplu Yürütücü .NET kitaplığını kullanarak güncelleştirin.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: cfb90dc31635001291b1661f31ec2ee1fc378404
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523350"
---
# <a name="use-bulk-executor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Azure Cosmos DB'de toplu işlemleri gerçekleştirmek için toplu Yürütücü .NET kitaplığı kullanma

Bu öğreticide Azure Cosmos DB'nin toplu Yürütücü kullanmaya ilişkin yönergeler almak ve Azure Cosmos DB kapsayıcısı için belgeleri güncelleştirmek için .NET kitaplığı sunulmaktadır. Toplu Yürütücü kitaplığı ve yüksek düzeyde işleme ve depolama yararlanmanıza nasıl yardımcı olduğunu öğrenmek için bkz. [toplu Yürütücü kitaplığına genel bakış](bulk-executor-overview.md) makalesi. Bu öğreticide, içeri aktarmalar rastgele toplu örnek bir .NET uygulama belgeleri bir Azure Cosmos DB kapsayıcısının içine oluşturulan görürsünüz. İçeri aktardıktan sonra bu belirli belge alanları üzerinde gerçekleştirilecek işlemleri düzeltme ekleri belirterek, nasıl toplu içeri aktarılan verileri güncelleştirmek gösterir. 

Şu anda toplu Yürütücü kitaplığı, Gremlin API hesapları yalnızca Azure Cosmos DB SQL API ile desteklenir. Bu makalede, SQL API hesaplarıyla toplu Yürütücü .NET kitaplığını kullanmayı açıklar. Toplu Yürütücü .NET kitaplığı ile Gremlin API kullanımı hakkında bilgi edinmek için [Azure Cosmos DB Gremlin API'SİNDE toplu işlemler gerçekleştirme](bulk-executor-graph-dotnet.md). 

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio 2017 yoksa, indirip kullanabilirsiniz [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Visual Studio Kurulumu sırasında Azure geliştirme etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun. 

* [Azure Cosmos DB’yi ücretsiz olarak](https://azure.microsoft.com/try/cosmosdb/) bir Azure aboneliği olmadan, ücretsiz ve herhangi bir taahhütte bulunmadan deneyebilirsiniz. Veya, kullanabileceğiniz [Azure Cosmos DB öykünücüsü'nü](https://docs.microsoft.com/azure/cosmos-db/local-emulator) ile `https://localhost:8081` uç noktası. Birincil Anahtar, [Kimlik doğrulama istekleri](local-emulator.md#authenticating-requests) bölümünde sağlanır.

* İçinde açıklanan adımları kullanarak bir Azure Cosmos DB SQL API hesabı oluşturma [veritabanı hesabı oluşturma](create-sql-api-dotnet.md#create-account) .NET hızlı başlangıç makalesi bölümü. 

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi Github'dan bazı örnek .NET uygulamalarını indirerek kod ile çalışmaya şimdi geçin. Bu uygulamalar, Azure Cosmos DB veriler üzerinde toplu işlem gerçekleştirin. Uygulamaları kopyalamak için bir komut istemi açın, aşağıdaki komutu çalıştırın ve bunları kopyalamak istediğiniz dizine gidin:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

Kopyalanan deponun içeren iki örnek "BulkImportSample" ve "BulkUpdateSample." Örnek uygulamalardan birini açın, Azure Cosmos DB hesabınızın ile bağlantı dizelerini App.config dosyasında bağlantı dizelerini güncelleştirmek, çözümü derleyin ve çalıştırın. 

"BulkImportSample" uygulama rastgele belgeler oluşturur ve bunları Azure Cosmos DB'ye toplu alır. "BulkUpdateSample" uygulama toplu içeri aktarılan belgeleri belirli belge alanları üzerinde gerçekleştirilecek işlemleri düzeltme ekleri belirterek güncelleştirir. Sonraki bölümlerde, bu örnek uygulamaları kod gözden geçirirsiniz.

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Azure Cosmos DB için toplu verileri içeri aktar

1. "BulkImportSample" klasörüne gidin ve "BulkImportSample.sln" dosyasını açın.  

2. Aşağıdaki kodda gösterildiği gibi Azure Cosmos DB bağlantı dizelerini App.config dosyasından alınır:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   Toplu içeri Aktarıcı, veritabanı adı, koleksiyon adı ve aktarım hızı değerleri App.config dosyasında belirtilen ile yeni bir veritabanı ve koleksiyonu oluşturur. 

3. Sonraki DocumentClient nesne ile doğrudan TCP bağlantı modu başlatılır:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. Bulkexecutor'a nesne bekleme süresi bir yüksek yeniden deneme değeri ile başlatılmış ve istek kısıtlanmış. Ve Tıkanıklık denetimi için yaşam süresi için Bulkexecutor'a geçirmek için 0 olarak ardından ayarlanırlar.  

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

5. Uygulamayı BulkImportAsync API'yi çağırır. .NET kitaplığı toplu iki aşırı yüklemesi API'sini - seri hale getirilmiş JSON belgelerinin listesini kabul eden bir içeri aktarma ve diğer seri durumdan çıkarılmış POCO belgelerin listesini kabul eder sağlar. Her birinin bu aşırı yüklenmiş yöntemler tanımları hakkında bilgi edinmek için bkz [API belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet).

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
   |enableUpsert    |   Upsert eder belgelerin etkinleştirmek için bir bayrak. Bir belge ile verilen kimliği zaten var, bunu güncelleştirilir. Varsayılan olarak false olarak ayarlanır.      |
   |disableAutomaticIdGeneration    |    Kimliği otomatik olarak oluşturulmasını devre dışı bırakmak için bayrak Varsayılan olarak ayarlanmış true.     |
   |maxConcurrencyPerPartitionKeyRange    | En büyük bölüm anahtar aralığı başına eşzamanlılık derecesini, null ayarına bir varsayılan değer 20 kitaplığı neden olur. |
   |maxInMemorySortingBatchSize     |  En fazla belge sayısına çekilen API'ye geçirilen belge Numaralandırıcı gelen her aşamasında çağırın.  Toplu içeri aktarmadan önce bellek içi ön işleme sıralama aşaması için varsayılan değeri min (documents.count, 1000000) kitaplığı null ayarına neden olur.       |
   |cancellationToken    |    Düzgün bir şekilde toplu olarak içeri aktarma çıkmak için iptal belirteci.     |

   **Toplu yanıt nesnesi tanımını içeri aktarma** aşağıdaki öznitelikleri toplu içeri aktarma API çağrısının sonucunu içerir:

   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |NumberOfDocumentsImported (uzun)   |  Toplu olarak sağlanan belgeleri dışında başarıyla içeri aktarıldı belgelerin toplam sayısı, API çağrısı içeri aktarın.       |
   |TotalRequestUnitsConsumed (çift)   |   Toplu tarafından tüketilen toplam istek birimi (RU) API çağrısı içeri aktarın.      |
   |TotalTimeTaken (zaman)    |   Toplu olarak içeri aktarma tarafından API çağrısı, yürütme tamamlamak için geçen toplam süre.      |
   |BadInputDocuments (liste\<Nesne >)   |     API çağrısı başarıyla toplu olarak içeri aktarılamadı, bozuk biçimli belgelerin listesini içeri aktarın. Kullanıcı, döndürülen belgelerin düzeltin ve içeri aktarmayı yeniden deneyin. Hatalı biçimlendirilmiş belgeleri kimliği değeri (null ya da başka herhangi bir veri türü geçersiz olarak kabul edilir) bir dize değil belgeleri içerir.    |

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Azure Cosmos DB'de toplu güncelleştirme verileri

Varolan belgeleri BulkUpdateAsync API'sini kullanarak güncelleştirebilirsiniz. Bu örnekte, ad alanına yeni bir değere ayarlayın ve açıklama alanı mevcut kaldırma. Eksiksiz bir desteklenen alan listesi için güncelleştirme, başvurmak [API belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet). 

1. "BulkUpdateSample" klasörüne gidin ve "BulkUpdateSample.sln" dosyasını açın.  

2. Karşılık gelen alan güncelleştirme işlemlerinin yanı sıra güncelleştirme öğeleri tanımlar. Bu örnekte ad alanı ve açıklama alanı tüm belgelerden kaldırma UnsetUpdateOperation güncelleştirilecek SetUpdateOperation kullanır. Diğer artış gibi bir belge alan belirli bir değere göre işlemleri, belirli değerleri bir dizi alanındaki anında iletme veya belirli bir değer bir dizi alanından kaldırın. Toplu güncelleştirme API'sı tarafından sağlanan farklı yöntemleri hakkında bilgi edinmek için başvurmak [API belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet).

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

3. Uygulamayı BulkUpdateAsync API'yi çağırır. BulkUpdateAsync yöntemin tanımı hakkında bilgi edinmek için bkz [API belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.ibulkexecutor.bulkupdateasync?view=azure-dotnet).  

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
   |maxConcurrencyPerPartitionKeyRange    |   En büyük bölüm anahtar aralığı başına eşzamanlılık derecesini, varsayılan değer 20 kitaplığı null ayarına neden olur.   |
   |maxInMemorySortingBatchSize    |    En fazla güncelleştirme öğe sayısını güncelleştirme öğeleri Numaralandırıcının çekilen önce toplu güncelleştirme bellek içi ön işleme sıralama aşaması için her bir aşamada API çağrısına geçirilen, null ayarına min (updateItems.count, varsayılan değeri kullanılacak kitaplık neden olur 1000000).     |
   | cancellationToken|Toplu güncelleştirme düzgün bir şekilde çıkmak için iptal belirteci. |

   **Toplu güncelleştirme yanıt nesne tanımı** aşağıdaki öznitelikleri toplu güncelleştirme API çağrısının sonucunu içerir:

   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |NumberOfDocumentsUpdated (uzun)    |   Toplu güncelleştirme API çağrısına sağlanan sürücüler dışında başarıyla güncelleştirildi belge toplam sayısı.      |
   |TotalRequestUnitsConsumed (çift)   |    Toplu güncelleştirme API çağrısı tarafından tüketilen toplam istek birimi (RU).    |
   |TotalTimeTaken (zaman)   | Yürütme tamamlanması API çağrısı tarafından toplu geçen toplam süreyi güncelleştirin. |
    
## <a name="performance-tips"></a>Performans ipuçları 

Toplu Yürütücü Kitaplığı kullanıldığında daha iyi performans için aşağıdaki noktaları göz önünde bulundurun:

* En iyi performans için Cosmos DB hesabı yazma bölgenizi aynı bölgede olan bir Azure sanal makinesinden uygulamanızı çalıştırın.  

* Belirli bir Cosmos DB kapsayıcısı için karşılık gelen tek bir sanal makine içindeki tüm uygulama için tek bir Bulkexecutor'a nesnesi örneklemek için önerilir.  

* Bu yana tek bir toplu işlem API yürütme istemcinin CPU ve ağ GÇ büyük bir yığın kullanır. Birden çok görev tarafından dahili olarak UNICODE böyle, yürütülen her toplu işlem API çağrıları, uygulama işlemi içinde birden çok eş zamanlı görevleri UNICODE özen göstermektir. Tek bir sanal makinede çalışan bir tek bir toplu işlem API çağrısı tüketen tüm kapsayıcının aktarım hızını kaydedemediği (varsa, kapsayıcının aktarım hızını > 1 milyon RU/sn), aynı anda yürütmek için ayrı sanal makineler oluşturmak için tercih edilir Toplu işlem API çağrıları.  

* InitializeAsync() hedef Cosmos DB kapsayıcısı bölüm haritasında getirilecek Bulkexecutor'a nesne başlatıldıktan sonra çağrılan emin olun.  

* Uygulamanızın App.Config dosyasında olun **gcServer** daha iyi performans için etkin
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* Kitaplığı, bir günlük dosyasına veya konsolunda toplanan izlemeleri yayar. İkisini de etkinleştirmek için uygulamanızın App.Config için aşağıdakileri ekleyin.

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
* Nuget paket ayrıntıları hakkında bilgi edinin ve sürüm notları toplu Yürütücü .NET Kitaplığı'nın için bkz:[toplu Yürütücü SDK ayrıntıları](sql-api-sdk-bulk-executor-dot-net.md). 
