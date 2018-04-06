---
title: Zaman uyumsuz Java için Azure Cosmos DB performans ipuçları | Microsoft Docs
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
ms.date: 03/27/2018
ms.author: mimig
ms.openlocfilehash: 88d9859ade8f2cd3c5f11dffa8e754e83fc8b4d4
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 
> 

# <a name="performance-tips-for-azure-cosmos-db-and-async-java"></a>Azure Cosmos DB ve zaman uyumsuz Java için performans ipuçları
Azure Cosmos DB garantili gecikme süresi ve verimlilik ile sorunsuz bir şekilde ölçeklendirilebilen bir hızlı ve esnek dağıtılmış veritabanıdır. Büyük mimari değişiklikler yaptığınızda veya Azure Cosmos DB veritabanınızla ölçeklendirmek için karmaşık kodlar yazmak zorunda değildir. Yukarı ve aşağı doğru ölçeklendirme, bir tek API çağrısı veya SDK yöntem çağrısı yaparak olarak kadar kolaydır. Ancak, Azure Cosmos DB ağ çağrıları erişilir vardır kullanırken en yüksek performans elde etmek için yapabilir istemci-tarafı iyileştirmeler [SQL zaman uyumsuz Java SDK'sı](sql-api-sdk-async-java.md).

Soran, "nasıl ı my veritabanı performansını geliştirebilir şekilde?" Aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ
   <a id="same-region"></a>
1. **Performans için aynı Azure bölgesinde istemciler birlikte bulundurma**

    Mümkün olduğunda, Azure Cosmos DB veritabanı ile aynı bölgede Azure Cosmos DB çağırma herhangi bir uygulama yerleştirin. Yaklaşık bir karşılaştırma için 1-2 ms içinde aynı bölge içinde Azure Cosmos DB'de çağrıları tamamlamak ancak Batı ABD, Doğu Yakası arasındaki gecikme süresi > 50 ms. Bu gecikme, Azure veri merkezi sınır istemciden geçerken istek tarafından izlenen yolu bağlı olarak istemek için büyük olasılıkla farklılık gösterebilir. Olası en düşük gecikme, çağıran uygulama sağlanan Azure Cosmos DB uç nokta olduğu Azure bölgesinin içinde bulunduğu sağlanarak elde edilir. Kullanılabilir bölgelerin bir listesi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

    ![Azure Cosmos DB bağlantı İlkesi çizimi](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>SDK kullanımı
1. **En son SDK'sını yükleyin**

    Azure Cosmos DB SDK'ları en iyi performansı sağlamak için sürekli geliştirilen. Bkz: [Azure Cosmos DB SDK](sql-api-sdk-async-java.md) en son SDK belirlemek ve geliştirmeleri gözden geçirmek için sayfaları.
2. **Bir singleton Azure Cosmos DB istemci uygulamanızın ömrü boyunca kullanın**

    Her AsyncDocumentClient örnek iş parçacığı açısından güvenli ve verimli bağlantı yönetimi ve adresi önbelleğe alma gerçekleştirir. Verimli bağlantı yönetimi ve daha iyi performans AsyncDocumentClient tarafından izin vermek için uygulama ömrü boyunca AsyncDocumentClient AppDomain başına tek bir örneğini kullanmak için önerilir.

   <a id="max-connection"></a>

3. **ConnectionPolicy ayarlama**

    Azure Cosmos DB isteklerini zaman uyumsuz Java SDK'yı kullanarak, HTTPS/REST yapılan ve olduğu için varsayılan en fazla bağlantı havuzu boyutu tabi (1000). Bu varsayılan değeri, çoğu kullanım durumu için ideal olmalıdır. Birçok bölüm ile çok büyük bir koleksiyon olması durumunda, ancak, en fazla bağlantı havuzu boyutu daha büyük bir sayı (say, 1500) ayarlayabileceğiniz setMaxPoolSize kullanarak.

4. **Paralel sorgular bölümlenmiş koleksiyonlar için ayarlama**

    Azure Cosmos DB SQL zaman uyumsuz Java SDK'yı destekleyen paralel bölümlendirilmiş bir koleksiyon sorgulamak etkinleştirmeniz paralel sorgular (bkz [SDK'ları ile çalışma](sql-api-partition-data.md#working-with-the-azure-cosmos-db-sdks) ve ilgili [kod örnekleri](https://github.com/Azure/azure-cosmosdb-java/tree/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples) daha fazla bilgi için). Paralel sorgular seri bunların karşılık gelen sorgu gecikme süresi ve üretilen işi artırmak için tasarlanmıştır.

    (a) ***setMaxDegreeOfParallelism ayarlama\:***  paralel iş birden çok bölümü paralel sorgulayarak sorgular. Ancak, tek tek bölümlendirilmiş bir koleksiyon verileri seri olarak sorgu göre getirilir. Bu nedenle, diğer tüm sistem koşulları sağlanan çoğu kullanıcı sorgu elde maksimum sıkıştırılabilmesi bölüm sayısı ayarlamak için kullanım setMaxDegreeOfParallelism aynı kalır. Bölüm sayısı bilmiyorsanız, yüksek bir sayı ayarlamak için setMaxDegreeOfParallelism kullanabilirsiniz ve maksimum paralellik derecesi (bölüm, kullanıcı tarafından sağlanan girdi sayısı) minimum sistem seçer. 

    Verileri sorgu göre tüm bölümleri arasında eşit olarak dağıtılır, paralel sorgular en iyi avantajları oluşturduğunun dikkate almak önemlidir. Bölümlenmiş koleksiyonu (en kötü durumda bir bölüm) birkaç bölümlerdeki tamamı veya bir sorgu tarafından döndürülen verilerin çoğunu bir yoğunlaşmıştır, ardından sorgu performansını tarafından bu bölümler nedeniyle düşük performansa şekilde bölümlenmiş durumunda.

    (b) ***setMaxBufferedItemCount ayarlama\:***  paralel sorgu sonuçlarının geçerli toplu işlem istemci tarafından gerçekleştirilirken sonuçları önceden getirmek için tasarlanmıştır. Önceden getirme sorgu genel gecikme gelişme yardımcı olur. setMaxBufferedItemCount önceden getirilen sonuç sayısını sınırlar. Döndürülen sonuçları beklenen sayıda setMaxBufferedItemCount (veya daha yüksek bir sayı) ayarı, önceden getirme maksimum avantajı almak sorgu sağlar.

    Önceden getirme MaxDegreeOfParallelism bağımsız olarak aynı şekilde çalışır ve tüm bölümleri verileri için tek bir arabellek yok.  

5. **Geri Çekilme getRetryAfterInMilliseconds aralıklarla uygulama**

    Performans testi sırasında istekleri küçük oranını kısıtlanan kadar yük artırmanız gerekir. Kısıtlanan, istemci uygulamasına geri Çekilme sunucu belirtilen yeniden deneme aralığını gerekir. Geri Çekilme uyarak denemeler arasındaki bekleme süresi en az miktarda harcamanızı sağlar. Daha fazla bilgi için bkz: [Exceeding ayrılmış işleme sınırları](request-units.md#RequestRateTooLarge) ve DocumentClientException.getRetryAfterInMilliseconds.
6. **İstemci-iş yükünü ölçeklendirme**

    Yüksek işleme düzeyleri sınıyorsanız (> 50.000 RU/s), istemci uygulaması makine çıkışı CPU veya ağ kullanımını capping nedeniyle ayak bağı. Bu noktaya ulaştıysanız, daha fazla Azure Cosmos DB hesabı birden çok sunucu arasında istemci uygulamalarının ölçeklendirme tarafından anında devam edebilirsiniz.

7. **Alan adı adresleme kullanın**

    Ad tabanlı adresleme, bağlantılar biçimi sahip olduğu kullanan `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, SelfLinks yerine (\_kendini), biçime sahip `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` ResourceId bağlantı oluşturmak için kullanılan tüm kaynakları alma önlemek için. Bu kaynakları (büyük olasılıkla aynı ada sahip) yeniden gibi Ayrıca, bunları önbelleğe alma yardımcı.

   <a id="tune-page-size"></a>
8. **Sorguları/okuma akışları daha iyi performans için sayfa boyutunu ayarlama**

    Bir toplu gerçekleştirme belgelerin okuma akışı işlevleri (örneğin, readDocuments) kullanarak veya bir SQL sorgusu verilirken okurken sonuç kümesi çok büyük ise sonuçları bölümlenmiş bir şekilde döndürülür. Varsayılan olarak, ilk isabet sınırlarından hangisi olduğunu, sonuçları 100 öğeleri ya da 1 MB yığınlar halinde döndürdü.

    Sayısını azaltmak için ağ gidiş dönüşleri uygulanabilir tüm sonuçları almak için gereken, sayfa boyutu kullanarak artırabilirsiniz [x-ms-max-öğesi-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) en fazla 1000 istek üstbilgisi. Burada yalnızca birkaç sonuçları görüntülemek için gereken durumlarda Örneğin, kullanıcı arabirimi veya uygulama API yalnızca 10 döndürürse birer sonuçları, aynı zamanda okuma ve sorgular için tüketilen verimlilik azaltmak için 10 sayfa boyutunu azaltabilirsiniz.

    Sayfa boyutu setMaxItemCount yöntemi kullanarak da ayarlayabilirsiniz.
    
9. **Uygun Zamanlayıcısı'nı (Eventloop GÇ Netty iş parçacığı çalarak kaçının)**

    Zaman uyumsuz Java SDK'sı [netty](https://netty.io/) engelleyici olmayan g/ç için. SDK'sı, g/ç işlemleri yürütmek için sabit sayıda g/ç netty eventloop iş parçacıkları (kadar CPU çekirdekleri makinenizde var.) kullanır. API tarafından döndürülen Observable paylaşılan GÇ eventloop netty iş parçacıklarının birini sonucuna yayar. Bu nedenle paylaşılan GÇ eventloop netty iş parçacığı engelleme değil önemlidir. CPU yoğunluklu iş yapıyor veya GÇ eventloop netty iş parçacığı üzerindeki işlemi engelleyen kilitlenmeye neden veya SDK verimlilik önemli ölçüde azaltabilir.

    Örneğin aşağıdaki kod bir cpu yoğunluklu iş eventloop GÇ netty iş parçacığı üzerinde çalıştırır:

    ```java
    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribe(
      resourceResponse -> {
        //this is executed on eventloop IO netty thread.
        //the eventloop thread is shared and is meant to return back quickly.
        //
        // DON'T do this on eventloop IO netty thread.
        veryCpuIntensiveWork();
      });
    ```

    CPU yoğunluklu iş vb. eventloop GÇ netty iş parçacığı yapılması kaçınmalısınız sonuç üzerinde yapmak istiyorsanız, sonuç alındıktan sonra. Bunun yerine, işinizi çalıştırmak için kendi iş parçacığı sağlamak için kendi Zamanlayıcı sağlayabilir.

    ```java
    import rx.schedulers;

    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribeOn(Schedulers.computation())
    subscribe(
      resourceResponse -> {
        // this is executed on threads provided by Scheduler.computation()
        // Schedulers.computation() should be used only when:
        //   1. The work is cpu intensive 
        //   2. You are not doing blocking IO, thread sleep, etc. in this thread against other resources.
        veryCpuIntensiveWork();
      });
    ```

    İş türüne göre çalışmanız için uygun olan RxJava Zamanlayıcısı'nı kullanmanız gerekir. Burada okuma [ ``Schedulers`` ](http://reactivex.io/RxJava/1.x/javadoc/rx/schedulers/Schedulers.html).

    Daha fazla bilgi için lütfen bakmak [Github sayfası](https://github.com/Azure/azure-cosmosdb-java) zaman uyumsuz Java SDK'sı.

10. **Netty'nın günlüğü devre dışı bırakmak** Netty kitaplığı günlük chatty ve kapatılması gerekiyor (günlük yapılandırmasında gizleme olmayabilir yeterli) ek CPU maliyetleri önlemek için. Hata ayıklama modunda değilse devre dışı bırakma netty'nın günlüğü tamamen. Log4j tarafından ek CPU maliyetleri kaldırmak için kullanıyorsanız, bunu ``org.apache.log4j.Category.callAppenders()`` netty temelinizde için aşağıdaki satırı ekleyin:

    ```java
    org.apache.log4j.Logger.getLogger("io.netty").setLevel(org.apache.log4j.Level.OFF);
    ```

11. **İşletim sistemi açık dosyaları kaynak sınırına** bazı Linux sistemleri (örneğin, Redhat) sahip bir üst sınır açık sayısına dosyaları ve bu nedenle bağlantıları toplam sayısı. Geçerli sınırlarını görüntülemek için aşağıdaki komutu çalıştırın:

    ```bash
    ulimit -a
    ```

    Açık dosyaları (nofile) sayısı yapılandırılan bağlantı havuzu boyutu ve işletim sistemi tarafından açık diğer dosyalar için yeterli alana sahip olması için büyük olması gerekir. Daha büyük bir bağlantı havuzu boyutu için izin verecek şekilde değiştirilebilir.

    Limits.conf dosyayı açın:

    ```bash
    vim /etc/security/limits.conf
    ```
    
    Ekleme/aşağıdaki satırları değiştirin:

    ```
    * - nofile 100000
    ```

12. **Yerel SSL uygulaması için netty kullanmak** Netty OpenSSL SSL uygulaması yığını için doğrudan daha iyi performans elde etmek için kullanabilirsiniz. Bu olmadığında yapılandırma netty kalan geri Java'nın varsayılan SSL uygulaması.

    Ubuntu üzerinde:
    ```bash
    sudo apt-get install openssl
    sudo apt-get install libapr1
    ```

    ve aşağıdaki bağımlılığı proje maven bağımlılıklarınızı ekleyin:
    ```xml
    <dependency>
      <groupId>io.netty</groupId>
      <artifactId>netty-tcnative</artifactId>
      <version>2.0.7.Final</version>
      <classifier>linux-x86_64</classifier>
    </dependency>
    ```

Diğer platformlar için (Redhat, Windows, Mac, vb.), bu yönergelerine başvurun https://netty.io/wiki/forked-tomcat-native.html

## <a name="indexing-policy"></a>Dizin Oluşturma İlkesi
 
1. **Daha hızlı yazmalar için dizin oluşturma kullanılmayan yolu dışla**

    Azure Cosmos veritabanı dizin oluşturma ilkesi dahil etmek veya hariç dizin yolları (setIncludedPaths ve setExcludedPaths) yararlanarak dizin hangi belge yolları belirtmenize olanak tanır. Dizin oluşturma maliyetleri doğrudan dizine benzersiz yolları sayısı bağıntılı gibi yolları dizin kullanımını geliştirilmiş yazma performansı ve sorgu desenlerine önceden bilinir senaryoları için alt dizini depolama sunabilir.  Örneğin, aşağıdaki kod belgeleri bölümünün tamamını (paketini dışlama gösterir bir alt ağacı) dizin kullanarak "*" joker karakter.

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

    Herhangi bir işlem yükü ölçmek için (oluşturma, güncelleştirme veya silme) incelemek [x-ms-istek-ücret](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) bu işlemler tarafından tüketilen isteği birim sayısını ölçmek için üstbilgi. ResourceResponse eşdeğer RequestCharge özelliğinde de bakabilirsiniz<T> veya FeedResponse<T>.

    ```Java
    ResourceResponse<Document> response = asyncClient.createDocument(collectionLink, documentDefinition, null,
                                                     false).toBlocking.single();
    response.getRequestCharge();
    ```             

    Bu üstbilgisinde döndürülen istek ücret bir sağlanan işleme bölümüdür. Örneğin, 2000 varsa RU/s sağlanan ve önceki sorgunun 1000 1 KB-belgeleri döndürürse, işlem maliyetini 1000'dir. Bu nedenle, bir saniye içinde sonraki istekler azaltma önce sunucunun yalnızca iki tür isteklere korur. Daha fazla bilgi için bkz: [istek birimleri](request-units.md) ve [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Tanıtıcı oranı sınırlama istek oranı çok büyük**

    Bir istemci bir hesap için ayrılmış işleme aşan girişiminde bulunduğunda, sunucuda bir performans düşüşü olmadan ve işleme kapasitesi ayrılmış düzeyinin ötesine hiçbir kullanımını yoktur. Sunucu erken önlem RequestRateTooLarge (HTTP durum kodu 429) istekle sonlandırmak ve dönmek [x-ms-yeniden deneme-sonra-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) kullanıcı reattempting önce beklemesi gereken milisaniye cinsinden süreyi belirten üstbilgisi İstek.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK'ları tüm örtük olarak bu yanıt catch, yeniden deneme sonra sunucu tarafından belirtilen üstbilgi saygı ve isteği yeniden deneyin. Hesabınızı eşzamanlı olarak birden çok istemci tarafından erişilen sürece, sonraki yeniden deneme işlemi başarılı olur.

    Birden fazla istemci üst üste istek hızı tutarlı bir şekilde işletim varsa, 9 istemci tarafından dahili olarak ayarlanmış varsayılan yeniden deneme sayısı yeterli değil; Bu durumda, istemci uygulamaya 429 durum koduyla bir DocumentClientException oluşturur. Varsayılan yeniden deneme sayısı ConnectionPolicy örneğinde setRetryOptions kullanılarak değiştirilebilir. İstek istek hızı çalışmaya devam ederse, varsayılan olarak, 30 saniye sonra bir toplam bekleme süresi 429 durum koduyla DocumentClientException döndürülür. Bu geçerli yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda bile oluşur, varsayılan 9 veya kullanıcı tanımlı bir değer olmalıdır.

    Dayanıklılık ve kullanılabilirlik çoğu uygulamalar geliştirmek için otomatik yeniden deneme davranışı yardımcı olsa da, bu performans kıyaslamaları, özellikle gecikme süresini ölçme zaman yaparken at odds gelebilir.  Denemeyi sunucu kısıtlama isabetler ve sessiz bir şekilde yeniden denemek için SDK istemci neden olursa istemci gözlenen gecikme çıkmasına. Performans denemeleri sırasında gecikme ani önlemek için her işlem tarafından döndürülen farkı ölçmek ve istekleri ayrılmış istek hızı çalıştığından emin olun. Daha fazla bilgi için bkz: [istek birimleri](request-units.md).
3. **Daha fazla üretilen işi için daha küçük belgeler için Tasarım**

    Belirli bir işlemi isteği giderleri (istek işleme maliyet) doğrudan belgenin boyutunu ilişkilendirilir. Büyük belgeleri işlemleri birden çok küçük belgeler için işlemleri maliyeti.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanız ölçeği ve yüksek performans için tasarlama hakkında daha fazla bilgi için bkz: [bölümleme ve Azure Cosmos DB'de ölçeklendirme](partition-data.md).
