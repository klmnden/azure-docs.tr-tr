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
ms.openlocfilehash: 2600565493a334c7227e5c0d67a5808f30751108
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261081"
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

* Microsoft.Azure.DocumentDB, sürüm 1.13.1 veya üstü 
* Newtonsoft.Json, sürüm 9.0.1 veya üstü

Daha sonra yüklemek [Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) ve bir başvuru olarak dahil edin.

Yapmanız gereken değişiklik akış işlemci kitaplığı uygulamak için aşağıdaki:

1. Uygulama bir **DocumentFeedObserver** uygulayan nesne **IChangeFeedObserver**.

2. Uygulama bir **DocumentFeedObserverFactory**, hangi uygulayan bir **IChangeFeedObserverFactory**.

3. İçinde **CreateObserver** yöntemi **DocumentFeedObserverFacory**, örneği **ChangeFeedObserver** 1. adımda oluşturduğunuz ve onu döndürür.

    ```
    public IChangeFeedObserver CreateObserver()
    {
              DocumentFeedObserver newObserver = new DocumentFeedObserver(this.client, this.collectionInfo);
              return newObserver;
    }
    ```

4. Örneği **DocumentObserverFactory**.

5. Örneği bir **ChangeFeedEventHost**:

    ```csharp
    ChangeFeedEventHost host = new ChangeFeedEventHost(
                     hostName,
                     documentCollectionLocation,
                     leaseCollectionLocation,
                     feedOptions,
                     feedHostOptions);
    ```

6. Kayıt **DocumentFeedObserverFactory** ana bilgisayarla.

4. adım 6-kodu verilmiştir: 

```
ChangeFeedOptions feedOptions = new ChangeFeedOptions();
feedOptions.StartFromBeginning = true;

ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();
 
// Customizing lease renewal interval to 15 seconds.
// Can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay
feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);
 
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
{
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);
        await host.RegisterObserverFactoryAsync(docObserverFactory);
        await host.UnregisterObserversAsync();
}
```

İşte bu kadar. Bu adımlarda sonra belgeleri giren başlatmak **DocumentFeedObserver ProcessChangesAsync** yöntemi. Yukarıdaki kod Bul [GitHub depo](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)

## <a name="faq"></a>SSS

### <a name="what-are-the-different-ways-you-can-read-change-feed-and-when-to-use-each-method"></a>Değişiklik akış okuyabilir farklı yolları nelerdir? ve ne zaman her bir yöntemin kullanılır?

Değişiklik akış okumak üç seçeneğiniz vardır:

* **[Azure Cosmos DB SQL API .NET SDK'sını kullanma](#sql-sdk)**
   
   Bu yöntemi kullanarak, düşük düzeyde değişiklik akış denetimi alın. Denetim noktası yönetebilir, belirli bir bölüm anahtarı vb. erişebilir. Birden çok okuyucular varsa, kullanabileceğiniz [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) farklı iş parçacıklarındaki ya da farklı istemcileri okuma yükü dağıtmak için. .

* **[Azure Cosmos DB değişikliği kullanarak akış işlemci kitaplığı](#change-feed-processor)**

   Değişiklik akışın karmaşıklıkta çok dış istiyorsanız, değişiklik akış işlemci kitaplığı kullanabilirsiniz. Bu kitaplık karmaşıklıkta çok gizler ancak hala akış, üzerinde tam denetim verir değiştirmek. Bu kitaplık izleyen bir [gözlemci deseni](https://en.wikipedia.org/wiki/Observer_pattern), işleme işlevinizi SDK tarafından çağrılır. 

   Akış bir yüksek verimlilik değişiklik varsa, değişiklik akış okumak için birden çok istemci örneğini oluşturabilirsiniz. "Akış işlemci kitaplığı Değiştir" kullandığından, otomatik olarak yükü farklı istemciler arasında böler. Bir şey yapmanız gerekmez. Tüm karmaşıklık SDK tarafından yapılır. Kendi yük dengeleyiciye sahip olmak istiyorsanız, ancak daha sonra IParitionLoadBalancingStrategy için özel bölüm stratejisi uygulayabilirsiniz. Bir bölüm özel işleme değişikliklerin IPartitionProcessor – uygulayın. SDK'sı, bölüm aralığı işlem ancak, belirli bir bölüm anahtarı işlemek istiyorsanız, daha sonra SDK SQL API'yi kullanmak zorunda.

* **[Azure işlevlerini kullanma](#azure-functions)** 
   
   Son seçenek Azure işlevi en basit bir seçenektir. Bu seçeneği kullanmanızı öneririz. Bir Azure işlevleri uygulamada bir Azure Cosmos DB tetikleyicisi oluşturduğunuzda, bağlanmak için Azure Cosmos DB koleksiyonu seçin ve koleksiyona bir değişiklik yapıldığında işlevi tetiklenir. Gözcü bir [ekran cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) Azure kullanarak işlev ve akış değiştirme

   Azure işlevleri Portalı'nda tetikleyicileri oluşturulabilir Azure Cosmos DB portalında veya program aracılığıyla. Visual Studio ve VS Code Azure işlevi yazma için harika desteğine sahiptir. Yazma ve masaüstünde kodda hata ayıklama ve ardından işlevi tek bir tıklatmayla dağıtın. Daha fazla bilgi için bkz: [Azure Cosmos DB: sunucusuz veritabanı Azure işlevleri kullanarak bilgi işlem](serverless-computing-database.md) makalesi.

### <a name="what-is-the-sort-order-of-documents-in-change-feed"></a>Akış değişiklik belgelerde sıralama düzenini nedir?

Belgeleri akış değişiklik kendi değiştirme saati sırasına göre gelir. Bu sıralama düzeni yalnızca bölüm başına sağlanır.

### <a name="for-a-multi-region-account-what-happens-to-the-change-feed-when-the-write-region-fails-over-does-the-change-feed-also-failover-would-the-change-feed-still-appear-contiguous-or-would-the-fail-over-cause-change-feed-to-reset"></a>Bölgeli hesabı için yazma bölge başarısız-üzerinden olduğunda akış değişiklik ne olur? Değişiklik de yük devretme akışı mu? Hala akış değişiklik bitişik görünür veya yük devretme neden sıfırlamak için akış değiştirmek?

Evet, değişiklik akış el ile yük devretme işlemi çalışır ve bitişik olmalı.

### <a name="how-long-change-feed-persist-the-changed-data-if-i-set-the-ttl-time-to-live-property-for-the-document-to--1"></a>Ne kadar süreyle değişiklik akış kalıcı değiştirilen verileri belge için TTL (yaşam süresi) özelliği -1 olarak ayarlarsanız?

Değişiklik akış sonsuza kadar korunur. Veri silinmez değişiklik Akışta geçersiz kalır.

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region-as-change-feed-is-available-in-all-the-read-regions-by-default"></a>Nasıl değişiklik akış, varsayılan olarak tüm okuma bölgelerde kullanılabilir olduğu gibi belirli bir bölgesinden okumak için Azure işlevleri yapılandırabilirsiniz?

Şu anda belirli bir bölgesinden okumak için Azure işlevleri yapılandırmak mümkün değil. Tüm Azure Cosmos DB bağlama ve tetikleyici tercih edilen bölge ayarlamak için Azure işlevleri depodaki GitHub sorun yoktur.

Azure işlevleri, varsayılan bağlantı İlkesi kullanır. Azure işlevleri ve varsayılan olarak bağlantı modu yapılandırabilirsiniz, Azure işlevleri aynı bölgede üzerinde birlikte bulunmasına en iyisidir yazma bölgesinden okur.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>Azure işlevleri'toplu varsayılan boyutu nedir?

Azure işlevleri her çalıştırılışı 100 belgeleri. Ancak, bu numara function.json dosyasının içinde yapılandırılabilir değildir. İşte tam [yapılandırma seçeneklerinin listesi](../azure-functions/functions-run-local.md). Yerel olarak geliştiriyorsanız, içinde uygulama ayarlarını güncelleştirme [local.settings.json](../azure-functions/functions-run-local.md) dosya.

### <a name="i-am-monitoring-a-collection-and-reading-its-change-feed-however-i-see-i-am-not-getting-all-the-inserted-document-some-documents-are-missing-what-is-going-on-here"></a>Bir koleksiyon izleme ve değişiklik okuma akışı, ancak eklenen tüm belge alıyorum değil görüyorum, bazı belgeleri eksik. Burada neler olup bittiğini?

Aynı kira derlemesiyle aynı koleksiyonda okuma hiçbir işlev olduğundan emin olun. Bana oldu ve daha sonra ı, ayrıca aynı kira kullanarak my diğer Azure işlevleri tarafından işlenen eksik belgeleri gerçekleşmiş.

Okumak için birden çok Azure işlevleri oluşturuyorsanız, sonra farklı kira koleksiyonunu kullanın veya aynı koleksiyonda paylaşmak için "leasePrefix" yapılandırmasını kullanın Bu nedenle, aynı akışın değiştirin. Ancak, değişiklik akış işlemci kitaplığı kullandığınızda işlevinizi birden çok örneğini başlayabilir ve SDK Belgeleri otomatik olarak için farklı örnekler arasında böler.

### <a name="my-document-is-updated-every-second-and-i-am-not-getting-all-the-changes-in-azure-functions-listening-to-change-feed"></a>My belge saniyede güncelleştirilir ve tüm değişiklikleri Azure işlevlerinde akış değiştirmek için dinleme alıyorum değil.

5 saniye arasında yapılan tüm değişiklikler kaybedilir şekilde azure işlevleri yoklamalar değişiklik 5 saniyede için akış. Belge üzerinde 5 değişiklik alırsınız için azure Cosmos DB tek sürüm 5 saniyede için depolar. Ancak, 5 saniye gidin ve saniyede akışı değişiklik yoklamak istediğiniz istiyorsanız, yoklama zaman "feedPollTime" yapılandırmak, yapabilirsiniz bkz [Azure Cosmos DB bağlamaları](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). Varsayılan değer 5000 milisaniye cinsinden tanımlanır. Daha fazla CPU yazma başlayacak şekilde 1 saniye olası ancak değil önerilir.

### <a name="i-inserted-a-document-in-the-mongo-api-collection-but-when-i-get-the-document-in-change-feed-it-shows-a-different-id-value-what-is-wrong-here"></a>Bir belge Mongo API koleksiyonunda ekledim, ancak Belge değişikliği akışta aldığınızda, farklı kimlik değeri gösterir. Burada yanlış nedir?

Koleksiyonunuz Mongo API koleksiyonudur. Değişiklik akış SQL istemcisini kullanarak okunur ve öğeleri JSON biçimine serileştiren unutmayın. Biçimlendirme, JSON nedeniyle istemciler yaşar MongoDB biçimlendirilmiş BSON belgeler ve JSON arasında bir uyuşmazlık akış değişiklik biçimlendirilmiş. Sizin gördüğünüz JSON BSON belgede gösterimidir. Mongo hesaplarında ikili öznitelikler kullanıyorsa, bunlar JSON biçiminde seri hale dönüştürülür.

### <a name="is-there-a-way-to-control-change-feed-for-updates-only-and-not-inserts"></a>Yalnızca güncelleştirmeler için akış değişiklik denetlemek için bir yol yoktur ve değil ekler?

Bugün değil, ancak bu işlevselliği üzerinde yol haritası. Bugün, belge güncelleştirmeleri için bir yumuşak işaret ekleyebilirsiniz.

### <a name="is-there-a-way-to-get-deletes-in-change-feed"></a>Değişiklik akışta siler almanın bir yolu var mı?

Şu anda değişiklik akış siler oturum değil. Değişiklik akış sürekli olarak gelişiyor ve yol haritası üzerinde bu işlevdir. Bugün, belge silme için bir yumuşak işaret ekleyebilirsiniz. Bir öznitelik "silinmiş" adlı "true" olarak ayarlarsanız ve bir TTL, belge üzerinde ayarlayabilir, böylece otomatik olarak silinebilir belgeyi ekleyin.

### <a name="can-i-read-change-feed-for-historic-documentsfor-example-documents-that-were-added-5-years-back-"></a>Geçmiş belgeleri (örneğin, 5 yıllık geri eklenen belgeler) akış değişiklik okuyabilir miyim?

Belge silinmez, Evet değişikliği okuyabilir, koleksiyonunuzu kaynak durum akış.

### <a name="can-i-read-change-feed-using-javascript"></a>JavaScript kullanarak değişiklik akış okuyabilir miyim?

Evet, Node.js SDK'sı ilk desteği değişiklik akış için en son eklenir. Kod çalıştırmadan önce aşağıdaki örnekte, Lütfen geçerli sürümüne güncelleştirme documentdb modülünü gösterildiği gibi kullanılabilir:

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
