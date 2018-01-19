---
title: "Değişiklik çalışmak akışı destek Azure Cosmos DB'de | Microsoft Docs"
description: "Belgelerdeki değişiklikleri izlemek ve tetikleyiciler gibi olay tabanlı işleme ve önbellekleri ve analizi sistemleri güncel tutma gerçekleştirmek için Azure Cosmos DB Değiştir Akış desteği kullanın."
keywords: "Akış Değiştir"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 10/30/2017
ms.author: arramac
ms.openlocfilehash: d1968e9fea0fb08edfdbf9e09acca9c4af00b048
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
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

## <a name="how-does-change-feed-work"></a>Nasıl Değiştir iş akışı?

Azure Cosmos DB works akış desteği, herhangi bir değişiklik için bir Azure Cosmos DB koleksiyonuna dinleyerek değiştirin. Ardından, değiştirilmiş sırada değiştirilen belge sıralanmış listesini çıkarır. Değişiklikler kalıcı, zaman uyumsuz olarak ve artımlı olarak işlenen ve çıkış paralel işleme için bir veya daha fazla tüketiciye üzerinden dağıtılabilir. 

Bu makalenin sonraki bölümlerinde açıklandığı gibi üç farklı yolla akış değişiklik okuyabilirsiniz:

1.  [Azure işlevlerini kullanma](#azure-functions)
2.  [Azure DB SDK Cosmos kullanma](#rest-apis)
3.  [Azure Cosmos DB Değiştir Akış işlemci kitaplığı kullanma](#change-feed-processor)

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

## <a name="use-cases-and-scenarios"></a>Kullanım örnekleri ve senaryoları

Değişiklik akış yüksek hacimli yazma ile büyük veri kümelerine verimli işlenmesini sağlar ve nelerin değiştiğini belirlemek için tüm bir veri kümesini sorgulama için bir alternatif sunar. 

Örneğin, akışı bir değişiklikle, aşağıdaki görevleri etkin şekilde gerçekleştirebilirsiniz:

* Bir önbellek, arama dizini ya da bir veri ambarı Azure Cosmos DB içinde depolanan veriler ile güncelleştirecek.
* Uygulama düzeyi verileri katmanlama ve arşivleme uygulamak, diğer bir deyişle, Azure Cosmos DB'de "etkin verileri" depolamak ve "soğuk veri çıkış" geçerlilik süresi [Azure Blob Storage](../storage/common/storage-introduction.md) veya [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Verileri kullanarak toplu analizi uygulamak [Apache Hadoop](run-hadoop-with-hdinsight.md).
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

<a id="rest-apis"></a>
## <a name="using-the-sdk"></a>SDK'sını kullanarak

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

Birden çok okuyucular varsa, kullanabileceğiniz **ChangeFeedOptions** farklı iş parçacıklarındaki ya da farklı istemcileri okuma yükü dağıtmak için.

İşte bu kadar bu birkaç kod satırıyla, değişiklik akışı okuma başlatabilirsiniz. Bu makalede kullanılan tam bir kod almak [GitHub deposuna](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor).

Yukarıdaki 4. adımda kodda **ResponseContinuation** son satırı bu sıra numarası sonra yeni belgeleri okuyun sonraki açışınızda kullanacağınız belgenin son mantıksal sıra numarası (LSN) içeriyor. Kullanarak **StartTime** , **ChangeFeedOption** belgeleri almak üzere, net genişletebilirsiniz. Böylece, Eğer, **ResponseContinuation** null, ancak, **StartTime** beri değiştirilen tüm belgeleri alırsınız sürede geri gider **StartTime**. Ancak, IF, **ResponseContinuation** sistem itibaren bu LSN tüm belgeleri alırsınız bir değere sahip.

Bu nedenle, kontrol noktası dizinizi yalnızca her bölüm için LSN engelliyor. Ancak bu bölümleri mücadele etmek istemiyorsanız, kontrol noktaları, LSN, başlangıç saati, vb. basit seçenek değişiklik akış işlemci kitaplığı kullanmaktır.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Değişiklik akış işlemci kitaplığı kullanma 

[Azure Cosmos DB Değiştir Akış işlemci Kitaplığı](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) kolayca olay işleme arasında birden çok tüketiciye dağıtmak yardımcı olabilir. Bu kitaplık, bölümler ve paralel çalışan birden çok iş parçacığı üzerinde okuma değişiklikleri basitleştirir.

Ana avantajı değişiklik akış işlemci kitaplığının her bölüm yönetmek zorunda değilsiniz ve devamlılık belirteci ve her bir koleksiyon el ile yoklamaya yok ' dir.

Değişiklik akış işlemci kitaplığı bölümler ve paralel çalışan birden çok iş parçacığı üzerinde okuma değişiklikleri basitleştirir.  Bir kiralama mekanizması kullanarak bölümleri arasında okuma değişiklikleri otomatik olarak yönetir. Değişiklik akış işlemci kitaplığını kullanarak iki istemcileri başlatırsanız, aşağıdaki görüntüde, gördüğünüz gibi kendi aralarında iş bölün. İstemcilerin artmaya devam ederken, aralarında iş bölerek tutun.

![Azure Cosmos DB değişimin dağıtılmış işlem akışı](./media/change-feed/change-feed-output.png)

Sol istemci ilk başlatıldığından ve tüm bölümleri sonra ikinci istemci başlatıldı ve ardından ilk izin bazı ikinci istemci kiraları Git izleme başlatıldı. Bu görebileceğiniz gibi farklı makinelerde ve istemciler arasındaki iş dağıtmak için iyi yoludur.

Aynı koleksiyonda izleme ve iki işlevi nasıl işlemci kitaplığı öğe için bölümleri karar bağlı bağlı olarak farklı belgeler alabilirsiniz aynı kira kullanılarak iki sunucusuz Azure funtions varsa unutmayın.

### <a name="understanding-the-change-feed-processor-library"></a>Değişiklik akış işlemci kitaplığı anlama

Akış değişiklik işlemci gerçekleştirilmesinin dört ana bileşen mevcuttur: izlenen koleksiyonu, kira koleksiyonu, işlemci ana bilgisayar ve tüketicilerin. 

> [!WARNING]
> Uygulamanın Azure Cosmos DB ile iletişim kurmak işleme ayırma gibi bir koleksiyon oluşturma, fiyatlandırmaya vardır. Daha fazla ayrıntı için lütfen ziyaret [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

**İzlenen koleksiyonu:** değişiklik akış oluşturulan verileri izlenen koleksiyonudur. Tüm eklemeleri ve değişiklikleri izlenen koleksiyonu için koleksiyon değişiklik akışta yansıtılır. 

**Kira koleksiyonu:** arasında birden çok Worker akış değişiklik işleme kira koleksiyonu koordinatları. Ayrı bir koleksiyon, bölüm başına bir kiralama ile kiraları depolamak için kullanılır. Akış değişiklik işlemci çalıştığı yazma bölgeyi daha yakın olan farklı bir hesap üzerinde bu kira koleksiyon depolamak için avantajlıdır. Bir kira nesnesi aşağıdaki öznitelikleri içerir: 
* Sahibi: kira sahibi olan konak belirtir
* Devamlılık: belirli bir bölüm için akış değişiklik (devamlılık belirteci) konumu belirtir
* Zaman damgası: kira güncelleştirildi; son zamanı zaman damgası, kira süresi dolmuş olarak kabul edilip edilmediğini kontrol etmek için kullanılabilir 

**İşlemci ana bilgisayarı:** etkin kiralamaları kaç ana örneklerini olmadığına göre işlem kaç bölümler her konak belirler. 
1.  Bir ana bilgisayar başlatıldığında tüm ana bilgisayarlar arasında iş yükünü dengelemek için kiraları alır. Kira etkin şekilde bir ana bilgisayar kiralama, düzenli aralıklarla yeniler. 
2.  Bir ana bilgisayar kontrol noktaları da son kirasını her devamlılık belirteci okuyun. Eşzamanlılık güvenliği sağlamak için her bir kira güncelleştirme ETag bir ana bilgisayar denetler. Diğer denetim noktası stratejileri de desteklenir.  
3.  Kapatma, bağlı bir konak tüm kira serbest bırakır, ancak depolanan denetim noktası dosyasından okuma daha sonra devam edebilmek için devamlılık bilgileri tutar. 

Şu anda konakların sayısı bölümleri (kiraları) sayısından büyük olamaz.

**Tüketiciler:** tüketici veya çalışanları olan her konak tarafından başlatılan akış değişiklik yönlendirilmeden iş parçacığı. Her işlemci ana bilgisayarın birden çok tüketiciye sahip olabilir. Her bir tüketici değişikliği atandığı bölümünden akış ve değişiklikleri ana bilgisayar bildirir ve kira süresi okur.

Daha fazla değişiklik akış işlemci dört bu unsuru birlikte nasıl çalıştığını anlamak için aşağıdaki diyagramda bir örnekte bakalım. İzlenen koleksiyonu belgeleri depolar ve "Şehir" Bölüm anahtarı olarak kullanır. Mavi bölüm "Şehir" alanını "A-E" belgeler vb. içerdiğini görürsünüz. Her iki tüketicileri paralel dört bölümden okuma ile iki ana vardır. Oklar, belirli bir nokta akış değişikliği okuma tüketicileri gösterir. Mavi akış değişiklik zaten okuma değişiklikleri temsil ederken, ilk bölümü Koyu mavi okunmamış değişiklikler temsil eder. Ana bilgisayarlar, her tüketici geçerli okuma konumunu izlemek için bir "devamlılık" değeri depolamak için kira koleksiyonunu kullanın. 

![Azure Cosmos DB değişikliği kullanarak akış işlemcisi konağı](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Değişiklik akış işlemci kitaplığı ile çalışma

Değişiklik akış işlemci NuGet paketini yüklemeden önce ilk yükleyin: 

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

İşte bu kadar. Bu adımlarda sonra belgeleri giren başlatmak **DocumentFeedObserver ProcessChangesAsync** yöntemi.

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleriyle Azure Cosmos DB kullanma hakkında daha fazla bilgi için bkz: [Azure Cosmos DB: sunucusuz veritabanı Azure işlevleri kullanarak bilgi işlem](serverless-computing-database.md).

Değişiklik akış işlemci kitaplığını kullanarak daha fazla bilgi için aşağıdaki kaynakları kullanın:

* [Bilgileri sayfası](sql-api-sdk-dotnet-changefeed.md) 
* [Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [1-6 yukarıdaki adımları gösteren örnek kod](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Github'da ek örnekleri](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

SDK akış değişiklik kullanma hakkında daha fazla bilgi için aşağıdaki kaynakları kullanın:

* [SDK bilgileri sayfası](sql-api-sdk-dotnet.md)
