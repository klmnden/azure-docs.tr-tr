---
title: Tanılama ve sorun giderme Azure Cosmos DB Java zaman uyumsuz SDK'sı
description: İstemci tarafı günlüğe kaydetme ve tanımlamak, tanılamak ve Azure Cosmos DB sorunlarını gidermek için diğer üçüncü taraf araçları gibi özellikleri kullanın.
author: moderakh
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 10/28/2018
ms.author: moderakh
ms.devlang: java
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 0a2bbb33182fcdef3cc6ed7ff213557f90be4544
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60404689"
---
# <a name="troubleshoot-issues-when-you-use-the-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Async Java SDK'sı ile Azure Cosmos DB SQL API hesabı kullandığınızda sorunlarını giderme
Bu makalede kullanırken yaygın sorunlar, geçici çözümler, tanılama adımları ve araçları kapsayan [Async Java SDK'sı](sql-api-sdk-async-java.md) Azure Cosmos DB SQL API hesabı olan.
Async Java SDK'sı, Azure Cosmos DB SQL API'sine erişmek için istemci tarafı mantıksal gösterim sağlar. Bu makalede, araçları ve yaklaşımları herhangi bir sorunla karşılaşırsanız çalıştırırsanız yardımcı olması için açıklar.

Bu liste başlatın:

* Bir göz atın [genel sorunlar ve çözümleri] bu makaledeki bir bölüm.
* Konum kullanılabilir SDK [GitHub üzerinde açık kaynak](https://github.com/Azure/azure-cosmosdb-java). Sahip bir [sorunları bölümüne](https://github.com/Azure/azure-cosmosdb-java/issues) , etkin olarak izlenir. Geçici bir çözüm ile benzer bir sorun önceden dosyalanmış denetleyin.
* Gözden geçirme [performans ipuçları](performance-tips-async-java.md)ve önerilen uygulamaları izleyin.
* Bir çözüm bulamadıysanız, bu makalenin geri kalanında okuyun. Ardından dosya bir [GitHub sorunu](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-workarounds"></a>Genel sorunlar ve çözümleri

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Ağ sorunları, zaman aşımı hatası, düşük aktarım hızı, gecikme süresi yüksek Netty okuyun

#### <a name="general-suggestions"></a>Genel öneriler
* Uygulamayı Azure Cosmos DB hesabınız ile aynı bölgede çalıştığından emin olun. 
* Uygulamanın çalıştığı konak üzerindeki CPU kullanımını denetleyin. CPU kullanımı yüzde 90 veya daha fazla ise, bir konakta daha yüksek bir yapılandırma ile uygulamanızı çalıştırın. Veya daha fazla makine üzerinde yük dağıtabilirsiniz.

#### <a name="connection-throttling"></a>Bağlantı daraltma
Bağlantı daraltma gerçekleşebilir nedeniyle ya da bir [konak makinedeki bağlantı sınırı] veya [Azure SNAT (PAT) bağlantı noktası tükenmesi].

##### <a name="connection-limit-on-host"></a>Konak makinedeki bağlantı sınırı
Red Hat gibi bazı Linux sistemleri, üst sınır açık dosyaların, toplam sayısına sahip. Bu sayı çok bağlantıları, toplam sayısını sınırlar için yuva Linux'ta dosyaları olarak uygulanır.
Aşağıdaki komutu çalıştırın.

```bash
ulimit -a
```
"Nofile" tanımlanır, maksimum izin verilen açık dosyaları sayısı, bağlantı havuzu boyutu en az iki katı olması gerekir. Daha fazla bilgi için [performans ipuçları](performance-tips-async-java.md).

##### <a name="snat"></a>Azure SNAT (PAT) bağlantı noktası tükenmesi

Uygulamanızı Azure sanal makineler üzerinde bir genel IP adresi, varsayılan olarak dağıtılırsa [Azure SNAT bağlantı noktaları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) VM'nizi dışında herhangi bir uç noktaya bağlantı. Sanal makineden Azure Cosmos DB uç noktasına izin verilen bağlantı sayısı tarafından sınırlı [Azure SNAT yapılandırma](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Yalnızca, sanal Makinenizin özel IP adresi vardır ve bir işlem VM'den bir genel IP adresine bağlanmaya Azure SNAT bağlantı noktaları kullanılır. Azure SNAT sınırlama önlemek için iki geçici çözüm vardır:

* Azure Cosmos DB Hizmeti uç noktanızı Azure sanal makineler sanal ağınızın alt ağa ekleyin. Daha fazla bilgi için [Azure sanal ağ hizmet uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    Hizmet uç noktası etkinleştirildiğinde, istekleri artık Azure Cosmos DB'ye bir genel IP ile gönderilir. Bunun yerine, sanal ağ ve alt ağ kimlik gönderilir. Bu değişiklik, genel IP'ler izin verilir, yalnızca güvenlik duvarı bırakmaları neden olabilir. Hizmet uç noktasını etkinleştirdiğinizde, bir güvenlik duvarı kullanıyorsanız, bir alt ağ için Güvenlik Duvarı'nı kullanarak eklemek [sanal ağ ACL'leri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Azure VM için genel bir IP atayın.

#### <a name="http-proxy"></a>HTTP Ara sunucusu

Bir HTTP Proxy'si kullanıyorsanız, SDK'yı yapılandırılmış bağlantı sayısını destekleyen emin `ConnectionPolicy`.
Aksi takdirde, bağlantı sorunlarını yönetmektir.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Geçersiz kodlama düzeni: Netty GÇ iş parçacığı engelleme

SDK'sı kullanır [Netty](https://netty.io/) Azure Cosmos DB ile iletişim kurmak için GÇ Kitaplığı. SDK, zaman uyumsuz API vardır ve engelleyici olmayan, g/ç API'leri Netty kullanır. SDK'ın GÇ iş GÇ Netty iş parçacığı üzerinde gerçekleştirilir. GÇ Netty iş parçacığı sayısı, uygulama makinesinin CPU çekirdek sayısı ile aynı olacak şekilde yapılandırılır. 

Netty GÇ iş parçacığı için kullanılması amaçlanmıştır engelleyici olmayan Netty GÇ iş için yalnızca. SDK'sı API çağırma sonucu uygulamanın kodu Netty GÇ iş parçacığı birini döndürür. SDK'sı Netty iş parçacığında sonuçları aldıktan sonra uygulamanın bir uzun süreli işlem yaparsa, iç GÇ işini gerçekleştirmek için yeterli g/ç iş parçacığı sahip olmayabilir. Bu tür bir uygulama kodlama düşük aktarım hızı gecikme süresi yüksek, neden olabilir ve `io.netty.handler.timeout.ReadTimeoutException` hataları. İşlem Sürüyor bildiğinizde iş parçacığına geçiş yapmak için çözüm olabilir.

Örneğin, aşağıdaki kod parçacığı göz atın. Fazla sayıda milisaniye Netty iş parçacığı üzerinde alan uzun süreli iş gerçekleştirebilir. Bu durumda, sonuçta Netty GÇ iş parçacığı g/ç işleri işlemek için mevcut olduğu bir duruma alabilirsiniz. Sonuç olarak, ReadTimeoutException hata alırsınız.
```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // Time-consuming work is, for example,
                        // writing to a file, computationally heavy work, or just sleep.
                        // Basically, it's anything that takes more than a few milliseconds.
                        // Doing such operations on the IO Netty thread
                        // without a proper scheduler will cause problems.
                        // The subscriber will get a ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //It will be io.netty.handler.timeout.ReadTimeoutException.
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
   Geçici çözüm, zaman alan işleri gerçekleştirmek iş parçacığı değiştirmektir. Uygulamanız için Zamanlayıcı tekil örneğini tanımlar.
   ```java
// Have a singleton instance of an executor and a scheduler.
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   Alır, örneğin, işlem bakımından yoğun iş veya engelleyici bir GÇ saat çalışmak gerekebilir. Bu durumda, iş parçacığı tarafından sağlanan bir çalışan geçin, `customScheduler` kullanarak `.observeOn(customScheduler)` API.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // Switches the thread.
        .subscribe(
            // ...
        );
```
Kullanarak `observeOn(customScheduler)`, Netty GÇ iş parçacığı bırakın ve kendi özel iş parçacığı özel Zamanlayıcı tarafından sağlanan geçin. Bu değişiklik, sorunu çözer. Size vermeyecektir bir `io.netty.handler.timeout.ReadTimeoutException` artık hata.

### <a name="connection-pool-exhausted-issue"></a>Bağlantı havuzu tükenmiş sorunu

`PoolExhaustedException` bir istemci-tarafı hatasıdır. Bu hata, uygulama yükünüz SDK bağlantı havuzu hizmet verebilir daha yüksek olduğunu gösterir. Bağlantı havuzu boyutu artırabilir ya da birden fazla uygulama üzerindeki yük dağıtabilirsiniz.

### <a name="request-rate-too-large"></a>İstek oranı çok büyük
Bu hata, bir sunucu tarafı hatasıdır. Bu, sağlanan aktarım hızınızı harcanan gösterir. Daha sonra yeniden deneyin. Genellikle bu hata alırsanız, koleksiyon işleme hacmini artış göz önünde bulundurun.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Azure Cosmos DB öykünücüsü'nü bağlanma başarısız

Azure Cosmos DB öykünücüsü'nü HTTPS sertifikası otomatik olarak imzalanır. Öykünücü ile çalışmak için SDK, bir Java TrustStore için öykünücü sertifikayı içeri aktarın. Daha fazla bilgi için [dışarı Azure Cosmos DB öykünücüsü'nü sertifikaları](local-emulator-export-ssl-certificates.md).

### <a name="dependency-conflict-issues"></a>Bağımlılık çakışma sorunları

```console
Exception in thread "main" java.lang.NoSuchMethodError: rx.Observable.toSingle()Lrx/Single;
```

Yukarıdaki özel durum RxJava lib (örneğin, 1.2.2) daha eski bir sürümü üzerinde bağımlılığa sahip önerir. SDK'mız API'leri RxJava önceki sürümünde kullanılamaz olduğu RxJava 1.3.8 kullanır. 

Bu tür issuses, bir bağımlılık tanımlamak için geçici çözüm RxJava-1.2.2 getirir ve RxJava 1.2.2 Karşılıklı bağımlılığı hariç tutun ve CosmosDB SDK izin sürüme getirin.

RxJava 1.2.2 proje pom.xml dosyanıza yanında aşağıdaki komutu çalıştırın, hangi kitaplığı getirir tanımlamak için:
```bash
mvn dependency:tree
```
Daha fazla bilgi için [maven bağımlılığı ağacı Kılavuzu](https://maven.apache.org/plugins/maven-dependency-plugin/examples/resolving-conflicts-using-the-dependency-tree.html).

RxJava 1.2.2 tanımladıktan sonra pom dosyası ve dışlama RxJava geçişli bağımlılık, lib hangi diğer bağımlılığı projenizin, üzerinde bağımlılığı değiştirebilirsiniz geçişli bağımlılık şöyledir:

```xml
<dependency>
  <groupId>${groupid-of-lib-which-brings-in-rxjava1.2.2}</groupId>
  <artifactId>${artifactId-of-lib-which-brings-in-rxjava1.2.2}</artifactId>
  <version>${version-of-lib-which-brings-in-rxjava1.2.2}</version>
  <exclusions>
    <exclusion>
      <groupId>io.reactivex</groupId>
      <artifactId>rxjava</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

Daha fazla bilgi için [geçişli bağımlılık Kılavuzu hariç](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html).


## <a name="enable-client-sice-logging"></a>İstemci SDK'sı günlüğe yazmayı etkinleştir

Log4j ve logback gibi popüler günlük altyapılarına oturum destekleyen günlük cephe olarak SLF4j Async Java SDK'sını kullanır.

Örneğin, log4j günlük altyapısının kullanmak istiyorsanız, aşağıdaki kitaplıklar içinde Java sınıf yolunuza ekleyin.

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Ayrıca bir log4j yapılandırması ekleyin.
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Daha fazla bilgi için [sfl4j günlük kaydını el ile](https://www.slf4j.org/manual.html).

## <a name="netstats"></a>İşletim sistemi bellek istatistikleri
Kaç tane bağlantılar gibi durumlarda, bir fikir netstat komut çalıştırma `ESTABLISHED` ve `CLOSE_WAIT`.

Linux üzerinde aşağıdaki komutu çalıştırabilirsiniz.
```bash
netstat -nap
```
Yalnızca Azure Cosmos DB uç noktası bağlantıları sonucu filtreleyin.

Azure Cosmos DB uç noktası için bağlantı sayısını `ESTABLISHED` durumu, yapılandırılan bağlantı havuzu boyutu büyük olamaz.

Azure Cosmos DB uç noktası bağlantı sayısı olabilir `CLOSE_WAIT` durumu. 1. 000'den fazla olabilir. Bu yüksek bir sayı, bağlantı kurulan ve hızlı bir şekilde bozuk belirtir. Bu durumun olası sorunlara neden olur. Daha fazla bilgi için [genel sorunlar ve çözümleri] bölümü.

 <!--Anchors-->
[Genel sorunlar ve çözümleri]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Konak makinedeki bağlantı sınırı]: #connection-limit-on-host
[Azure SNAT (PAT) bağlantı noktası tükenmesi]: #snat


