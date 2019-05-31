---
title: Java için Azure Cosmos DB performans ipuçları
description: Azure Cosmos DB veritabanı performansını artırmak üzere istemci yapılandırma seçenekleri öğrenin
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.openlocfilehash: 2ce8c0b369cd59ac61279fe3c7acd2cdecfc007c
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66225594"
---
# <a name="performance-tips-for-azure-cosmos-db-and-java"></a>Azure Cosmos DB ve Java için performans ipuçları

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB garantili gecikme süresi ve aktarım hızı ile sorunsuz bir şekilde ölçeklenen bir hızlı ve esnek dağıtılmış bir veritabanıdır. Azure Cosmos DB ile veritabanınızı ölçeklendirmek için karmaşık kodlar yazmanıza ya da büyük mimari değişiklikler yapmak gerekmez. Yukarı ve aşağı ölçeklendirme olarak tek bir API çağrısı yapmak oldukça kolaydır. Daha fazla bilgi için bkz. [kapsayıcının aktarım hızını sağlamak nasıl](how-to-provision-container-throughput.md) veya [veritabanı aktarım hızını sağlamak nasıl](how-to-provision-database-throughput.md). Ancak, Azure Cosmos DB ağ çağrıları erişilir vardır kullanırken en yüksek performans elde etmek için olun istemci-tarafı iyileştirmeleri [SQL Java SDK'sı](documentdb-sdk-java.md).

Açmanızı isteyen, "nasıl veritabanı performansımı geliştirebilirim şekilde?" Aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ
<a id="direct-connection"></a>

1. **Bağlantı modu: DirectHttps kullanın**

    Bir istemci, Azure Cosmos DB'ye nasıl bağlanır? performansını gözlemler istemci tarafı gecikme süresi açısından özellikle önemli etkilere sahiptir. İstemciyi yapılandırmak için kullanılabilen bir anahtar yapılandırma [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy) – [ConnectionMode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode).  İki kullanılabilir ConnectionModes şunlardır:

   1. [Ağ Geçidi (varsayılan)](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode)
   2. [DirectHttps](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode)

      Ağ geçidi modu, tüm SDK platformlarında desteklenir ve yapılandırılmış varsayılandır.  Uygulamanız, kurumsal ağ içinden katı güvenlik duvarı kısıtlamalarıyla çalışıyorsa, standart HTTPS bağlantı noktası ve tek bir uç nokta kullandığından ağ geçidi en iyi seçenektir. Performansta düşüş, ancak veri okuma veya Azure Cosmos DB için yazılan her zaman ağ geçidi modu ek ağ atlama içermesidir. Bu nedenle, DirectHttps modu daha az ağ atlamaları nedeniyle daha iyi performans sunar. 

      Java SDK'sı bir Aktarım Protokolü HTTPS kullanır. HTTPS, ilk kimlik doğrulaması ve trafiği şifrelemek için SSL kullanır. Java SDK'sı kullanırken, yalnızca HTTPS 443 numaralı bağlantı noktası açık olması gerekir. 

      ConnectionMode ConnectionPolicy parametresi ile DocumentClient örneği oluşturma sırasında yapılandırılır. 

      ```Java
      public ConnectionPolicy getConnectionPolicy() {
        ConnectionPolicy policy = new ConnectionPolicy();
        policy.setConnectionMode(ConnectionMode.DirectHttps);
        policy.setMaxPoolSize(1000);
        return policy;
      }
        
      ConnectionPolicy connectionPolicy = new ConnectionPolicy();
      DocumentClient client = new DocumentClient(HOST, MASTER_KEY, connectionPolicy, null);
      ```

      ![Azure Cosmos DB bağlantı İlkesi gösterimi](./media/performance-tips-java/connection-policy.png)

   <a id="same-region"></a>
2. **İstemci performansı için aynı Azure bölgesinde ISS'de**

    Mümkün olduğunda, Azure Cosmos DB veritabanı ile aynı bölgede Azure Cosmos DB çağırma herhangi bir uygulama yerleştirin. Yaklaşık bir karşılaştırması için aynı bölgedeki Azure Cosmos DB için çağrılar 1-2 ms içinde tamamlayın, ancak ABD'nin Doğu Yakası ve Batı arasındaki gecikme süresi > 50 ms. Bu gecikme süresi, Azure veri merkezi sınırına istemciden geçerken istek tarafından gerçekleştirilen rota bağlı olarak istemek için büyük olasılıkla farklılık gösterebilir. En düşük gecikmeyi çağıran uygulama için aynı Azure bölgesindeki sağlanan Azure Cosmos DB uç nokta olarak bulunduğu sağlayarak gerçekleştirilir. Kullanılabilir bölgelerin listesi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

    ![Azure Cosmos DB bağlantı İlkesi gösterimi](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>SDK kullanımı
1. **En son SDK'sını yükleyin**

    Azure Cosmos DB SDK'ları en iyi performansı sağlamak için sürekli geliştirilen. Bkz: [Azure Cosmos DB SDK'sı](documentdb-sdk-java.md) sayfalarının en son SDK'sı belirlemek ve geliştirmeleri gözden geçirin.
2. **Tek bir Azure Cosmos DB istemci uygulama ömrü boyunca kullanın**

    Her [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient) örneği iş parçacığı açısından güvenli ve verimli bağlantı yönetimi ve doğrudan modunda çalışırken, adresi önbelleğe alma gerçekleştirir. Etkin bağlantı yönetimi ve daha iyi performans DocumentClient tarafından izin vermek için tek bir uygulama etki alanı başına DocumentClient örneğini uygulama ömrü boyunca kullanılması önerilir.

   <a id="max-connection"></a>
3. **Ağ geçidi modunu kullanırken, ana bilgisayar başına MaxPoolSize artırın**

    Azure Cosmos DB, ağ geçidi modunu kullanırken HTTPS/REST yapılır ve olan istekleri için konak adı veya IP adresi başına varsayılan bağlantı üst sınırına tabi. İstemci Kitaplığı, Azure Cosmos DB için çok sayıda eşzamanlı bağlantıyı kullanabilir, böylece daha yüksek bir değere (200-1000) MaxPoolSize ayarlamanız gerekebilir. Varsayılan değer için Java SDK'sında [ConnectionPolicy.getMaxPoolSize](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.getmaxpoolsize) 100'dür. Kullanım [setMaxPoolSize]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.setmaxpoolsize) değeri değiştirmek için.

4. **Bölümlenmiş koleksiyonlar için paralel sorgular ayarlama**

    Azure Cosmos DB SQL Java SDK'sı sürüm 1.9.0 ve üstü bölünmüş bir koleksiyona paralel sorgu sağlayan destek paralel sorgular. Daha fazla bilgi için [kod örnekleri](https://github.com/Azure/azure-documentdb-java/tree/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples) SDK'ları ile çalışmayla ilgili. Paralel sorgular seri kendi karşılığı sorgunun gecikme süresi ve aktarım hızı artırmak için tasarlanmıştır.

    (a) ***setMaxDegreeOfParallelism ayarlama\:***  paralel iş birden çok bölümü paralel sorgulayarak sorgular. Ancak, verileri ayrı bölünmüş bir koleksiyona göre sorgu seri olarak getirilir. Bu nedenle, kullanın [setMaxDegreeOfParallelism](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptions.setmaxdegreeofparallelism) en yüksek performanslı sorgu başarmaya maksimum olasılığını olan bölüm sayısı ayarlamak için sağlanan tüm sistem koşullar aynı kalır. Bölüm sayısı bilmiyorsanız, setMaxDegreeOfParallelism yüksek bir sayı ayarlamak için kullanabileceğiniz ve sistem (bölüm, kullanıcı tarafından sağlanan giriş sayısı) en düşük maksimum paralellik derecesi seçer. 

    Sorgu ile ilgili tüm bölümler arasında verileri eşit olarak dağıtılmış, paralel sorgular en iyi avantajları oluşturduğunun dikkat edin önemlidir. Bölünmüş bir koleksiyona birkaç bölümlerde (bir bölüm en kötü durumda) tüm veya bir sorgu tarafından döndürülen verilerin çoğunu yoğunlaşmıştır ve ardından sorgu performansını bu bölümler tarafından performansı düşürdüğünü gösterir şekilde bölümlenmiş durumunda.

    (b) ***setMaxBufferedItemCount ayarlama\:***  paralel sorgu sonuçları istemci tarafından sonuçlarının geçerli toplu iş işlenirken önceden getirme için tasarlanmıştır. Önceden getirme, bir sorgunun toplam gecikme süresi gelişme yardımcı olur. setMaxBufferedItemCount önceden getirilen sonuç sayısını sınırlar. Ayarlayarak [setMaxBufferedItemCount](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptions.setmaxbuffereditemcount) beklenen döndürülen sonuç sayısı (veya daha yüksek bir sayı) için bu en büyük avantajı önceden getirme almak sorgu sağlar.

    Önceden getirme Maxanalyticsunits bağımsız olarak aynı şekilde çalışır ve tüm bölümleri veriler için tek bir arabellek yoktur.  

5. **Geri alma getRetryAfterInMilliseconds aralıklarla uygulayın**

    Performans testi sırasında istekleri küçük bir oranını kısıtlanan kadar yük yükseltmeniz gerekir. Kısıtlanmış, istemci uygulama kısıtlama üzerinde geri alma için sunucu tarafından belirtilen yeniden deneme aralığı gerekir. Geri alma uyarak bekleme süresi yeniden denemeler arasındaki en az miktarda harcama sağlar. Yeniden deneme ilkesi desteği dahildir sürümünde 1.8.0 ve üstü, [Java SDK'sı](documentdb-sdk-java.md). Daha fazla bilgi için [getRetryAfterInMilliseconds](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclientexception.getretryafterinmilliseconds).

6. **İstemci iş yükü ölçeklendirin**

    Yüksek performans düzeylerinde sınıyorsanız (> 50.000 RU/sn), istemci uygulama makine kullanıma CPU veya ağ kullanımı capping nedeniyle performans sorunu hale gelebilir. Bu noktaya ulaşın, istemci uygulamalarınızı birden çok sunucu arasında ölçeklendirme gerçekleştirerek Azure Cosmos DB hesabı daha fazla göndermeye devam edebilirsiniz.

7. **Ad tabanlı adresleme kullanın**

    Ad tabanlı adresleme, bağlantıları biçimi olduğu kullanın `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, SelfLinks yerine (\_kendi), biçime sahip `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` Resourceıds bağlantı oluşturmak için kullanılan tüm kaynakları alınıyor önlemek için. (Büyük olasılıkla aynı ada sahip) bu kaynakları yeniden gibi Ayrıca, bu önbelleğe alma yardımcı.

   <a id="tune-page-size"></a>
8. **Daha iyi performans için sorgu/okuma akışlarına yönelik sayfa boyutunu ayarlama**

    Akışı, bir toplu gerçekleştirme belgeleri okuma kullanarak okuma (örneğin, [readDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.readdocuments)) ya da sonuç kümesi çok büyükse bir SQL sorgusu gönderirken, sonuçları bölümlenmiş bir biçimde döndürülür. Varsayılan olarak, sonuçları 100 öğe 1 MB veya öbekler halinde döndürülür, ilk isabet sınırlarından hangisi.

    Sayısını azaltmak için ağ gidiş dönüşleri yürürlükteki tüm sonuçları almak için gereken, sayfa boyutu kullanarak artırabilirsiniz [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) en fazla 1000 istek üstbilgisi. Yalnızca birkaç sonuçları görüntülemek için gerek duyduğunuz durumlarda Örneğin, kullanıcı arabirimi veya uygulama API'nizi yalnızca 10 döndürürse birer sonuçları, 10 okuma ve sorgular için kullanılan aktarım hızını azaltmak için sayfa boyutunu da azaltabilirsiniz.

    Sayfa boyutu kullanarak da ayarlayabilir [setPageSize yöntemi](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptionsbase.setpagesize).

## <a name="indexing-policy"></a>Dizin Oluşturma İlkesi
 
1. **Kullanılmayan yolları daha hızlı yazmalar için dizine elmadan hariç tut**

    Azure Cosmos DB'nin dizin oluşturma ilkesini dahil etmek veya dizin yolları yararlanarak dizine elmadan hariç tutmak için hangi belge yolları belirtmenize olanak verir ([setIncludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.indexingpolicy.setincludedpaths) ve [setExcludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.indexingpolicy.setexcludedpaths)). Dizin oluşturma maliyetleri doğrudan dizini benzersiz yollara sayısını bağıntılı olan gibi dizin yolları kullanımını geliştirilmiş yazma performansını ve sorgu desenleri önceden bilinmektedir senaryoları için daha düşük dizin depolaması sunabilir.  Örneğin, aşağıdaki kod belgeleri bölümünün tamamını (diğer adıyla) hariç tutmak nasıl gösterir bir alt ağacı) dizin oluşturma kullanarak "*" joker karakter.

    ```Java
    Index numberIndex = Index.Range(DataType.Number);
    numberIndex.set("precision", -1);
    indexes.add(numberIndex);
    includedPath.setIndexes(indexes);
    includedPaths.add(includedPath);
    indexingPolicy.setIncludedPaths(includedPaths);
    collectionDefinition.setIndexingPolicy(indexingPolicy);
    ```

    Daha fazla bilgi için [Azure Cosmos DB dizinleme ilkeleri](indexing-policies.md).

## <a name="throughput"></a>Aktarım hızı
<a id="measure-rus"></a>

1. **Ölçün ve için alt istek birimi/saniye kullanım ayarlayın.**

    Azure Cosmos DB veritabanı işlemleri UDF'ler, saklı yordamlar ve tetikleyicilerle bir veritabanı koleksiyonu içindeki belgeler üzerinde ilişkisel ve hiyerarşik sorgular da dahil olmak üzere zengin bir özellik kümesi sunar. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, CPU, GÇ ve işlemi tamamlamak için gerekli belleğe göre değişir. Hakkında düşünmek ve donanım kaynaklarını yönetmek yerine, bir istek Birimi'ni (RU), çeşitli veritabanı işlemlerini gerçekleştirmek ve uygulama isteğine hizmet sağlamak için gereken kaynaklar için tek ölçü olarak düşünebilirsiniz.

    Üretilen iş sayısına göre hazırlanır [istek birimi](request-units.md) her kapsayıcı için ayarlayın. İstek birimi tüketimini Saniyedeki oranı olarak değerlendirilir. Kendi kapsayıcı için sağlanan istek birimi hızı aşan uygulamaları oranı kapsayıcı için sağlanan düzeyinin altına düşene kadar sınırlıdır. Uygulamanıza daha yüksek bir üretilen iş düzeyi gerekiyorsa, ek istek birimi sağlayarak aktarım hızınızı artırabilirsiniz. 

    Kaç tane istek birimi bir işlem için kullanılan bir sorgu karmaşıklığı etkiler. Sorgu işlemlerinin maliyetini doğrulamaları sayısı, koşullarına, UDF'ler sayısı ve boyutu, kaynak veri kümesinin tüm genel yapısını etkiler.

    Herhangi bir işlem yükü ölçmek için (oluşturma, güncelleştirme veya silme) İnceleme [x-ms-istek-ücretsiz olarak](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üst bilgisi (veya eşdeğer RequestCharge özelliği [ResourceResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.resourceresponse) veya [FeedResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedresponse) bu işlemleri tarafından kullanılan istek birimleri sayısını ölçmek için.

    ```Java
    ResourceResponse<Document> response = client.createDocument(collectionLink, documentDefinition, null, false);

    response.getRequestCharge();
    ```             

    Bu üst bilgisinde döndürülen istek hızınız sağladığınız aktarım avantajlarının ücrettir. Örneğin, 2000 varsa sağlanan RU/s ve işlem maliyeti önceki sorgunun 1000 1 KB-belgeler döndürürse, 1000'dir. Bu nedenle, bir saniye içinde sonraki istekleri hız sınırı önce yalnızca iki tür isteklere sunucunun geliştirir. Daha fazla bilgi için [istek birimi](request-units.md) ve [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).
   <a id="429"></a>
1. **Tanıtıcı oran sınırlama/istek oranı çok büyük**

    Bir istemci bir hesap için ayrılmış aktarım hızını aşmayı dener, sunucuda performans düşüşü olmadan ve aktarım hızı kapasitesine ayrılmış düzeyi dışında hiçbir kullanımı yoktur. Sunucu sıd'lerde istek RequestRateTooLarge (HTTP durum kodu 429) ile bitemez ve dönüş [x-ms-yeniden-sonra-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) kullanıcı çakıştığını önce beklemesi gereken milisaniye cinsinden süre miktarını belirten üstbilgisi İstek.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK'ları tüm örtük olarak bu yanıt catch, sunucu tarafından belirtilen retry-after üst bilgisi saygı ve isteği yeniden deneyin. Hesabınızda aynı anda birden çok istemci tarafından erişilen sürece sonraki yeniden deneme işlemi başarılı olur.

    Üst üste istek hızı tutarlı bir şekilde çalışan birden fazla istemciniz varsa, 9 istemci tarafından dahili olarak ayarlanmış varsayılan yeniden deneme sayısı yeterli değil; Bu durumda, istemci oluşturur bir [DocumentClientException](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclientexception) 429 uygulama durumuyla kod. Varsayılan yeniden deneme sayısı kullanarak değiştirilebilir [setRetryOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.setretryoptions) üzerinde [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy) örneği. İstek, istek hızı üzerinde çalışmaya devam ederse varsayılan olarak, durum kodu 429 DocumentClientException 30 saniye sonra bir toplam bekleme süresi döndürülür. Bu geçerli bir yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda bile oluşur, varsayılan 9 veya kullanıcı tanımlı bir değer olmalıdır.

    Dayanıklılık ve kullanılabilirlik uygulamalarının çoğu için iyileştirmek için otomatik yeniden deneme davranışı yardımcı olsa da, bu performans kıyaslamaları, özellikle gecikme süresini ölçme olduğunda yaparken at odds gelebilir.  Denemeyi sunucu kısıtlama denk gelir ve istemci SDK'sı sessiz bir şekilde yeniden denemek için neden olan istemci gözlemlenen gecikme çıkmasına. Ani gecikme süresi artışlarına sırasında performans denemelerini önlemek için her bir işlem tarafından döndürülen ücret ölçün ve istekleri ayrılmış istek hızı altında çalıştığından emin olun. Daha fazla bilgi için [istek birimi](request-units.md).
3. **Daha yüksek verimliğe yönelik daha küçük belgeleri için Tasarım**

    Belirli bir işlemi istek ücreti (istek işleme maliyeti) doğrudan belge boyutunu ilişkilendirilir. Büyük belgelerin işlemleri daha küçük belgeleri işlemlerinde daha fazla maliyet.

## <a name="next-steps"></a>Sonraki adımlar
Uygulama ölçeklendirme ve yüksek performans için tasarlama hakkında daha fazla bilgi için bkz: [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).
