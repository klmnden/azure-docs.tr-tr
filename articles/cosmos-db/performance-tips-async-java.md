---
title: Async Java için Azure Cosmos DB performans ipuçları
description: Azure Cosmos DB veritabanı performansını artırmak üzere istemci yapılandırma seçenekleri öğrenin
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: sngun
ms.openlocfilehash: 07da7f8905d7b8952db852d3da1dab12884de509
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60932927"
---
# <a name="performance-tips-for-azure-cosmos-db-and-async-java"></a>Azure Cosmos DB ile Async Java için performans ipuçları

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB garantili gecikme süresi ve aktarım hızı ile sorunsuz bir şekilde ölçeklenen bir hızlı ve esnek dağıtılmış bir veritabanıdır. Azure Cosmos DB ile veritabanınızı ölçeklendirmek için karmaşık kodlar yazmanıza ya da büyük mimari değişiklikler yapmak gerekmez. Yukarı ve aşağı ölçeklendirme olarak tek bir API çağrısı veya SDK yöntem çağrısı yapmak oldukça kolaydır. Ancak, Azure Cosmos DB ağ çağrıları erişilir vardır kullanırken en yüksek performans elde etmek için olun istemci-tarafı iyileştirmeleri [SQL Async Java SDK'sı](sql-api-sdk-async-java.md).

Açmanızı isteyen, "nasıl veritabanı performansımı geliştirebilirim şekilde?" Aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ
   <a id="same-region"></a>
1. **İstemci performansı için aynı Azure bölgesinde ISS'de**

    Mümkün olduğunda, Azure Cosmos DB veritabanı ile aynı bölgede Azure Cosmos DB çağırma herhangi bir uygulama yerleştirin. Yaklaşık bir karşılaştırması için aynı bölgedeki Azure Cosmos DB için çağrılar 1-2 ms içinde tamamlayın, ancak ABD'nin Doğu Yakası ve Batı arasındaki gecikme süresi > 50 ms. Bu gecikme süresi, Azure veri merkezi sınırına istemciden geçerken istek tarafından gerçekleştirilen rota bağlı olarak istemek için büyük olasılıkla farklılık gösterebilir. En düşük gecikmeyi çağıran uygulama için aynı Azure bölgesindeki sağlanan Azure Cosmos DB uç nokta olarak bulunduğu sağlayarak gerçekleştirilir. Kullanılabilir bölgelerin listesi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

    ![Azure Cosmos DB bağlantı İlkesi gösterimi](./media/performance-tips/same-region.png)

## <a name="sdk-usage"></a>SDK kullanımı
1. **En son SDK'sını yükleyin**

    Azure Cosmos DB SDK'ları en iyi performansı sağlamak için sürekli geliştirilen. Bkz: [Azure Cosmos DB SDK'sı](sql-api-sdk-async-java.md) sayfalarının en son SDK'sı belirlemek ve geliştirmeleri gözden geçirin.
2. **Tek bir Azure Cosmos DB istemci uygulama ömrü boyunca kullanın**

    Her AsyncDocumentClient örnek iş parçacığı açısından güvenli ve verimli bağlantı yönetimi ve adresi önbelleğe alma gerçekleştirir. Etkin bağlantı yönetimi ve daha iyi performans AsyncDocumentClient tarafından izin vermek için bir uygulama ömrü boyunca AsyncDocumentClient uygulama etki alanı başına tek bir örneğini kullanmak için önerilir.

   <a id="max-connection"></a>

3. **ConnectionPolicy ayarlama**

    Azure Cosmos DB, zaman uyumsuz Java SDK'sı kullanırken HTTPS/REST yapılır ve olan istekleri için varsayılan en fazla bağlantı havuzu boyutu tabi (1000). Bu varsayılan değer, çoğu kullanım için ideal olmalıdır. Çok sayıda bölümleri olan büyük bir koleksiyon olması durumunda, ancak en fazla bağlantı havuzu boyutu (örneğin, 1500) daha büyük bir sayıya ayarlayabilirsiniz setMaxPoolSize kullanma.

4. **Bölümlenmiş koleksiyonlar için paralel sorgular ayarlama**

    Azure Cosmos DB SQL Async Java SDK bölünmüş bir koleksiyona paralel sorgu sağlayan paralel sorguları destekler. Daha fazla bilgi için [kod örnekleri](https://github.com/Azure/azure-cosmosdb-java/tree/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples) SDK'ları ile çalışmayla ilgili. Paralel sorgular seri kendi karşılığı sorgunun gecikme süresi ve aktarım hızı artırmak için tasarlanmıştır.

    (a) ***setMaxDegreeOfParallelism ayarlama\:***  paralel iş birden çok bölümü paralel sorgulayarak sorgular. Ancak, verileri ayrı bölünmüş bir koleksiyona göre sorgu seri olarak getirilir. Bu nedenle, diğer tüm sistem koşulları sağlanan en yüksek performanslı sorgu başarmaya maksimum olasılığını olan bölüm sayısı ayarlanacak kullanım setMaxDegreeOfParallelism aynı kalır. Bölüm sayısı bilmiyorsanız, setMaxDegreeOfParallelism yüksek bir sayı ayarlamak için kullanabileceğiniz ve sistem (bölüm, kullanıcı tarafından sağlanan giriş sayısı) en düşük maksimum paralellik derecesi seçer.

    Sorgu ile ilgili tüm bölümler arasında verileri eşit olarak dağıtılmış, paralel sorgular en iyi avantajları oluşturduğunun dikkat edin önemlidir. Bölünmüş bir koleksiyona birkaç bölümlerde (bir bölüm en kötü durumda) tüm veya bir sorgu tarafından döndürülen verilerin çoğunu yoğunlaşmıştır ve ardından sorgu performansını bu bölümler tarafından performansı düşürdüğünü gösterir şekilde bölümlenmiş durumunda.

    (b) ***setMaxBufferedItemCount ayarlama\:***  paralel sorgu sonuçları istemci tarafından sonuçlarının geçerli toplu iş işlenirken önceden getirme için tasarlanmıştır. Önceden getirme, bir sorgunun toplam gecikme süresi gelişme yardımcı olur. setMaxBufferedItemCount önceden getirilen sonuç sayısını sınırlar. Döndürülen sonuçlar beklenen sayıda setMaxBufferedItemCount (veya daha yüksek bir sayı) ayarlamak, önceden getirme en büyük avantajı almak sorgu sağlar.

    Önceden getirme Maxanalyticsunits bağımsız olarak aynı şekilde çalışır ve tüm bölümleri veriler için tek bir arabellek yoktur.

5. **Geri alma getRetryAfterInMilliseconds aralıklarla uygulayın**

    Performans testi sırasında istekleri küçük bir oranını kısıtlanan kadar yük yükseltmeniz gerekir. Kısıtlanmış, istemci uygulamasına geri alma için sunucu tarafından belirtilen yeniden deneme aralığı gerekir. Geri alma uyarak bekleme süresi yeniden denemeler arasındaki en az miktarda harcama sağlar.
6. **İstemci iş yükü ölçeklendirin**

    Yüksek performans düzeylerinde sınıyorsanız (> 50.000 RU/sn), istemci uygulama makine kullanıma CPU veya ağ kullanımı capping nedeniyle performans sorunu hale gelebilir. Bu noktaya ulaşın, istemci uygulamalarınızı birden çok sunucu arasında ölçeklendirme gerçekleştirerek Azure Cosmos DB hesabı daha fazla göndermeye devam edebilirsiniz.

7. **Ad tabanlı adresleme kullanın**

    Ad tabanlı adresleme, bağlantıları biçimi olduğu kullanın `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, SelfLinks yerine (\_kendi), biçime sahip `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` Resourceıds bağlantı oluşturmak için kullanılan tüm kaynakları alınıyor önlemek için. (Büyük olasılıkla aynı ada sahip) bu kaynakları yeniden gibi Ayrıca bunları önbelleğe alma yardımcı.

   <a id="tune-page-size"></a>
8. **Daha iyi performans için sorgu/okuma akışlarına yönelik sayfa boyutunu ayarlama**

    Bir toplu gerçekleştirme belgeleri (örneğin, readDocuments) akışı okuma kullanarak veya bir SQL sorgusu gönderirken, okuma, sonuç kümesi çok büyükse sonuçları bölümlenmiş bir biçimde döndürülür. Varsayılan olarak, sonuçları 100 öğe 1 MB veya öbekler halinde döndürülür, ilk isabet sınırlarından hangisi.

    Sayısını azaltmak için ağ gidiş dönüşleri yürürlükteki tüm sonuçları almak için gereken, sayfa boyutu kullanarak artırabilirsiniz [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) en fazla 1000 istek üstbilgisi. Yalnızca birkaç sonuçları görüntülemek için gerek duyduğunuz durumlarda Örneğin, kullanıcı arabirimi veya uygulama API'nizi yalnızca 10 döndürürse birer sonuçları, 10 okuma ve sorgular için kullanılan aktarım hızını azaltmak için sayfa boyutunu da azaltabilirsiniz.

    Sayfa boyutu setMaxItemCount yöntemi kullanarak da ayarlayabilir.

9. **Uygun Zamanlayıcısı'nı (olay döngüsü GÇ Netty iş parçacıkları çalarak kaçının)**

    Async Java SDK'sını kullanan [netty](https://netty.io/) engelleyici olmayan g/ç için. SDK'sı, g/ç işlemleri yürütmek için sabit sayıda GÇ netty olay döngüsü iş parçacıkları (makinenizde var. kadar CPU çekirdeği) kullanır. Observable API'si tarafından döndürülen sonuç paylaşılan GÇ olay döngüsü netty iş parçacıklarından yayar. Bu nedenle paylaşılan GÇ olay döngüsü netty iş parçacıklarının engellemediğinizden önemlidir. CPU kullanımı yoğun iş yapıyor veya GÇ olay döngüsü netty iş parçacığı üzerindeki işlemi engelleyen kilitlenmeye neden veya SDK aktarım hızını önemli ölçüde azaltabilir.

    Örneğin aşağıdaki kodu olay döngüsünü GÇ netty iş parçacığı cpu yoğunluklu bir iş çalıştırır:

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

    Yapmak istiyorsanız, sonuç alındıktan sonra CPU yoğunluklu vb. olay döngüsü GÇ netty iş parçacığı yapmaktan kaçının sonucu üzerinde çalışır. Bunun yerine, işlerinizi çalıştırmak için kendi iş parçacığı sağlamak için kendi Zamanlayıcı sağlayabilir.

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

    İş türüne göre uygun mevcut RxJava Zamanlayıcı iş için kullanmanız gerekir. Burada okuma [ ``Schedulers`` ](http://reactivex.io/RxJava/1.x/javadoc/rx/schedulers/Schedulers.html).

    Daha fazla bilgi için lütfen bakmak [GitHub sayfasına](https://github.com/Azure/azure-cosmosdb-java) Async Java SDK'sı.

10. **Netty'nın günlüğünü devre dışı** Netty kitaplığı günlük geveze ve kapatılması gerekiyor (oturum yapılandırmasında gizleme olmayabilir yeterli) ek CPU maliyetleri önlemek için. Hata ayıklama modunda değilse, kaydetme işlemini tamamen devre dışı bırakma netty'nın günlüğe kaydetme. Log4j tarafından ek CPU maliyetleri kaldırmak için kullanıyorsanız, bunu ``org.apache.log4j.Category.callAppenders()`` netty kod temelinizde yapılan aşağıdaki satırı ekleyin:

    ```java
    org.apache.log4j.Logger.getLogger("io.netty").setLevel(org.apache.log4j.Level.OFF);
    ```

11. **İşletim sistemi açık kaynak sınırı dosyaları** (Red Hat gibi) bazı Linux sistemlerine sahip bir üst sınır Aç sayısına dosyaları ve bu nedenle bağlantı toplam sayısı. Geçerli sınırların görüntülemek için aşağıdaki komutu çalıştırın:

    ```bash
    ulimit -a
    ```

    Açık dosyaları (nofile) sayısı, yapılandırılan bağlantı havuzu boyutu ve işletim sistemi tarafından açık diğer dosyalar için yeterli alana sahip büyük olması gerekir. Daha büyük bir bağlantı havuzu boyutu için izin verecek şekilde değiştirilebilir.

    Limits.conf dosyayı açın:

    ```bash
    vim /etc/security/limits.conf
    ```
    
    Ekle/aşağıdaki satırları Değiştir:

    ```
    * - nofile 100000
    ```

12. **Yerel SSL uygulaması kullanmak için netty** Netty daha iyi performans elde etmek için doğrudan SSL uygulaması yığını için OpenSSL kullanabilirsiniz. Bu mevcut olmadığında yapılandırma netty Java'nın varsayılan SSL uygulaması için geri döner.

    Ubuntu üzerinde:
    ```bash
    sudo apt-get install openssl
    sudo apt-get install libapr1
    ```

    ve project maven bağımlılıklarınızı aşağıdaki bağımlılığı ekleyin:
    ```xml
    <dependency>
      <groupId>io.netty</groupId>
      <artifactId>netty-tcnative</artifactId>
      <version>2.0.7.Final</version>
      <classifier>linux-x86_64</classifier>
    </dependency>
    ```

Diğer platformlar için (Red Hat, Windows, Mac, vb.) başvurmak için bu yönergeleri https://netty.io/wiki/forked-tomcat-native.html

## <a name="indexing-policy"></a>Dizin Oluşturma İlkesi
 
1. **Kullanılmayan yolları daha hızlı yazmalar için dizine elmadan hariç tut**

    Azure Cosmos DB'nin dizin oluşturma ilkesini dahil etmek veya dizin yolları (setIncludedPaths ve setExcludedPaths) yararlanarak dizine elmadan hariç tutmak için hangi belge yolları belirtmenizi sağlar. Dizin oluşturma maliyetleri doğrudan dizini benzersiz yollara sayısını bağıntılı olan gibi dizin yolları kullanımını geliştirilmiş yazma performansını ve sorgu desenleri önceden bilinmektedir senaryoları için daha düşük dizin depolaması sunabilir. Örneğin, aşağıdaki kod belgeleri bölümünün tamamını (diğer adıyla) hariç tutmak nasıl gösterir bir alt ağacı) dizin oluşturma kullanarak "*" joker karakter.

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

    Herhangi bir işlem yükü ölçmek için (oluşturma, güncelleştirme veya silme) İnceleme [x-ms-istek-ücretsiz olarak](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) bu işlemleri tarafından kullanılan istek birimleri sayısını ölçmek için üst bilgi. Eşdeğer ResourceResponse RequestCharge özelliği de bakabilirsiniz<T> veya FeedResponse<T>.

    ```Java
    ResourceResponse<Document> response = asyncClient.createDocument(collectionLink, documentDefinition, null,
                                                     false).toBlocking.single();
    response.getRequestCharge();
    ```

    Bu üst bilgisinde döndürülen istek hızınız sağladığınız aktarım avantajlarının ücrettir. Örneğin, 2000 varsa sağlanan RU/s ve işlem maliyeti önceki sorgunun 1000 1 KB-belgeler döndürürse, 1000'dir. Bu nedenle, bir saniye içinde sonraki istekleri hız sınırı önce yalnızca iki tür isteklere sunucunun geliştirir. Daha fazla bilgi için [istek birimi](request-units.md) ve [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Tanıtıcı oran sınırlama/istek oranı çok büyük**

    Bir istemci bir hesap için ayrılmış aktarım hızını aşmayı dener, sunucuda performans düşüşü olmadan ve aktarım hızı kapasitesine ayrılmış düzeyi dışında hiçbir kullanımı yoktur. Sunucu sıd'lerde istek RequestRateTooLarge (HTTP durum kodu 429) ile bitemez ve dönüş [x-ms-yeniden-sonra-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) kullanıcı çakıştığını önce beklemesi gereken milisaniye cinsinden süre miktarını belirten üstbilgisi İstek.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK'ları tüm örtük olarak bu yanıt catch, sunucu tarafından belirtilen retry-after üst bilgisi saygı ve isteği yeniden deneyin. Hesabınızda aynı anda birden çok istemci tarafından erişilen sürece sonraki yeniden deneme işlemi başarılı olur.

    Üst üste istek hızı tutarlı bir şekilde çalışan birden fazla istemciniz varsa, 9 istemci tarafından dahili olarak ayarlanmış varsayılan yeniden deneme sayısı yeterli değil; Bu durumda, istemci uygulamanın 429 durum koduyla bir DocumentClientException oluşturur. Varsayılan yeniden deneme sayısı ConnectionPolicy örneğinde setRetryOptions kullanılarak değiştirilebilir. İstek, istek hızı üzerinde çalışmaya devam ederse varsayılan olarak, durum kodu 429 DocumentClientException 30 saniye sonra bir toplam bekleme süresi döndürülür. Bu geçerli bir yeniden deneme sayısı en fazla yeniden deneme sayısından daha az olduğunda bile oluşur, varsayılan 9 veya kullanıcı tanımlı bir değer olmalıdır.

    Dayanıklılık ve kullanılabilirlik uygulamalarının çoğu için iyileştirmek için otomatik yeniden deneme davranışı yardımcı olsa da, bu performans kıyaslamaları, özellikle gecikme süresini ölçme olduğunda yaparken at odds gelebilir. Denemeyi sunucu kısıtlama denk gelir ve istemci SDK'sı sessiz bir şekilde yeniden denemek için neden olan istemci gözlemlenen gecikme çıkmasına. Ani gecikme süresi artışlarına sırasında performans denemelerini önlemek için her bir işlem tarafından döndürülen ücret ölçün ve istekleri ayrılmış istek hızı altında çalıştığından emin olun. Daha fazla bilgi için [istek birimi](request-units.md).
3. **Daha yüksek verimliğe yönelik daha küçük belgeleri için Tasarım**

    Belirli bir işlemi istek ücreti (istek işleme maliyeti) doğrudan belge boyutunu ilişkilendirilir. Büyük belgelerin işlemleri daha küçük belgeleri işlemlerinde daha fazla maliyet.

## <a name="next-steps"></a>Sonraki adımlar
Uygulama ölçeklendirme ve yüksek performans için tasarlama hakkında daha fazla bilgi için bkz: [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).
