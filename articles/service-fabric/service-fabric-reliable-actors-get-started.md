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
ms.date: 11/20/2017
ms.author: vturecek
ms.openlocfilehash: ea17cf744779f390fe4b3f4049deb0c1ad985024
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="getting-started-with-reliable-actors"></a>Reliable Actors ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-actors-get-started.md)
> * [Linux üzerinde Java](service-fabric-reliable-actors-get-started-java.md)

Bu makalede oluşturma ve basit bir güvenilir aktör uygulaması Visual Studio'da hata ayıklama anlatılmaktadır. Reliable Actors hakkında daha fazla bilgi için bkz: [Service Fabric Reliable Actors giriş](service-fabric-reliable-actors-introduction.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Visual Studio, makinenizde ayarlama dahil olmak üzere Service Fabric geliştirme ortamı olduğundan emin olun. Ayrıntılar için bkz [geliştirme ortamını ayarlama konusunda](service-fabric-get-started.md).

## <a name="create-a-new-project-in-visual-studio"></a>Visual Studio'da yeni proje oluşturma

Visual Studio 2015 veya sonraki bir yönetici olarak başlatın ve ardından yeni bir **Service Fabric uygulaması** proje:

![Visual Studio - yeni proje için Service Fabric araçları][1]

Sonraki iletişim kutusunda seçin **aktör hizmeti** ve hizmet için bir ad girin.

![Service Fabric proje şablonları][5]

Oluşturulan proje aşağıdaki yapısını gösterir:

![Service Fabric Proje yapısı][2]

## <a name="examine-the-solution"></a>Çözüm inceleyin

Çözümü üç projeleri içerir:

* **Uygulama projesi (MyApplication)**. Bu proje tüm hizmetlerin birlikte dağıtım paketleri. İçerdiği *ApplicationManifest.xml* ve uygulama yönetmek için PowerShell komut dosyaları.

* **Arabirim proje (HelloWorld.Interfaces)**. Bu proje aktör arabirimi tanımını içerir. Aktör arabirimleri, herhangi bir ad ile herhangi bir projede tanımlanabilir.  Aktör uygulama ve aktör çağırma istemcileri tarafından paylaşılan aktör sözleşme arabirimi tanımlar.  İstemci projeleri bağımlı çünkü genellikle aktör uygulamasından farklı bir derlemede tanımlamak mantıklıdır.

* **Aktör hizmeti projesi (HelloWorld)**. Bu proje aktör barındırmak için giderek Service Fabric hizmeti tanımlar. Aktör uyarlamasını içeren *HellowWorld.cs*. Taban türünden türeyen bir sınıf bir aktör uygulamasıdır `Actor` ve içinde tanımlanan arabirimlerini uygular *MyActor.Interfaces* projesi. Aktör sınıfı da kabul eden bir oluşturucu uygulamalıdır bir `ActorService` örneği ve bir `ActorId` ve tabanına geçirir `Actor` sınıfı.
    
    Bu projede de içeren *Program.cs*, böylece aktör sınıflarını kullanarak Service Fabric çalışma zamanı `ActorRuntime.RegisterActorAsync<T>()`. `HelloWorld` Sınıf zaten kayıtlı. Projeye eklenen ek aktör belirtilmesinden ayrıca kaydedilmelidir `Main()` yöntemi.

## <a name="customize-the-helloworld-actor"></a>HelloWorld aktör özelleştirme

Proje şablonu bazı yöntemleri tanımlar `IHelloWorld` arabirim ve bunları uygulayan `HelloWorld` aktör uygulama.  Bu yöntemlerin aktör hizmeti basit bir "Hello World" dize döndürecek şekilde değiştirin.

İçinde *HelloWorld.Interfaces* içinde proje *IHelloWorld.cs* dosya, arabirim tanımı aşağıdaki gibi değiştirin:

```csharp
public interface IHelloWorld : IActor
{
    Task<string> GetHelloWorldAsync();
}
```

İçinde **HelloWorld** içinde proje **HelloWorld.cs**, tüm sınıf tanımını aşağıdaki gibi değiştirin:

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

## <a name="add-a-client"></a>Bir istemci ekleyin

Aktör hizmeti çağırmak için basit bir konsol uygulaması oluşturun.

1. Çözüm Gezgini'ndeki çözüme sağ tıklayın > **Ekle** > **yeni proje...** .

2. Altında **.NET Core** proje türleri, seçin **konsol uygulaması (.NET Core)**.  Proje adı *ActorClient*.
    
    ![Yeni Proje iletişim kutusu ekleme][6]    
    
    > [!NOTE]
    > Bir konsol uygulaması genellikle istemci Service Fabric olarak kullanacağınız uygulama türü değil, ancak hata ayıklama ve yerel Service Fabric kümesi kullanarak test etme için kullanışlı bir örnek sağlar.

3. Konsol uygulaması arabirimi proje ve başka bir bağımlılık uyumluluğu korumak için 64 bit bir uygulama olmalıdır.  Çözüm Gezgini'nde sağ **ActorClient** proje ve ardından **özellikleri**.  Üzerinde **yapı** sekmesinde, ayarlamak **Platform hedefi** için **x64**.
    
    ![Özellikleri oluşturma][8]

4. İstemci projesi güvenilir aktörler NuGet paketi gerektirir.  **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’na tıklayın.  Paket Yöneticisi konsolunda aşağıdaki komutu girin:
    
    ```powershell
    Install-Package Microsoft.ServiceFabric.Actors -IncludePrerelease -ProjectName ActorClient
    ```

    NuGet paketi ve tüm bağımlılıklarını ActorClient projesinde yüklenir.

5. İstemci projesi ayrıca arabirimleri projesine bir başvuru gerektirir.  ActorClient projeye sağ tıklayın **bağımlılıkları** ve ardından **Başvuru Ekle...** .  Seçin **projeleri > Çözüm** (seçili değilse) ve onay kutusunun yanındaki değer **HelloWorld.Interfaces**.  **Tamam**’a tıklayın.
    
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

## <a name="running-and-debugging"></a>Çalıştıran ve hata ayıklama

Tuşuna **F5** oluşturmak için dağıtmak ve Service Fabric geliştirme kümede uygulamayı yerel olarak çalıştırın.  Dağıtım işlemi sırasında devam eden görebilirsiniz **çıkış** penceresi.

![Service Fabric hata ayıklama çıktı penceresi][3]

Çıktı metin içerdiğinde *uygulama hazır*, ActorClient uygulaması kullanarak hizmeti test mümkündür.  Çözüm Gezgini'nde sağ tıklayın **ActorClient** proje ve ardından **hata ayıklama** > **başlangıç yeni örnek**.  Komut satırı uygulaması aktör hizmeti çıktısını görüntülemelidir.

![Uygulama çıktısı][9]

> [!TIP]
> Service Fabric aktör çalışma zamanı bazı yayar [olaylar ve performans sayaçları ilgili aktör yöntemler](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Bunlar, tanılama ve performans izlemesi kullanışlıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Reliable Actors Service Fabric platformundan kullanma](service-fabric-reliable-actors-platform.md).


[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
[6]: ./media/service-fabric-reliable-actors-get-started/new-console-app.png
[7]: ./media/service-fabric-reliable-actors-get-started/add-reference.png
[8]: ./media/service-fabric-reliable-actors-get-started/build-props.png
[9]: ./media/service-fabric-reliable-actors-get-started/app-output.png