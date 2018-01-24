---
title: "Bir web ön uç için ASP.NET Core kullanarak Azure Service Fabric uygulamanızı oluşturma | Microsoft Docs"
description: "Web Service Fabric uygulamanızı bir ASP.NET Core proje ve hizmet Remoting aracılığıyla hizmet içi iletişim kullanarak kullanıma sunar."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: d4f78c63117e5c54eb855178c75d6c294957f2a1
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Web hizmeti ön uç için ASP.NET Core kullanarak uygulamanızı oluşturun
Varsayılan olarak, Azure Service Fabric Hizmetleri Web ortak bir arabirim sağlamaz. HTTP istemcilere, uygulamanın işlevselliğini kullanıma sunmak için bir giriş noktası olarak davranmasına ve tek tek hizmetlerinizi buradan iletişim kurmak için bir web projesi oluşturmanız gerekir.

Bu öğreticide, biz ayrılacağı yeri biz de seçmek [Visual Studio'da ilk uygulamanızı oluşturma](service-fabric-create-your-first-application-in-visual-studio.md) öğretici ve ASP.NET Core web hizmetini durum bilgisi olan sayacı hizmetini önüne ekleyin. Zaten yapmadıysanız, geri dönün ve Bu öğreticide ilk adım gerekir.

## <a name="set-up-your-environment-for-aspnet-core"></a>ASP.NET Core için ortamınızı ayarlayın

Bu öğretici ile birlikte izlemek için Visual Studio 2017 gerekir. Herhangi bir sürümünü yapın. 

 - [Visual Studio 2017 yükleyin](https://www.visualstudio.com/)

ASP.NET Core Service Fabric uygulamaları geliştirmek için aşağıdaki iş yüklerini yüklü olmalıdır:
 - **Azure geliştirme** (altında *Web ve bulut*)
 - **ASP.NET ve web geliştirme** (altında *Web ve bulut*)
 - **.NET core platformlar arası geliştirme** (altında *diğer Toolsets*)

>[!Note] 
>Visual Studio 2015 kullanmaya devam ediyorsanız olması gerekiyor ancak .NET Core araçları Visual Studio 2015 için artık, güncelleştirilmekte olan [.NET Core VS 2015 Tooling Önizleme 2](https://www.microsoft.com/net/download/core) yüklü.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Uygulamanız için bir ASP.NET Core Hizmet Ekle
ASP.NET Core API web ve modern web kullanıcı Arabirimi oluşturmak için kullanabileceğiniz bir basit, platformlar arası web geliştirme altyapısıdır. 

Bir ASP.NET Web API projesi mevcut uygulamamız ekleyelim.

1. Çözüm Gezgini'nde sağ **Hizmetleri** uygulama içinde proje ve seçin **Ekle > Yeni yapı hizmeti**.
   
    ![Varolan bir uygulamaya yeni bir hizmet ekleme][vs-add-new-service]
2. Üzerinde **bir hizmet oluşturma** sayfasında, **ASP.NET Core** ve bir ad verin.
   
    ![ASP.NET web hizmeti yeni hizmet iletişim kutusunda seçme][vs-new-service-dialog]

3. Sonraki sayfanın bir dizi ASP.NET Core proje şablonları sağlar. Bu hizmet Service Fabric çalışma zamanı ile kaydetmek için ek kod küçük miktarda bir Service Fabric uygulaması dışında bir ASP.NET Core proje oluşturduysanız görür aynı seçenek olduğuna dikkat edin. Bu öğretici için seçin **Web API**. Ancak, tam web uygulamasını oluşturmak için aynı kavramları uygulayabilirsiniz.
   
    ![ASP.NET proje türü seçme][vs-new-aspnet-project-dialog]
   
    Web API projeniz oluşturulduktan sonra iki hizmet uygulamanızda olmalıdır. Uygulamanızı yapılandırmaya devam ederken, daha fazla hizmet tam olarak aynı şekilde ekleyebilirsiniz. Her bağımsız olarak sürümlü hem de yükseltilmiş olabilir.

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Biz ne yaptıklarını bir fikir almak için şimdi yeni uygulama dağıtın ve ASP.NET Core Web API şablonu sağlar varsayılan davranışı göz atın.

1. Uygulama hata ayıklamak için Visual Studio'da F5'e basın.
2. Dağıtım tamamlandığında, Visual Studio ASP.NET Web API hizmetinin kökü tarayıcıya başlatır. 404 hatası tarayıcıda görmeniz gerekir böylece ASP.NET çekirdek Web API şablon kökü için varsayılan davranış sağlamaz.
3. Ekleme `/api/values` tarayıcıda konumuna. Bu çağırır `Get` Web API şablonunda ValuesController yöntemi. Şablon--iki dizeyi içeren bir JSON dizisi tarafından sağlanan varsayılan yanıt verir:
   
    ![ASP.NET Core Web API şablondan döndürülen varsayılan değerler][browser-aspnet-template-values]
   
    Öğreticide sonuna kadar bu sayfayı durum bilgisi olan hizmetimizi varsayılan dizeler yerine en son sayaç değerini gösterir.

## <a name="connect-the-services"></a>Hizmetlere bağlanma
Service Fabric reliable services ile nasıl iletişim kuracağını tam esneklik sağlar. Tek bir uygulama içinde TCP, bir HTTP REST API üzerinden erişilebilen diğer hizmetler ve hala web yuvalarını erişilebilir diğer hizmetler aracılığıyla erişilebilen hizmetler olabilir. Kullanılabilir seçenekler ve söz konusu bileşim arka plan için bkz: [hizmetleriyle iletişim kurmasını](service-fabric-connect-and-communicate-with-services.md). Bu öğreticide, kullandığımız [Service Fabric hizmeti Remoting](service-fabric-reliable-services-communication-remoting.md), sağlanan SDK.

(Uzaktan yordam çağrısı veya RPC'ler Modellenen) hizmeti Remoting yaklaşım ortak sözleşme hizmeti olarak görev yapması için bir arabirim tanımlayın. Ardından, bu arabirim hizmetiyle etkileşim için bir proxy sınıfı oluşturmak için kullanın.

### <a name="create-the-remoting-interface"></a>Uzaktan iletişim arabirimi oluşturma
Durum bilgisi olan hizmeti ve diğer hizmetler arasında sözleşme olarak davranmak için bu durumda ASP.NET Core project web arabirimi oluşturarak başlayalım. Bu arabirim, kendi sınıf kitaplığı proje oluşturacağız RPC çağrısı, olmak için kullanan tüm hizmetleri tarafından paylaşılan gerekir.

1. Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve seçin **Ekle** > **yeni proje**.

2. Seçin **Visual C#** girişi seçin ve sol gezinti bölmesindeki **sınıf kitaplığı** şablonu. .NET Framework sürümünü ayarlandığından emin olun **4.5.2**.
   
    ![Durum bilgisi olan hizmetiniz için bir arabirim projesi oluşturma][vs-add-class-library-project]

3. Yükleme **Microsoft.ServiceFabric.Services.Remoting** NuGet paketi. Arama **Microsoft.ServiceFabric.Services.Remoting** NuGet Paket Yöneticisi ve hizmet Remoting kullanan tüm projeleri için yükleme de dahil olmak üzere:
   - Hizmet arabirimi içeren sınıf kitaplığı proje
   - Durum bilgisi olan hizmet projesi
   - ASP.NET Core web hizmeti projesi
   
    ![Hizmetleri NuGet paketi ekleme][vs-services-nuget-package]

4. Sınıf Kitaplığı'nda tek bir yönteme bir arabirim oluşturmak `GetCountAsync`, ve arabirimden genişletmek `Microsoft.ServiceFabric.Services.Remoting.IService`. Uzaktan iletişim arabirimi hizmet Remoting arabirimi olduğunu belirtmek için bu arabirimden türetilmesi gerekir.
   
    ```csharp
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-the-interface-in-your-stateful-service"></a>Durum bilgisi olan hizmetinizi arabirimini uygulama
Arabirimi tanımlamış olduğunuz, durum bilgisi olan hizmette uygulamanız gerekir.

1. Durum bilgisi olan hizmetinizi arabirimi içeren sınıf kitaplığı projesine bir başvuru ekleyin.
   
    ![Durum bilgisi olan hizmeti sınıf kitaplığı projesine bir başvuru ekleme][vs-add-class-library-reference]
2. Devraldığı sınıfı bulun `StatefulService`, gibi `MyStatefulService`, bunu uygulamak için genişletebilirsiniz `ICounter` arabirimi.
   
    ```csharp
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. Şimdi tanımlanan tek bir yöntem uygulamak `ICounter` arabirimi, `GetCountAsync`.
   
    ```csharp
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Bir hizmet remoting dinleyici kullanarak durum bilgisi olan hizmet kullanıma sunma
İle `ICounter` uygulanan arabirimi, son adımdır hizmet Remoting iletişim kanalı açmak için. Durum bilgisi olan hizmetler için Service Fabric adlı geçersiz kılınabilir bir yöntem sağlar. `CreateServiceReplicaListeners`. Bu yöntemde, hizmetiniz için etkinleştirmek istediğiniz iletişim türüne göre bir veya daha fazla iletişim dinleyicileri belirtebilirsiniz.

> [!NOTE]
> Durum bilgisi olmayan hizmetler için bir iletişim kanalı açmak için eşdeğer yöntemi çağrılır `CreateServiceInstanceListeners`.

Bu durumda, mevcut Değiştir `CreateServiceReplicaListeners` yöntemi ve örneği sağlayan `ServiceRemotingListener`, istemcilerden aranabilir, RPC bitiş noktası oluşturur `ServiceProxy`.  

`CreateServiceRemotingListener` Genişletme yöntemi `IService` arabirimi kolayca oluşturmanıza olanak sağlayan bir `ServiceRemotingListener` tüm varsayılan ayarlarla. Bu uzantı yöntemi kullanmak için olduğundan emin olun `Microsoft.ServiceFabric.Services.Remoting.Runtime` içeri aktarılan ad alanı. 

```csharp
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Kullanım hizmeti ile etkileşim kurmak için ServiceProxy sınıfı
Durum bilgisi olan hizmetimizi artık trafiği üzerinden RPC hizmetlerinden almak hazırdır. Bu nedenle tüm kalır ekleme ile ASP.NET web hizmeti ile iletişim kurmak için kodu.

1. ASP.NET projenizde içeren sınıf kitaplığına bir başvuru ekleyin `ICounter` arabirimi.

2. Daha önce eklediğiniz **Microsoft.ServiceFabric.Services.Remoting** ASP.NET projesi için NuGet paketi. Bu paket sağlar `ServiceProxy` RPC yapmak için kullanacağınız sınıfı için durum bilgisi olan hizmet çağırır. Bu NuGet paketi ASP.NET Core web hizmeti projesi yüklendiğinden emin olun.

4. İçinde **denetleyicileri** klasörü, açık `ValuesController` sınıfı. Unutmayın `Get` yöntemi şu anda yalnızca "değer1" ve "önceki tarayıcıda ne gördüğümüz eşleşen değer2"--sabit kodlanmış bir dize dizisi döndürür. Bu uygulama aşağıdaki kodla değiştirin:
   
    ```csharp
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    Kodun ilk satırı anahtar adrestir. Durum bilgisi olan hizmet ICounter proxy'sine oluşturmak için iki parça bilgi sağlamanız gerekir: bir bölüm kimliği ve hizmetin adı.
   
    Ölçek durum bilgisi olan hizmetlere durumlarına farklı demet içine oluşturan bir müşteri kimliği veya posta kodu gibi tanımlayan bir anahtara göre bölerek bölümleme kullanabilirsiniz. Önemsiz uygulamamız durum bilgisi olan hizmet yalnızca bir bölüm sahiptir, böylece anahtar önemli değildir. Sağladığınız herhangi bir tuşa aynı bölüme götürür. Hizmetleri bölümleme hakkında daha fazla bilgi için bkz: [bölüm Service Fabric Reliable Services nasıl](service-fabric-concepts-partitioning.md).
   
    Hizmet adı form doku URI'dir: /&lt;Uygulama_adı&gt;/&lt;servıce_name&gt;.
   
    Bu iki parça bilgi ile Service Fabric istekleri gönderilmesi gereken makine benzersiz şekilde tanımlayabilir. `ServiceProxy` Sınıfı, durum bilgisi olan hizmet bölüm barındıran makine başarısız olduğu ve başka bir makine yerini alacak yükseltilmelidir durum aynı zamanda sorunsuz bir şekilde işler. Bu soyutlama önemli ölçüde daha basit diğer hizmetlerle dağıtılacak istemci kod yazmayı kolaylaştırır.
   
    Biz proxy olduktan sonra biz yalnızca çağırma `GetCountAsync` yöntemi ve sonucunu döndürür.

5. Yeniden değiştirilmiş uygulamayı çalıştırmak için F5 tuşuna basın. Olarak daha önce Visual Studio otomatik olarak web projesi kökündeki tarayıcıya başlatır. "API/değerleri" yolu ekleyin ve döndürülen geçerli sayaç değeri görmeniz gerekir.
   
    ![Tarayıcıda görüntülenen durum bilgisi olan bir sayaç değeri][browser-aspnet-counter-value]
   
    Düzenli aralıklarla güncelleştirme sayaç değeri görmek için tarayıcıyı yenileyin.

## <a name="connecting-to-a-reliable-actor-service"></a>Bir güvenilir aktör hizmetine bağlanılıyor
Bu öğretici bir durum bilgisi olan hizmeti ile iletişim web ön ucu ekleme odaklanır. Ancak, aktörler için anlaşmak için çok benzer bir model izleyebilirsiniz. Bir güvenilir aktör projesi oluşturduğunuzda, Visual Studio, sizin için otomatik olarak bir arabirim projesi oluşturur. Bu arabirimi gerçekleştiren proxy aktör ile iletişim kurmak için web projesi oluşturmak için kullanabilirsiniz. Bir iletişim kanalı otomatik olarak sağlanır. Oluşturulmasında eşdeğer olan bir şey yapmanız gerekmez şekilde bir `ServiceRemotingListener` Bu öğreticide durum bilgisi olan hizmeti için yaptığınız gibi.

## <a name="how-web-services-work-on-your-local-cluster"></a>Web hizmetleri yerel kümenizde nasıl çalışır
Genel olarak, tam olarak aynı Service Fabric uygulamanızı yerel kümenizde dağıtılan çoklu makine bir küme dağıtmak ve beklendiği gibi çalıştığını yüksek oranda emin. Yerel kümenizdeki yalnızca tek bir makineye daraltılmış bir beş düğüm yapılandırması olmasıdır.

Web hizmetlerine gelir ancak olduğunda bir anahtar ayrıntı. Azure'da yaptığı gibi bir yük dengeleyicinin arkasına kümenizi bulunur, bu yana yük dengeleyici her makinede web hizmetlerinizi dağıtılan emin olmalısınız yalnızca hepsini-bir kez deneme trafik makinelerde. Bunu ayarlayarak yapabilirsiniz `InstanceCount` özel değere "-1" hizmeti için.

Bunun aksine, bir web hizmeti yerel olarak çalıştırdığınızda, yalnızca bir örneğini emin olmak gerektiğinde hizmeti çalışıyor. Aksi takdirde aynı yol ve bağlantı noktası üzerinde dinleme birden çok işlemlerden çakışmaları çalıştırın. Sonuç olarak, web hizmeti örnek sayısı "1" yerel dağıtımlar için ayarlamanız gerekir.

Farklı ortam için farklı değerler yapılandırma konusunda bilgi edinmek için [birden çok ortamlar için uygulama parametreleri yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar
ASP.NET Core uygulamanızla için kümesi sonlandırmak için daha fazla bilgi edinmek için bir web ön sahip olduğunuza [Service Fabric Reliable Services ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) ASP.NET Core Service Fabric ile nasıl çalıştığı bir derinlemesine anlamak için.

Ardından, [hizmetleriyle iletişim kurmasını hakkında daha fazla bilgi](service-fabric-connect-and-communicate-with-services.md) tamamı almak için genel olarak nasıl hizmet iletişimi Service Fabric çalışır resim.

Hizmet iletişimi nasıl çalıştığını, iyi anlamış olmanız sonra [bir küme oluşturmak ve uygulamanızı buluta dağıtmak](service-fabric-cluster-creation-via-portal.md).

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
