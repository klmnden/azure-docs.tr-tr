---
title: ".NET için Azure Cosmos DB performans ipuçları | Microsoft Docs"
description: "Azure Cosmos DB veritabanı performansını iyileştirmek için istemci yapılandırma seçenekleri öğrenin"
keywords: "Veritabanı performansı nasıl"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 94ff155e-f9bc-488f-8c7a-5e7037091bb9
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: mimig
ms.openlocfilehash: 242ec5bfbe33acd4731809efed9b70897b7a9608
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
> [!div class="op_single_selector"]
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 
> 

# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Azure Cosmos DB ve .NET Performans İpuçları

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure Cosmos DB garantili gecikme süresi ve verimlilik ile sorunsuz bir şekilde ölçeklendirilebilen bir hızlı ve esnek dağıtılmış veritabanıdır. Büyük mimari değişiklikler yaptığınızda veya Azure Cosmos DB veritabanınızla ölçeklendirmek için karmaşık kodlar yazmak zorunda değildir. Yukarı ve aşağı doğru ölçeklendirme tek bir API çağrısı yapma olarak kadar kolay veya [SDK yöntem çağrısı](set-throughput.md#set-throughput-sdk). Ancak, Azure Cosmos DB ağ çağrıları erişilir vardır kullanırken en yüksek performans elde etmek için yapabilir istemci-tarafı iyileştirmeler [SQL .NET SDK'sı](documentdb-sdk-dotnet.md).

Soran, "nasıl ı my veritabanı performansını geliştirebilir şekilde?" Aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ
<a id="direct-connection"></a>

1. **Bağlantı İlkesi: doğrudan bağlantı modu kullan**

    Bir istemci için Azure Cosmos DB nasıl bağlandığını gözlemlenen istemci-tarafı gecikme açısından özellikle performansı önemli etkilere sahiptir. İki anahtar yapılandırma ayarlarını istemci bağlantı İlkesi – bağlantı yapılandırmak için kullanılabilir *modu* ve [bağlantı *Protokolü*](#connection-protocol).  İki kullanılabilir modları şunlardır:

   1. Ağ geçidi modunda (varsayılan)
   2. Doğrudan modu

      Ağ geçidi modu tüm SDK platformlarda desteklenir ve yapılandırılmış varsayılandır.  Uygulamanız bir şirket ağı içinde katı güvenlik duvarı kısıtlamalarıyla çalışıyorsa standart HTTPS bağlantı noktası ve tek bir uç nokta kullandığından ağ geçidi modu en iyi seçimdir. Performans kolaylığını ancak, veri okuma veya Azure Cosmos Veritabanına yazılan her zaman ağ geçidi modu ek ağ atlama içermesidir. Bu nedenle, doğrudan modu daha az ağ atlamalarını nedeniyle daha iyi performans sunar.
<a id="use-tcp"></a>
2. **Bağlantı İlkesi: TCP protokolünü kullanır**

    Doğrudan modu kullanırken, kullanılabilir iki protokolü seçenek vardır:

   * TCP
   * HTTPS

     Azure Cosmos DB HTTPS üzerinden bir basit ve açık RESTful programlama modeli sunar. Ayrıca, aynı zamanda RESTful kendi iletişim modelini ve .NET İstemci SDK kullanılabilir verimli bir TCP protokolü sunar. Doğrudan TCP ve HTTPS SSL ilk kimlik doğrulaması ve şifreleme trafiği için kullanın. En iyi performans için mümkün olduğunda TCP protokolünü kullanır.

     TCP ağ geçidi modunda kullanılırken, TCP bağlantı noktası 443 Azure Cosmos DB bağlantı noktası ve 10255 değerini bulur MongoDB API bağlantı noktası. Bağlantı noktası sağlamak zorunda TCP ağ geçidi bağlantı noktalarına ek olarak doğrudan modunda kullanırken, Azure Cosmos DB dinamik TCP bağlantı noktaları kullandığından 10000 20000 arasındaki aralığı açın. Bu bağlantı noktalarının açık değildir ve TCP kullanma girişimi 503 Hizmet kullanılamıyor hatası alıyorsunuz.

     Bağlantı modunu ConnectionPolicy parametresi DocumentClient örneğiyle yapımı sırasında yapılandırılır. Doğrudan modunda kullanılırsa, Protokolü içinde ConnectionPolicy parametresi de ayarlayabilirsiniz.

    ```csharp
    var serviceEndpoint = new Uri("https://contoso.documents.net");
    var authKey = new "your authKey from the Azure portal";
    DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
    new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp
    });
    ```

    Ağ geçidi modunda kullanılırsa, TCP yalnızca doğrudan modunda desteklendiğinden, HTTPS protokolünü her zaman ağ geçidi ile iletişim kurmak için kullanılır ve ConnectionPolicy Protokolü değeri yoksayılır.

    ![Azure Cosmos DB bağlantı İlkesi çizimi](./media/performance-tips/connection-policy.png)

3. **İlk istek başlatma gecikmesine önlemek için OpenAsync çağırın**

    Adres yönlendirme tablosu getirilemedi içerdiğinden varsayılan olarak, ilk isteği daha yüksek gecikme vardır. Bu ilk istek başlatma gecikmesine önlemek için OpenAsync() kez başlatma sırasında şu şekilde çağırmalıdır.

        await client.OpenAsync();
   <a id="same-region"></a>
4. **Performans için aynı Azure bölgesinde istemciler birlikte bulundurma**

    Mümkün olduğunda, Azure Cosmos DB veritabanı ile aynı bölgede Azure Cosmos DB çağırma herhangi bir uygulama yerleştirin. Yaklaşık bir karşılaştırma için 1-2 ms içinde aynı bölge içinde Azure Cosmos DB'de çağrıları tamamlamak ancak Batı ABD, Doğu Yakası arasındaki gecikme süresi > 50 ms. Bu gecikme, Azure veri merkezi sınır istemciden geçerken istek tarafından izlenen yolu bağlı olarak istemek için büyük olasılıkla farklılık gösterebilir. Olası en düşük gecikme, çağıran uygulama sağlanan Azure Cosmos DB uç nokta olduğu Azure bölgesinin içinde bulunduğu sağlanarak elde edilir. Kullanılabilir bölgelerin bir listesi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

    ![Azure Cosmos DB bağlantı İlkesi çizimi](./media/performance-tips/same-region.png)
   <a id="increase-threads"></a>
5. **İş parçacıkları/görevleri sayısını artırın**

    Azure Cosmos DB çağrıları ağ üzerinden yapılan olduğundan, böylece istemci uygulaması bekleyen istekler arasında çok az zaman harcayan, istekleri paralellik derecesini farklılık gerekebilir. Örneğin, kullanıyorsanız. NET'in [görev paralel Kitaplığı](https://msdn.microsoft.com//library/dd460717.aspx), 100s okunurken veya yazılırken Azure Cosmos DB görev sırasına göre oluşturun.

## <a name="sdk-usage"></a>SDK kullanımı
1. **En son SDK'sını yükleyin**

    Azure Cosmos DB SDK'ları en iyi performansı sağlamak için sürekli geliştirilen. Bkz: [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) en son SDK belirlemek ve geliştirmeleri gözden geçirmek için sayfaları.
2. **Bir singleton Azure Cosmos DB istemci uygulamanızın ömrü boyunca kullanın**

    Efach DocumentClient örnek iş parçacığı açısından güvenli ve verimli bağlantı yönetimi ve doğrudan modunda çalışırken adresi önbelleğe alma gerçekleştirir. Verimli bağlantı yönetimi ve daha iyi performans DocumentClient ile izin vermek için uygulama yaşam süresi için DocumentClient AppDomain başına tek bir örneğini kullanmak için önerilir.

   <a id="max-connection"></a>
3. **Ağ geçidi modu kullanırken System.Net MaxConnections ana bilgisayar başına artırın**

    Azure Cosmos DB istekleri HTTPS/REST ağ geçidi modu kullanırken yapılır ve olan varsayılan bağlantı sınırını ana bilgisayar adı veya IP adresi başına tabi. Böylece istemci kitaplığı Azure Cosmos DB birden çok eşzamanlı bağlantıların kullanabilir MaxConnections daha yüksek bir değer (100-1000) ayarlamanız gerekebilir. .NET SDK'sındaki 1.8.0 ve varsayılan değerin üzerinde [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) 50'dir ve değerini değiştirmek için ayarlayabileceğiniz [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx)daha yüksek bir değere.   
4. **Paralel sorgular bölümlenmiş koleksiyonlar için ayarlama**

     SQL .NET SDK sürüm 1.9.0 ve paralel bölümlendirilmiş bir koleksiyon sorgulamak etkinleştirme desteği paralel sorgular yukarıda (bkz [SDK'ları ile çalışma](sql-api-partition-data.md#working-with-the-azure-cosmos-db-sdks) ve ilgili [kod örnekleri](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) daha fazla bilgi için). Paralel sorgular seri bunların karşılık gelen sorgu gecikme süresi ve üretilen işi artırmak için tasarlanmıştır. Paralel sorguları, kullanıcılar için özel Sığdır gereksinimlerine, (a) MaxDegreeOfParallelism ayarlayabilirsiniz iki parametre sağlar: en çok sonra bölüm sayısı sorgulanan paralel ve (b) MaxBufferedItemCount denetlemek için: sayısını kontrol etmek için önceden getirilen sonuç.

    (a) ***ayarlama MaxDegreeOfParallelism\:***  paralel sorgu works birden çok bölümü paralel sorgulayarak. Ancak, tek bir bölümlenmiş toplama verileri seri olarak sorgu göre getirilir. Bu nedenle, diğer tüm sistem koşulları aynı kalır sağlanan çoğu kullanıcı sorgu elde maksimum fırsat bir bölüm sayısı MaxDegreeOfParallelism ayarlama sahiptir. Bölüm sayısı bilmiyorsanız, yüksek bir sayıya MaxDegreeOfParallelism ayarlayabilir ve sistem MaxDegreeOfParallelism en az (bölüm, kullanıcı tarafından sağlanan girdi sayısı) seçer.

    Verileri sorgu göre tüm bölümleri arasında eşit olarak dağıtılır, paralel sorgular en iyi avantajları oluşturduğunun dikkate almak önemlidir. Bölümlenmiş koleksiyonu (en kötü durumda bir bölüm) birkaç bölümlerdeki tamamı veya bir sorgu tarafından döndürülen verilerin çoğunu bir yoğunlaşmıştır, ardından sorgu performansını tarafından bu bölümler nedeniyle düşük performansa şekilde bölümlenmiş durumunda.

    (b) ***ayarlama MaxBufferedItemCount\:***  paralel sorgu sonuçlarının geçerli toplu işlem istemci tarafından gerçekleştirilirken sonuçları önceden getirmek için tasarlanmıştır. Önceden getirme sorgu genel gecikme gelişme yardımcı olur. MaxBufferedItemCount önceden getirilen sonuç sayısını sınırlamak için parametresidir. Döndürülen sonuçları beklenen sayıda MaxBufferedItemCount (veya daha yüksek bir sayı) ayarlamayı önceden getirme maksimum avantajı almak sorgu sağlar.

    Önceden getirme MaxDegreeOfParallelism bağımsız olarak aynı şekilde çalışır ve tüm bölümleri verileri için tek bir arabellek yok.  
5. **Sunucu tarafı GC üzerinde Aç**

    Çöp toplama sıklığını azaltmayı bazı durumlarda yardımcı olabilir. .NET ile ayarlamak [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) true.
6. **Geri Çekilme RetryAfter aralıklarla uygulama**

    Performans testi sırasında istekleri küçük oranını kısıtlanan kadar yük artırmanız gerekir. Kısıtlanan, istemci uygulamasına geri Çekilme kısıtlama üzerinde sunucu belirtilen yeniden deneme aralığını gerekir. Geri Çekilme uyarak denemeler arasındaki bekleme süresi en az miktarda harcamanızı sağlar. Yeniden deneme ilkesi desteği eklenmiştir sürümünde 1.8.0 ve yukarıda SQL [.NET](sql-api-sdk-dotnet.md) ve [Java](sql-api-sdk-java.md), sürüm 1.9.0 ve üstü, [Node.js](sql-api-sdk-node.md) ve [Python](sql-api-sdk-python.md), ve tüm desteklenen sürümlerinde [.NET Core](sql-api-sdk-dotnet-core.md) SDK'ları. Daha fazla bilgi için bkz: [Exceeding ayrılmış işleme sınırları](request-units.md#RequestRateTooLarge) ve [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
7. **İstemci-iş yükünü ölçeklendirme**

    Yüksek işleme düzeyleri sınıyorsanız (> 50.000 RU/s), istemci uygulaması makine çıkışı CPU veya ağ kullanımını capping nedeniyle ayak bağı. Bu noktaya ulaştıysanız, daha fazla Azure Cosmos DB hesabı birden çok sunucu arasında istemci uygulamalarının ölçeklendirme tarafından anında devam edebilirsiniz.
8. **Daha düşük okuma gecikmesi için önbellek belge URI'ler**

    Önbellek belge URI'ler okuma en iyi performans için mümkün.
   <a id="tune-page-size"></a>
9. **Sorguları/okuma akışları daha iyi performans için sayfa boyutunu ayarlama**

    Bir toplu gerçekleştirme belgeleri okuma akışı işlevleri (örneğin, ReadDocumentFeedAsync) kullanarak veya bir SQL sorgusu verilirken okurken sonuç kümesi çok büyük ise sonuçları bölümlenmiş bir şekilde döndürülür. Varsayılan olarak, ilk isabet sınırlarından hangisi olduğunu, sonuçları 100 öğeleri ya da 1 MB yığınlar halinde döndürdü.

    Sayısını azaltmak için ağ gidiş dönüşleri uygulanabilir tüm sonuçları almak için gereken, sayfa boyutu kullanarak artırabilirsiniz [x-ms-max-öğesi-count](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-request-headers) en fazla 1000 istek üstbilgisi. Burada yalnızca birkaç sonuçları görüntülemek için gereken durumlarda Örneğin, kullanıcı arabirimi veya uygulama API yalnızca 10 döndürürse birer sonuçları, aynı zamanda okuma ve sorgular için tüketilen verimlilik azaltmak için 10 sayfa boyutunu azaltabilirsiniz.

    Sayfa boyutu kullanılabilir Azure Cosmos DB SDK'ları kullanarak da ayarlayabilirsiniz.  Örneğin:

        IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
10. **İş parçacıkları/görevleri sayısını artırın**

    Bkz: [iş parçacıkları/görevlerin sayısını artırmak](#increase-threads) ağ bölümünde.

11. **64-bit ana bilgisayar işleme kullanın**

    SQL SDK'sı SQL .NET SDK sürüm 1.11.4 kullanırken ve üzerini 32-bit ana işleminde çalışır. Ancak, çapraz bölüm sorguları kullanıyorsanız, 64-bit ana bilgisayar işleme Gelişmiş performans için önerilir. 64 bit izleyin değiştirmek için aşağıdaki adımları uygulamanız türüne bağlı şekilde aşağıdaki uygulama türlerini varsayılan olarak, 32-bit ana bilgisayar işlemi vardır:

    - Yürütülebilir uygulamalar için bu kaldırarak yapılabilir **tercih 32-bit** seçeneğini **proje özelliklerini** penceresi, **yapı** sekmesi.

    - Test projeleri VSTest tabanlı için bu seçerek yapılabilir **Test**->**Test ayarlarını**->**varsayılan İşlemci mimarisi X64 olarak**, gelen **Visual Studio Test** menü seçeneği.

    - Yerel olarak dağıtılan ASP.NET Web uygulamaları için bu kontrol ederek yapılabilir **web siteleri ve projeler için IIS Express 64-bit sürümünü kullanmanız**altında **Araçları**->**seçenekleri**  -> **Projeler ve çözümler**->**Web projeleri**.

    - Azure üzerinde dağıtılan ASP.NET Web uygulamaları için bu seçerek yapılabilir **64-bit olarak Platform** içinde **uygulama ayarları** Azure portalındaki.

## <a name="indexing-policy"></a>Dizin Oluşturma İlkesi
 
1. **Daha hızlı yazmalar için dizin oluşturma kullanılmayan yolu dışla**

    Cosmos veritabanı dizin oluşturma ilkesini de dahil etmek veya hariç dizin yolları (IndexingPolicy.IncludedPaths ve IndexingPolicy.ExcludedPaths) yararlanarak dizin hangi belge yolları belirtmenize olanak tanır. Dizin oluşturma maliyetleri doğrudan dizine benzersiz yolları sayısı bağıntılı gibi yolları dizin kullanımını geliştirilmiş yazma performansı ve sorgu desenlerine önceden bilinir senaryoları için alt dizini depolama sunabilir.  Örneğin, aşağıdaki kod belgeleri bölümünün tamamını (paketini dışlama gösterir bir alt ağacı) dizin kullanarak "*" joker karakter.

    ```csharp
    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
    ```

    Daha fazla bilgi için bkz: [Azure Cosmos ilkeleri dizin DB](indexing-policies.md).

## <a name="throughput"></a>Aktarım hızı
<a id="measure-rus"></a>

1. **Ölçmek ve alt istek için saniye başına birim kullanım ayarlayın.**

    Azure Cosmos DB zengin bir ilişkisel ve hiyerarşik sorgularıyla UDF'ler, saklı yordamları ve Tetikleyicileri – tüm işletim veritabanı koleksiyon içindeki belgelerde dahil olmak üzere veritabanı işlemleri sunar. Bu işlemlerin her biri ile ilişkili maliyet CPU, g/ç ve işlemi tamamlamak için gereken bellek göre değişir. Göz önünde bulundurulması ve donanım kaynaklarını yönetmek yerine, bir uygulama isteği hizmet ve çeşitli veritabanı işlemlerini gerçekleştirmek için gereken kaynakları için tek bir ölçü olarak bir istek birimi (RU) düşünebilirsiniz.

    Üretilen iş sayısına göre hazırlanır [istek birimleri](request-units.md) her kapsayıcıya ayarlayın. İstek birimi tüketim saniye başına oranı olarak değerlendirilir. Hızı kapsayıcısı için sağlanan düzeyinin altına düşene kadar kendi kapsayıcısı için sağlanan istek birimi hızı aşan uygulamaları sınırlıdır. Uygulamanız daha yüksek düzeyde üretilen iş gerektiriyorsa, ek istek birimleri sağlama tarafından üretilen işi artırabilir. 

    Kaç tane istek birimlerine bir işlem için kullanılan bir sorgu karmaşıklığını etkiler. Koşulları sayısı, koşulları, UDF'ler sayısı ve tüm kaynak veri kümesi boyutunu yapısını sorgu işlemlerinin maliyetini etkiler.

    Herhangi bir işlem yükü ölçmek için (oluşturma, güncelleştirme veya silme) incelemek [x-ms-istek-ücret](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-response-headers) üstbilgi (veya eşdeğer RequestCharge özelliği ResourceResponse<T> veya FeedResponse<T> içinde. Bu işlemler tarafından tüketilen isteği birim sayısını ölçmek için NET SDK).

    ```csharp
    // Measure the performance (request units) of writes
    ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
    Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
    // Measure the performance (request units) of queries
    IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
    while (queryable.HasMoreResults)
         {
              FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
              Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
         }
    ```             

    Bu üstbilgisinde döndürülen istek ücret bir sağlanan işleme bölümüdür (yani, 2000 RUs / saniye). Örneğin, önceki sorgunun 1000 1 KB-belgeleri döndürürse, işlem maliyetini 1000'dir. Bu nedenle, bir saniye içinde sonraki istekler azaltma önce sunucunun yalnızca iki tür isteklere korur. Daha fazla bilgi için bkz: [istek birimleri](request-units.md) ve [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Tanıtıcı oranı sınırlama istek oranı çok büyük**

    Bir istemci bir hesap için ayrılmış işleme aşan girişiminde bulunduğunda, sunucuda bir performans düşüşü olmadan ve işleme kapasitesi ayrılmış düzeyinin ötesine hiçbir kullanımını yoktur. Sunucu erken önlem RequestRateTooLarge (HTTP durum kodu 429) istekle sonlandırmak ve dönmek [x-ms-yeniden deneme-sonra-ms](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-response-headers) kullanıcı reattempting önce beklemesi gereken milisaniye cinsinden süreyi belirten üstbilgisi İstek.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK'ları tüm örtük olarak bu yanıt catch, yeniden deneme sonra sunucu tarafından belirtilen üstbilgi saygı ve isteği yeniden deneyin. Hesabınızı eşzamanlı olarak birden çok istemci tarafından erişilen sürece, sonraki yeniden deneme işlemi başarılı olur.

    Birden fazla istemci üst üste istek hızı tutarlı bir şekilde işletim varsa, 9 istemci tarafından dahili olarak ayarlanmış varsayılan yeniden deneme sayısı yeterli değil; Bu durumda, istemci uygulamaya 429 durum koduyla bir DocumentClientException oluşturur. Varsayılan yeniden deneme sayısı ConnectionPolicy örneğinde RetryOptions ayarlayarak değiştirilebilir. İstek istek hızı çalışmaya devam ederse, varsayılan olarak, 30 saniye sonra bir toplam bekleme süresi 429 durum koduyla DocumentClientException döndürülür. Bu geçerli yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda bile oluşur, varsayılan 9 veya kullanıcı tanımlı bir değer olmalıdır.

    Dayanıklılık ve kullanılabilirlik çoğu uygulamalar geliştirmek için otomatik yeniden deneme davranışı yardımcı olsa da, bu performans kıyaslamaları, özellikle gecikme süresini ölçme zaman yaparken at odds gelebilir.  Denemeyi sunucu kısıtlama isabetler ve sessiz bir şekilde yeniden denemek için SDK istemci neden olursa istemci gözlenen gecikme çıkmasına. Performans denemeleri sırasında gecikme ani önlemek için her işlem tarafından döndürülen farkı ölçmek ve istekleri ayrılmış istek hızı çalıştığından emin olun. Daha fazla bilgi için bkz: [istek birimleri](request-units.md).
3. **Daha fazla üretilen işi için daha küçük belgeler için Tasarım**

    Belirli bir işlemi isteği giderleri (yani istek işleme maliyet) doğrudan belgenin boyutunu ilişkilendirilir. Büyük belgeleri işlemleri birden çok küçük belgeler için işlemleri maliyeti.

## <a name="next-steps"></a>Sonraki adımlar
Birkaç istemci makineler yüksek performanslı senaryoları için Azure Cosmos DB değerlendirmek için kullanılan örnek bir uygulama için bkz: [performansı ve ölçeği Azure Cosmos DB ile test](performance-testing.md).

Ayrıca, uygulamanız ölçeği ve yüksek performans için tasarlama hakkında daha fazla bilgi için bkz. [bölümleme ve Azure Cosmos DB'de ölçeklendirme](partition-data.md).
