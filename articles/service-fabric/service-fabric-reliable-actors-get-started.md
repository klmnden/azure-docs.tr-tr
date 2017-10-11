---
title: "İlk aktör tabanlı Azure mikro hizmet C# ' ta oluşturma | Microsoft Docs"
description: "Bu öğretici oluşturma, hata ayıklama ve Service Fabric Reliable Actors kullanarak basit bir aktör tabanlı hizmet dağıtma adımları açıklanmaktadır."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 3f447e049ccd33c77f422e8aa703ad6646f9ffa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-reliable-actors"></a>Reliable Actors ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-actors-get-started.md)
> * [Linux üzerinde Java](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Bu makalede, Azure Service Fabric Reliable Actors temelleri açıklanır ve oluşturma, hata ayıklama ve Visual Studio basit bir güvenilir aktör uygulama dağıtma size yol gösterir.

## <a name="installation-and-setup"></a>Yükleme ve Kurulum
Başlamadan önce makinenizde ayarlanan Service Fabric geliştirme ortamı olduğundan emin olun.
Ayrıntılı yönergeler ayarlamanız gerekiyorsa, bakın [geliştirme ortamını ayarlama konusunda](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Temel kavramlar
Reliable Actors ile çalışmaya başlamak için yalnızca birkaç temel kavramları anlamanız gerekir:

* **Aktör hizmeti**. Güvenilir aktörler Service Fabric altyapısında dağıtılabilir Reliable Services paketlenir. Aktör örnekleri adlı hizmet örneği içinde etkinleştirilir.
* **Aktör kayıt**. Olarak Reliable Services ile güvenilir aktör hizmeti Service Fabric çalışma zamanı ile kayıtlı olması gerekir. Ayrıca, aktör türü aktör çalışma zamanı ile kayıtlı olması gerekir.
* **Aktör arabirimi**. Aktör arabirimi bir aktör, türü kesin belirlenmiş bir ortak arabirimi tanımlamak için kullanılır. Güvenilir aktör modeli terminolojisinde aktör arabirimi aktör anlayabileceği iletileri ve işlem türlerini tanımlar. Aktör arabirimi "(uyumsuz) aktör göndermek için" diğer aktörler ve istemci uygulamalar tarafından kullanılır. Güvenilir aktörler birden çok arabirimi uygulayabilirsiniz.
* **ActorProxy sınıfı**. ActorProxy sınıfı aktör arabirimi aracılığıyla kullanıma sunulan yöntemleri çağırmak için istemci uygulamaları tarafından kullanılır. ActorProxy sınıfı iki önemli işlevleri sağlar:
  
  * Ad çözümlemesi: küme (Bul onu barındırıldığı küme düğümünün) aktör bulabilir.
  * Hata işleme: yöntem çağrılarına yeniden deneyin ve yeniden aktör konumu, örneğin, kümedeki başka bir düğüme yeniden konumlandırılmasını aktör gerektiren bir hatadan sonra çözümleyin.

Aktör arabirimlerine ilgilidir aşağıdaki kurallar tümleştirilmediği şunlardır:

* Aktör arabirim yöntemleri aşırı yüklenemez.
* Aktör arabirimi yöntemlerini çıkışı olmamalıdır, ref veya isteğe bağlı parametreler.
* Genel arabirimler desteklenmez.

## <a name="create-a-new-project-in-visual-studio"></a>Visual Studio'da yeni proje oluşturma
Visual Studio 2015 veya Visual Studio 2017 yönetici olarak başlatın ve yeni bir Service Fabric uygulaması projesi oluşturun:

![Visual Studio - yeni proje için Service Fabric araçları][1]

Sonraki iletişim kutusunda, oluşturmak istediğiniz proje türünü seçebilirsiniz.

![Service Fabric proje şablonları][5]

HelloWorld projesi için Service Fabric Reliable Actors hizmet kullanalım.

Çözüm oluşturduktan sonra aşağıdaki yapısını görmeniz gerekir:

![Service Fabric Proje yapısı][2]

## <a name="reliable-actors-basic-building-blocks"></a>Güvenilir aktörler temel yapı taşları
Tipik bir Reliable Actors çözümü üç projeyi oluşur:

* **Uygulama projesi (MyActorApplication)**. Bu, tüm hizmetlerin birlikte dağıtım paketleri projesidir. İçerdiği *ApplicationManifest.xml* ve uygulama yönetmek için PowerShell komut dosyaları.
* **Arabirim proje (MyActor.Interfaces)**. Aktör arabirim tanımı içeren projeye budur. MyActor.Interfaces projesinde çözümdeki aktör tarafından kullanılacak olan arabirimler tanımlayabilirsiniz. Aktör uygulaması tarafından paylaşılan aktör sözleşme arabirimi tanımlar ancak aktör arabirimlerinizi herhangi bir ad ile herhangi bir projeye tanımlanabilir ve aktör çağırma istemcileri, bu nedenle genellikle aktör uygulamasından ayrıdır ve diğer birden çok proje tarafından paylaşılan bir bütünleştirilmiş tanımlamak için mantıklıdır.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Aktör hizmeti projesi (MyActor)**. Aktör barındırmak için giderek Service Fabric hizmeti tanımlamak için kullanılan proje budur. Aktör uygulamasını içerir. Taban türünden türeyen bir sınıf bir aktör uygulamasıdır `Actor` ve MyActor.Interfaces projesinde tanımlanan arabirimlere uygulanır. Aktör sınıfı da kabul eden bir oluşturucu uygulamalıdır bir `ActorService` örneği ve bir `ActorId` ve tabanına geçirir `Actor` sınıfı. Bu oluşturucu bağımlılık ekleme platform bağımlılıklar için sağlar.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Aktör hizmeti Service Fabric çalışma zamanında bir hizmet türü ile kayıtlı olması gerekir. Aktör hizmetinin aktör örneklerinizi çalışması sırayla aktör türünüz aktör hizmeti ile kaydedilmelidir. `ActorRuntime` Kayıt yöntemi, bu iş aktörleri gerçekleştirir.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Visual Studio'da yeni bir proje başlangıç ve yalnızca bir aktör tanımı varsa, kayıt Visual Studio'nun oluşturduğu kodu varsayılan olarak dahil edilir. Diğer aktörler hizmetinde tanımlarsanız kullanarak aktör kayıt eklemeniz gerekir:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> Service Fabric aktör çalışma zamanı bazı yayar [olaylar ve performans sayaçları ilgili aktör yöntemler](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Bunlar, tanılama ve performans izlemesi kullanışlıdır.
> 
> 

## <a name="debugging"></a>Hata ayıklama
Visual Studio için Service Fabric araçları yerel makinenizde hata ayıklama desteği. F5 tuşuna basmak tarafından bir hata ayıklama oturumu başlatabilirsiniz. Derlemeler (gerekirse) Visual Studio paketler. Ayrıca, yerel Service Fabric kümesi uygulamayı dağıtır ve hata ayıklayıcı ekler.

Dağıtım işlemi sırasında devam eden görebilirsiniz **çıkış** penceresi.

![Service Fabric hata ayıklama çıktı penceresi][3]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Reliable Actors Service Fabric platformundan kullanma](service-fabric-reliable-actors-platform.md).

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
