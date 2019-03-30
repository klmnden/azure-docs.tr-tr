---
title: İlk Service Fabric uygulamanızı oluşturma C# | Microsoft Docs
description: Microsoft Azure Service Fabric uygulaması ile durum bilgisiz ve durum bilgisi olan hizmetler oluşturmaya giriş.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/16/2018
ms.author: vturecek
ms.openlocfilehash: d27702983a4378becdbc67f3f156c92be3dc3af6
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665143"
---
# <a name="get-started-with-reliable-services"></a>Reliable Services özelliğini kullanmaya başlayın
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-quick-start.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-quick-start-java.md)
> 
> 

Bir Azure Service Fabric uygulaması kodunuzu çalıştıran bir veya daha fazla hizmet içeriyor. Bu kılavuz, durum bilgisiz ve durum bilgisi olan Service Fabric uygulamaları oluşturmak nasıl gösterir [Reliable Services](service-fabric-reliable-services-introduction.md).  

## <a name="basic-concepts"></a>Temel kavramlar
Reliable Services ile çalışmaya başlamak için yalnızca birkaç temel kavramları anlamanız gerekir:

* **Hizmet türünü**: Hizmet uygulamanız budur. Genişleten yazdığınız sınıfı tarafından tanımlanır `StatelessService` ve diğer kodu veya bağımlılıkları, bir ad ve bir sürüm numarası ile birlikte kullanılır.
* **Hizmet örneği adlı**: Sınıf türü nesne örneklerini oluşturmak gibi hizmetinizi çalıştırmak için adlandırılmış örneklerde, hizmet türünün çok oluşturun. Bir hizmet örneği kullanarak bir URI biçiminde olan bir ada sahip "fabric: /" gibi Düzen "fabric: / Uygulamam/Hizmetim".
* **Hizmet ana bilgisayarı**: Bir barındırma işlemi içinde çalıştırmak oluşturduğunuz adlandırılmış hizmet örnekleri gerekir. Hizmet ana bilgisayarı, hizmetin örneklerine çalıştırdığı yalnızca bir işlemdir.
* **Hizmet kayıt**: Kayıt olan her şeyi birlikte sunar. Çalıştırmak için Service Fabric çalışma zamanıyla bir hizmet ana örneği oluşturmak Service Fabric izin vermek için hizmet türü kayıtlı olması gerekir.  

## <a name="create-a-stateless-service"></a>Durum bilgisi olmayan hizmet oluşturma
Durum bilgisi olmayan hizmet, bulut uygulamalarındaki norm olan hizmetine türüdür. Hizmet, güvenilir bir şekilde depolanan ya da yüksek oranda kullanılabilir hale gereken veri içermediğinden durum bilgisiz olarak değerlendirilir. Durum bilgisi olmayan hizmet örneğini kapanırsa, kendi iç durumu kaybolur. Bu türde hizmet durumu Azure tabloları veya, yüksek kullanılabilirlik ve güvenilirlik yapılması için bir SQL veritabanı gibi bir dış depolama kalıcı gerekir.

Visual Studio 2015 veya Visual Studio 2017'yi yönetici olarak başlatın ve adlı yeni bir Service Fabric uygulaması projesi oluşturma *HelloWorld*:

![Yeni bir Service Fabric uygulaması oluşturmak için yeni proje iletişim kutusunu kullanın.](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Kullanarak bir durum bilgisi olmayan hizmet projesi oluşturup **.NET Core 2.0** adlı *HelloWorldStateless*:

![İkinci iletişim kutusunda, bir durum bilgisi olmayan hizmet projesi oluşturma](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

Çözümünüzün artık iki proje içermektedir:

* *HelloWorld*. Bu *uygulama* içeren proje, *Hizmetleri*. Ayrıca, bir dizi yardımıyla uygulamanızı dağıtmak için PowerShell Betiği yanı sıra uygulama açıklayan uygulama bildirimini içerir.
* *HelloWorldStateless*. Bu hizmet projedir. Bu durum bilgisi olmayan hizmet uygulaması içerir.

## <a name="implement-the-service"></a>Hizmet ekleme
Açık **HelloWorldStateless.cs** hizmet proje dosyasında. Service Fabric'te hizmet iş mantığı çalıştırabilirsiniz. Hizmet API'si, kodunuz için iki giriş noktası sağlar:

* Adlı bir açık uçlu bir giriş noktası yönteminin *RunAsync*, burada, uzun süre çalışan işlem iş yükleri de dahil olmak üzere tüm iş yükleri çalıştırma başlayabilirsiniz.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* Burada, ASP.NET Core gibi tercih ettiğiniz iletişim yığını takabilirsiniz iletişim giriş noktası. Kullanıcıların ve diğer hizmetlerden istekleri alma başlayabileceğiniz budur.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

Bu öğreticide, odaklanacağız `RunAsync()` giriş noktası yöntemi. Hemen kodunuzu çalıştıran başlayabileceğiniz budur.
Bir örnek uygulaması proje şablonu içerir `RunAsync()` , sıralı bir sayısını artırır.

> [!NOTE]
> Bir iletişim yığını ile çalışma hakkında daha fazla bilgi için bkz. [OWIN kendi kendine barındırma ile Service Fabric Web API Hizmetleri](service-fabric-reliable-services-communication-webapi.md)
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

        ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

Bir hizmet örneği yerleştirilmiş ve çalıştırılmaya hazır olduğunda platform bu yöntemi çağırır. Hizmet örneği açıldığında bir durum bilgisi olmayan hizmet için yalnızca anlamına gelir. Bir iptal belirteci, hizmet Örneğinizdeki kapatılması gerektiğinde koordine etmek için sağlanır. Service Fabric'te bu açma/kapatma döngüsü bir hizmet örneği üzerinde hizmet ömrünü birden çok kez bir bütün olarak ortaya çıkabilir. Bu durum çeşitli nedenlerle oluşabilir:

* Sistem hizmeti örnekleriniz için kaynak Dengeleme taşır.
* Kodunuzdaki hataları oluşur.
* Uygulama veya sistem yükseltilir.
* Temel alınan donanım kesinti karşılaşır.

Bu düzenleme yönetilen yüksek oranda kullanılabilir ve düzgün şekilde dengeli hizmetinizi korumak için sistem tarafından.

`RunAsync()` zaman uyumlu olarak Engellemesi gereken değil. Uygulamanıza RunAsync, bir görev döndürür veya devam etmek çalışma zamanı izin vermek için uzun süreli veya engelleme işlemleri üzerinde await gerekir. İçinde Not `while(true)` önceki örnekte, bir görev döndüren bir döngüde `await Task.Delay()` kullanılır. İş yükünüz zaman uyumlu olarak engellemeniz gerekir, yeni bir görev ile programlamalısınız `Task.Run()` içinde `RunAsync` uygulaması.

İş yükünüzün iptal tarafından sağlanan iptali belirtecinin düzenlenen işbirliği yapan bir işlemdir. Sistem (başarıyla tamamlandığında, iptal veya hata), taşınmadan önce sona erdirmek, görev bekler. İptal belirteci uymanız, işlerinizi tamamlamak ve çıkmak önemlidir `RunAsync()` sistem iptal isteğinde bulunduğunda mümkün olan en kısa sürede.

Bu durum bilgisi olmayan hizmet örneğinde, sayı yerel bir değişkende depolanır. Ancak bu durum bilgisi olmayan bir hizmet olduğundan, yalnızca geçerli yaşam döngüsünü, hizmet örneği için depolanan değeri yok. Hizmet taşır veya yeniden başlatır, değeri kaybolur.

## <a name="create-a-stateful-service"></a>Durum bilgisi olan hizmet oluşturma
Service Fabric durum bilgisi olan hizmeti yeni bir tür tanıtır. Durum bilgisi olan hizmet durumu içinde kullandıkları koduyla birlikte bulunan güvenilir bir şekilde hizmetin kendisini koruyabilir. Durumunu Service Fabric tarafından bir dış depolama durumu kalıcı hale getirmek zorunda kalmadan yüksek oranda kullanılabilir hale getirilir.

Hizmet taşır veya yeniden olsa bile sayaç değerini durum bilgisi olmayan yüksek oranda kullanılabilir kalıcı dönüştürmek için bir durum bilgisi olan hizmet gerekir.

Aynı *HelloWorld* uygulama ekleyebileceğiniz yeni bir hizmet uygulaması projesi başvuruları Services'ta sağ tıklatıp seçerek **Ekle -> Yeni Service Fabric hizmeti**.

![Service Fabric uygulamanızı hizmet ekleme](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

Seçin **.NET Core 2.0 ->, durum bilgisi olan hizmet** ve adlandırın *HelloWorldStateful*. **Tamam**'ı tıklatın.

![Yeni bir Service Fabric durum bilgisi olan hizmet oluşturmak için yeni proje iletişim kutusunu kullanın.](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Uygulamanız artık iki hizmet vardır: durum bilgisi olmayan hizmet *HelloWorldStateless* ve durum bilgisi olan hizmet *HelloWorldStateful*.

Bir durum bilgisi olan hizmeti durum bilgisi olmayan hizmet olarak aynı giriş noktaları içerir. Ana fark kullanılabilirliğini bir *durumu sağlayıcısı* , depolayabileceğiniz durumu güvenilir bir şekilde. Service Fabric adlı durumu sağlayıcısı uygulaması ile birlikte gelen [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md), çoğaltılan güvenilir durum Yöneticisi aracılığıyla veri yapıları, oluşturmanızı sağlar. Durum bilgisi olan güvenilir bir hizmet, varsayılan olarak bu durum sağlayıcısı kullanır.

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

            ServiceEventSource.Current.ServiceMessage(this.Context, "Current Counter Value: {0}",
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
`RunAsync()` durum bilgisi olan ve olmayan hizmetleri benzer şekilde çalışır. Yürütülmeden önce ancak durum bilgisi olan bir hizmet platform ek iş sizin adınıza gerçekleştirdiği `RunAsync()`. Bu iş, güvenilir durum Yöneticisi ve güvenilir koleksiyonlar, kullanıma hazır olan sağlama dahil olabilir.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Güvenilir koleksiyonlar ve güvenilir durum Yöneticisi
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) güvenilir hizmeti durumunu depolamak için kullanabileceğiniz bir sözlük uygulamasıdır. Service Fabric ve güvenilir koleksiyonlar, hizmetiniz bir dış kalıcı depoya gerek kalmadan doğrudan veri depolayabilirsiniz. Güvenilir koleksiyonlar, verilerinizi yüksek oranda kullanılabilir yap. Service Fabric gerçekleştirir, bu, oluşturma ve birden çok yönetme *çoğaltmaları* hizmetinizin sizin için. Ayrıca, yinelemeler ve bunların durumu geçişleri yönetme karmaşasından dengelediği bir API sağlar.

Güvenilir koleksiyonlar birkaç uyarılar, özel türler dahil olmak üzere, herhangi bir .NET türü depolayabilir:

* Service Fabric durumunuzu tarafından yüksek oranda kullanılabilir kılar *çoğaltma* durumunu düğüme ve güvenilir koleksiyonlar, her bir çoğaltma üzerinde verilerinizi yerel diske depolamak. Güvenilir koleksiyonlar halinde depolanır, her şeyin olması gerektiği anlamına gelir *seri hale getirilebilir*. Varsayılan olarak, Reliable Collections kullanımı [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) seri hale getirme için bu nedenle önemlidir, türlerinizi olduğundan emin olmak [veri sözleşme seri hale getirici tarafından desteklenen](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) kullandığınızda varsayılan serileştirici.
* Reliable Collections hareketlerde işlerseniz nesneleri yüksek kullanılabilirlik için çoğaltılır. Güvenilir koleksiyonlar depolanan nesnelere hizmetinizi yerel bellekte tutulur. Bu, yerel başvuru nesnesine sahip olduğunuz anlamına gelir.
  
   Bu nesne yerel örnekleri bir işlemde güvenilir koleksiyon bir güncelleştirme işlemini gerçekleştirmeden bulunmamalıdır değil, önemlidir. Yerel nesnelerin örneklerini değişiklikleri otomatik olarak çoğaltılmayacak olmasıdır. Nesneyi yeniden sözlük içine ekleyin veya birini *güncelleştirme* sözlük yöntemleri.

Reliable Collections güvenilir durum Yöneticisi sizin yerinize yönetir. Yalnızca güvenilir durum Yöneticisi için güvenilir bir koleksiyon adıyla dilediğiniz zaman ve herhangi bir yerdeki hizmetinizde sorabilirsiniz. Güvenilir durum Yöneticisi, bir başvuru geri almasını sağlar. Yoksa önerilir değişkenleri veya özellikleri sınıf üyesinde güvenilir koleksiyon örnekleri başvuruları kaydettiğinizden emin. Başvuru örneğine hizmet yaşam döngüsü içinde her zaman ayarlandığından emin olmak için özel dikkatli olunması gerekir. Güvenilir durum Yöneticisi bu işi sizin için yapar ve tekrar ziyaret için optimize edilmiştir.

### <a name="transactional-and-asynchronous-operations"></a>İşlem ve zaman uyumsuz işlemler
```csharp
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Güvenilir koleksiyonlar sahip aynı işlemlerinin birçoğu, bunların `System.Collections.Generic` ve `System.Collections.Concurrent` ortaklarınıza LINQ hariç. Reliable Collections işlemleri zaman uyumsuzdur. Güvenilir koleksiyonlar ile yazma işlemleri çoğaltın ve verileri diske kalıcı hale getirmek için g/ç işlemleri olmasıdır.

Güvenilir koleksiyon işlemleri *işlem*, böylece, durumu tutarlı birden çok güvenilir koleksiyonlar ve işlemleri tutabilirsiniz. Örneğin, bir iş öğesi güvenilir bir kuyruktan çıkarabileceğinizi bir işlem gerçekleştirmek ve güvenilir bir sözlük, tek bir işlem içinde sonuç kayıt. Bu bir atomik işlem olarak kabul edilir ve işlemin tamamı başarılı veya tüm işlemi geri alma garanti eder. Öğe sıradan çıkarma sonra bir hata oluşursa, ancak sonuç kaydetmeden önce işlemin tümü geri alınır ve öğe işleme kuyruğunda kalır.

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Biz artık dönmek *HelloWorld* uygulama. Artık, derleme ve hizmetlerinizi dağıtabilirsiniz. Bastığınızda **F5**, uygulamanızın yerleşik ve yerel kümenize dağıtılan.

Çalışan hizmetler başladıktan sonra oluşturulan olay izleme için Windows (ETW) olayları görüntüleyebileceğiniz bir **tanılama olayları** penceresi. Durum bilgisi olmayan hizmet ve durum bilgisi olan hizmeti uygulamasında görüntülenen olayları olduğunu unutmayın. Tıklayarak akış duraklatabilirsiniz **duraklatmak** düğmesi. Ardından, bu iletiyi genişleterek bir iletisinin ayrıntıları inceleyebilirsiniz.

> [!NOTE]
> Uygulamayı çalıştırmadan önce çalışan bir yerel geliştirme kümesine sahip olduğunuzdan emin olun. Kullanıma [Başlangıç Kılavuzu](service-fabric-get-started.md) yerel ortamınızı ayarlama hakkında bilgi.
> 
> 

![Visual Studio'da tanılama olaylarını görüntüle](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric uygulamanızı Visual Studio'da hata ayıklama](service-fabric-debugging-your-application.md)

[Kullanmaya başlayın: OWIN kendi kendine barındırma ile Service Fabric Web API Hizmetleri](service-fabric-reliable-services-communication-webapi.md)

[Güvenilir Koleksiyonlar hakkında daha fazla bilgi edinin](service-fabric-reliable-services-reliable-collections.md)

[Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[Uygulama yükseltme](service-fabric-application-upgrade.md)

[Reliable Services için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/dn706529.aspx)

