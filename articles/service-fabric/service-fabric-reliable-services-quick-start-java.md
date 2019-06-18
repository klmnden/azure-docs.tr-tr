---
title: Java kullanarak ilk Azure Service Fabric güvenilir hizmeti oluşturma | Microsoft Docs
description: Microsoft Azure Service Fabric uygulaması ile durum bilgisiz ve durum bilgisi olan hizmetler oluşturmaya giriş.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: chackdan
editor: ''
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 6bf8c632a7513d018745bc74aa0a1db95a39af8b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62130135"
---
# <a name="get-started-with-reliable-services"></a>Reliable Services özelliğini kullanmaya başlayın
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-quick-start.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-quick-start-java.md)
>
>

Bu makalede, Azure Service Fabric güvenilir hizmetler temelleri açıklanır ve oluşturma ve Java dilinde yazılan basit bir güvenilir hizmet uygulaması dağıtma gösterilmektedir. 

## <a name="installation-and-setup"></a>Yükleme ve Kurulum
Başlamadan önce makinenizde ayarlanan Service Fabric geliştirme ortamına sahip olduğunuzdan emin olun.
Bu örneği ayarlamanız gerekirse, Git [Mac üzerinde çalışmaya başlama](service-fabric-get-started-mac.md) veya [Linux'ta kullanmaya başlama](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Temel kavramlar
Reliable Services ile çalışmaya başlamak için yalnızca birkaç temel kavramları anlamanız gerekir:

* **Hizmet türünü**: Hizmet uygulamanız budur. Genişleten yazdığınız sınıfı tarafından tanımlanır `StatelessService` ve diğer kodu veya bağımlılıkları, bir ad ve bir sürüm numarası ile birlikte kullanılır.
* **Hizmet örneği adlı**: Sınıf türü nesne örneklerini oluşturmak gibi hizmetinizi çalıştırmak için adlandırılmış örneklerde, hizmet türünün çok oluşturun. Hizmet örnekleri, aslında yazdığınız, hizmet sınıfı nesne öğesinin örneklemeleri olan.
* **Hizmet ana bilgisayarı**: Konak içinde çalıştırmak oluşturduğunuz adlandırılmış hizmet örnekleri gerekir. Hizmet ana bilgisayarı, hizmetin örneklerine çalıştırdığı yalnızca bir işlemdir.
* **Hizmet kayıt**: Kayıt olan her şeyi birlikte sunar. Çalıştırmak için Service Fabric çalışma zamanıyla bir hizmet ana örneği oluşturmak Service Fabric izin vermek için hizmet türü kayıtlı olması gerekir.  

## <a name="create-a-stateless-service"></a>Durum bilgisi olmayan hizmet oluşturma
Service Fabric uygulaması oluşturarak başlayın. Linux için Service Fabric SDK'sı bir Yeoman içeren bir durum bilgisi olmayan hizmet ile bir Service Fabric uygulaması için iskele sağlamak için oluşturucu. Aşağıdaki Yeoman çalıştırarak komutu:

```bash
$ yo azuresfjava
```

Oluşturmak için yönergeleri izleyin bir **güvenilir durum bilgisi olmayan hizmet**. Bu öğreticide, "HelloWorldApplication" uygulama adı ve "HelloWorld" hizmeti. Sonuç için dizinler içeren `HelloWorldApplication` ve `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```
### <a name="service-registration"></a>Hizmet kaydı
Hizmet türlerini Service Fabric çalışma zamanıyla kaydedilmesi gerekir. Hizmet türünün tanımlanan `ServiceManifest.xml` ve uygular, hizmet sınıfı `StatelessService`. Hizmet kayıt işlemin ana girdi noktası içinde gerçekleştirilir. Bu örnekte, işlem ana giriş noktasıdır `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="implement-the-service"></a>Hizmet ekleme

Açık **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Bu sınıf, hizmet türünü tanımlar ve herhangi bir kodu çalıştırabilir. Hizmet API'si, kodunuz için iki giriş noktası sağlar:

* Adlı bir açık uçlu bir giriş noktası yönteminin `runAsync()`, burada, uzun süre çalışan işlem iş yükleri de dahil olmak üzere tüm iş yükleri çalıştırma başlayabilirsiniz.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Burada, tercih ettiğiniz iletişim yığınında takabilirsiniz iletişim giriş noktası. Kullanıcıların ve diğer hizmetlerden istekleri alma başlayabileceğiniz budur.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Bu öğreticide odaklanır `runAsync()` giriş noktası yöntemi. Hemen kodunuzu çalıştıran başlayabileceğiniz budur.

### <a name="runasync"></a>RunAsync
Bir hizmet örneği yerleştirilmiş ve çalıştırılmaya hazır olduğunda platform bu yöntemi çağırır. Hizmet örneği açıldığında bir durum bilgisi olmayan hizmet için anlamına gelir. Bir iptal belirteci, hizmet Örneğinizdeki kapatılması gerektiğinde koordine etmek için sağlanır. Service Fabric'te bu açma/kapatma döngüsü bir hizmet örneği üzerinde hizmet ömrünü birden çok kez bir bütün olarak ortaya çıkabilir. Bu durum çeşitli nedenlerle oluşabilir:

* Sistem hizmeti örnekleriniz için kaynak Dengeleme taşır.
* Kodunuzdaki hataları oluşur.
* Uygulama veya sistem yükseltilir.
* Temel alınan donanım kesinti karşılaşır.

Bu düzenleme yönetilen yüksek oranda kullanılabilir ve düzgün şekilde dengeli hizmetinizi korumak için Service Fabric tarafından.

`runAsync()` zaman uyumlu olarak Engellemesi gereken değil. Uygulamanıza runAsync devam etmek çalışma zamanı izin vermek için bir CompletableFuture döndürmelidir. İş yükünüz CompletableFuture içinde yapılması gerektiğini uzun süre çalışan bir görev uygulamak gerekiyorsa.

#### <a name="cancellation"></a>İptal etme
İş yükünüzün iptal tarafından sağlanan iptali belirtecinin düzenlenen işbirliği yapan bir işlemdir. Sistem (başarıyla tamamlandığında, iptal veya hata), taşınmadan önce sona erdirmek, görev bekler. İptal belirteci uymanız, işlerinizi tamamlamak ve çıkmak önemlidir `runAsync()` sistem iptal isteğinde bulunduğunda mümkün olan en kısa sürede. Aşağıdaki örnek, bir iptal olayı işlemek gösterilmektedir:

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

    // TODO: Replace the following sample code with your own logic
    // or remove this runAsync override if it's not needed in your service.

    return CompletableFuture.runAsync(() -> {
        long iterations = 0;
        while(true)
        {
        cancellationToken.throwIfCancellationRequested();
        logger.log(Level.INFO, "Working-{0}", ++iterations);

        try {
            Thread.sleep(1000);
        } catch (InterruptedException ex){}
        }
    });
}
```

Bu durum bilgisi olmayan hizmet örneğinde, sayı yerel bir değişkende depolanır. Ancak bu durum bilgisi olmayan bir hizmet olduğundan, yalnızca geçerli yaşam döngüsünü, hizmet örneği için depolanan değeri yok. Hizmet taşır veya yeniden başlatır, değeri kaybolur.

## <a name="create-a-stateful-service"></a>Durum bilgisi olan hizmet oluşturma
Service Fabric durum bilgisi olan hizmeti yeni bir tür tanıtır. Durum bilgisi olan hizmet durumu içinde kullandıkları koduyla birlikte bulunan güvenilir bir şekilde hizmetin kendisini koruyabilir. Durumunu Service Fabric tarafından bir dış depolama durumu kalıcı hale getirmek zorunda kalmadan yüksek oranda kullanılabilir hale getirilir.

Hizmet taşır veya yeniden olsa bile sayaç değerini durum bilgisi olmayan yüksek oranda kullanılabilir kalıcı dönüştürmek için bir durum bilgisi olan hizmet gerekir.

HelloWorld uygulamasını olarak aynı dizinde yeni bir hizmet çalıştırarak ekleyebilirsiniz `yo azuresfjava:AddService` komutu. "Güvenilir durum bilgisi olan hizmet" çerçevenizin seçin ve "HelloWorldStateful" hizmet olarak adlandırın. 

Uygulamanız artık iki hizmet vardır: HelloWorld durum bilgisi olmayan hizmet ve durum bilgisi olan hizmet HelloWorldStateful.

Bir durum bilgisi olan hizmeti durum bilgisi olmayan hizmet olarak aynı giriş noktaları içerir. Ana kullanılabilirlik durumu güvenilir bir şekilde depolamak bir durum sağlayıcısının farktır. Service Fabric, güvenilir durum Yöneticisi yoluyla çoğaltılan veri yapılarını oluşturmanıza olanak sağlayan güvenilir koleksiyon adı verilen bir durumu sağlayıcısı uygulaması ile birlikte gelir. Durum bilgisi olan güvenilir bir hizmet, varsayılan olarak bu durum sağlayıcısı kullanır.

İçinde HelloWorldStateful.java açın **HelloWorldStateful src ->** , aşağıdaki RunAsync yöntemi içerir:

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    Transaction tx = stateManager.createTransaction();
    return this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap").thenCompose((map) -> {
        return map.computeAsync(tx, "counter", (k, v) -> {
            if (v == null)
                return 1L;
            else
                return ++v;
            }, Duration.ofSeconds(4), cancellationToken)
                .thenCompose((r) -> tx.commitAsync())
                .whenComplete((r, e) -> {
            try {
                tx.close();
            } catch (Exception e) {
                logger.log(Level.SEVERE, e.getMessage());
            }
        });
    });
}
```

### <a name="runasync"></a>RunAsync
`RunAsync()` durum bilgisi olan ve olmayan hizmetleri benzer şekilde çalışır. Yürütülmeden önce ancak durum bilgisi olan bir hizmet platform ek iş sizin adınıza gerçekleştirdiği `RunAsync()`. Bu iş, güvenilir durum Yöneticisi ve güvenilir koleksiyonlar, kullanıma hazır olan sağlama dahil olabilir.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Güvenilir koleksiyonlar ve güvenilir durum Yöneticisi
```java
ReliableHashMap<String,Long> map = this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap")
```

[ReliableHashMap](https://docs.microsoft.com/java/api/microsoft.servicefabric.data.collections.reliablehashmap) güvenilir hizmeti durumunu depolamak için kullanabileceğiniz bir sözlük uygulamasıdır. Service Fabric ve güvenilir HashMaps, hizmetiniz bir dış kalıcı depoya gerek kalmadan doğrudan veri depolayabilirsiniz. Güvenilir HashMaps verilerinizi yüksek oranda kullanılabilir yap. Service Fabric gerçekleştirir, bu, oluşturma ve birden çok yönetme *çoğaltmaları* hizmetinizin sizin için. Ayrıca, yinelemeler ve bunların durumu geçişleri yönetme karmaşasından dengelediği bir API sağlar.

Güvenilir koleksiyonlar birkaç uyarılar, özel türler dahil olmak üzere, herhangi bir Java türü depolayabilir:

* Service Fabric durumunuzu tarafından yüksek oranda kullanılabilir kılar *çoğaltma* düğüme ve güvenilir HashMap durumu her çoğaltma üzerinde verilerinizi yerel diskte depolar. Bu güvenilir HashMaps içinde depolanan her şey olması gerektiği anlamına gelir *seri hale getirilebilir*. 
* Güvenilir HashMaps hareketlerde işlerseniz nesneleri yüksek kullanılabilirlik için çoğaltılır. Güvenilir HashMaps içinde depolanan nesneleri hizmetinizi yerel bellekte tutulur. Bu, yerel başvuru nesnesine sahip olduğunuz anlamına gelir.
  
   Bu nesne yerel örnekleri bir işlemde güvenilir koleksiyon bir güncelleştirme işlemini gerçekleştirmeden bulunmamalıdır değil, önemlidir. Yerel nesnelerin örneklerini değişiklikleri otomatik olarak çoğaltılmayacak olmasıdır. Nesne sözlük içine takın veya şunlardan birini kullanın *güncelleştirme* sözlük yöntemleri.

Güvenilir durum Yöneticisi güvenilir HashMaps sizin yerinize yönetir. Güvenilir durum Yöneticisi için güvenilir bir koleksiyon adıyla dilediğiniz zaman ve herhangi bir yerdeki hizmetinizde sorabilirsiniz. Güvenilir durum Yöneticisi, bir başvuru geri almasını sağlar. Sınıf üyesi değişkenleri veya özellikleri güvenilir koleksiyon örnekleri başvuruları kaydetmeniz önerilmemektedir. Başvuru örneğine hizmet yaşam döngüsü içinde her zaman ayarlandığından emin olmak için özel dikkatli olunması gerekir. Güvenilir durum Yöneticisi bu işi sizin için yapar ve tekrar ziyaret için optimize edilmiştir.


### <a name="transactional-and-asynchronous-operations"></a>İşlem ve zaman uyumsuz işlemler
```java
return map.computeAsync(tx, "counter", (k, v) -> {
    if (v == null)
        return 1L;
    else
        return ++v;
    }, Duration.ofSeconds(4), cancellationToken)
        .thenCompose((r) -> tx.commitAsync())
        .whenComplete((r, e) -> {
    try {
        tx.close();
    } catch (Exception e) {
        logger.log(Level.SEVERE, e.getMessage());
    }
});
```

Güvenilir HashMaps işlemleri zaman uyumsuzdur. Güvenilir koleksiyonlar ile yazma işlemleri çoğaltın ve verileri diske kalıcı hale getirmek için g/ç işlemleri olmasıdır.

Güvenilir HashMap işlemleri *işlem*, böylece, durumu tutarlı birden çok güvenilir HashMaps ve işlemleri tutabilirsiniz. Örneğin, bir iş öğesi bir güvenilir sözlüğü almak, bir işlem gerçekleştirmek ve sonucu tek bir işlem içinde başka bir güvenilir HashMap kaydetmek. Bu bir atomik işlem olarak kabul edilir ve işlemin tamamı başarılı veya tüm işlemi geri alma garanti eder. Öğe sıradan çıkarma sonra bir hata oluşursa, ancak sonuç kaydetmeden önce işlemin tümü geri alınır ve öğe işleme kuyruğunda kalır.


## <a name="build-the-application"></a>Uygulama oluşturma

Yeoman yapı iskelesi uygulamayı derleyin ve bash betiklerine dağıtmak ve bu uygulamayı kaldırmak için bir gradle betiğini içerir. Uygulamayı çalıştırmak için ilk uygulama gradle ile derleme:

```bash
$ gradle
```

Bu, Service Fabric CLI kullanılarak dağıtılabilir bir Service Fabric uygulama paketi oluşturur.

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Uygulama oluşturulduktan sonra uygulamayı yerel kümeye dağıtabilirsiniz.

1. Yerel Service Fabric kümesine bağlanın.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini çalıştırın.

    ```bash
    ./install.sh
    ```

Oluşturulan uygulamayı dağıtma işlemi, diğer tüm Service Fabric uygulamalarında olduğu gibidir. Ayrıntılı yönergeler için [Service Fabric uygulamasını Service Fabric CLI ile yönetme](service-fabric-application-lifecycle-sfctl.md) ile ilgili belgelere bakın.

Bu komutların parametreleri, uygulama paketi içinde oluşturulmuş bildirimlerde bulunabilir.

Uygulama dağıtıldığında bir tarayıcı açın ve [http://localhost:19080/Explorer](http://localhost:19080/Explorer) konumundaki [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)'a gidin. Ardından, **Uygulamalar** düğümünü genişletin ve geçerli olarak uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

> [!IMPORTANT]
> Uygulamayı azure'da güvenli bir Linux kümesi dağıtmak için Service Fabric çalışma zamanı uygulamanızla doğrulamak için bir sertifika yapılandırmanız gerekir. Bunun yapılması, temel alınan Service Fabric çalışma API'leri ile iletişim kurmak Reliable Services hizmetlerinizi sağlar. Daha fazla bilgi için bkz. [Linux kümelerinde çalıştırmak için bir Reliable Services uygulaması yapılandırırsınız](./service-fabric-configure-certificates-linux.md#configure-a-reliable-services-app-to-run-on-linux-clusters).  
>

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md)
