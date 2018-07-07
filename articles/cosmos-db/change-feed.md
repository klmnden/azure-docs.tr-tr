---
title: Azure Cosmos DB'de çalışma değişiklik akışı desteği | Microsoft Docs
description: Azure Cosmos DB değişiklik akışı desteği, belgelerdeki değişiklikleri izlemek ve Tetikleyicileri gibi olay tabanlı işleme ve önbelleğe alır ve analiz sistemlerinin güncel tutarak gerçekleştirmek için kullanın.
keywords: akışı değiştirme
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: e53f1e62b9265d2eec2f49537cc05c865e1436f3
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902971"
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Azure Cosmos DB'de destek akış değişiklik ile çalışma

[Azure Cosmos DB](../cosmos-db/introduction.md) bir hızlı ve esnek genel veritabanı, perakende, oyun, IOT için uygundur ve işletimsel günlüğe kaydetme uygulamaları çoğaltılır. Bu uygulamalar bir ortak tasarım modelinde, ek eylemler istiyorsanız verilerde yapılan değişiklikleri kullanmaktır. Bu ek eylemler aşağıdakilerden biri olabilir: 

* Bir belge eklendiğinde veya değiştirildiğinde bir bildirim ya da bir API çağrısına tetikleniyor.
* Stream için IOT işleme veya Analiz gerçekleştirme.
* Ek veri taşıma, önbellek, arama motoru veya veri ambarı eşitleme veya soğuk depolama için veri arşivleme.

**Değişiklik akışı desteği** aşağıdaki görüntüde gösterildiği gibi Azure Cosmos DB'de her biri bu desenleri için etkili ve ölçeklenebilir çözümleri oluşturmanıza olanak sağlar:

![Güç gerçek zamanlı analiz ve olay temelli bilgi işlem senaryoları kullanarak Azure Cosmos DB değişiklik akışı](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Değişiklik akışı desteği, tüm veri modelleri ve Azure Cosmos DB kapsayıcıları için sağlanır. Ancak, değişiklik akışı SQL istemcisi kullanılarak okunur ve öğeleri JSON biçimine serileştiren. Biçimlendirme, JSON nedeniyle istemciler yaşar MongoDB BSON biçimlendirilmiş belgeleri ve JSON arasında bir uyuşmazlık değişiklik akışı biçimlendirilmiş.

Aşağıdaki videoda, Azure Cosmos DB Program Yöneticisi Manager Andrew Liu nasıl çalışır Azure Cosmos DB değişiklik akışı gösterilmektedir.

> [!VIDEO https://www.youtube.com/embed/mFnxoxeXlaU]
>
>

## <a name="how-does-change-feed-work"></a>Nasıl değişiklik iş akışı?

Azure Cosmos DB geliştirilme akış desteği, herhangi bir değişiklik için bir Azure Cosmos DB koleksiyonu için dinleyerek değiştirin. Ardından, değiştirilmiş olan sırayla değiştirilen belgelerin sıralanmış listesini çıkarır. Değişiklikleri kalıcı, zaman uyumsuz olarak ve artımlı olarak işlenebilir ve çıkış paralel işleme için bir veya daha fazla tüketicileri arasında dağıtılabilir. 

Bu makalenin sonraki bölümlerinde açıklandığı gibi üç farklı yolla akış değişiklik okuyabilirsiniz:

*   [Azure işlevleri'ni kullanarak](#azure-functions)
*   [Azure Cosmos DB SDK'sını kullanma](#sql-sdk)
*   [Kullanarak Azure Cosmos DB değişiklik akışı işlemci kitaplığı](#change-feed-processor)

Değişiklik akışı her bölüm anahtar aralığı içinde belge koleksiyonu için kullanılabilir ve bu nedenle paralel işleme için bir veya daha fazla tüketicileri arasında aşağıdaki görüntüde gösterildiği gibi dağıtılabilir.

![Azure Cosmos DB değişiklik akışı, dağıtılan işleme](./media/change-feed/changefeedvisual.png)

Ek ayrıntılar:
* Değişiklik akışı, tüm hesaplar için varsayılan olarak etkindir.
* Kullanabileceğiniz, [sağlanan aktarım hızı](request-units.md) yazma bölgenizi ya da tüm [bölgesinde](distribute-data-globally.md) değişiklik akışı okumak için olduğu gibi başka bir Azure Cosmos DB işlem.
* Değişiklik akışı, ekler ve koleksiyonu içindeki belgeler yapılan güncelleştirme işlemlerini içerir. Siler yakalayabilirsiniz siler yerine, belgelerinizi içinde "geçici silme" bayrak ayarlayarak. Alternatif olarak, sınırlı bir süre için belgelerinizi ayarlayabilirsiniz [TTL özelliği](time-to-live.md), örneğin, 24 saat ve silmeleri yakalamak için bu özelliğin değerini kullanın. Bu çözüm sayesinde TTL sona erme süresinden daha kısa bir zaman aralığında değişikliklerini işlemesi gerekir.
* Her değişiklik için bir belge değişiklik akışı tam bir kez görünür ve bunların denetim noktası mantığına istemcileri yönetme. Değişiklik akışı işlemci kitaplığı otomatik denetim noktası oluşturma ve "en az bir kez" semantiği sağlar.
* Belirli bir belge için yalnızca en son değişikliği değişiklik günlüğünde bulunur. Ara değişikliklerin kullanılamayabilir.
* Değişiklik akışı, her bölüm anahtarı değeri içinde değişiklik göre sıralanır. Bölüm anahtarı değerleri arasında yürütülme sırası yoktur.
* Değişiklikleri tüm-belirli bir noktaya eşitlenebilir, diğer bir deyişle, değişiklikler kullanılabilir sabit veri saklama dönemi yoktur.
* Değişiklikler bölüm anahtar aralığı bir öbekler halinde kullanılabilir. Bu özellik paralel olarak birden fazla tüketici/sunucuları tarafından işlenmek üzere, büyük koleksiyonların değişiklik izin verir.
* Uygulama, aynı koleksiyon üzerinde aynı anda birden çok değişiklik akışlarından talep edebilir.
* ChangeFeedOptions.StartTime kullanılabilir ilk bir başlangıç noktası, örneğin sağlamak için karşılık gelen saatin devamlılık belirteci bulunamadı. ContinuationToken belirtilmişse StartTime ve StartFromBeginning değerlerin kazanır. ~ 5 saniye duyarlığını ChangeFeedOptions.StartTime olur. 

## <a name="use-cases-and-scenarios"></a>Kullanım örnekleri ve senaryoları

Değişiklik akışı, yüksek hacimli yazma ile büyük veri kümelerini verimli işlenmesini sağlar ve nelerin değiştiğini belirlemek için bir veri kümesinin tamamında sorgulama için bir alternatif sunar. 

Örneğin, bir değişiklik akışı, aşağıdaki görevleri verimli bir şekilde gerçekleştirebilirsiniz:

* Azure Cosmos DB'de depolanan verilerle bir cache, search dizini veya bir veri ambarı'nı güncelleştirin.
* Uygulama düzeyi verileri katmanlama ve Arşiv uygulamak, diğer bir deyişle, "Sık erişimli veriler" Azure Cosmos DB'de depolamak ve için "soğuk verileri" Yaş [Azure Blob Depolama](../storage/common/storage-introduction.md) veya [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Farklı bir bölümleme düzeni ile başka bir Azure Cosmos DB hesabına sıfır kapalı kalma süresini geçişler gerçekleştirin.
* Uygulama [lambda işlem hatları azure'da](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) Azure Cosmos DB ile. Azure Cosmos DB hem alma hem de sorgu işleme ve daha düşük ile lambda mimarisi uygulama ölçeklenebilir veritabanı çözümü sağlar. 
* Bu olaylar ile gerçek zamanlı işleme alma ve cihaz, sensör, altyapı ve uygulamalardan gelen olay verilerini [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/storm/apache-storm-overview.md), veya [Apache Spark](../hdinsight/spark/apache-spark-overview.md). 

Aşağıdaki görüntüde, her ikisi de alıp Azure Cosmos DB kullanarak sorgu kullanabilirsiniz lambda işlem hatları akış desteği nasıl değiştiğini gösterir: 

![Kesintisiz alım ve sorgu için Azure Cosmos DB tabanlı lambda işlem hattı](./media/change-feed/lambda.png)

Ayrıca, içinde [sunucusuz](http://azure.com/serverless) , web ve mobil uygulamalar, müşterinizin profili, tercihlerine veya konum 'nıkullanarakcihazlarınıanındailetmebildirimlerigöndermegibibelirlieylemleritetiklemekiçindeğişikliklergibiolaylarıizlemekiçin[Azure işlevleri](#azure-functions). Bir oyun oluşturmak için Azure Cosmos DB kullanıyorsanız, şunları yapabilirsiniz, örneğin, kullanım değişiklik akışı tamamlanmış oyunlardan puanları göre gerçek zamanlı puan tabloları uygulamak için.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Azure işlevleri'ni kullanarak 

Azure işlevleri'ni kullanıyorsanız, en basit yolu için bir Azure Cosmos DB değişiklik akışı bağlanmak için Azure işlevleri uygulamanızı bir Azure Cosmos DB tetikleyicisi eklemektir. Bir Azure işlev uygulaması bir Azure Cosmos DB tetikleyicisi oluşturduğunuzda, bağlanmak için Azure Cosmos DB koleksiyonu seçin ve her koleksiyona bir değişiklik yapıldığında işlevi tetiklenir. 

Azure işlevleri Portalı'nda Tetikleyiciler oluşturulabilir Azure Cosmos DB portalında veya programlama yoluyla. Daha fazla bilgi için [Azure Cosmos DB: Azure işlevleri ile sunucusuz veritabanı computing](serverless-computing-database.md).

<a id="sql-sdk"></a>
## <a name="using-the-sdk"></a>SDK’yı kullanarak

[SQL SDK'sı](sql-api-sdk-dotnet.md) için Azure Cosmos DB, okuma ve bir değişiklik akışı yönetmek için tüm güç sağlar. Ancak büyük güç, çok sayıda sorumlulukları gelir. Kontrol noktalarını yönetme, belge sırası numaralarıyla Dağıt ve bölüm anahtarları üzerinde ayrıntılı denetime sahip istiyorsanız, daha sonra SDK'sını kullanarak aldığı doğru yaklaşım olabilir.

Bu bölümde bir değişiklik akışı ile çalışmak için SQL SDK'sını kullanma konusunda yol göstermektedir.

1. Appconfig aşağıdaki kaynakları okuyarak başlatın. Uç nokta ve yetkilendirme anahtarını alma yönergeleri kullanılabilir [bağlantı dizenizi güncelleştirme](create-sql-api-dotnet.md#update-your-connection-string).

    ``` csharp
    DocumentClient client;
    string DatabaseName = ConfigurationManager.AppSettings["database"];
    string CollectionName = ConfigurationManager.AppSettings["collection"];
    string endpointUrl = ConfigurationManager.AppSettings["endpoint"];
    string authorizationKey = ConfigurationManager.AppSettings["authKey"];
    ```

2. İstemci gibi oluşturun:

    ```csharp
    using (client = new DocumentClient(new Uri(endpointUrl), authorizationKey,
    new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    {
    }
    ```

3. Bölüm anahtar aralığı alın:

    ```csharp
    FeedResponse pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri,
        new FeedOptions
            {RequestContinuation = pkRangesResponseContinuation });
     
    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    ```

4. Çağrı ExecuteNextAsync için her bölüm anahtar aralığı:

    ```csharp
    foreach (PartitionKeyRange pkRange in partitionKeyRanges){
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);
        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collectionUri,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = -1,
                // Set reading time: only show change feed results modified since StartTime
                StartTime = DateTime.Now - TimeSpan.FromSeconds(30)
            });
        while (query.HasMoreResults)
            {
                FeedResponse<dynamic> readChangesResponse = query.ExecuteNextAsync<dynamic>().Result;
    
                foreach (dynamic changedDocument in readChangesResponse)
                    {
                         Console.WriteLine("document: {0}", changedDocument);
                    }
                checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
            }
    }
    ```

> [!NOTE]
> Yerine `ChangeFeedOptions.PartitionKeyRangeId`, kullanabileceğiniz `ChangeFeedOptions.PartitionKey` değişiklik akışı alınacağı tek bölüm anahtarı belirtmek için. Örneğin, `PartitionKey = new PartitionKey("D8CFA2FD-486A-4F3E-8EA6-F3AA94E5BD44")`.
> 
>

Birden fazla okuyucuyu kapsayacak varsa, kullanabileceğiniz **ChangeFeedOptions** farklı iş parçacıkları veya farklı istemcilerin okuma yükünü dağıtmak için.

Ve İşte bu kadar bu birkaç kod satırıyla, değişiklik akışı okuma başlayabilirsiniz. Bu makalede kullanılan tam kod alabilirsiniz [GitHub deposunu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed).

Yukarıdaki 4. adımdaki kod **ResponseContinuation** bu sıra numarası sonra yeni belgeleri okuyun sonraki açışınızda kullanacağınız belgenin son mantıksal sıra numarası (LSN) son satırı içeriyor. Kullanarak **StartTime** , **ChangeFeedOption** belgeleri almak için net genişletebilirsiniz. Yani, **ResponseContinuation** null; ancak, **StartTime** beri değiştirilen tüm belgelerin olacağı süre döner **StartTime**. Ancak, Eğer, **ResponseContinuation** sistem tüm belgelerin bu yana bu LSN olacağı bir değere sahip.

Bu nedenle, denetim noktası diziniz yalnızca her bölüm için LSN engelliyor. Ancak bu bölümleri ile uğraşmak istemiyorsanız, kontrol noktaları, LSN, başlangıç zamanı, vb. Basit seçeneği değişiklik akışı işlemci kitaplığı kullanmaktır.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Kullanarak değişiklik akışı işlemci kitaplığı 

[Azure Cosmos DB değişiklik akışı işlemci Kitaplığı](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) olay işleme çeşitli tüketicilere kolayca dağıtmak yardımcı olabilir. Bu kitaplık, bölümler ve paralel olarak çalışan birden çok iş parçacığı üzerinde okuma değişiklikleri basitleştirir.

Değişiklik akışı işlemci kitaplığı ana avantajı, her bölüm yönetmek zorunda olmadığınız ve devamlılık belirteci ve her bir koleksiyon el ile yoklamaya yoksa ' dir.

Değişiklik akışı işlemci kitaplığı, bölümler ve paralel olarak çalışan birden çok iş parçacığı üzerinde okuma değişiklikleri basitleştirir.  Kiralama mekanizması kullanılarak bölümler arasında okuma değişiklikleri otomatik olarak yönetir. Değişiklik akışı işlemci kitaplığı kullanan iki istemciler başlatırsanız, aşağıdaki görüntüde görebileceğiniz gibi bunlar kendi aralarında iş bölün. İstemcilerin artmaya devam ederken, kendi aralarında iş bölme tutun.

![Azure Cosmos DB değişiklik akışı, dağıtılan işleme](./media/change-feed/change-feed-output.png)

Sol istemci ilk olarak başlatıldığından ve tüm bölümleri ve ardından ikinci bir istemci başlatıldı ve ardından ilk izin bazı ikinci istemci kiraları Git izleme başlatıldı. Bu gördüğünüz gibi farklı makinelerde ve istemciler arasındaki iş dağıtmak için kullanışlı yoludur.

Aynı koleksiyona izleme ve iki işlev nasıl işlemci kitaplığı için öğe bölümleri karar bağlı bağlı olarak farklı belgeler alabilirsiniz aynı kira kullanılarak iki sunucusuz Azure funtions varsa unutmayın.

<a id="understand-cf"></a>
### <a name="understanding-the-change-feed-processor-library"></a>Anlama değişiklik akışı işlemci kitaplığı

Değişiklik akışı işlemci kitaplığı uygulama dört ana bileşen vardır: izlenen koleksiyonu, kira koleksiyonu, işleyicisi ana bilgisayarı ve tüketiciler. 

> [!WARNING]
> Koleksiyon oluşturmanın bir fiyatı vardır çünkü Azure Cosmos DB'yle iletişim kurması için uygulamaya aktarım hızı ayırırsınız. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin
> 
> 

**İzlenen koleksiyonu:** değişiklik akışı oluşturulan veriler izlenen koleksiyonudur. Tüm eklemeleri ve değişiklikleri izlenen koleksiyon için koleksiyon değişiklik akışını yansıtılır. 

**Kira koleksiyonu:** değişiklik arasında birden fazla çalışana akışı işleme kira koleksiyonu koordinatlar. Ayrı bir koleksiyon ile bir kira bölüm başına kiraları depolamak için kullanılır. Değişiklik akışı işlemci çalıştığı için yazma bölgesi yakın farklı bir hesapla bu kira koleksiyonu depolamak için avantajlıdır. Bir kira nesnesi aşağıdaki öznitelikleri içerir: 
* Sahibi: kira sahibi olan konak belirtir
* Devamlılık: belirli bir bölüm için akış değişikliği (devamlılık belirteci) konumu belirtir.
* Zaman damgası: kira güncelleştirildi; son saat zaman damgası, kiralama süresi olarak kabul edilip edilmediğini kontrol etmek için kullanılabilir 

**İşleyicisi ana bilgisayarı:** her ana bilgisayar işlemi konakların kaç tane Etkin kiralar olmadığına göre kaç bölümlerini belirler. 
1.  Bir ana bilgisayar başlatıldığında tüm konaklar arasında iş yükünü dengelemek için kiraları alır. Kira etkin şekilde bir konak kiralama, düzenli aralıklarla yeniler. 
2.  Bir ana bilgisayar denetim noktaları son kirasını her devamlılık belirteci okuyun. Eşzamanlılık güvenliği sağlamak için her bir kira güncelleştirme ETag bir konak denetler. Diğer denetim noktası stratejileri de desteklenir.  
3.  Kapatma, bağlı bir konak tüm kira serbest bırakır, ancak depolanan denetim noktası daha sonra okumaya devam edebilmek için devamlılık bilgileri tutar. 

Şu anda ana bilgisayar sayısı bölüm (kiraları) sayısından büyük olamaz.

**Tüketiciler:** tüketiciler veya çalışanlar, olan değişiklik akışı, her konak tarafından başlatılan işlemleri gerçekleştiren iş parçacıkları. Her işleyicisi ana bilgisayarı, birden fazla tüketici olabilir. Her tüketici, değişiklik atandığı bölümünden akışı ve değişiklikleri ana bilgisayar bildirir ve kiralama süresi okur.

Daha fazla değişiklik akışı nasıl bu dört öğeden anlamak için işlemci iş birlikte bakalım, aşağıdaki diyagramda bir örnek. İzlenen koleksiyonu, belgeleri depolayan ve "city" Bölüm anahtarı olarak kullanır. Mavi bölüm "A-E" "city" alanından belgelerle vb. içeren görüyoruz. Her iki tüketici paralel dört bölümden okuma içeren iki ana vardır. Oklar, değişiklik akışı belirli bir noktada okuma tüketiciler gösterir. Açık mavi değişiklik akışı zaten okuma değişiklikleri temsil ederken, ilk bölümü, koyu mavi okunmamış değişiklikleri temsil eder. Ana bilgisayarlar, her bir tüketicinin geçerli okuma konumunu izlemek için bir "Devam" değerini depolamak için kira koleksiyonu kullanın. 

![Kullanarak Azure Cosmos DB değişiklik akışı işlemci konak](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Değişiklik akışı işlemci kitaplığı ile çalışma

İşlemci NuGet paketi yükleme değişiklik akışı önce ilk yükleyin: 

* Microsoft.Azure.DocumentDB, en son sürümü.
* Newtonsoft.Json, en son sürümü

Yüklemeyi [Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget paketini](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) ve bir başvuru olarak eklediğiniz.

Yapmanız gereken değişiklik akışı işlemci kitaplığı uygulamak için aşağıdaki:

1. Uygulama bir **DocumentFeedObserver** uygulayan nesne **IChangeFeedObserver**.
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;
    using Microsoft.Azure.Documents.Client;

    /// <summary>
    /// This class implements the IChangeFeedObserver interface and is used to observe 
    /// changes on change feed. ChangeFeedEventHost will create as many instances of 
    /// this class as needed. 
    /// </summary>
    public class DocumentFeedObserver : IChangeFeedObserver
    {
    private static int totalDocs = 0;

        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserver" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        /// <param name="client"> Client connected to destination collection </param>
        /// <param name="destCollInfo"> Destination collection information </param>
        public DocumentFeedObserver()
        {
            
        }

        /// <summary>
        /// Called when change feed observer is opened; 
        /// this function prints out observer partition key id. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task OpenAsync(IChangeFeedObserverContext context)
        {
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine("Observer opened for partition Key Range: {0}", context.PartitionKeyRangeId);
            return Task.CompletedTask;
        }

        /// <summary>
        /// Called when change feed observer is closed; 
        /// this function prints out observer partition key id and reason for shut down. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <param name="reason">Specifies the reason the observer is closed.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task CloseAsync(IChangeFeedObserverContext context, ChangeFeedObserverCloseReason reason)
        {
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("Observer closed, {0}", context.PartitionKeyRangeId);
            Console.WriteLine("Reason for shutdown, {0}", reason);
            return Task.CompletedTask;
        }

        public Task ProcessChangesAsync(IChangeFeedObserverContext context, IReadOnlyList<Document> docs, CancellationToken cancellationToken)
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Change feed: PartitionId {0} total {1} doc(s)", context.PartitionKeyRangeId, Interlocked.Add(ref totalDocs, docs.Count));
            foreach (Document doc in docs)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine(doc.Id.ToString());
            }

            return Task.CompletedTask;
        }
    }
    ```

2. Uygulama bir **DocumentFeedObserverFactory**, uygulayan bir **IChangeFeedObserverFactory**.
    ```csharp
     using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;

    /// <summary>
    /// Factory class to create instance of document feed observer. 
    /// </summary>
    public class DocumentFeedObserverFactory : IChangeFeedObserverFactory
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserverFactory" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        public DocumentFeedObserverFactory()
        {
        }

        /// <summary>
        /// Creates document observer instance with client and destination collection information
        /// </summary>
        /// <returns>DocumentFeedObserver with client and destination collection information</returns>
        public IChangeFeedObserver CreateObserver()
        {
            DocumentFeedObserver newObserver = new DocumentFeedObserver();
            return newObserver as IChangeFeedObserver;
        }
    }
    ```

3. Tanımlama *CancellationTokenSource* ve *ChangeFeedProcessorBuilder*

    ```csharp
    private readonly CancellationTokenSource cancellationTokenSource = new CancellationTokenSource();
    private readonly ChangeFeedProcessorBuilder builder = new ChangeFeedProcessorBuilder();
    ```

5. derleme **ChangeFeedProcessorBuilder** sonra ilgili nesneleri tanımlama 

    ```csharp
            string hostName = Guid.NewGuid().ToString();
      
            // monitored collection info 
            DocumentCollectionInfo documentCollectionInfo = new DocumentCollectionInfo
            {
                Uri = new Uri(this.monitoredUri),
                MasterKey = this.monitoredSecretKey,
                DatabaseName = this.monitoredDbName,
                CollectionName = this.monitoredCollectionName
            };
            
            DocumentCollectionInfo leaseCollectionInfo = new DocumentCollectionInfo
                {
                    Uri = new Uri(this.leaseUri),
                    MasterKey = this.leaseSecretKey,
                    DatabaseName = this.leaseDbName,
                    CollectionName = this.leaseCollectionName
                };
            DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory();
            ChangeFeedOptions feedOptions = new ChangeFeedOptions();

            /* ie customize StartFromBeginning so change feed reads from beginning
                can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
            */

            feedOptions.StartFromBeginning = true;
        
            ChangeFeedProcessorOptions feedProcessorOptions = new ChangeFeedProcessorOptions();

            // ie. customizing lease renewal interval to 15 seconds
            // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
            feedProcessorOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

            this.builder
                .WithHostName(hostName)
                .WithFeedCollection(documentCollectionInfo)
                .WithLeaseCollection(leaseCollectionInfo)
                .WithProcessorOptions (feedProcessorOptions)
                .WithObserverFactory(new DocumentFeedObserverFactory());               
                //.WithObserver<DocumentFeedObserver>();  If no factory then just pass an observer

            var result =  await this.builder.BuildAsync();
            await result.StartAsync();
            Console.Read();
            await result.StopAsync();    
    ```

İşte bu kadar. Bu adımlarda sonra belgeleri içine görünmeye başlar **DocumentFeedObserver.ProcessChangesAsync** yöntemi.

Yukarıdaki kodu farklı türde nesneler ve onların etkileşimi göstermek gösterim amacıyla ' dir. Uygun değişkenleri tanımlayın ve bunları doğru değerlerle başlatmak gerekir. Bu makalede kullanılan tam kod alabilirsiniz [GitHub deposunu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessorV2).

> [!NOTE]
> Hiçbir zaman, kodunuzda veya kod gösterildiği gibi yapılandırma dosyasında bir ana anahtar olması gerekir. Lütfen [Key-Vault kuralından anahtarları kullanmak nasıl](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/).


## <a name="faq"></a>SSS

### <a name="what-are-the-different-ways-you-can-read-change-feed-and-when-to-use-each-method"></a>Değişiklik akışı okuma farklı yolları nelerdir? ve her yöntemi kullanmak ne zaman?

Değişiklik akışı okumanız için üç seçenek vardır:

* **[Azure Cosmos DB SQL API .NET SDK'sını kullanma](#sql-sdk)**
   
   Bu yöntemi kullanarak, değişiklik akışı denetimde düşük düzeyde alın. Denetim noktası yönetebilir, belirli bir bölüm anahtarı vb. erişebilir. Birden fazla okuyucuyu kapsayacak varsa, kullanabileceğiniz [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) farklı iş parçacıkları veya farklı istemcilerin okuma yükünü dağıtmak için. .

* **[Kullanarak Azure Cosmos DB değişiklik akışı işlemci kitaplığı](#change-feed-processor)**

   Değişiklik akışı karmaşıklığını çok fazla dış istiyorsanız, değişiklik akışı işlemci kitaplığı kullanabilirsiniz. Bu kitaplığı çok sayıda karmaşıklığını gizler, ancak yine de değişiklik akışını, üzerinde tam denetim verir. Bu kitaplık izleyen bir [gözlemci deseni](https://en.wikipedia.org/wiki/Observer_pattern), işleme işlevinizi SDK tarafından çağrılır. 

   Bir yüksek aktarım hızı değişiklik akışı varsa, değişiklik akışını okumak için birden çok istemci örneği oluşturabilir. "Değişiklik akışı işlemci kitaplığı" kullandığından, otomatik olarak yükü farklı istemciler arasında böler. Herhangi bir şey gerekmez. Tüm karmaşıklığı SDK tarafından yapılır. Yük dengeleyicinizi olmasını istiyorsanız, ancak daha sonra IParitionLoadBalancingStrategy özel bölüm stratejisi uygulayabilir. Özel işleme değişikliklerin bir bölüme IPartitionProcessor – uygulayın. Ancak, SDK'sı ile bir bölüm aralığı işleyebilir ancak belirli bir bölüm anahtarı işlemek isterseniz daha sonra SDK'sı SQL API'si için kullanmak zorunda.

* **[Azure işlevleri'ni kullanarak](#azure-functions)** 
   
   Son seçenek Azure işlevi en basit bir seçenektir. Bu seçeneği kullanmanızı öneririz. Bir Azure işlev uygulaması bir Azure Cosmos DB tetikleyicisi oluşturduğunuzda, bağlanmak için Azure Cosmos DB koleksiyonu seçin ve her koleksiyona bir değişiklik yapıldığında işlevi tetiklenir. İzleme bir [ekran atama](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) değişiklik akışını ve Azure'ı kullanarak bu işlevi

   Azure işlevleri Portalı'nda Tetikleyiciler oluşturulabilir Azure Cosmos DB portalında veya programlama yoluyla. Visual Studio ve VS Code Azure işlevi yazmak için mükemmel destek vardır. Yazma ve masaüstünüzde kodda hata ayıklama ve ardından işlev tek bir tıklamayla dağıtın. Daha fazla bilgi için [Azure Cosmos DB: Azure işlevleri ile sunucusuz veritabanı computing](serverless-computing-database.md) makalesi.

### <a name="what-is-the-sort-order-of-documents-in-change-feed"></a>Değişiklik akışı belgelerde sıralama düzeni nedir?

Değişiklik akışı belgeleri kendi değiştirme saati sırasına göre sunulur. Bu sıralama düzeni yalnızca bölüm başına garanti edilir.

### <a name="for-a-multi-region-account-what-happens-to-the-change-feed-when-the-write-region-fails-over-does-the-change-feed-also-failover-would-the-change-feed-still-appear-contiguous-or-would-the-fail-over-cause-change-feed-to-reset"></a>Bir çok bölgeli hesabı için yazma bölgesi başarısız-üzerinden zaman akışı değişikliğin ne olur? Değişiklik de yük devretme akışı mu? Değişiklik hala akışı bitişik görünür veya yük devretme nedeni değişiklik akışını sıfırlamak için?

Evet, değişiklik akışı el ile yük devretme işlemi çalışır ve bitişik olacaktır.

### <a name="how-long-change-feed-persist-the-changed-data-if-i-set-the-ttl-time-to-live-property-for-the-document-to--1"></a>Ne kadar süreyle değişiklik akışı persist değiştirilen verileri belge TTL (yaşam süresi) özelliği -1 olarak ayarlarsanız?

Değişiklik akışı her zaman açık kalır. Veriler silinmez, değişiklik akışı içinde kalır.

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region-as-change-feed-is-available-in-all-the-read-regions-by-default"></a>Değişiklik akışı, varsayılan olarak tüm okuma bölgelerinde kullanılabilir olduğu gibi belirli bir bölgeden okumak için Azure işlevleri nasıl yapılandırabilirim?

Şu anda Azure işlevleri, belirli bir bölgeden okumak için yapılandırmak mümkün değildir. Herhangi bir Azure Cosmos DB bağlama ve tetikleme tercih edilen bölge ayarlamak için Azure işlevleri depoda bir GitHub sorunu yoktur.

Azure işlevleri, varsayılan bağlantı İlkesi kullanır. Azure işlevleri'nde ve varsayılan olarak bağlantı modu yapılandırma, aynı bölgede bulunan Azure işlevleri birlikte bulunacak en iyisidir yazma bölgesinden okur.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>Azure işlevleri'nde toplu varsayılan boyutu nedir?

Azure işlevleri'nin her çağırma 100 belgeleri. Ancak, bu sayı function.json dosyasında yapılandırılabilir. İşte tam [yapılandırma seçeneklerinin listesi](../azure-functions/functions-run-local.md). Yerel olarak geliştiriyorsanız, uygulama ayarları içinde güncelleştirme [local.settings.json](../azure-functions/functions-run-local.md) dosya.

### <a name="i-am-monitoring-a-collection-and-reading-its-change-feed-however-i-see-i-am-not-getting-all-the-inserted-document-some-documents-are-missing-what-is-going-on-here"></a>Bir koleksiyon izleme ve okuma değişiklik akışı, ancak tüm ekli belge almıyorum görüyorum, bazı belgeler eksik. Ne anlama geliyor?

Aynı koleksiyon ile aynı kira koleksiyonu okuma hiçbir işlev olduğundan emin olun. Bana oldu ve daha sonra Ayrıca aynı kira kullanarak my diğer Azure işlevleri tarafından işlenen eksik belgeleri miyim gerçekleşmiş.

Okunacak birden çok Azure işlevleri oluşturuyorsanız farklı kira koleksiyonu kullanın veya aynı koleksiyona paylaşmak için "leasePrefix" yapılandırmasını kullanın. Bu nedenle, aynı akışı değiştirin. Ancak, değişiklik akışı işlemci kitaplığı kullandığınızda, işlevinizi birden çok örneğini başlayabilir ve SDK Belgeleri otomatik olarak farklı örnekleri arasında böler.

### <a name="my-document-is-updated-every-second-and-i-am-not-getting-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Saniyede Uygulamam belge güncelleştirilir ve tüm değişiklikleri Azure işlevleri'nde değişiklik akışını dinleme almıyorum.

5 saniye arasında yapılan tüm değişiklikler kaybedilir için azure işlevleri yoklamalar değişiklik her 5 saniyede akışı. 5 değişikliğin belgede erişmenizi sağlayacak şekilde azure Cosmos DB, tek bir sürüm 5 saniyede depolar. Ancak, 5 saniye gidin ve değişiklik saniyede akışı yoklamak istediğiniz istiyorsanız, yoklama süresi "feedPollTime" yapılandırmak, bakın [Azure Cosmos DB bağlamaları](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). Varsayılan değer 5000 milisaniye cinsinden tanımlanır. Daha fazla CPU yazma başlayacak şekilde olası ancak önerilir, 1 saniye.

### <a name="i-inserted-a-document-in-the-mongo-api-collection-but-when-i-get-the-document-in-change-feed-it-shows-a-different-id-value-what-is-wrong-here"></a>Mongo API koleksiyonda bir belge ekledim ancak belgenin içinde değişiklik akışı aldığınızda farklı kimliği değerini gösterir. Burada sorun nedir?

Koleksiyonunuz Mongo API koleksiyonudur. Değişiklik akışı bir SQL istemcisi kullanılarak okunur ve öğeleri JSON biçimine serileştiren unutmayın. Biçimlendirme, JSON nedeniyle istemciler yaşar MongoDB BSON biçimlendirilmiş belgeleri ve JSON arasında bir uyuşmazlık değişiklik akışı biçimlendirilmiş. Sizin gördüğünüz BSON belgeye JSON gösterimidir. Mongo hesaplarında ikili öznitelikler kullanıyorsa, bunlar JSON'a dönüştürülür.

### <a name="is-there-a-way-to-control-change-feed-for-updates-only-and-not-inserts"></a>Değişiklik akışı yalnızca güncelleştirmeleri denetlemek için bir yol yoktur ve değil ekler?

Bugün değil, ancak bu işlev, yol haritası kapsamındadır. Bugün, üzerinde belge güncelleştirmeleri için yazılım işaretleyici ekleyebilirsiniz.

### <a name="is-there-a-way-to-get-deletes-in-change-feed"></a>Değişiklik akışı silmeleri almak için bir yol var mı?

Değişiklik akışı siler oturum şu anda değil. Değişiklik akışı sürekli olarak geliştirmek ve bu işlev, yol haritası kapsamındadır. Bugün, belge silme için geçici bir işaretleyici ekleyebilirsiniz. Bir öznitelik "silindi" adlı "true" olarak ayarlayın ve otomatik olarak silinebilir belge üzerinde bir TTL ayarlamak belge ekleyin.

### <a name="can-i-read-change-feed-for-historic-documentsfor-example-documents-that-were-added-5-years-back-"></a>Ben, değişiklik akışı geçmiş belgeleri (örneğin, 5 yıl eklenmiş olan belgeleri) okuyabilir miyim?

Belge silinmedi, Evet değişiklik okuyabilirsiniz koleksiyonunuzun kaynağı sunulan ürünün kendinde akış.

### <a name="can-i-read-change-feed-using-javascript"></a>Ben, JavaScript kullanarak değişiklik akışı okuyabilir miyim?

Evet, Node.js SDK'sı ilk değişiklik akışı desteği kısa süre önce eklenir. Aşağıdaki örnekte, kod çalıştırmadan önce lütfen geçerli sürüme güncelleştirme documentdb modülünü gösterildiği gibi kullanılabilir:

```js

var DocumentDBClient = require('documentdb').DocumentClient;
const host = "https://your_host:443/";
const masterKey = "your_master_key==";
const databaseId = "db";
const collectionId = "c1";
const dbLink = 'dbs/' + databaseId;
const collLink = dbLink + '/colls/' + collectionId;
var client = new DocumentDBClient(host, { masterKey: masterKey });
let options = {
    a_im: "Incremental feed",
    accessCondition: {
        type: "IfNoneMatch",        // Use: - empty condition (or remove accessCondition entirely) to start from beginning.
        //      - '*' to start from current.
        //      - specific etag value to start from specific continuation.
        condition: ""
    }
};
 
var query = client.readDocuments(collLink, options);
query.executeNext((err, results, headers) =&gt; {
    // Now we have headers.etag, which can be used in next readDocuments in accessCondition option.
    console.log(results);
    console.log(headers.etag);
    console.log(results.length);
    options.accessCondition = { type: "IfNoneMatch", condition: headers.etag };
    var query = client.readDocuments(collLink, options);
    query.executeNext((err, results, headers) =&gt; {
        console.log("next one:", results[0]);
    });
});<span id="mce_SELREST_start" style="overflow:hidden;line-height:0;"></span>

```

### <a name="can-i-read-change-feed-using-java"></a>Ben, Java kullanarak değişiklik akışı okuyabilir miyim?

Java kitaplığı, değişiklik akışı okumak için kullanılabilir [Github deposu](https://github.com/Azure/azure-documentdb-changefeedprocessor-java). Ancak, şu anda Java kitaplık .NET kitaplığı arkasına birkaç sürümleri vardır. Yakında hem kitaplıkları eşit olacaktır.

### <a name="can-i-use-etag-lsn-or-ts-for-internal-bookkeeping-which-i-get-in-response"></a>Yanıtta alabilirim iç muhasebe için _etag, _lsn veya _ts kullanabilir miyim?

_etag biçimidir iç ve, kendisine bağlı olmaması gerekir (ayrıştırmak değil) için dilediğiniz zaman değiştirebilirsiniz.
_ts değişiklik ya da oluşturma zaman damgası ' dir. _Ts kronolojik bir karşılaştırması için kullanabilirsiniz.
_lsn ise yalnızca değişiklik akışı için eklenen bir toplu iş kimliği, mağaza'dan temsil ettiği için işlem kimliği... Birçok belge aynı _lsn olabilir.
Not için ETag FeedResponse üzerinde belgeyle ilgili gördüğünüz _etag farklı bir şey daha. _etag dahili bir tanımlayıcıdır ve eşzamanlılık için kullanıldığında, belge sürümü hakkında bildirir ve ETag akışın sıralama için kullanılır.

### <a name="does-reading-change-feed-add-any-additional-cost-"></a>Değişiklik akışı okuma herhangi ek bir maliyet ekliyor mu?

Yani RU'ın tüketilen için Azure Cosmos DB koleksiyonları ve dışındaki veri hareketleri her zaman tüketen RU ücretlendirilir. Kullanıcılar, kiralama koleksiyon tarafından tüketilen RU için ücretlendirilirsiniz.

### <a name="can-multiple-azure-functions-read-one-collections-change-feed"></a>Birden çok Azure işlevleri, bir koleksiyonun değişiklik akışı okuyabilir miyim?

Evet. Birden çok Azure işlevleri aynı koleksiyonun değişiklik akışı okuyabilirsiniz. Ancak, Azure işlevleri tanımlanan ayrı bir leaseCollectionPrefix olması gerekir.

### <a name="should-the-lease-collection-be-partitioned"></a>Kira koleksiyonu bölümlere ayrılması gerekir?

Hayır, kira koleksiyonu düzeltilebilir. Bölümlenmiş kira koleksiyonu gerekli değildir ve şu anda desteklenmiyor.

### <a name="can-i-read-change-feed-from-spark"></a>Değişiklik okumak Spark'tan akışı?

Evet, uygulayabilirsiniz. Lütfen [Azure Cosmos DB Spark Bağlayıcısı](spark-connector.md). İşte bir [ekran atama](https://www.youtube.com/watch?v=P9Qz4pwKm_0&t=1519s) değişiklik yapılandırılmış bir akış akışı işleminin nasıl gösteriliyor.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-say-a-batch-of-10-documents-and-i-get-an-error-at-7th-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-documentie-7th-document-in-my-next-feed"></a>Değişiklik akışı Azure işlevleri'ni kullanarak işleme, 10 belgeleri toplu söyleyin ve 7 belgeye bir hata alıyorum. Bu durumda son üç belgeler işleme başarısız belge (yani nasıl github'da işlenir Belge 7) sonraki my akışta?

Hatayı işlemeye yönelik önerilen Düzen try-catch bloğu ile kodunuzu sarmaktır. Hata catch ve belge bir sıra (edilemeyen) koyun ve mantık hatası üretilen belgelerle tanımlayabilirsiniz. 200 belge toplu ve başarısız, tek bir belge varsa bu yöntemle, hemen tüm batch throw gerekmez.

Hata, Kontrol noktasına geri başına geri değil durumunda başka bu belgeleri değişiklik akışı'ndan almaya devam. Unutmayın, değişiklik akışı tutar, bu nedenle belgelerin son son ek görüntüsü belgedeki önceki anlık görüntüye kaybedebilir. değişiklik akışı yalnızca bir belgenin son sürümünü tutar ve arasında diğer işlemleri gelir ve belgeyi değiştirme.

Kod çözme tutmak gibi eski ileti sırası üzerinde hiçbir belge yakında bulabilirsiniz.
Azure işlevleri, değişiklik akışı sistem tarafından otomatik olarak çağrılır ve denetim noktası vb. Azure işlevi tarafından dahili olarak korunur. Denetim noktası geri alma ve her yönüyle denetlemek istiyorsanız, kullanarak değişiklik akışı işlemci SDK düşünmelisiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB, Azure işlevleri ile kullanma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB: Azure işlevleri ile sunucusuz veritabanı computing](serverless-computing-database.md).

Değişiklik akışı işlemci kitaplığı kullanma hakkında daha fazla bilgi için aşağıdaki kaynakları kullanın:

* [Bilgileri sayfası](sql-api-sdk-dotnet-changefeed.md) 
* [Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [1-6 yukarıdaki adımları gösteren örnek kod](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [GitHub üzerinde ek örnekleri](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Değişiklik ve SDK üzerinden akışı kullanma hakkında daha fazla bilgi için aşağıdaki kaynakları kullanın:

* [SDK bilgileri sayfası](sql-api-sdk-dotnet.md)
