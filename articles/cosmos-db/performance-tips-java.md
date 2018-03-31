---
title: Java için Azure Cosmos DB performans ipuçları | Microsoft Docs
description: Azure Cosmos DB veritabanı performansını iyileştirmek için istemci yapılandırma seçenekleri öğrenin
keywords: Veritabanı performansı nasıl
services: cosmos-db
author: mimig1
manager: jhubbard
editor: ''
documentationcenter: ''
ms.assetid: dfe8f426-3c98-4edc-8094-092d41f2795e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: mimig
ms.openlocfilehash: 3a6c7c51810375574895643cea2e0e24508fa382
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
> [!div class="op_single_selector"]
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 
> 

# <a name="performance-tips-for-azure-cosmos-db-and-java"></a>Azure Cosmos DB ve Java için performans ipuçları
Azure Cosmos DB garantili gecikme süresi ve verimlilik ile sorunsuz bir şekilde ölçeklendirilebilen bir hızlı ve esnek dağıtılmış veritabanıdır. Büyük mimari değişiklikler yaptığınızda veya Azure Cosmos DB veritabanınızla ölçeklendirmek için karmaşık kodlar yazmak zorunda değildir. Yukarı ve aşağı doğru ölçeklendirme tek bir API çağrısı yapma olarak kadar kolay veya [SDK yöntem çağrısı](set-throughput.md#set-throughput-java). Ancak, Azure Cosmos DB ağ çağrıları erişilir vardır kullanırken en yüksek performans elde etmek için yapabilir istemci-tarafı iyileştirmeler [SQL Java SDK'sı](documentdb-sdk-java.md).

Soran, "nasıl ı my veritabanı performansını geliştirebilir şekilde?" Aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ
<a id="direct-connection"></a>

1. **Bağlantı modu: kullanım DirectHttps**

    Bir istemci için Azure Cosmos DB nasıl bağlandığını gözlemlenen istemci-tarafı gecikme açısından özellikle performansı önemli etkilere sahiptir. İstemciyi yapılandırmak için kullanılabilen bir anahtar yapılandırma [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy) – [ConnectionMode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode).  İki kullanılabilir ConnectionModes şunlardır:

   1. [Ağ Geçidi (varsayılan)](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode.gateway)
   2. [DirectHttps](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode.directhttps)

    Ağ geçidi modu tüm SDK platformlarda desteklenir ve yapılandırılmış varsayılandır.  Uygulamanız bir şirket ağı içinde katı güvenlik duvarı kısıtlamalarıyla çalışıyorsa standart HTTPS bağlantı noktası ve tek bir uç nokta kullandığından ağ geçidi en iyi seçimdir. Performans kolaylığını ancak, veri okuma veya Azure Cosmos Veritabanına yazılan her zaman ağ geçidi modu ek ağ atlama içermesidir. Bu nedenle, DirectHttps modu daha az ağ atlamalarını nedeniyle daha iyi performans sunar. 

    Java SDK'sı Aktarım Protokolü HTTPS kullanır. HTTPS, ilk kimlik doğrulaması ve trafik şifreleme için SSL kullanır. Java SDK'yı kullanarak, yalnızca HTTPS 443 numaralı bağlantı noktası açık olması gerekir. 

    ConnectionMode ConnectionPolicy parametresi DocumentClient örneğiyle yapımı sırasında yapılandırılır. 

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

    ![Azure Cosmos DB bağlantı İlkesi çizimi](./media/performance-tips-java/connection-policy.png)

   <a id="same-region"></a>
2. **Performans için aynı Azure bölgesinde istemciler birlikte bulundurma**

    Mümkün olduğunda, Azure Cosmos DB veritabanı ile aynı bölgede Azure Cosmos DB çağırma herhangi bir uygulama yerleştirin. Yaklaşık bir karşılaştırma için 1-2 ms içinde aynı bölge içinde Azure Cosmos DB'de çağrıları tamamlamak ancak Batı ABD, Doğu Yakası arasındaki gecikme süresi > 50 ms. Bu gecikme, Azure veri merkezi sınır istemciden geçerken istek tarafından izlenen yolu bağlı olarak istemek için büyük olasılıkla farklılık gösterebilir. Olası en düşük gecikme, çağıran uygulama sağlanan Azure Cosmos DB uç nokta olduğu Azure bölgesinin içinde bulunduğu sağlanarak elde edilir. Kullanılabilir bölgelerin bir listesi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

    ![Azure Cosmos DB bağlantı İlkesi çizimi](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>SDK kullanımı
1. **En son SDK'sını yükleyin**

    Azure Cosmos DB SDK'ları en iyi performansı sağlamak için sürekli geliştirilen. Bkz: [Azure Cosmos DB SDK](documentdb-sdk-java.md) en son SDK belirlemek ve geliştirmeleri gözden geçirmek için sayfaları.
2. **Bir singleton Azure Cosmos DB istemci uygulamanızın ömrü boyunca kullanın**

    Her [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) örnek iş parçacığı açısından güvenli ve verimli bağlantı yönetimi ve doğrudan modunda çalışırken adresi önbelleğe alma gerçekleştirir. Verimli bağlantı yönetimi ve daha iyi performans DocumentClient ile izin vermek için uygulama yaşam süresi için DocumentClient AppDomain başına tek bir örneğini kullanmak için önerilir.

   <a id="max-connection"></a>
3. **Ağ geçidi modu kullanırken ana bilgisayar başına MaxPoolSize artırın**

    Azure Cosmos DB istekleri HTTPS/REST ağ geçidi modu kullanırken yapılır ve olan varsayılan bağlantı sınırını ana bilgisayar adı veya IP adresi başına tabi. Böylece istemci kitaplığı Azure Cosmos DB birden çok eşzamanlı bağlantıların kullanabilir MaxPoolSize daha yüksek bir değer (200-1000) ayarlamanız gerekebilir. İçin varsayılan değer Java SDK [ConnectionPolicy.getMaxPoolSize](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.gsetmaxpoolsize) 100'dür. Kullanım [setMaxPoolSize]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.setmaxpoolsize) değerini değiştirmek için.

4. **Paralel sorgular bölümlenmiş koleksiyonlar için ayarlama**

    Azure Cosmos DB SQL Java SDK sürüm 1.9.0 ve paralel bölümlendirilmiş bir koleksiyon sorgulamak etkinleştirme desteği paralel sorgular yukarıda (bkz [SDK'ları ile çalışma](sql-api-partition-data.md#working-with-the-azure-cosmos-db-sdks) ve ilgili [kod örnekleri](https://github.com/Azure/azure-documentdb-java/tree/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples) için Daha fazla bilgi). Paralel sorgular seri bunların karşılık gelen sorgu gecikme süresi ve üretilen işi artırmak için tasarlanmıştır.

    (a) ***setMaxDegreeOfParallelism ayarlama\:***  paralel iş birden çok bölümü paralel sorgulayarak sorgular. Ancak, tek tek bölümlendirilmiş bir koleksiyon verileri seri olarak sorgu göre getirilir. Bu nedenle, kullanmak [setMaxDegreeOfParallelism](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setmaxdegreeofparallelism) en kullanıcı sorgu elde maksimum sıkıştırılabilmesi bölüm sayısı ayarlamak için sağlanan diğer tüm sistem koşulları aynı kalır. Bölüm sayısı bilmiyorsanız, yüksek bir sayı ayarlamak için setMaxDegreeOfParallelism kullanabilirsiniz ve maksimum paralellik derecesi (bölüm, kullanıcı tarafından sağlanan girdi sayısı) minimum sistem seçer. 

    Verileri sorgu göre tüm bölümleri arasında eşit olarak dağıtılır, paralel sorgular en iyi avantajları oluşturduğunun dikkate almak önemlidir. Bölümlenmiş koleksiyonu (en kötü durumda bir bölüm) birkaç bölümlerdeki tamamı veya bir sorgu tarafından döndürülen verilerin çoğunu bir yoğunlaşmıştır, ardından sorgu performansını tarafından bu bölümler nedeniyle düşük performansa şekilde bölümlenmiş durumunda.

    (b) ***setMaxBufferedItemCount ayarlama\:***  paralel sorgu sonuçlarının geçerli toplu işlem istemci tarafından gerçekleştirilirken sonuçları önceden getirmek için tasarlanmıştır. Önceden getirme sorgu genel gecikme gelişme yardımcı olur. setMaxBufferedItemCount önceden getirilen sonuç sayısını sınırlar. Ayarlayarak [setMaxBufferedItemCount](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setmaxbuffereditemcount) beklenen sayıda sonuç döndürdü (veya daha yüksek bir sayı), bu önceden getirme maksimum avantajı almak sorgu sağlar.

    Önceden getirme MaxDegreeOfParallelism bağımsız olarak aynı şekilde çalışır ve tüm bölümleri verileri için tek bir arabellek yok.  

5. **Geri Çekilme getRetryAfterInMilliseconds aralıklarla uygulama**

    Performans testi sırasında istekleri küçük oranını kısıtlanan kadar yük artırmanız gerekir. Kısıtlanan, istemci uygulamasına geri Çekilme kısıtlama üzerinde sunucu belirtilen yeniden deneme aralığını gerekir. Geri Çekilme uyarak denemeler arasındaki bekleme süresi en az miktarda harcamanızı sağlar. Yeniden deneme ilkesi desteği eklenmiştir sürüm 1.8.0 ve üzeri, [Java SDK'sı](documentdb-sdk-java.md). Daha fazla bilgi için bkz: [Exceeding ayrılmış işleme sınırları](request-units.md#RequestRateTooLarge) ve [getRetryAfterInMilliseconds](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client_exception.getretryafterinmilliseconds).
6. **İstemci-iş yükünü ölçeklendirme**

    Yüksek işleme düzeyleri sınıyorsanız (> 50.000 RU/s), istemci uygulaması makine çıkışı CPU veya ağ kullanımını capping nedeniyle ayak bağı. Bu noktaya ulaştıysanız, daha fazla Azure Cosmos DB hesabı birden çok sunucu arasında istemci uygulamalarının ölçeklendirme tarafından anında devam edebilirsiniz.

7. **Alan adı adresleme kullanın**

    Ad tabanlı adresleme, bağlantılar biçimi sahip olduğu kullanan `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, SelfLinks (_self) yerine sahip olduğunuz biçimi `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` ResourceId bağlantı oluşturmak için kullanılan tüm kaynakları alma önlemek için. Bu kaynakları (büyük olasılıkla aynı ada sahip) yeniden gibi Ayrıca, bu önbelleğe alma yardımcı.

   <a id="tune-page-size"></a>
8. **Sorguları/okuma akışları daha iyi performans için sayfa boyutunu ayarlama**

    Ne zaman bir toplu gerçekleştirme belgelerin okuma kullanarak okunur işlevselliği akış (örneğin, [readDocuments]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readdocuments#com_microsoft_azure_documentdb__document_client_readDocuments_String_FeedOptions_c) veya sonuç kümesi çok büyük ise bir SQL sorgusu verilirken sonuçlar bölümlenmiş bir şekilde döndürülür. Varsayılan olarak, ilk isabet sınırlarından hangisi olduğunu, sonuçları 100 öğeleri ya da 1 MB yığınlar halinde döndürdü.

    Sayısını azaltmak için ağ gidiş dönüşleri uygulanabilir tüm sonuçları almak için gereken, sayfa boyutu kullanarak artırabilirsiniz [x-ms-max-öğesi-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) en fazla 1000 istek üstbilgisi. Burada yalnızca birkaç sonuçları görüntülemek için gereken durumlarda Örneğin, kullanıcı arabirimi veya uygulama API yalnızca 10 döndürürse birer sonuçları, aynı zamanda okuma ve sorgular için tüketilen verimlilik azaltmak için 10 sayfa boyutunu azaltabilirsiniz.

    Sayfa boyutu kullanarak da yerleştirebilir [setPageSize yöntemi](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options_base.setpagesize#com_microsoft_azure_documentdb__feed_options_base_setPageSize_Integer).

## <a name="indexing-policy"></a>Dizin Oluşturma İlkesi
 
1. **Daha hızlı yazmalar için dizin oluşturma kullanılmayan yolu dışla**

    Azure Cosmos veritabanı dizin oluşturma ilkesi dahil etmek veya hariç dizin yolları yararlanarak dizin hangi belge yolları belirtmenize olanak verir ([setIncludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy.setincludedpaths) ve [setExcludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy.setexcludedpaths)). Dizin oluşturma maliyetleri doğrudan dizine benzersiz yolları sayısı bağıntılı gibi yolları dizin kullanımını geliştirilmiş yazma performansı ve sorgu desenlerine önceden bilinir senaryoları için alt dizini depolama sunabilir.  Örneğin, aşağıdaki kod belgeleri bölümünün tamamını (paketini dışlama gösterir bir alt ağacı) dizin kullanarak "*" joker karakter.

    ```Java
    Index numberIndex = Index.Range(DataType.Number);
    numberIndex.set("precision", -1);
    indexes.add(numberIndex);
    includedPath.setIndexes(indexes);
    includedPaths.add(includedPath);
    indexingPolicy.setIncludedPaths(includedPaths);
    collectionDefinition.setIndexingPolicy(indexingPolicy);
    ```

    Daha fazla bilgi için bkz: [Azure Cosmos ilkeleri dizin DB](indexing-policies.md).

## <a name="throughput"></a>Aktarım hızı
<a id="measure-rus"></a>

1. **Ölçmek ve alt istek için saniye başına birim kullanım ayarlayın.**

    Azure Cosmos DB zengin bir ilişkisel ve hiyerarşik sorgularıyla UDF'ler, saklı yordamları ve Tetikleyicileri – tüm işletim veritabanı koleksiyon içindeki belgelerde dahil olmak üzere veritabanı işlemleri sunar. Bu işlemlerin her biri ile ilişkili maliyet CPU, g/ç ve işlemi tamamlamak için gereken bellek göre değişir. Göz önünde bulundurulması ve donanım kaynaklarını yönetmek yerine, bir uygulama isteği hizmet ve çeşitli veritabanı işlemlerini gerçekleştirmek için gereken kaynakları için tek bir ölçü olarak bir istek birimi (RU) düşünebilirsiniz.

    Üretilen iş sayısına göre hazırlanır [istek birimleri](request-units.md) her kapsayıcıya ayarlayın. İstek birimi tüketim saniye başına oranı olarak değerlendirilir. Hızı kapsayıcısı için sağlanan düzeyinin altına düşene kadar kendi kapsayıcısı için sağlanan istek birimi hızı aşan uygulamaları sınırlıdır. Uygulamanız daha yüksek düzeyde üretilen iş gerektiriyorsa, ek istek birimleri sağlama tarafından üretilen işi artırabilir. 

    Bir sorgu karmaşıklığını kaç tane istek birimi için bir işlem tüketilen etkiler. Koşulları sayısı, koşulları, UDF'ler sayısı ve tüm kaynak veri kümesi boyutunu yapısını sorgu işlemlerinin maliyetini etkiler.

    Herhangi bir işlem yükü ölçmek için (oluşturma, güncelleştirme veya silme) incelemek [x-ms-istek-ücret](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üstbilgi (veya eşdeğer RequestCharge özelliği [ResourceResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._resource_response) veya [FeedResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_response) bu işlemler tarafından tüketilen isteği birim sayısını ölçmek için.

    ```Java
    ResourceResponse<Document> response = client.createDocument(collectionLink, documentDefinition, null, false);

    response.getRequestCharge();
    ```             

    Bu üstbilgisinde döndürülen istek ücret bir sağlanan işleme bölümüdür. Örneğin, 2000 varsa RU/s sağlanan ve önceki sorgunun 1000 1 KB-belgeleri döndürürse, işlem maliyetini 1000'dir. Bu nedenle, bir saniye içinde sonraki istekler azaltma önce sunucunun yalnızca iki tür isteklere korur. Daha fazla bilgi için bkz: [istek birimleri](request-units.md) ve [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Tanıtıcı oranı sınırlama istek oranı çok büyük**

    Bir istemci bir hesap için ayrılmış işleme aşan girişiminde bulunduğunda, sunucuda bir performans düşüşü olmadan ve işleme kapasitesi ayrılmış düzeyinin ötesine hiçbir kullanımını yoktur. Sunucu erken önlem RequestRateTooLarge (HTTP durum kodu 429) istekle sonlandırmak ve dönmek [x-ms-yeniden deneme-sonra-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) kullanıcı reattempting önce beklemesi gereken milisaniye cinsinden süreyi belirten üstbilgisi İstek.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK'ları tüm örtük olarak bu yanıt catch, yeniden deneme sonra sunucu tarafından belirtilen üstbilgi saygı ve isteği yeniden deneyin. Hesabınızı eşzamanlı olarak birden çok istemci tarafından erişilen sürece, sonraki yeniden deneme işlemi başarılı olur.

    Birden fazla istemci üst üste istek hızı tutarlı bir şekilde işletim varsa, 9 istemci tarafından dahili olarak ayarlanmış varsayılan yeniden deneme sayısı yeterli değil; Bu durumda, istemci oluşturur bir [DocumentClientException](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client_exception) uygulamaya 429 ile durum kodu. Varsayılan yeniden deneme sayısı kullanarak değiştirilebilir [setRetryOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.setretryoptions) üzerinde [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy) örneği. İstek istek hızı çalışmaya devam ederse, varsayılan olarak, 30 saniye sonra bir toplam bekleme süresi 429 durum koduyla DocumentClientException döndürülür. Bu geçerli yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda bile oluşur, varsayılan 9 veya kullanıcı tanımlı bir değer olmalıdır.

    Dayanıklılık ve kullanılabilirlik çoğu uygulamalar geliştirmek için otomatik yeniden deneme davranışı yardımcı olsa da, bu performans kıyaslamaları, özellikle gecikme süresini ölçme zaman yaparken at odds gelebilir.  Denemeyi sunucu kısıtlama isabetler ve sessiz bir şekilde yeniden denemek için SDK istemci neden olursa istemci gözlenen gecikme çıkmasına. Performans denemeleri sırasında gecikme ani önlemek için her işlem tarafından döndürülen farkı ölçmek ve istekleri ayrılmış istek hızı çalıştığından emin olun. Daha fazla bilgi için bkz: [istek birimleri](request-units.md).
3. **Daha fazla üretilen işi için daha küçük belgeler için Tasarım**

    Belirli bir işlemi isteği giderleri (istek işleme maliyet) doğrudan belgenin boyutunu ilişkilendirilir. Büyük belgeleri işlemleri birden çok küçük belgeler için işlemleri maliyeti.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanız ölçeği ve yüksek performans için tasarlama hakkında daha fazla bilgi için bkz: [bölümleme ve Azure Cosmos DB'de ölçeklendirme](partition-data.md).
