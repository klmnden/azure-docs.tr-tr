---
title: Tanılama ve sorun giderme Azure Cosmos DB Java zaman uyumsuz SDK'sı | Microsoft Docs
description: İstemci tarafı günlüğe kaydetme gibi kullanım özellikleri ve diğer üçüncü taraf araçları tanımlamak için tanılama ve Azure Cosmos DB sorunlarını giderme.
services: cosmos-db
author: moderakh
ms.service: cosmos-db
ms.topic: troubleshoot
ms.date: 10/28/2018
ms.author: moderakh
ms.devlang: java
ms.component: cosmosdb-sql
ms.openlocfilehash: ef1d2d0751bf1b1a7ee88fbf37e44e6316dee8f8
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249888"
---
# <a name="troubleshooting-issues-when-using-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Async Java SDK'sı ile Azure Cosmos DB SQL API hesabı kullanırken sorunlarını giderme
Bu makale ortak sorunları, geçici çözümler, tanılama adımları ve araçları kullanırken kapsar [Java zaman uyumsuz ADK](sql-api-sdk-async-java.md) ile Azure Cosmos DB SQL API hesabı.
Async Java SDK'sı, Azure Cosmos DB SQL API'sine erişmek için istemci tarafı mantıksal gösterim sağlar. Bu makalede, araçları ve yaklaşımları herhangi bir sorunla karşılaşırsanız çalıştırırsanız yardımcı olması için açıklar.

Bu liste başlatın:
    * Bir göz atın [genel sorunlar ve çözümleri] bu makaledeki bir bölüm.
    * SDK'mız olduğu [github üzerinde açık kaynaklı](https://github.com/Azure/azure-cosmosdb-java) ve sahip olduğumuz [sorunları bölümüne](https://github.com/Azure/azure-cosmosdb-java/issues) , etkin bir şekilde izleyin. Zaten kaydedilen bir geçici çözüm ile benzer bir sorun bulursanız denetleyin.
    * Gözden geçirme [performans ipuçları](performance-tips-async-java.md) ve önerilen uygulamaları izleyin.
    * Bu makalenin geri kalanında izleyin bir çözüm bulamadıysanız, dosya bir [GitHub sorunu](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-workarounds"></a>Genel sorunlar ve çözümleri

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Ağ sorunları, zaman aşımı hatası, düşük aktarım hızı, gecikme süresi yüksek Netty okuyun

#### <a name="general-suggestions"></a>Genel öneriler
* Uygulama, Cosmos DB hesabınızla aynı bölgede üzerinde çalıştığından emin olun. 
* Uygulamanın çalıştığı konak üzerindeki CPU kullanımını denetleyin. CPU kullanımı % 90 veya daha fazla ise, daha yüksek bir yapılandırmaya sahip bir konağa uygulamanızı çalıştırmayı deneyin veya daha fazla makine üzerinde yük dağıtabilirsiniz.

#### <a name="connection-throttling"></a>Bağlantı daraltma
Bağlantı daraltma gerçekleşebilir nedeniyle [konak makinedeki bağlantı sınırı], veya [Azure SNAT (PAT) bağlantı noktası tükenmesi]:

##### <a name="connection-limit-on-host"></a>Konak makinedeki bağlantı sınırı
Bazı Linux sistemleri (örneğin, 'Red Hat'), üst sınır açık dosyaların, toplam sayısına sahip. Bu sayı çok bağlantı toplam sayısını sınırlar için yuva Linux'ta dosyaları olarak uygulanır.
Şu komutu çalıştırın:

```bash
ulimit -a
```
Açık sayısı ("nofile") (en az olarak çift, bağlantı havuzu boyutu olarak) yeterince büyük olması gerekiyor dosyaları. Daha ayrıntılı olarak okuma [performans ipuçları](performance-tips-async-java.md).

##### <a name="snat"></a>Azure SNAT (PAT) bağlantı noktası tükenmesi

Uygulamanızı Azure VM'deki varsayılan olarak dağıtılıp dağıtılmadığını [Azure SNAT bağlantı noktaları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) VM'nizi dışında herhangi bir uç noktaya bağlantı kurmak için kullanılır. Sanal makineden Cosmos DB uç noktasına izin verilen bağlantı sayısı tarafından sınırlı [Azure SNAT yapılandırma](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

Azure SNAT bağlantı noktaları, yalnızca Azure sanal makinenizin özel IP adresi vardır ve bir genel IP adresi için bir bağlantı kurmak bir işlem VM'den çalışır olduğunda kullanılır. Bu nedenle, Azure SNAT sınırlama önlemek için iki geçici çözüm vardır:
    * Azure Cosmos DB hizmet uç noktanıza açıklandığı gibi Azure VM ağınızın alt ağa eklemek [sanal ağ hizmet uç noktası etkinleştirme](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). Hizmet uç noktası etkinleştirildiğinde, artık bir genel IP ile cosmos DB'ye yerine VNET gönderdiği istekleri ve alt ağ kimlik gönderilir. Bu değişiklik, genel IP'ler izin verilir, yalnızca güvenlik duvarı bırakmaları neden olabilir. Hizmet uç noktası etkinleştirilirken güvenlik duvarı kullanıyorsanız, güvenlik duvarını kullanarak alt ağ Ekle [VNET ACL'leri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
    * Azure VM için genel bir IP atayın.

#### <a name="http-proxy"></a>HTTP Ara sunucusu

Bir HTTP proxy kullanıyorsanız, HttpProxy SDK'da yapılandırılmış bağlantı sayısını destekleyebilir unutmayın `ConnectionPolicy`.
Aksi takdirde, bağlantı sorunlarını yönetmektir.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Kodlama düzeni geçersiz: Netty GÇ iş parçacığı engelleme

SDK'sı kullanır [Netty](https://netty.io/) iletişim kurmak için Azure Cosmos DB hizmetini GÇ Kitaplığı. Zaman uyumsuz API sunuyoruz ve engelleyici olmayan netty g/ç API'leri kullanın. SDK'ın GÇ iş netty GÇ iş parçacığı üzerinde gerçekleştirilir. GÇ netty iş parçacığı sayısı, uygulama makinesinin CPU çekirdek sayısı ile aynı olacak şekilde yapılandırılır. Netty GÇ iş parçacığı için kullanılacak yalnızca yöneliktir engelleyici olmayan netty GÇ iş. SDK'sı API çağırma sonucu uygulamalar'ın kod netty GÇ iş parçacığı birini döndürür. Netty iş parçacığında sonuçları aldıktan sonra uygulamayı netty iş parçacığında uzun süreli bir işlem yaparsa, SDK'sı GÇ iş parçacığı, iç GÇ işini gerçekleştirmek için yeterli sayıda olmamasına neden olabilir. Bu tür bir uygulama kodlama düşük aktarım hızı gecikme süresi yüksek, neden olabilir ve `io.netty.handler.timeout.ReadTimeoutException` hataları. İşlemi sürmesi bildiğinizde iş parçacığına geçiş yapmak için çözüm olabilir.

   Örneğin, aşağıdaki kod parçacığı netty iş parçacığı üzerinde birden fazla birkaç milisaniye, uzun süreli iş gerçekleştirirseniz, burada netty bir GÇ iş parçacığı g/ç işleri işlemek için mevcut ve sonuç olarak, ReadTimeou elde bir duruma sonunda alabilirsiniz gösterir. tException:
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
                        // time consuming work. For example:
                        // writing to a file, computationally heavy work, or just sleep
                        // basically anything which takes more than a few milliseconds
                        // doing such operation on the IO netty thread
                        // without a proper scheduler, will cause problems.
                        // The subscriber will get ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //will be io.netty.handler.timeout.ReadTimeoutException
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
   Geçici çözüm, iş parçacığı zaman alma iş gerçekleştirdiğiniz değiştirmektir. Uygulamanız için tekil bir zamanlayıcı örneğini tanımlayın:
   ```java
// have a singleton instance of executor and scheduler
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   (Örneğin, g/ç engelleme hesaplama açısından yoğun iş) alma iş zaman ihtiyaç duyduğunuzda tarafından sağlanan bir çalışan iş parçacığı geçin, `customScheduler` kullanarak `.observeOn(customScheduler)` API.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // switches the thread.
        .subscribe(
            // ...
        );
```
Kullanarak `observeOn(customScheduler)`, netty GÇ iş parçacığı bırakın ve kendi özel iş parçacığı customScheduler tarafından sağlanan geçin. Sorunu çözmek için bu değişikliği ve size vermeyecektir `io.netty.handler.timeout.ReadTimeoutException` artık hata.

### <a name="connection-pool-exhausted-issue"></a>Bağlantı havuzu tükenmiş sorunu

`PoolExhaustedException` bir istemci-tarafı hatasıdır. Genellikle bu hata alırsanız, uygulama yükünüz SDK bağlantı havuzu verebilen daha büyük bir gösterge olmasıdır. Bağlantı havuzu boyutu artırmayı veya birden fazla uygulama üzerindeki yükü dağıtma yardımcı olabilir.

### <a name="request-rate-too-large"></a>İstek oranı çok büyük
Bu hata, sağlanan aktarım hızınızı harcanan ve daha sonra yeniden denemesi gerektiğini gösteren bir sunucu tarafı hatasıdır. Genellikle bu hata alırsanız, koleksiyon işleme hacmini artırmayı düşünün.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Azure Cosmos DB öykünücüsü'nü bağlanma başarısız

Cosmos DB öykünücüsü'nü HTTPS sertifikası otomatik olarak imzalanır. Öykünücü ile çalışmanız için SDK, Java TrustStore için öykünücü sertifikayı içeri aktarmalısınız. Açıklandığı gibi [burada](local-emulator-export-ssl-certificates.md).


## <a name="enable-client-sice-logging"></a>İstemci SDK'sı günlüğe yazmayı etkinleştir

Log4j ve logback gibi popüler günlük altyapılarına oturum destekleyen günlük cephe olarak SLF4j async Java SDK'sını kullanır.

Örneğin, log4j günlük altyapısının kullanmak istiyorsanız, aşağıdaki kitaplıklar içinde Java sınıf yolunuza ekleyin:

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

Ayrıca bir log4j yapılandırması ekleyin:
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

Gözden geçirme [sfl4j günlük kaydını el ile](https://www.slf4j.org/manual.html) daha fazla bilgi için.

## <a name="netstats"></a>İşletim sistemi bellek istatistikleri
Kaç bağlantıları olduğundan, fikir netstat komut çalıştırma `Established` durumu `CLOSE_WAIT` vb. durumu.

Linux üzerinde aşağıdaki komutu çalıştırabilirsiniz:
```bash
netstat -nap
```
Cosmos DB uç noktası yalnızca bağlantı sonucu filtreleyin.

Görünüşe göre Cosmos DB uç noktası, bağlantı sayısı `Established` durumu değil, yapılandırılan bağlantı havuzu boyutu büyük olmalıdır.

Cosmos DB uç noktası, bağlantı sayısı olup olmadığını `CLOSE_WAIT` durum için bir gösterge bağlantıları kullanıcının 1000 bağlantıları, birden çok örnek kurulan ve hızlı bir şekilde, olası sorunlara yol açabilir aşağı bozuk. Gözden geçirme [genel sorunlar ve çözümleri] bölümünde daha ayrıntılı bilgi için.

 <!--Anchors-->
[Genel sorunlar ve çözümleri]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Konak makinedeki bağlantı sınırı]: #connection-limit-on-host
[Azure SNAT (PAT) bağlantı noktası tükenmesi]: #snat


