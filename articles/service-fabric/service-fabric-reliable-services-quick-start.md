---
title: "C# ' ilk Service Fabric uygulamanızı oluşturma | Microsoft Docs"
description: "Durum bilgisiz ve durum bilgisi olan Hizmetleri ile Microsoft Azure Service Fabric uygulaması oluşturma giriş."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: f67b69e7ad1f7588280de82669040bad5ec6172b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-with-reliable-services"></a>Reliable Services özelliğini kullanmaya başlayın
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-quick-start.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-quick-start-java.md)
> 
> 

Azure Service Fabric uygulaması kodunuzu çalıştırmak bir veya daha fazla hizmet içeriyor. Bu kılavuz, durum bilgisiz ve durum bilgisi olan Service Fabric uygulamalarının nasıl oluşturulacağını göstermektedir [Reliable Services](service-fabric-reliable-services-introduction.md).  Bu Microsoft Virtual Academy video ayrıca durum bilgisiz güvenilir bir hizmetin nasıl oluşturulacağını gösterir:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965">  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a>Temel kavramlar
Reliable Services ile çalışmaya başlamak için yalnızca birkaç temel kavramları anlamanız gerekir:

* **Hizmet türü**: hizmet uygulamanızın budur. Genişleten yazma sınıf tarafından tanımlanan `StatelessService` ve diğer kod veya bağımlılıkları, bir ad ve sürüm numarası ile birlikte kullanılır.
* **Hizmet örneği adlı**: sınıf türü nesne örneklerini oluşturmak gibi hizmetinizi çalıştırmak için adlandırılmış örnekleri, hizmet türüne ilişkin çok oluşturmak. Bir hizmet örneği kullanarak bir URI biçiminde olan bir ada sahip "fabric: /" gibi Düzen "fabric: / MyApp/MyService".
* **Hizmet ana bilgisayarı**: oluşturduğunuz adlı hizmet örneği içinde bir ana bilgisayar işlemi çalıştırmanız gerekir. Hizmet ana bilgisayarı hizmet örneklerini çalıştırdığı yalnızca bir işlemdir.
* **Hizmet kayıt**: kayıt getirir her şeyi birlikte. Hizmet türü çalıştırmak için Service Fabric çalışma zamanı ile örnekleri oluşturmak Service Fabric izin vermek için bir hizmet ana bilgisayarı kayıtlı olması gerekir.  

## <a name="create-a-stateless-service"></a>Durum bilgisi olmayan bir hizmet oluşturma
Durum bilgisiz Hizmet şu anda norm bulut uygulamalarında hizmet türüdür. Hizmetin kendisini güvenilir bir şekilde depolanır veya yüksek oranda kullanılabilir hale gereken veri içermediğinden durum bilgisiz olarak değerlendirilir. Durum bilgisiz Hizmeti'nin bir örneğini kapanırsa, iç durumu kaybolur. Bu türde hizmet durumu Azure tablo veya bir SQL veritabanı, daha yüksek oranda kullanılabilir ve güvenilir yapılması gibi bir dış depolama kalıcı gerekir.

Visual Studio 2015 veya Visual Studio 2017 yönetici olarak başlatın ve adlı yeni bir Service Fabric uygulaması projesi oluşturduğunuzda *HelloWorld*:

![Yeni bir Service Fabric uygulaması oluşturmak için yeni proje iletişim kutusunu kullanın](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Adlı bir durum bilgisiz hizmet projesi oluşturma *HelloWorldStateless*:

![İkinci iletişim kutusunda bir durum bilgisiz hizmet projesi oluşturma](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

Çözümünüzü şimdi iki proje içerir:

* *HelloWorld*. Bu *uygulama* içeren projesi, *Hizmetleri*. Ayrıca, uygulamanızı dağıtmak için yardımcı PowerShell komut dosyaları sayısı yanı sıra, uygulamayı tanımlayan uygulama bildirimi içerir.
* *HelloWorldStateless*. Hizmet projesinin budur. Durum bilgisiz hizmet uygulaması içerir.

## <a name="implement-the-service"></a>Uygulama hizmeti
Açık **HelloWorldStateless.cs** hizmet proje dosyasında. Service Fabric içinde bir hizmet iş mantığı çalıştırabilirsiniz. Hizmeti API'si kodunuz için iki giriş noktaları sağlar:

* Adlı bir uçlu giriş noktası yöntemi *RunAsync*, burada uzun süre çalışan işlem iş yükleri de dahil olmak üzere tüm iş yükleri yürütme başlayabilirsiniz.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* Burada, iletişim yığınında ASP.NET Core gibi tercih ettiğiniz ekleyebilirsiniz iletişimi giriş noktası. Kullanıcılar ve diğer hizmetler isteklerini almak başlayabileceğiniz budur.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

Bu öğreticide, biz üzerine odaklanacaktır `RunAsync()` giriş noktası yöntemi. Hemen kodunuzu çalıştırmaya başlayabileceğiniz budur.
Bir örnek uygulaması proje şablonu içerir `RunAsync()` , çalışırken sayısını artırır.

> [!NOTE]
> Bir iletişim yığını ile çalışma hakkında daha fazla bilgi için bkz [OWIN kendi kendine barındırma ile Service Fabric Web API Hizmetleri](service-fabric-reliable-services-communication-webapi.md)
> 
> 

### <a name="runasync"></a>RunAsync
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

Bir hizmet örneği yerleştirilen ve çalıştırılmaya hazır olduğunda platform bu yöntemi çağırır. Hizmet örneği açıldığında durumsuz bir hizmet için basitçe anlamına gelir. Hizmet örneğinizi kapatılması gerektiğinde koordine etmek için bir iptal belirteci sağlanır. Service Fabric bu hizmet örneği Aç/Kapat döngüsünü birçok kez ömrü hizmeti bir bütün olarak ortaya çıkabilir. Bu da dahil olmak üzere çeşitli nedenlerden kaynaklanabilir:

* Sistem, hizmet örneği için kaynak Dengeleme taşır.
* Kodunuzdaki hataları oluşur.
* Uygulama veya sistem yükseltilir.
* Temel alınan donanım kesinti karşılaşır.

Bu düzenlemesi yönetilen hizmetinizi yüksek oranda kullanılabilir ve düzgün şekilde dengeli tutmak için sistem tarafından.

`RunAsync()`zaman uyumlu olarak bloğu değil. Uygulamanıza RunAsync, bir görev döndürür veya devam etmek çalışma zamanı izin vermek için uzun süre çalışan veya engelleyici işlemleri üzerinde bekler. İçinde Not `while(true)` önceki örnekte, bir görev döndüren bir döngüde `await Task.Delay()` kullanılır. İş yükünüzün zaman uyumlu olarak engellemelidir, yeni bir görev ile zamanlamalısınız `Task.Run()` içinde `RunAsync` uygulaması.

İş yükünüzün iptaline tarafından sağlanan iptal belirteci bağımsızlıklar işbirlikçi çaba ' dir. Sistem göreviniz (başarılı bir şekilde tamamlandığında, iptal veya hataya göre) taşır önce sona erdirmek bekler. İptal belirteci dikkate için herhangi bir iş bitirip çıkmak önemlidir `RunAsync()` sistem iptal istediğinde mümkün olan en kısa sürede.

Bu durum bilgisiz hizmeti örnek sayısı yerel bir değişkende depolanır. Ancak bu durum bilgisi olmayan bir hizmet olduğundan, yalnızca geçerli yaşam döngüsü, hizmet örneği için depolanan değer yok. Değer, hizmet taşır ya da yeniden yüklediğinizde kaybolur.

## <a name="create-a-stateful-service"></a>Durum bilgisi olan bir hizmet oluşturma
Service Fabric hizmetinin durum bilgisi olan yeni bir tür tanıtır. Durum bilgisi olan hizmet tarafından kullanıldığı kod ile birlikte bulunan güvenilir bir şekilde hizmetin kendisini içinde durumu koruyabilirsiniz. Durum Service Fabric tarafından bir dış depolama durumu devam ettirmek için gerek kalmadan yüksek oranda kullanılabilir hale getirilir.

Hizmet taşır veya yeniden olsa bile bir sayaç değeri durum bilgisiz yüksek oranda kullanılabilir ve kalıcı dönüştürün bir durum bilgisi olan hizmet gerekir.

Aynı *HelloWorld* uygulama ekleyebileceğiniz yeni bir hizmet uygulama projesi başvurularında Services'de sağ tıklayıp seçerek **Ekle -> Yeni yapı hizmeti**.

![Service Fabric uygulamanızı bir hizmet Ekle](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

Seçin **durum bilgisi olan hizmet** ve adlandırın *HelloWorldStateful*. **Tamam** düğmesine tıklayın.

![Yeni bir Service Fabric durum bilgisi olan hizmet oluşturmak için yeni proje iletişim kutusunu kullanın](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Uygulamanızı şimdi iki Hizmetleri olmalıdır: durum bilgisi olmayan hizmetin *HelloWorldStateless* ve durum bilgisi olan hizmet *HelloWorldStateful*.

Durum bilgisi olan bir hizmet durum bilgisi olmayan hizmet olarak aynı giriş noktaları sahiptir. Kullanılabilirliğini ana farktır bir *durum sağlayıcısı* , saklayabilir durumu güvenilir. Service Fabric adlı durum sağlayıcısı bir uygulama ile birlikte gelen [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md), hangi, oluşturmanızı sağlar çoğaltılan veri yapılarını güvenilir durumu Yöneticisi aracılığıyla. Durum bilgisi olan güvenilir hizmet varsayılan olarak bu durum sağlayıcısı kullanır.

Açık **HelloWorldStateful.cs** içinde *HelloWorldStateful*, aşağıdaki RunAsync yöntemi içerir:

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, the transaction aborts, all changes are
            // discarded, and nothing is saved to the secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a>RunAsync
`RunAsync()`durum bilgisi olan ve durum bilgisiz Hizmetleri'nde benzer şekilde çalışır. Yürütülmeden önce ancak, durum bilgisi olan bir hizmet platform ek çalışma sizin adınıza gerçekleştirir `RunAsync()`. Bu iş, durum Yöneticisi'ni güvenilir ve güvenilir koleksiyonları kullanıma hazır olduktan içerebilir.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Güvenilir koleksiyonlar ve güvenilir durum Yöneticisi
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) durumunu hizmetinde güvenilir bir şekilde depolamak için kullanabileceğiniz bir sözlük uygulamasıdır. Service Fabric ve güvenilir koleksiyonlarla, hizmetiniz bir dış kalıcı depoya gerek kalmadan doğrudan veri depolayabilirsiniz. Güvenilir koleksiyonları verilerinizin yüksek oranda kullanılabilir yap. Service Fabric gerçekleştirir, bu, oluşturma ve birden çok yönetme *çoğaltmaları* hizmetinizin sizin için. Ayrıca, hemen yinelemeler ve bunların durumu geçişleri yönetme karmaşıklıkları soyutlar bir API sağlar.

Güvenilir koleksiyonları birkaç uyarılar, özel türleri dahil olmak üzere, herhangi bir .NET türü depolayabilirsiniz:

* Service Fabric durumunuza tarafından yüksek oranda kullanılabilir kılar *çoğaltma* durumunu düğümler ve güvenilir koleksiyonları arasında her kopyada yerel diske verilerinizi depolamak. Bu güvenilir koleksiyonu içinde depolanan her şey olması gerektiği anlamına gelir *seri hale getirilebilir*. Varsayılan olarak, güvenilir koleksiyonları kullanmaya [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) seri hale getirme için bu nedenle, önemlidir türlerinizi olduğundan emin olmak [veri sözleşmesi seri hale getirici tarafından desteklenen](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) varsayılan serileştirici kullandığınızda.
* Güvenilir derlemeler işlemleri yaparsanız nesneleri yüksek kullanılabilirlik için çoğaltılır. Güvenilir koleksiyonu içinde depolanan nesneleri hizmetinizi yerel bellekte tutulur. Bu nesne için yerel bir referans olduğu anlamına gelir.
  
   Bu nesne yerel örnekleri işlemde güvenilir koleksiyonu bir güncelleştirme işlemi gerçekleştirilirken olmadan mutate değil, önemlidir. Yerel nesnelerin örneklerini değişiklikler otomatik olarak çoğaltılmayacak olmasıdır. Nesneyi sözlüğün geri yeniden eklemek veya birini kullanın *güncelleştirme* sözlük yöntemleri.

Güvenilir durum Yöneticisi güvenilir koleksiyonları tarafından yönetilir. Yalnızca güvenilir durum Yöneticisi için güvenilir bir koleksiyon adıyla herhangi bir zamanda ve herhangi bir yerde hizmetinizi sorabilirsiniz. Bir başvuru ulaşırsınız güvenilir durum Yöneticisi'ni sağlar. Verme öneririz, güvenilir koleksiyonu örneklerine başvuruları sınıf üyesi değişkenleri veya özellikleri kaydetmeniz. Başvuru hizmet yaşam döngüsü içinde her zaman bir örneğine ayarlandığından emin olmak için özel dikkatli olunması gerekir. Güvenilir durum Yöneticisi bu iş, işler ve yineleme ziyaretleriniz için optimize edilmiştir.

### <a name="transactional-and-asynchronous-operations"></a>İşlem ve zaman uyumsuz işlemler
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Güvenilir koleksiyonları aynı işlemlerin çoğunu sahiptir, kendi `System.Collections.Generic` ve `System.Collections.Concurrent` ortaklarınıza LINQ hariç. Güvenilir derlemeler işlemleri zaman uyumsuzdur. Yazma işlemleri güvenilir koleksiyonlarıyla çoğaltabilir ve verileri diske kalıcı hale getirmek için g/ç işlemleri olmasıdır.

Güvenilir toplama işlemleri *işlem*, böylece, durumu tutarlı birden çok güvenilir koleksiyonları ve işlemler arasında tutabilirsiniz. Örneğin, bir iş öğesi güvenilir bir kuyruktan dequeue bir işlem gerçekleştirmek ve sözlükteki güvenilir, tümü tek bir işlem içinde sonucu kaydetmek. Bu atomik bir işlem olarak kabul edilir ve tüm işlemi başarılı olur veya tüm işlem geri alma güvence altına alır. Öğe dequeue sonra bir hata oluşursa, ancak sonuç kaydetmeden önce tüm işlem geri alındı ve işleme sırasındaki öğesi kalır.

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Biz şimdi dönmek *HelloWorld* uygulama. Artık, yapı ve hizmetlerinizi dağıtabilirsiniz. Bastığınızda **F5**, uygulamanızın yerleşik ve yerel kümenizdeki dağıtılabilir.

Çalışan hizmetleri başlattıktan sonra oluşturulan olay Windows için izleme (ETW) olayları görüntüleyebilirsiniz bir **tanılama olayları** penceresi. Durum bilgisi olmayan hizmet ve durum bilgisi olan hizmeti uygulamasında görüntülenen olayları olduğunu unutmayın. Tıklayarak akış duraklatabilirsiniz **duraklatma** düğmesi. Bu iletiyi genişleterek sonra bir ileti ayrıntılarını inceleyebilirsiniz.

> [!NOTE]
> Uygulamayı çalıştırmadan önce çalıştıran bir yerel geliştirme kümesine sahip olduğunuzdan emin olun. Kullanıma [Başlangıç Kılavuzu](service-fabric-get-started.md) yerel ortamınızı ayarlama hakkında bilgi için.
> 
> 

![Visual Studio tanılama olaylarını görüntüle](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric uygulamanızı Visual Studio'da hata ayıklama](service-fabric-debugging-your-application.md)

[Kullanmaya başlama: OWIN kendi kendine barındırma ile Service Fabric Web API Hizmetleri](service-fabric-reliable-services-communication-webapi.md)

[Güvenilir Koleksiyonlar hakkında daha fazla bilgi edinin](service-fabric-reliable-services-reliable-collections.md)

[Bir uygulamayı dağıtma](service-fabric-deploy-remove-applications.md)

[Uygulama yükseltme](service-fabric-application-upgrade.md)

[Güvenilir hizmetler için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/dn706529.aspx)

