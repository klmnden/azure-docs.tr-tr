---
title: Değişiklik çalışmak akışı destek Azure Cosmos DB'de | Microsoft Docs
description: Belgelerdeki değişiklikleri izlemek ve tetikleyiciler gibi olay tabanlı işleme ve önbellekleri ve analizi sistemleri güncel tutma gerçekleştirmek için Azure Cosmos DB Değiştir Akış desteği kullanın.
keywords: Akış Değiştir
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 8475c79782730e989f9590566c31ccd50af9f144
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36302055"
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Destek Azure Cosmos DB'de akış değişiklik ile çalışma

[Azure Cosmos DB](../cosmos-db/introduction.md) bir hızlı ve esnek genel veritabanı, oyun, perakende, IOT için oldukça uygun ve işletimsel günlüğe kaydetme uygulamaları çoğaltılır. Bu uygulamalar ortak tasarım modelinde ek eylemleri atarlar için verilerde yapılan değişiklikleri kullanmaktır. Bu ek eylemleri aşağıdakilerden biri olabilir: 

* Bir belge eklendiğinde veya değiştiren bir bildirim ya da bir API çağrısı tetikleniyor.
* IOT için işleme veya Analiz gerçekleştirme akış.
* Önbellek, arama motoru veya veri ambarı eşitleme ya da verileri soğuk depolama arşivleme ek veri taşıma.

**Değişiklik akışı Destek** aşağıdaki görüntüde gösterildiği gibi Azure Cosmos DB'de, verimli ve ölçeklenebilir çözümler her bu düzenlerin oluşturmanıza olanak sağlar:

![Akış güç gerçek zamanlı analiz ve bilgi işlem senaryolarına olay denetimli için Azure Cosmos DB Değiştir kullanma](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Destek akış değişiklik tüm veri modelleri ve Azure Cosmos DB kapsayıcılarında sağlanır. Ancak, değişiklik akış SQL istemcisini kullanarak okuyun ve öğeleri JSON biçimine serileştiren. Biçimlendirme, JSON nedeniyle istemciler yaşar MongoDB biçimlendirilmiş BSON belgeler ve JSON arasında bir uyuşmazlık akış değişiklik biçimlendirilmiş.

Aşağıdaki videoda Azure Cosmos DB Program Yöneticisi Barış Liu nasıl çalıştığı Azure Cosmos DB değişiklik akışı gösterilmektedir.

> [!VIDEO https://www.youtube.com/embed/mFnxoxeXlaU]
>
>

## <a name="how-does-change-feed-work"></a>Nasıl Değiştir iş akışı?

Azure Cosmos DB works akış desteği, herhangi bir değişiklik için bir Azure Cosmos DB koleksiyonuna dinleyerek değiştirin. Ardından, değiştirilmiş sırada değiştirilen belge sıralanmış listesini çıkarır. Değişiklikler kalıcı, zaman uyumsuz olarak ve artımlı olarak işlenen ve çıkış paralel işleme için bir veya daha fazla tüketiciye üzerinden dağıtılabilir. 

Bu makalenin sonraki bölümlerinde açıklandığı gibi üç farklı yolla akış değişiklik okuyabilirsiniz:

*   [Azure işlevlerini kullanma](#azure-functions)
*   [Azure DB SDK Cosmos kullanma](#sql-sdk)
*   [Azure Cosmos DB değişikliği kullanarak akış işlemci kitaplığı](#change-feed-processor)

Değişiklik akışı her bölüm anahtarı aralığının belge koleksiyonundaki için kullanılabilir ve böylece dizisine paralel işleme için bir veya daha fazla tüketiciye aşağıdaki görüntüde gösterildiği gibi dağıtılabilir.

![Azure Cosmos DB değişimin dağıtılmış işlem akışı](./media/change-feed/changefeedvisual.png)

Ek ayrıntılar:
* Değişiklik akış tüm hesapları için varsayılan olarak etkindir.
* Kullanabilirsiniz, [sağlanan işleme](request-units.md) yazma bölgenize veya herhangi bir [bölge okuma](distribute-data-globally.md) akış değişikliği okumak için olduğu gibi başka bir Azure Cosmos DB işlemi.
* Değişiklik akış ekler ve koleksiyon içindeki belgelerde yapılan güncelleştirme işlemlerini içerir. Siler yakalayabilirsiniz belgelerinizi siler yerine içinde "soft-Sil" bayrağını ayarlayarak. Alternatif olarak, sınırlı bir süre belgeleriniz için ayarlayabileceğiniz [TTL yetenek](time-to-live.md), örneğin, 24 saat ve siler yakalamak için o özelliğin değerini kullanın. Bu çözüm ile TTL sona erme süresinden daha kısa bir zaman aralığındaki değişiklikleri işlemeye sahip.
* Bir belgeyi her değişiklik akış değişikliği tam olarak bir kez görünür ve bunların denetim noktası oluşturma mantığı istemcileri yönetmek. Değişiklik akış işlemci kitaplığı otomatik denetim noktası oluşturma ve "en az bir kez" semantiği sağlar.
* Yalnızca en son değişiklik belirli bir belge için değişiklik günlüğünde yer alır. Ara değişikliklerin kullanılamayabilir.
* Değişiklik akışı, her bölüm anahtarı değerini içinde değişiklik sırasına göre sıralanır. Bölüm anahtarı değerleri arasında hiçbir garanti edilen sipariş yoktur.
* Değişiklikleri herhangi noktası zaman eşitlenebilir, diğer bir deyişle, değişiklikler kullanılabilir hiçbir sabit veri saklama süresi vardır.
* Değişiklikler bölüm anahtar aralıklarına yığınlar halinde kullanılabilir. Bu özellik paralel olarak birden çok tüketiciye/sunucuları tarafından işlenmek üzere büyük topluluklara değişikliklerden sağlar.
* Uygulamalar aynı koleksiyonda üzerinde eşzamanlı olarak birden çok değişikliği akışları isteyebilir.
* ChangeFeedOptions.StartTime kullanılabilir ilk bir başlangıç noktası, örneğin sağlamak için saatin karşılık gelen devamlılık belirteci bulunamıyor. ContinuationToken belirtilmişse StartTime ve StartFromBeginning değerleri WINS. ChangeFeedOptions.StartTime kesinliğini ~ 5 saniye olur. 

## <a name="use-cases-and-scenarios"></a>Kullanım örnekleri ve senaryoları

Değişiklik akış yüksek hacimli yazma ile büyük veri kümelerine verimli işlenmesini sağlar ve nelerin değiştiğini belirlemek için tüm bir veri kümesini sorgulama için bir alternatif sunar. 

Örneğin, akışı bir değişiklikle, aşağıdaki görevleri etkin şekilde gerçekleştirebilirsiniz:

* Bir önbellek, arama dizini ya da bir veri ambarı Azure Cosmos DB içinde depolanan veriler ile güncelleştirecek.
* Uygulama düzeyi verileri katmanlama ve arşivleme uygulamak, diğer bir deyişle, Azure Cosmos DB'de "etkin verileri" depolamak ve "soğuk veri çıkış" geçerlilik süresi [Azure Blob Storage](../storage/common/storage-introduction.md) veya [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Farklı bir bölümleme düzeni ile başka bir Azure Cosmos DB hesabına sıfır kapalı kalma süresi geçişler gerçekleştirin.
* Uygulama [lambda işlem hatları Azure üzerinde](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) Azure Cosmos DB ile. Azure Cosmos DB alımı ve sorgu işleyen ve düşük ile lambda mimariyi uygulayan ölçeklenebilir veritabanı çözümü sağlar. 
* Almak ve cihazlar, algılayıcılar, altyapı ve uygulamaları olay verileri depolamak ve gerçek zamanlı bu olayları işlemek [Azure akış analizi](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/storm/apache-storm-overview.md), veya [Apache Spark](../hdinsight/spark/apache-spark-overview.md). 

Aşağıdaki resimde, her ikisi de alma ve Azure Cosmos DB kullanarak sorgu kullanabilirsiniz lambda ardışık düzen akış destek nasıl değiştiğini gösterir: 

![Azure Cosmos DB tabanlı lambda ardışık düzeni alımı ve sorgu için](./media/change-feed/lambda.png)

Ayrıca, içinde [sunucusuz](http://azure.com/serverless) web ve mobil uygulamaları, yapabilecekleriniz müşterinizin profili, tercihlerine veya konumu kullanarakcihazlarınıanındailetmebildirimlerigöndermegibibelirlieylemleritetiklemekiçindeğişiklikizlemeolaylarına[Azure işlevleri](#azure-functions). Oyun oluşturmak için Azure Cosmos DB kullanıyorsanız, şunları yapabilirsiniz, örneğin, kullanım tamamlanmış oyunlar gelen puanları göre gerçek zamanlı liderlik uygulamak için akış değiştirin.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Azure işlevlerini kullanma 

Azure işlevleri kullanıyorsanız, bir Azure Cosmos DB Değiştir akışına bağlanmak için en basit yolu, Azure işlevleri uygulamanızın Azure Cosmos DB tetikleyici eklemektir. Bir Azure işlevleri uygulamada bir Azure Cosmos DB tetikleyicisi oluşturduğunuzda, bağlanmak için Azure Cosmos DB koleksiyonu seçin ve koleksiyona bir değişiklik yapıldığında işlevi tetiklenir. 

Azure işlevleri Portalı'nda tetikleyicileri oluşturulabilir Azure Cosmos DB portalında veya program aracılığıyla. Daha fazla bilgi için bkz: [Azure Cosmos DB: sunucusuz veritabanı Azure işlevleri kullanarak bilgi işlem](serverless-computing-database.md).

<a id="sql-sdk"></a>
## <a name="using-the-sdk"></a>SDK’yı kullanarak

[SQL SDK](sql-api-sdk-dotnet.md) Azure Cosmos DB okumak ve akış bir değişiklik yönetmek için tüm güç getirdiği için. Ancak çok sorumlulukları, çok sayıda harika güç ile gelir. Kontrol noktalarını yönetme, belge sıra numaraları ile ilgilidir ve bölüm anahtarlarını üzerinde ayrıntılı denetim sahibi istiyorsanız, SDK'sını kullanarak sağ yaklaşım olabilir.

Bu bölümde SQL SDK'sı akışı bir değişiklik ile çalışmak için nasıl kullanılacağı anlatılmaktadır.

1. Appconfig aşağıdaki kaynakları okuyarak başlatın. Uç nokta ve yetkilendirme anahtar alınırken yönergeleri kullanılabilir [bağlantı dizenizi güncelleştirme](create-sql-api-dotnet.md#update-your-connection-string).

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

3. Bölüm anahtarı aralıklarını alma:

    ```csharp
    FeedResponse pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri,
        new FeedOptions
            {RequestContinuation = pkRangesResponseContinuation });
     
    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    ```

4. ExecuteNextAsync her bölüm anahtarı aralığının arayın:

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
> Yerine `ChangeFeedOptions.PartitionKeyRangeId`, kullanabileceğiniz `ChangeFeedOptions.PartitionKey` akışı bir değişiklik almak istediğiniz için tek bölüm anahtarı belirtmek için. Örneğin, `PartitionKey = new PartitionKey("D8CFA2FD-486A-4F3E-8EA6-F3AA94E5BD44")`.
> 
>

Birden çok okuyucular varsa, kullanabileceğiniz **ChangeFeedOptions** farklı iş parçacıklarındaki ya da farklı istemcileri okuma yükü dağıtmak için.

İşte bu kadar bu birkaç kod satırıyla, değişiklik akışı okuma başlatabilirsiniz. Bu makalede kullanılan tam bir kod almak [GitHub deposuna](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed).

Yukarıdaki 4. adımda kodda **ResponseContinuation** son satırı bu sıra numarası sonra yeni belgeleri okuyun sonraki açışınızda kullanacağınız belgenin son mantıksal sıra numarası (LSN) içeriyor. Kullanarak **StartTime** , **ChangeFeedOption** belgeleri almak üzere, net genişletebilirsiniz. Böylece, Eğer, **ResponseContinuation** null, ancak, **StartTime** beri değiştirilen tüm belgeleri alırsınız sürede geri gider **StartTime**. Ancak, IF, **ResponseContinuation** sistem itibaren bu LSN tüm belgeleri alırsınız bir değere sahip.

Bu nedenle, kontrol noktası dizinizi yalnızca her bölüm için LSN engelliyor. Ancak bu bölümleri mücadele etmek istemiyorsanız, kontrol noktaları, LSN, başlangıç saati, vb. basit seçenek işlemci kitaplığı akış değişiklik kullanmaktır.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Değişiklik kullanarak akış işlemci kitaplığı 

[Azure Cosmos DB Değiştir Akış işlemci Kitaplığı](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) kolayca olay işleme arasında birden çok tüketiciye dağıtmak yardımcı olabilir. Bu kitaplık, bölümler ve paralel çalışan birden çok iş parçacığı üzerinde okuma değişiklikleri basitleştirir.

Değişiklik akış işlemci kitaplığı ana avantajı, her bölüm yönetmek zorunda değilsiniz ve devamlılık belirteci ve her bir koleksiyon el ile yoklamaya yok ' dir.

Değişiklik akış işlemci kitaplığı, bölümler ve paralel çalışan birden çok iş parçacığı üzerinde okuma değişiklikleri basitleştirir.  Bir kiralama mekanizması kullanarak bölümleri arasında okuma değişiklikleri otomatik olarak yönetir. İşlemci kitaplığı akış değişiklik kullanan iki istemciler başlatırsanız aşağıdaki resimde gördüğünüz gibi kendi aralarında iş bölün. İstemcilerin artmaya devam ederken, aralarında iş bölerek tutun.

![Azure Cosmos DB değişimin dağıtılmış işlem akışı](./media/change-feed/change-feed-output.png)

Sol istemci ilk başlatıldığından ve tüm bölümleri sonra ikinci istemci başlatıldı ve ardından ilk izin bazı ikinci istemci kiraları Git izleme başlatıldı. Bu görebileceğiniz gibi farklı makinelerde ve istemciler arasındaki iş dağıtmak için iyi yoludur.

Aynı koleksiyonda izleme ve iki işlevi nasıl işlemci kitaplığı öğe için bölümleri karar bağlı bağlı olarak farklı belgeler alabilirsiniz aynı kira kullanılarak iki sunucusuz Azure funtions varsa unutmayın.

<a id="understand-cf"></a>
### <a name="understanding-the-change-feed-processor-library"></a>İşlemci kitaplığı akış değişikliği anlama

Değişiklik akış işlemci kitaplığı gerçekleştirilmesinin dört ana bileşen mevcuttur: izlenen koleksiyonu, kira koleksiyonu, işlemci ana bilgisayar ve tüketicilerin. 

> [!WARNING]
> Koleksiyon oluşturmanın bir fiyatı vardır çünkü Azure Cosmos DB'yle iletişim kurması için uygulamaya aktarım hızı ayırırsınız. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin
> 
> 

**İzlenen koleksiyonu:** değişiklik akış oluşturulan verileri izlenen koleksiyonudur. Tüm eklemeleri ve değişiklikleri izlenen koleksiyonu için koleksiyon değişiklik akışta yansıtılır. 

**Kira koleksiyonu:** arasında birden çok Worker akış değişiklik işleme kira koleksiyonu koordinatları. Ayrı bir koleksiyon, bölüm başına bir kiralama ile kiraları depolamak için kullanılır. Farklı bir hesap işlemci akış değişiklik çalıştığı yazma bölgeyi daha yakın olan bu kira koleksiyon depolamak için avantajlıdır. Bir kira nesnesi aşağıdaki öznitelikleri içerir: 
* Sahibi: kira sahibi olan konak belirtir
* Devamlılık: belirli bir bölüm için akış değişiklik (devamlılık belirteci) konumu belirtir
* Zaman damgası: kira güncelleştirildi; son zamanı zaman damgası, kira süresi dolmuş olarak kabul edilip edilmediğini kontrol etmek için kullanılabilir 

**İşlemci ana bilgisayarı:** etkin kiralamaları kaç ana örneklerini olmadığına göre işlem kaç bölümler her konak belirler. 
1.  Bir ana bilgisayar başlatıldığında tüm ana bilgisayarlar arasında iş yükünü dengelemek için kiraları alır. Kira etkin şekilde bir ana bilgisayar kiralama, düzenli aralıklarla yeniler. 
2.  Bir ana bilgisayar kontrol noktaları da son kirasını her devamlılık belirteci okuyun. Eşzamanlılık güvenliği sağlamak için her bir kira güncelleştirme ETag bir ana bilgisayar denetler. Diğer denetim noktası stratejileri de desteklenir.  
3.  Kapatma, bağlı bir konak tüm kira serbest bırakır, ancak depolanan denetim noktası dosyasından okuma daha sonra devam edebilmek için devamlılık bilgileri tutar. 

Şu anda konakların sayısı bölümleri (kiraları) sayısından büyük olamaz.

**Tüketiciler:** tüketici veya çalışanları olan her konak tarafından başlatılan akış değişiklik yönlendirilmeden iş parçacığı. Her işlemci ana bilgisayarın birden çok tüketiciye sahip olabilir. Her bir tüketici değişikliği atandığı bölümünden akış ve değişiklikleri ana bilgisayar bildirir ve kira süresi okur.

Daha fazla değişiklik akış dört nasıl bu unsuru anlamak için işlemci iş birlikte bakalım örneği aşağıdaki diyagramda. İzlenen koleksiyonu belgeleri depolar ve "Şehir" Bölüm anahtarı olarak kullanır. Mavi bölüm "Şehir" alanını "A-E" belgeler vb. içerdiğini görürsünüz. Her iki tüketicileri paralel dört bölümden okuma ile iki ana vardır. Oklar, belirli bir nokta akış değişikliği okuma tüketicileri gösterir. Mavi akış değişiklik zaten okuma değişiklikleri temsil ederken, ilk bölümü Koyu mavi okunmamış değişiklikler temsil eder. Ana bilgisayarlar, her tüketici geçerli okuma konumunu izlemek için bir "devamlılık" değeri depolamak için kira koleksiyonunu kullanın. 

![Azure Cosmos DB değişikliği kullanarak akış işlemcisi konağı](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>İşlemci kitaplığı akış değişiklik ile çalışma

İşlemci NuGet paketi değişikliği yükleme akışı önce ilk yükleyin: 

* Microsoft.Azure.DocumentDB, en son sürümü.
* Newtonsoft.Json, en son sürümü

Daha sonra yüklemek [Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) ve bir başvuru olarak dahil edin.

Yapmanız gereken değişiklik akış işlemci kitaplığı uygulamak için aşağıdaki:

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

2. Uygulama bir **DocumentFeedObserverFactory**, hangi uygulayan bir **IChangeFeedObserverFactory**.
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

3. Tanımlamak *CancellationTokenSource* ve *ChangeFeedProcessorBuilder*

    ```csharp
    private readonly CancellationTokenSource cancellationTokenSource = new CancellationTokenSource();
    private readonly ChangeFeedProcessorBuilder builder = new ChangeFeedProcessorBuilder();
    ```

5. Yapı **ChangeFeedProcessorBuilder** sonra ilgili nesneleri tanımlama 

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

That’s it. After these few steps documents will start showing up into the **DocumentFeedObserver.ProcessChangesAsync** method.

Above code is for illustration purpose to show different kind of objects and their interaction. You have to define proper variables and initiate them with correct values. You can get the complete code used in this article from the [GitHub repo](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor).

> [!NOTE]
> You should never have a master key in your code or in config file as shown in above code. Please see [how to use Key-Vault to retrive the keys](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/).


## FAQ

### What are the different ways you can read Change Feed? and when to use each method?

There are three options for you to read change feed:

* **[Using Azure Cosmos DB SQL API .NET SDK](#sql-sdk)**
   
   By using this method, you get low level of control on change feed. You can manage the checkpoint, you can access a particular partition key etc. If you have multiple readers, you can use [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) to distribute read load to different threads or different clients. .

* **[Using the Azure Cosmos DB change feed processor library](#change-feed-processor)**

   If you want to outsource lot of complexity of change feed then you can use change feed processor library. This library hides lot of complexity, but still gives you complete control on change feed. This library follows an [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern), your processing function is called by the SDK. 

   If you have a high throughput change feed, you can instantiate multiple clients to read the change feed. Because you are using “change feed processor library”, it will automatically divide the load among different clients. You do not have to do anything. All the complexity is handled by SDK. However, if you want to have your own load balancer, then you can implement IParitionLoadBalancingStrategy for custom partition strategy. Implement IPartitionProcessor – for custom processing changes on a partition. However, with SDK, you can process a partition range but if you want to process a particular partition key then you have to use SDK for SQL API.

* **[Using Azure Functions](#azure-functions)** 
   
   The last option Azure Function is the simplest option. We recommend using this option. When you create an Azure Cosmos DB trigger in an Azure Functions app, you select the Azure Cosmos DB collection to connect to and the function is triggered whenever a change to the collection is made. watch a [screen cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) of using Azure function and change feed

   Triggers can be created in the Azure Functions portal, in the Azure Cosmos DB portal, or programmatically. Visual Studio and VS Code has great support to write Azure Function. You can write and debug the code on your desktop, and then deploy the function with one click. For more information, see [Azure Cosmos DB: Serverless database computing using Azure Functions](serverless-computing-database.md) article.

### What is the sort order of documents in change feed?

Change feed documents comes in order of their modification time. This sort order is guaranteed only per partition.

### For a multi-region account, what happens to the change feed when the write-region fails-over? Does the change feed also failover? Would the change feed still appear contiguous or would the fail-over cause change feed to reset?

Yes, change feed will work across the manual failover operation and it will be contiguous.

### How long change feed persist the changed data if I set the TTL (Time to Live) property for the document to -1?

Change feed will persist forever. If data is not deleted, it will remain in change feed.

### How can I configure Azure functions to read from a particular region, as change feed is available in all the read regions by default?

Currently it’s not possible to configure Azure Functions to read from a particular region. There is a GitHub issue in the Azure Functions repo to set the preferred regions of any Azure Cosmos DB binding and trigger.

Azure Functions uses the default connection policy. You can configure connection mode in Azure Functions and by default, it reads from the write region, so it is best to co-locate Azure Functions on the same region.

### What is the default size of batches in Azure Functions?

100 documents at every invocation of Azure Functions. However, this number is configurable within the function.json file. Here is complete [list of configuration options](../azure-functions/functions-run-local.md). If you are developing locally, update the application settings within the [local.settings.json](../azure-functions/functions-run-local.md) file.

### I am monitoring a collection and reading its change feed, however I see I am not getting all the inserted document, some documents are missing. What is going on here?

Please make sure that there is no other function reading the same collection with the same lease collection. It happened to me, and later I realized the missing documents are processed by my other Azure functions, which is also using the same lease.

Therefore, if you are creating multiple Azure Functions to read the same change feed then they must use different lease collection or use the “leasePrefix” configuration to share the same collection. However, when you use change feed processor library you can start multiple instances of your function and SDK will divide the documents between different instances automatically for you.

### My document is updated every second, and I am not getting all the changes in Azure Functions listening to change feed.

Azure Functions polls change feed for every 5 seconds, so any changes made between 5 seconds are lost. Azure Cosmos DB stores just one version for every 5 seconds so you will get the 5th change on the document. However, if you want to go below 5 second, and want to poll change Feed every second, You can configure the polling time “feedPollTime”, see [Azure Cosmos DB bindings](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). It is defined in milliseconds with a default of 5000. Below 1 second is possible but not advisable, as you will start burning more CPU.

### I inserted a document in the Mongo API collection, but when I get the document in change feed, it shows a different id value. What is wrong here?

Your collection is Mongo API collection. Remember, change feed is read using the SQL client and serializes items into JSON format. Because of the JSON formatting, MongoDB clients will experience a mismatch between BSON formatted documents and the JSON formatted change feed. You are seeing is the representation of a BSON document in JSON. If you use binary attributes in a Mongo accounts, they are converted to JSON.

### Is there a way to control change feed for updates only and not inserts?

Not today, but this functionality is on roadmap. Today, you can add a soft marker on the document for updates.

### Is there a way to get deletes in change feed?

Currently change feed doesn’t log deletes. Change feed is continuously improving, and this functionality is on roadmap. Today, you can add a soft marker on the document for delete. Add an attribute on the document called “deleted” and set it to “true” and set a TTL on the document so that it can be automatically deleted.

### Can I read change feed for historic documents(for example, documents that were added 5 years back) ?

Yes, if the document is not deleted you can read the change feed as far as the origin of your collection.

### Can I read change feed using JavaScript?

Yes, Node.js SDK initial support for change feed is recently added. It can be used as shown in the following example, please update documentdb module to current version before you run the code:

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

### <a name="can-i-read-change-feed-using-java"></a>Java kullanarak değişiklik akış okuyabilir miyim?

Değişiklik akış okumak için Java kitaplığı sağlanmıştır [Github deposunu](https://github.com/Azure/azure-documentdb-changefeedprocessor-java). Ancak, şu anda Java kitaplığı .NET kitaplığı arkasında birkaç sürümleri gerekir. Yakında hem kitaplıkları eşit olacaktır.

### <a name="can-i-use-etag-lsn-or-ts-for-internal-bookkeeping-which-i-get-in-response"></a>Yanıtta alma iç muhasebe için _etag, _lsn veya _ts kullanabilir miyim?

_etag biçimidir iç ve, ona bağlı olmaması gerekir (bunu ayrıştırabilir değil) için istediğiniz zaman değiştirebilirsiniz.
_ts değişiklik ya da oluşturma zaman damgası ' dir. _Ts kronolojik karşılaştırma için kullanabilirsiniz.
_lsn ise yalnızca değişiklik akış için eklenen bir toplu iş kimliği, mağaza'dan işlem kimliğini temsil eden... Birçok belgeleri aynı _lsn olabilir.
Dikkat edilecek ETag FeedResponse üzerinde belge üzerinde gördüğünüz _etag farklı bir daha fazla şey. _etag bir iç tanımlayıcı ve eşzamanlılık için kullanıldığında, belge sürümü hakkında bildirir ve ETag akış sıralama için kullanılır.

### <a name="does-reading-change-feed-add-any-additional-cost-"></a>Değişiklik akışı okuma ek bir maliyet ekler?

Yani RU ait harcanan için Azure Cosmos DB koleksiyonları ve dışındaki veri hareketleri her zaman RU tüketen sizden ücret kesilir. Kullanıcıların kira koleksiyon tarafından tüketilen RU ücretlendirilir.

### <a name="can-multiple-azure-functions-read-one-collections-change-feed"></a>Birden çok Azure işlevleri, bir koleksiyona ait değişiklik akış okuyabilir miyim?

Evet. Birden çok Azure işlevleri aynı koleksiyonunun değişiklik akış okuyabilir. Ancak, Azure işlevleri tanımlanan ayrı bir leaseCollectionPrefix olması gerekir.

### <a name="should-the-lease-collection-be-partitioned"></a>Kira koleksiyon bölümlendiğinde?

Hayır, kira koleksiyonu düzeltilebilir. Bölümlenmiş kira koleksiyonu gerekli değildir ve şu anda desteklenmiyor.

### <a name="can-i-read-change-feed-from-spark"></a>Değişiklik bilgi edinebilirsiniz Spark'tan akış?

Evet, uygulayabilirsiniz. Lütfen bakın [Azure Cosmos DB Spark Bağlayıcısı](spark-connector.md). Burada bir [ekran cast](https://www.youtube.com/watch?v=P9Qz4pwKm_0&t=1519s) yapılandırılmış bir Akış akış değiştirme işleminin nasıl gösteriliyor.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-say-a-batch-of-10-documents-and-i-get-an-error-at-7th-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-documentie-7th-document-in-my-next-feed"></a>Azure işlevlerini kullanarak akış değişiklik işleme, 10 belgeleri toplu söyleyin ve 7 belge hata alıyorum. Bu durumda son üç belgeleri nasıl ı başarısız olan belgeden (yani işleme başlayabilirsiniz işlenmez 7 belge) my sonraki akışındaki?

Hata, önerilen desenini try-catch bloğu ile kodunuzu sarmalamak için yönetmektir. Hata catch ve bu belge bir sıraya (teslim edilemeyen) alıp hata üretilen belgelerle dağıtılacak mantığı tanımlayın. 200 belge toplu ve başarısız oldu, tek bir belge varsa, bu yöntemle, hemen tüm toplu throw gerekmez.

Hata, onay noktasına başına Geri Sar olmayan olasılığına başka bu belgeleri değişiklik akıştan almaya devam. Değişiklik akış tutar, bu nedenle belgelerin son son ek görüntüsü belgenin önceki anlık kaybedebilir unutmayın. belgeyi yalnızca bir son sürümü değişiklik akış tutar ve arasında diğer işlemleri dönün ve belgeyi değiştirme.

Kodunuzu düzelttikten tutmak gibi yakında sahipsiz sırayı hiçbir belge bulabilirsiniz.
Azure işlevleri değişiklik akış sistem tarafından otomatik olarak çağrılır ve denetim noktası vb. Azure işlevi tarafından dahili olarak korunur. Denetim noktası geri ve her yönüyle denetlemek istiyorsanız, değişikliği kullanarak işlemci SDK akış düşünmelisiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleriyle Azure Cosmos DB kullanma hakkında daha fazla bilgi için bkz: [Azure Cosmos DB: sunucusuz veritabanı Azure işlevleri kullanarak bilgi işlem](serverless-computing-database.md).

İşlemci kitaplığı akış değişiklik kullanma hakkında daha fazla bilgi için aşağıdaki kaynakları kullanın:

* [Bilgileri sayfası](sql-api-sdk-dotnet-changefeed.md) 
* [Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [1-6 yukarıdaki adımları gösteren örnek kod](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Github'da ek örnekleri](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

SDK akış değişiklik kullanma hakkında daha fazla bilgi için aşağıdaki kaynakları kullanın:

* [SDK bilgileri sayfası](sql-api-sdk-dotnet.md)
