---
title: .NET için Azure Cosmos DB performans ipuçları
description: Azure Cosmos DB veritabanı performansını artırmak üzere istemci yapılandırma seçenekleri öğrenin
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: sngun
ms.openlocfilehash: c8907f1b1c8069a3a3e92d01a5fa6341c06ec952
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688812"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Azure Cosmos DB ile .NET için performans ipuçları

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB garantili gecikme süresi ve aktarım hızı ile sorunsuz bir şekilde ölçeklenen bir hızlı ve esnek dağıtılmış bir veritabanıdır. Azure Cosmos DB ile veritabanınızı ölçeklendirmek için karmaşık kodlar yazmanıza ya da büyük mimari değişiklikler yapmak gerekmez. Yukarı ve aşağı ölçeklendirme olarak tek bir API çağrısı yapmak oldukça kolaydır. Daha fazla bilgi için bkz. [kapsayıcının aktarım hızını sağlamak nasıl](how-to-provision-container-throughput.md) veya [veritabanı aktarım hızını sağlamak nasıl](how-to-provision-database-throughput.md). Ancak, Azure Cosmos DB ağ çağrıları erişilir vardır kullanırken en yüksek performans elde etmek için olun istemci-tarafı iyileştirmeleri [SQL .NET SDK'sı](documentdb-sdk-dotnet.md).

Açmanızı isteyen, "nasıl veritabanı performansımı geliştirebilirim şekilde?" Aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ
<a id="direct-connection"></a>

1. **Bağlantı İlkesi: Doğrudan bağlantı modunu kullan**

    Bir istemci, Azure Cosmos DB'ye nasıl bağlanır? performansını gözlemler istemci tarafı gecikme süresi açısından özellikle önemli etkilere sahiptir. İstemci bağlantı İlkesi – bağlantı yapılandırmak için kullanılabilen iki anahtar yapılandırma ayarları vardır *modu* ve bağlantı *Protokolü*.  Kullanılabilir iki mod şunlardır:

   * Ağ geçidi modu (varsayılan)
      
     Ağ geçidi modu, tüm SDK platformlarında desteklenir ve yapılandırılmış varsayılandır. Uygulamanız, kurumsal ağ içinden katı güvenlik duvarı kısıtlamalarıyla çalışıyorsa, standart HTTPS bağlantı noktası ve tek bir uç nokta kullandığından ağ geçidi modu en iyi seçenektir. Performansta düşüş, ancak veri okuma veya Azure Cosmos DB için yazılan her zaman ağ geçidi modu ek ağ atlama içermesidir. Bu nedenle, doğrudan modu daha az ağ atlamaları nedeniyle daha iyi performans sunar. Sınırlı sayıda Azure işlevleri veya bir tüketim planında olup olmadığını kullanırken örneğin yuva bağlantı içeren ortamlarda uygulamaları çalıştırdığınızda, ağ geçidi bağlantı modu da önerilir. 

   * Doğrudan modu

     TCP ve HTTPS protokolleri üzerinden bağlantı doğrudan modu destekler. .NET SDK'sının en son sürümü kullanıyorsanız, .NET framework ve .NET Standard 2.0 ile doğrudan bağlantı modu desteklenir. Doğrudan modunu kullanırken, kullanılabilir iki protokol seçenek vardır:

     * TCP
     * HTTPS

     Ağ geçidi modunu kullanırken, Cosmos DB bağlantı noktası 443 ve bağlantı noktaları 10250 10255 olarak ve 10256 Azure Cosmos DB'nin MongoDB kullanarak kullanır. Coğrafi çoğaltma ve coğrafi çoğaltma işlevselliği ile bir MongoDB örneğine 10255 olarak/10256 bağlantı noktalarını eşleme olmadan varsayılan bir MongoDB örneğine 10250 bağlantı noktası eşlenir. Bağlantı noktası emin olmanız TCP doğrudan modda, ağ geçidi bağlantı noktalarına ek olarak kullanırken, Azure Cosmos DB dinamik TCP bağlantı noktaları kullandığından 10000 ve 20000 aralığında açın. Bu bağlantı noktalarını açık değildir ve TCP kullanmaya çalışırsanız, 503 Hizmet kullanılamıyor hatası alırsınız. Aşağıdaki tabloda her bir API için farklı API'ler ve hizmet bağlantı noktası kullanıcı için kullanılabilir bağlantı modları gösterilmektedir:

     |Bağlantı modu  |Desteklenen protokol  |Desteklenen SDK'ları  |API/hizmet bağlantı noktası  |
     |---------|---------|---------|---------|
     |Ağ geçidi  |   HTTPS    |  Tüm SDK'lar    |   Mongo (10250, 10255 olarak, 10256), Table(443), Cassandra(10350), Graph(443) SQL(443)    |
     |Doğrudan    |    HTTPS     |  .NET ve Java SDK'sı    |   10.000 20.000 aralıkta bağlantı noktaları    |
     |Doğrudan    |     TCP    |  .NET SDK    | 10.000 20.000 aralıkta bağlantı noktaları |

     Azure Cosmos DB, HTTPS üzerinden bir basit ve açık RESTful programlama modeli sunar. Ayrıca, aynı zamanda RESTful kendi iletişim modelinde yer alır ve .NET İstemci SDK'sı kullanılabilen verimli bir TCP protokolü sunar. Hem doğrudan TCP hem de HTTPS, SSL ilk kimlik doğrulaması ve şifreleme trafiği için kullanın. En iyi performans için mümkün olduğunda'de TCP protokolünü kullanır.

     Bağlantı modunu ConnectionPolicy parametresi ile DocumentClient örneği oluşturma sırasında yapılandırılır. Doğrudan modunda kullanılırsa, Protokolü içinde ConnectionPolicy parametresi de ayarlayabilirsiniz.

     ```csharp
     var serviceEndpoint = new Uri("https://contoso.documents.net");
     var authKey = "your authKey from the Azure portal";
     DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
     new ConnectionPolicy
     {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp
     });
     ```

     Ağ geçidi modunda kullanılırsa, TCP doğrudan modda, yalnızca desteklenmediği için HTTPS protokolünü her zaman ağ geçidi ile iletişim kurmak için kullanılır ve ConnectionPolicy Protokolü değeri yoksayılır.

     ![Azure Cosmos DB bağlantı İlkesi gösterimi](./media/performance-tips/connection-policy.png)

2. **İlk istek üzerine başlangıç gecikmeyi önlemek için OpenAsync çağırın**

    Adres yönlendirme tablosu getirilecek olduğundan varsayılan olarak, ilk isteği daha yüksek gecikme vardır. Bu başlangıç gecikmesi ilk isteğe önlemek için OpenAsync() bir kez başlatma sırasında şu şekilde çağırmalısınız.

        await client.OpenAsync();
   <a id="same-region"></a>
3. **İstemci performansı için aynı Azure bölgesinde ISS'de**

    Mümkün olduğunda, Azure Cosmos DB veritabanı ile aynı bölgede Azure Cosmos DB çağırma herhangi bir uygulama yerleştirin. Yaklaşık bir karşılaştırması için aynı bölgedeki Azure Cosmos DB için çağrılar 1-2 ms içinde tamamlayın, ancak ABD'nin Doğu Yakası ve Batı arasındaki gecikme süresi > 50 ms. Bu gecikme süresi, Azure veri merkezi sınırına istemciden geçerken istek tarafından gerçekleştirilen rota bağlı olarak istemek için büyük olasılıkla farklılık gösterebilir. En düşük gecikmeyi çağıran uygulama için aynı Azure bölgesindeki sağlanan Azure Cosmos DB uç nokta olarak bulunduğu sağlayarak gerçekleştirilir. Kullanılabilir bölgelerin listesi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

    ![Azure Cosmos DB bağlantı İlkesi gösterimi](./media/performance-tips/same-region.png)
   <a id="increase-threads"></a>
4. **İş parçacıklarının/görevlerin sayısını artırın**

    Ağ üzerinden yapılan çağrıları Azure Cosmos DB için olduğundan, istemci uygulamanın istekleri arasında beklerken çok az süre geçirdiği böylece, isteklerin paralellik derecesini farklılık gerekebilir. Örneğin, kullanıyorsanız. NET [görev paralel Kitaplığı](https://msdn.microsoft.com//library/dd460717.aspx), 100'lük bloklar okuma veya yazma için Azure Cosmos DB görev sırasına göre oluşturun.

5. **Hızlandırılmış ağ iletişimi etkinleştirin**

   Gecikme süresi ve CPU değişimi azaltmak için istemci sanal makinelerin etkin ağ hızlandırılmış olmasını öneririz. Bkz: [hızlandırılmış ağ ile Windows sanal makine oluşturma](../virtual-network/create-vm-accelerated-networking-powershell.md) veya [hızlandırılmış ağ ile bir Linux sanal makinesi oluşturma](../virtual-network/create-vm-accelerated-networking-cli.md) hızlandırılmış ağ iletişimi etkinleştirmek için makaleler.


## <a name="sdk-usage"></a>SDK kullanımı
1. **En son SDK'sını yükleyin**

    Azure Cosmos DB SDK'ları en iyi performansı sağlamak için sürekli geliştirilen. Bkz: [Azure Cosmos DB SDK'sı](documentdb-sdk-dotnet.md) sayfalarının en son SDK'sı belirlemek ve geliştirmeleri gözden geçirin.
2. **Tek bir Azure Cosmos DB istemci uygulama ömrü boyunca kullanın**

    Her bir DocumentClient örneğini, iş parçacığı açısından güvenli ve verimli bağlantı yönetimi ve doğrudan modunda çalışırken, adresi önbelleğe alma gerçekleştirir. Etkin bağlantı yönetimi ve daha iyi performans DocumentClient tarafından izin vermek için tek bir uygulama etki alanı başına DocumentClient örneğini uygulama ömrü boyunca kullanılması önerilir.

   <a id="max-connection"></a>
3. **System.Net MaxConnections konak, ağ geçidi modunu kullanırken artırın.**

    Azure Cosmos DB, ağ geçidi modunu kullanırken HTTPS/REST yapılır ve olan istekleri için konak adı veya IP adresi başına varsayılan bağlantı üst sınırına tabi. İstemci Kitaplığı, Azure Cosmos DB için çok sayıda eşzamanlı bağlantıyı kullanabilir, böylece daha yüksek bir değere (100-1000) MaxConnections ayarlamanız gerekebilir. .NET SDK 1.8.0 ve üzeri için varsayılan değer [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) 50'dir ve değeri değiştirmek için ayarlayabileceğiniz [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx)daha yüksek bir değer.   
4. **Bölümlenmiş koleksiyonlar için paralel sorgular ayarlama**

     SQL .NET SDK'sı sürüm 1.9.0 ve üstü bölünmüş bir koleksiyona paralel sorgu sağlayan destek paralel sorgular. Daha fazla bilgi için [kod örnekleri](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) SDK'ları ile çalışmayla ilgili. Paralel sorgular seri kendi karşılığı sorgunun gecikme süresi ve aktarım hızı artırmak için tasarlanmıştır. Paralel sorguları, kullanıcıların özel-(a) Maxanalyticsunits gereksinimlerine uyacak şekilde ayarlayabilirsiniz iki parametre sağlar: daha sonra bölüm sayısı, paralel ve (b) MaxBufferedItemCount sorgulanabilen denetimine: sayısını denetlemek için önceden getirilen sonuç.

    (a) ***ayarlama Maxanalyticsunits\:***  paralel sorgu works birden çok bölümü paralel sorgulayarak. Ancak, tek bir bölümlenmiş toplama verilerden göre sorgu seri olarak getirilir. Bu nedenle, diğer tüm sistem koşulları aynı kalması koşuluyla en yüksek performanslı sorgu başarmaya maksimum olasılığını bir bölüm sayısı Maxanalyticsunits ayarlama sahiptir. Bölüm sayısı bilmiyorsanız, Maxanalyticsunits yüksek bir sayıya ayarlayabilirsiniz ve sistem (bölüm, kullanıcı tarafından sağlanan giriş sayısı) en düşük Maxanalyticsunits olarak seçer.

    Sorgu ile ilgili tüm bölümler arasında verileri eşit olarak dağıtılmış, paralel sorgular en iyi avantajları oluşturduğunun dikkat edin önemlidir. Bölünmüş bir koleksiyona birkaç bölümlerde (bir bölüm en kötü durumda) tüm veya bir sorgu tarafından döndürülen verilerin çoğunu yoğunlaşmıştır ve ardından sorgu performansını bu bölümler tarafından performansı düşürdüğünü gösterir şekilde bölümlenmiş durumunda.

    (b) ***ayarlama MaxBufferedItemCount\:***  paralel sorgu sonuçları istemci tarafından sonuçlarının geçerli toplu iş işlenirken önceden getirme için tasarlanmıştır. Önceden getirme, bir sorgunun toplam gecikme süresi gelişme yardımcı olur. MaxBufferedItemCount önceden getirilen sonuç sayısını sınırlamak için parametredir. Döndürülen sonuçları beklenen sayıda MaxBufferedItemCount (veya daha yüksek bir sayı) ayarı, önceden getirme en büyük avantajı almak sorgu sağlar.

    Önceden getirme Maxanalyticsunits bağımsız olarak aynı şekilde çalışır ve tüm bölümleri veriler için tek bir arabellek yoktur.  
5. **Sunucu tarafı GC'de yansıtılmalıdır Aç**

    Çöp toplama sıklığını azaltmayı, bazı durumlarda yardımcı olabilir. . NET'te, ayarlama [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) true.
6. **Geri alma RetryAfter aralıklarla uygulayın**

    Performans testi sırasında istekleri küçük bir oranını kısıtlanan kadar yük yükseltmeniz gerekir. Kısıtlanmış, istemci uygulama kısıtlama üzerinde geri alma için sunucu tarafından belirtilen yeniden deneme aralığı gerekir. Geri alma uyarak bekleme süresi yeniden denemeler arasındaki en az miktarda harcama sağlar. Yeniden deneme ilkesi desteği dahildir sürüm 1.8.0 ve sonraki sürümlerde SQL [.NET](sql-api-sdk-dotnet.md) ve [Java](sql-api-sdk-java.md), sürüm 1.9.0 ve üstü, [Node.js](sql-api-sdk-node.md) ve [Python](sql-api-sdk-python.md), ve tüm desteklenen sürümlerinde [.NET Core](sql-api-sdk-dotnet-core.md) SDK'ları. Daha fazla bilgi için [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
    
    1.19 ve .NET SDK'sının daha yeni sürüm ile ek tanılama bilgilerini günlüğe kaydetme ve aşağıdaki örnekte gösterildiği gibi gecikme sorunlarını gidermek için bir mekanizma yoktur. Daha yüksek bir okuma gecikme süresi olan istekler için tanılama dize oturum açabilirsiniz. Yakalanan tanılama dize 429s belirli bir istek için gözlemlenen kaç kez anlamanıza yardımcı olur.
    ```csharp
    ResourceResponse<Document> readDocument = await this.readClient.ReadDocumentAsync(oldDocuments[i].SelfLink);
    readDocument.RequestDiagnosticsString 
    ```
    
7. **İstemci iş yükü ölçeklendirin**

    Yüksek performans düzeylerinde sınıyorsanız (> 50.000 RU/sn), istemci uygulama makine kullanıma CPU veya ağ kullanımı capping nedeniyle performans sorunu hale gelebilir. Bu noktaya ulaşın, istemci uygulamalarınızı birden çok sunucu arasında ölçeklendirme gerçekleştirerek Azure Cosmos DB hesabı daha fazla göndermeye devam edebilirsiniz.
8. **Belge URI'ler alt okuma gecikme süresi için önbelleğe alma**

    Belge okuma en iyi performans için mümkün olduğunca bir URI'leri önbelleğe alın. Kaynak oluştururken ResourceId önbelleğe alma mantığını tanımlamak zorunda. ResourceId dayalı aramalar tabanlı ad arama hızlı olduğundan bu değerleri önbelleğe alma performansını artırır. 

   <a id="tune-page-size"></a>
1. **Daha iyi performans için sorgu/okuma akışlarına yönelik sayfa boyutunu ayarlama**

   Bir toplu gerçekleştirme belgelerini okuma akışı işlevleri (örneğin, ReadDocumentFeedAsync) kullanarak veya bir SQL sorgusu gönderirken, okuma, sonuç kümesi çok büyükse sonuçları bölümlenmiş bir biçimde döndürülür. Varsayılan olarak, sonuçları 100 öğe 1 MB veya öbekler halinde döndürülür, ilk isabet sınırlarından hangisi.

   Sayısını azaltmak için ağ gidiş dönüşleri yürürlükteki tüm sonuçları almak için gereken, sayfa boyutu kullanarak artırabilirsiniz [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) en fazla 1000 istek üstbilgisi. Yalnızca birkaç sonuçları görüntülemek için gerek duyduğunuz durumlarda Örneğin, kullanıcı arabirimi veya uygulama API'nizi yalnızca 10 döndürürse birer sonuçları, 10 okuma ve sorgular için kullanılan aktarım hızını azaltmak için sayfa boyutunu da azaltabilirsiniz.

   > [!NOTE] 
   > MaxItemCount özelliği yalnızca sayfalandırma amaçla kullanılmamalıdır. Ana kullanım olan tek bir sayfasında en fazla öğe sayısını azaltarak sorguların performansını geliştirmek için döndürülen.  

   Kullanılabilir Azure Cosmos DB SDK'larını kullanarak sayfa boyutunu da ayarlayabilirsiniz. [MaxItemCount](/dotnet/api/microsoft.azure.documents.client.feedoptions.maxitemcount?view=azure-dotnet) FeedOptions özelliğinde öğeleri enmuration işlemde döndürülecek en fazla sayısını ayarlamanızı sağlar. Zaman `maxItemCount` ayarlanır -1 olarak SDK belge boyutuna bağlı olarak en iyi değeri otomatik olarak bulur. Örneğin:
    
   ```csharp
    IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
   ```
    
   Bir sorgu yürütüldüğünde sonuç verileri TCP paket içinde gönderilir. İçin çok düşük bir değer belirtirseniz `maxItemCount`, TCP paketin içindeki veri göndermek için gereken dönüş sayısı yüksek, hangi performansını etkiler. Ne için ayarlanacak değer olmadığından emin değilseniz, bu nedenle `maxItemCount` özelliği -1 olarak ayarlayın ve varsayılan değer seçin SDK'sı izin en iyisidir. 

10. **İş parçacıklarının/görevlerin sayısını artırın**

    Bkz: [iş parçacıklarının/görevlerin sayısını artırın](#increase-threads) ağ bölümünde.

11. **64-bit ana işleme kullanın**

    SQL SDK'sı bir 32-bit ana işlemde SQL .NET SDK'sı sürüm 1.11.4 kullanırken ve üstünde çalışır. Ancak, çapraz bölüm sorguları kullanıyorsanız, 64-bit ana işleme daha iyi performans için önerilir. 64 bit izleyin değiştirmek için aşağıdaki adımları uygulamanız türüne bağlı şekilde aşağıdaki uygulama türlerini varsayılan olarak 32-bit ana bilgisayar işlemi vardır:

    - Yürütülebilir uygulamalar için bu kaldırarak yapılabilir **32 bit tercih et** seçeneğini **proje özellikleri** penceresi, **derleme** sekmesi.

    - VSTest test projeleri tabanlı için bu seçerek yapılabilir **Test**->**Test ayarlarını**->**varsayılan İşlemci mimarisi X64 olarak**, gelen **Visual Studio Test** menü seçeneği.

    - Yerel olarak dağıtılan ASP.NET Web uygulamaları için bu kontrol ederek yapılabilir **web siteleri ve projeleri için IIS Express'in 64 bit sürümünü kullanan**altında **Araçları**->**seçenekleri**  -> **Projeler ve çözümler**->**Web projeleri**.

    - Azure'da dağıtılan ASP.NET Web uygulamaları için bu seçerek yapılabilir **64-bit olarak Platform** içinde **uygulama ayarları** Azure portalında.

## <a name="indexing-policy"></a>Dizin Oluşturma İlkesi
 
1. **Kullanılmayan yolları daha hızlı yazmalar için dizine elmadan hariç tut**

    Cosmos DB'nin dizin oluşturma ilkesini dahil etmek veya dizin yolları (IndexingPolicy.IncludedPaths ve IndexingPolicy.ExcludedPaths) yararlanarak dizine elmadan hariç tutmak için hangi belge yolları belirtmenizi sağlar. Dizin oluşturma maliyetleri doğrudan dizini benzersiz yollara sayısını bağıntılı olan gibi dizin yolları kullanımını geliştirilmiş yazma performansını ve sorgu desenleri önceden bilinmektedir senaryoları için daha düşük dizin depolaması sunabilir.  Örneğin, aşağıdaki kod dizin oluşturma kullanarak belge (bir alt ağacı) bölümünün tamamını dışlama gösterir "*" joker karakter.

    ```csharp
    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
    ```

    Daha fazla bilgi için [Azure Cosmos DB dizinleme ilkeleri](index-policy.md).

## <a name="throughput"></a>Aktarım hızı
<a id="measure-rus"></a>

1. **Ölçün ve için alt istek birimi/saniye kullanım ayarlayın.**

    Azure Cosmos DB veritabanı işlemleri UDF'ler, saklı yordamlar ve tetikleyicilerle bir veritabanı koleksiyonu içindeki belgeler üzerinde ilişkisel ve hiyerarşik sorgular da dahil olmak üzere zengin bir özellik kümesi sunar. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, CPU, GÇ ve işlemi tamamlamak için gerekli belleğe göre değişir. Hakkında düşünmek ve donanım kaynaklarını yönetmek yerine, bir istek Birimi'ni (RU), çeşitli veritabanı işlemlerini gerçekleştirmek ve uygulama isteğine hizmet sağlamak için gereken kaynaklar için tek ölçü olarak düşünebilirsiniz.

    Üretilen iş sayısına göre hazırlanır [istek birimi](request-units.md) her kapsayıcı için ayarlayın. İstek birimi tüketimini Saniyedeki oranı olarak değerlendirilir. Kendi kapsayıcı için sağlanan istek birimi hızı aşan uygulamaları oranı kapsayıcı için sağlanan düzeyinin altına düşene kadar sınırlıdır. Uygulamanıza daha yüksek bir üretilen iş düzeyi gerekiyorsa, ek istek birimi sağlayarak aktarım hızınızı artırabilirsiniz. 

    Kaç tane istek birimi bir işlem için kullanılan bir sorgu karmaşıklığı etkiler. Sorgu işlemlerinin maliyetini doğrulamaları sayısı, koşullarına, UDF'ler sayısı ve boyutu, kaynak veri kümesinin tüm genel yapısını etkiler.

    Herhangi bir işlem yükü ölçmek için (oluşturma, güncelleştirme veya silme) incelemek [x-ms-istek-ücretsiz olarak](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üst bilgisi (veya eşdeğer RequestCharge özelliği ResourceResponse<T> veya FeedResponse<T> içinde. Bu işlemler tarafından kullanılan istek birimleri sayısını ölçmek için NET SDK).

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

    Bu üst bilgisinde döndürülen istek hızınız sağladığınız aktarım avantajlarının ücrettir (yani, 2000 RU / saniye). Örneğin, yukarıdaki sorgu 1000 1 KB-belgeler döndürürse, işlemin maliyeti 1000'dir. Bu nedenle, bir saniye içinde sonraki istekleri hız sınırı önce yalnızca iki tür isteklere sunucunun geliştirir. Daha fazla bilgi için [istek birimi](request-units.md) ve [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Tanıtıcı oran sınırlama/istek oranı çok büyük**

    Bir istemci bir hesap için ayrılmış aktarım hızını aşmayı dener, sunucuda performans düşüşü olmadan ve aktarım hızı kapasitesine ayrılmış düzeyi dışında hiçbir kullanımı yoktur. Sunucu sıd'lerde istek RequestRateTooLarge (HTTP durum kodu 429) ile bitemez ve dönüş [x-ms-yeniden-sonra-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) kullanıcı çakıştığını önce beklemesi gereken milisaniye cinsinden süre miktarını belirten üstbilgisi İstek.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK'ları tüm örtük olarak bu yanıt catch, sunucu tarafından belirtilen retry-after üst bilgisi saygı ve isteği yeniden deneyin. Hesabınızda aynı anda birden çok istemci tarafından erişilen sürece sonraki yeniden deneme işlemi başarılı olur.

    Üst üste istek hızı tutarlı bir şekilde çalışan birden fazla istemciniz varsa, 9 istemci tarafından dahili olarak ayarlanmış varsayılan yeniden deneme sayısı yeterli değil; Bu durumda, istemci uygulamanın 429 durum koduyla bir DocumentClientException oluşturur. Varsayılan yeniden deneme sayısı ConnectionPolicy örneğinde RetryOptions ayarlayarak değiştirilebilir. İstek, istek hızı üzerinde çalışmaya devam ederse varsayılan olarak, durum kodu 429 DocumentClientException 30 saniye sonra bir toplam bekleme süresi döndürülür. Bu geçerli bir yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda bile oluşur, varsayılan 9 veya kullanıcı tanımlı bir değer olmalıdır.

    Dayanıklılık ve kullanılabilirlik uygulamalarının çoğu için iyileştirmek için otomatik yeniden deneme davranışı yardımcı olsa da, bu performans kıyaslamaları, özellikle gecikme süresini ölçme olduğunda yaparken at odds gelebilir.  Denemeyi sunucu kısıtlama denk gelir ve istemci SDK'sı sessiz bir şekilde yeniden denemek için neden olan istemci gözlemlenen gecikme çıkmasına. Ani gecikme süresi artışlarına sırasında performans denemelerini önlemek için her bir işlem tarafından döndürülen ücret ölçün ve istekleri ayrılmış istek hızı altında çalıştığından emin olun. Daha fazla bilgi için [istek birimi](request-units.md).
3. **Daha yüksek verimliğe yönelik daha küçük belgeleri için Tasarım**

    Belirli bir işlemi (yani, istek işleme maliyeti) istek ücreti, doğrudan belge boyutunu ilişkilendirilir. Büyük belgelerin işlemleri daha küçük belgeleri işlemlerinde daha fazla maliyet.

## <a name="next-steps"></a>Sonraki adımlar
Bazı istemci makinelerde yüksek performanslı senaryoları için Azure Cosmos DB değerlendirmek için kullanılan bir örnek uygulama için bkz [performans ve ölçek testi Azure Cosmos DB ile](performance-testing.md).

Ayrıca, uygulamanızı ölçeklendirme ve yüksek performans için tasarlama hakkında daha fazla bilgi için bkz: [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).
