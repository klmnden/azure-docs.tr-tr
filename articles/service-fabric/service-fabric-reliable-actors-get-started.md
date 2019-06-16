---
title: Üzerindeki Azure Service Fabric aktör tabanlı bir hizmet oluşturun | Microsoft Docs
description: Oluşturma, hata ayıklama ve C# kullanarak Service Fabric Reliable Actors, ilk aktör tabanlı hizmet dağıtma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/16/2018
ms.author: vturecek
ms.openlocfilehash: b6ca4810d86bb3c8413f0a740ac4483a848b8e10
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60726391"
---
# <a name="getting-started-with-reliable-actors"></a>Reliable Actors hizmetini kullanmaya başlama
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-actors-get-started.md)
> * [Linux üzerinde Java](service-fabric-reliable-actors-get-started-java.md)

Bu makalede, oluşturma ve hata ayıklama Visual Studio'da basit bir Reliable Actor uygulaması gösterilmektedir. Reliable Actors hakkında daha fazla bilgi için bkz. [Service Fabric Reliable Actors hizmetine giriş](service-fabric-reliable-actors-introduction.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Visual Studio, makinenizde ayarlama dahil olmak üzere Service Fabric geliştirme ortamı olduğundan emin olun. Ayrıntılar için bkz [geliştirme ortamını ayarlama](service-fabric-get-started.md).

## <a name="create-a-new-project-in-visual-studio"></a>Visual Studio'da yeni proje oluşturma

Visual Studio 2015 veya üzeri bir yönetici olarak başlatın ve ardından yeni bir oluşturma **Service Fabric uygulaması** proje:

![Visual Studio - yeni proje için Service Fabric araçları][1]

Sonraki iletişim kutusunda **Actor hizmetinin** altında **.NET Core 2.0** ve hizmet için bir ad girin.

![Service Fabric projesi şablonları][5]

Oluşturulan proje aşağıdaki yapısını gösterir:

![Service Fabric Proje yapısı][2]

## <a name="examine-the-solution"></a>Çözüm inceleyin

Çözüm, üç projeler içeriyor:

* **Uygulama projesi (MyApplication)** . Bu projenin tüm hizmetlerin birlikte dağıtım paketleri. İçerdiği *ApplicationManifest.xml* ve uygulamayı yönetmek için PowerShell betikleri.

* **Arabirim projesi (HelloWorld.Interfaces)** . Bu proje, aktör arabirimi tanımını içerir. Aktör arabirimi, herhangi bir projede herhangi bir ad ile tanımlanabilir.  Arabirim aktör uygulaması ve aktörü çağıran istemciler tarafından paylaşılan aktör anlaşmasını tanımlar.  İstemci projeler üzerinde bağlı olabileceği için genellikle aktör uygulamasından ayrı bir derleme tanımlamak için mantıklıdır.

* **Aktör hizmeti projesini (HelloWorld)** . Bu proje, aktör barındırmak için giderek Service Fabric hizmeti tanımlar. Aktör uygulamasını içerir *: HelloWorld.cs'ye*. Aktör temel türünden türetilen bir sınıf uygulamasıdır `Actor` ve içinde tanımlanan arabirimlerini uygular *MyActor.Interfaces* proje. Aktör sınıfı da kabul eden bir oluşturucu uygulamalıdır bir `ActorService` örneği ve bir `ActorId` ve bunları tabanına geçirir `Actor` sınıfı.
    
    Bu proje de içeren *Program.cs*, yapan aktör sınıfları kullanarak Service Fabric çalışma zamanı `ActorRuntime.RegisterActorAsync<T>()`. `HelloWorld` Sınıf zaten kayıtlı. Projeye eklenen herhangi bir ek aktör uygulamaları da kayıtlı olması gerekir `Main()` yöntemi.

## <a name="customize-the-helloworld-actor"></a>HelloWorld aktör özelleştirme

Proje şablonu bazı yöntemlerini `IHelloWorld` arabirim ve bunları uygulayan `HelloWorld` aktör uygulaması.  Bu yöntemler, actor hizmetinin basit bir "Merhaba Dünya" dize döndürecek şekilde değiştirin.

İçinde *HelloWorld.Interfaces* içinde proje *IHelloWorld.cs* dosya, arabirim tanımı aşağıdaki gibi değiştirin:

```csharp
public interface IHelloWorld : IActor
{
    Task<string> GetHelloWorldAsync();
}
```

İçinde **HelloWorld** içinde proje **: HelloWorld.cs'ye**, tüm sınıf tanımına aşağıdaki gibi değiştirin:

```csharp
[StatePersistence(StatePersistence.Persisted)]
internal class HelloWorld : Actor, IHelloWorld
{
    public HelloWorld(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> GetHelloWorldAsync()
    {
        return Task.FromResult("Hello from my reliable actor!");
    }
}
```

Tuşuna **Ctrl-Shift-B** projeyi oluşturun ve her şeyi emin olmak için derler.

## <a name="add-a-client"></a>Bir istemci Ekle

Aktör hizmeti çağırmak için basit bir konsol uygulaması oluşturun.

1. Çözüm Gezgini'nde çözüme sağ tıklayın > **Ekle** > **yeni proje...** .

2. Altında **.NET Core** projesinin türleri **konsol uygulaması (.NET Core)** .  Projeyi adlandırın *ActorClient*.
    
    ![Yeni Proje iletişim kutusu Ekle][6]    
    
    > [!NOTE]
    > Bir konsol uygulaması, genellikle istemci Service Fabric olarak kullanacağınız uygulamanın türüne değil, ancak hata ayıklama ve yerel Service Fabric kümesi kullanarak test için kullanışlı bir örnek sağlar.

3. Konsol uygulaması arabirim projesi ve diğer bağımlılıklar uyumluluğu korumak için 64 bitlik bir uygulama olmalıdır.  Çözüm Gezgini'nde sağ **ActorClient** proje ve ardından **özellikleri**.  Üzerinde **derleme** sekmesinde, belirleyin **Platform hedefi** için **x64**.
    
    ![Derleme özellikleri][8]

4. İstemci projesi reliable actors NuGet paketini gerektirir.  **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’na tıklayın.  Paket Yöneticisi konsolunda aşağıdaki komutu girin:
    
    ```powershell
    Install-Package Microsoft.ServiceFabric.Actors -IncludePrerelease -ProjectName ActorClient
    ```

    ActorClient projesinde NuGet paketi ve tüm bağımlılıklar yüklenir.

5. İstemci projesi ayrıca arabirimleri projeye bir başvuru gerektirir.  ActorClient projeye sağ **bağımlılıkları** ve ardından **Başvurusu Ekle...** .  Seçin **projeleri > Çözüm** (seçili değilse) ve ardından onay kutusunun yanındaki işaretleyerek **HelloWorld.Interfaces**.  **Tamam** düğmesine tıklayın.
    
    ![Başvurusu Ekle iletişim kutusu][7]

6. Tüm içeriğini ActorClient projesinde değiştirin *Program.cs* aşağıdaki kod ile:
    
    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.ServiceFabric.Actors;
    using Microsoft.ServiceFabric.Actors.Client;
    using HelloWorld.Interfaces;
    
    namespace ActorClient
    {
        class Program
        {
            static void Main(string[] args)
            {
                IHelloWorld actor = ActorProxy.Create<IHelloWorld>(ActorId.CreateRandom(), new Uri("fabric:/MyApplication/HelloWorldActorService"));
                Task<string> retval = actor.GetHelloWorldAsync();
                Console.Write(retval.Result);
                Console.ReadLine();
            }
        }
    }
    ```

## <a name="running-and-debugging"></a>Çalıştırma ve hata ayıklama

Tuşuna **F5** derlemek, dağıtmak veya uygulamayı Service Fabric geliştirme kümesi yerel olarak çalıştırmak.  Dağıtım işlemi sırasında ilerleme durumunu görebilirsiniz **çıkış** penceresi.

![Service Fabric hata ayıklama çıkış penceresi][3]

Çıktı, metin içerdiğinde *uygulama hazır*, ActorClient uygulamasını kullanarak hizmeti test etmek mümkündür.  Çözüm Gezgini'nde sağ **ActorClient** proje'a tıklayın **hata ayıklama** > **yeni örnek Başlat**.  Komut satırı uygulaması actor hizmetinin çıktısını görüntülemelidir.

![Uygulama çıktısı][9]

> [!TIP]
> Service Fabric aktörleri çalışma zamanı bazı yayan [ilgili olaylar ve performans sayaçları, aktör yöntemlere](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Bunlar, tanılama ve performans izleme kullanışlıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Reliable Actors, Service Fabric platform kullanımını](service-fabric-reliable-actors-platform.md).


[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
[6]: ./media/service-fabric-reliable-actors-get-started/new-console-app.png
[7]: ./media/service-fabric-reliable-actors-get-started/add-reference.png
[8]: ./media/service-fabric-reliable-actors-get-started/build-props.png
[9]: ./media/service-fabric-reliable-actors-get-started/app-output.png