---
title: Kullanmaya başlayın. NET'te Azure geçiş WCF geçişleri | Microsoft Docs
description: Azure geçişi WCF geçişleri farklı konumlarda barındırılan iki uygulamayı bağlamak için kullanmayı öğrenin.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 5493281a-c2e5-49f2-87ee-9d3ffb782c75
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/20/2017
ms.author: spelluru
ms.openlocfilehash: ee78227f645cbeded7a5c689750db835faf1055f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60420232"
---
# <a name="how-to-use-azure-relay-wcf-relays-with-net"></a>Geçişleri .NET ile Azure geçişi WCF kullanma
Bu makalede, Azure geçişi hizmetini kullanmayı açıklar. Bu örnekler, C# dilinde yazılmıştır ve Service Bus derlemesinde bulundurulan uzantıları içeren Windows Communication Foundation (WCF) API'sini kullanır. Azure geçişi hakkında daha fazla bilgi için bkz: [Azure geçişine genel bakış](relay-what-is-it.md).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a>WCF geçişi nedir?

Azure [ *WCF geçişi* ](relay-what-is-it.md) hizmeti, hem Azure veri merkezinde hem de şirket içi Kurumsal ortamınızda çalışan karma uygulamalar oluşturmanıza olanak sağlar. Geçiş hizmeti bu bir güvenlik duvarı bağlantısını açmanıza gerek kalmadan veya müdahale eden gerek olmadan genel bulut için Kurumsal işletme ağında bulunan Windows Communication Foundation (WCF) hizmetlerini kullanıma sunmanıza olanak sağlayarak gerçekleştirir. Kurumsal ağ altyapısına yapılan değişiklikler.

![WCF Geçişi Kavramları](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Azure geçişi, WCF hizmetlerini var olan Kuruluş ortamınızda barındırmak sağlar. Ardından gelen dinleme devredebilirsiniz oturumları ve istekleri bu WCF hizmetleri Azure'da çalışan geçiş hizmetine. Böylece hizmetleri Azure'da çalışan uygulama koduna, mobil çalışanlara veya extranet iş ortağı ortamlarına yönelik olarak kullanıma sunabilirsiniz. Geçiş, güvenli bir şekilde hizmetlerin ayrıntılı bir düzeyde erişebilen denetim sağlar. Var olan kuruluş çözümlerinizin verilerini ve uygulama işlevselliğini kullanmanız ve bunlardan bulutta faydalanmanız için güçlü ve güvenli bir yol sunar.

Bu makalede, Azure geçişi iki taraf arasında güvenli bir konuşma uygulayan bir TCP kanal bağlaması kullanılarak kullanıma sunulan bir WCF web hizmeti oluşturmak için nasıl kullanılacağı açıklanmaktadır.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Service Bus NuGet paketi alma
[Service Bus NuGet paketi](https://www.nuget.org/packages/WindowsAzure.ServiceBus), Service Bus API'sini almanın ve uygulamanızı tüm Service Bus bağımlılıklarıyla yapılandırmanın en kolay yoludur. NuGet paketini projenize yüklemek için aşağıdakileri yapın:

1. Çözüm Gezgini'nde, **Başvurular**'a sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
2. "Service Bus" için arama yapın ve **Microsoft Azure Service Bus** öğesini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından aşağıdaki iletişim kutusunu kapatın:
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a>Kullanımına sunun ve TCP ile bir SOAP web hizmetini kullanma
Var olan WCF SOAP web hizmetini dışarıdan kullanıma açmak için hizmet bağlamalarında ve adreslerinde değişiklik yapmanız gerekir. Bu işlem, WCF hizmetlerinizi nasıl yapılandırdığınıza veya ayarladığınıza bağlı olarak yapılandırma dosyanızda ve kodda değişiklikler yapmanızı gerektirebilir. WCF aynı hizmet üzerinden birden çok ağ uç noktaları aynı anda dış erişim için geçiş uç noktaları eklerken var olan iç Uç noktalara koruyabileceğinizi olmasını verdiğini unutmayın.

Bu görevde, basit bir WCF hizmeti oluşturma ve aktarma dinleyicisi ekleyeceksiniz. Bu alıştırma Visual Studio'ya aşina olduğunuzu varsayar ve proje oluşturmanın tüm ayrıntılarını içermez. Bunun yerine, kod üzerine odaklanır.

Bu adımlarla işe koyulmadan önce ortamınızı oluşturmak için aşağıdaki prosedürü tamamlayın:

1. Visual Studio'da, çözüm içinde "İstemci" ve "Hizmet" olmak üzere iki proje içeren bir konsol uygulaması oluşturun.
2. Service Bus NuGet paketine, her iki projeye ekleyin. Bu paket gerekli olan tüm derleme başvurularını projelerinize ekler.

### <a name="how-to-create-the-service"></a>Hizmet oluşturma
Hizmetin kendisini oluşturmak ilk adımdır. Tüm WCF hizmetleri en az üç farklı bölümden oluşur:

* Değiştirilen mesajları ve çağrılan işlemleri açıklayan sözleşme tanımı.
* Bu sözleşmenin uygulaması.
* WCF hizmetini barındıran ve birkaç uç noktayı kullanıma sokan ana bilgisayar.

Bu bölümdeki kod örnekleri, bu bileşenlerden tümüne yöneliktir.

Sözleşme, iki sayı ekleyen ve sonuç döndüren tek bir işlemi (`AddNumbers`) açıklar. `IProblemSolverChannel` arabirimi, istemcinin ara sunucu ömrünü kolayca yönetmesine olanak sağlar. Böyle bir arabirim oluşturmak en iyi uygulama olarak değerlendirilir. Hem "İstemci" hem de "Hizmet" projenizden sözleşme tanımına başvurabilmeniz için bu dosyayı farklı bir dosyaya koymak faydalıdır. Ayrıca, kodu her iki projeye de kopyalayabilirsiniz.

```csharp
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Sözleşme mevcutsa, uygulama aşağıdaki gibidir:

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Hizmet ana bilgisayarını programlamayla yapılandırma
Sözleşme ve uygulama elverişli olduğunda artık hizmeti barındırabilirsiniz. Barındırma işlemi, hizmetin yönetim örneklerinin sorumluluğunu üstlenen ve iletiler için dinleme yapan uç noktalarını barındıran [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) nesnesinde gerçekleşir. Aşağıdaki kod, normal yerel uç noktayla ve noktaların, iç ve dış uç yan yana görünmesini sağlamak için bir geçiş uç noktası ile hizmetini yapılandırır. *yourKey* dizesini önceki kurulum adımında elde ettiğiniz SAS anahtarıyla ve *namespace* dizesini de ad alanındaki adla değiştirin.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

Verilen örnekte, aynı sözleşme uygulamasında bulunan iki uç nokta oluşturursunuz. Yerel biridir ve bir Azure geçişi üzerinden gösterilmiyor. Arasındaki temel farklılıklar bağlamalar işlevleridir. [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) için yerel ve [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) geçiş uç noktası ve adresi. Yerel uç nokta, farklı bir bağlantı noktası içeren yerel ağ adresine sahiptir. Dizenin oluşan bir uç nokta adresi geçiş uç noktaya sahip `sb`, ad alanı adınızı ve "solver" yolu Bu URI sonuçları `sb://[serviceNamespace].servicebus.windows.net/solver`, hizmet uç noktası tam bir dış DNS adı ile bir Service Bus (geçiş) TCP uç noktası olarak tanımlayan. **Hizmet** uygulamasının `Main` işlevinde yer tutucuları kaldırıp kodları yerleştirirseniz işlevsel bir hizmet elde edersiniz. Özel olarak geçişi üzerinde dinlenecek hizmetinizi istiyorsanız yerel uç nokta bildirimini kaldırın.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>App.config dosyasında bir hizmet ana bilgisayarı yapılandırma
App.config dosyasını kullanarak da ana bilgisayarı yapılandırabilirsiniz. Bu durumda kullanılan hizmet barındırma kodu bir sonraki örnekte gösterilir.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Uç nokta tanımları App.config dosyasına taşınır. NuGet paketi, Azure geçişi için gerekli yapılandırma uzantıları App.config dosyasına bir dizi tanımı zaten eklemiştir. Önceki kodun birebir eş değeri olan aşağıdaki örnek, doğrudan **system.serviceModel** ögesinin altında görünmelidir. Bu kod örneği, projenizin C# ad alanının **Hizmet** olarak adlandırıldığını varsayar.
Yer tutucuları, geçiş ad alanı adını ve SAS anahtarınızla değiştirin.

```xml
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://<namespaceName>.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Bu değişiklikleri yaptıktan sonra, uygulama önceden olduğu gibi ancak biri yerel ve biri bulutta dinleyen olmak üzere iki dinamik uç noktayla birlikte başlar.

### <a name="create-the-client"></a>İstemci oluşturma
#### <a name="configure-a-client-programmatically"></a>Programlama ile istemci yapılandırma
Hizmeti kullanmak için [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) nesnesini kullanan bir WCF istemcisi oluşturabilirsiniz. Service Bus, SAS kullanılarak uygulanan belirteç tabanlı bir güvenlik modeli kullanır. [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) sınıfı, bazı iyi bilinen belirteç sağlayıcılarını döndüren, fabrikada yerleştirilen yöntemleri içeren bir güvenlik belirteci sağlayıcısı sunar. Aşağıdaki örnek, uygun SAS belirtecini edinmeyi ele alırken [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) yöntemini kullanır. Ad ve anahtar, önceki bölümde açıklanan şekilde portaldan alınanlardır.

İlk olarak, `IProblemSolver` sözleşmesi koduna hizmetten başvurun veya bu kodu hizmetten alarak istemci projenize kopyalayın.

Ardından, değiştirin `Main` yöntemi istemcisinin yeniden geçiş ad alanı ve SAS anahtarınızla yer tutucu metni değiştirme.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "<namespaceName>", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Artık istemciyi ve hizmeti oluşturup çalıştırabilirsiniz (önce hizmeti çalıştırın) ve istemci hizmeti çağırıp **9** yazdırır. İstemciyi ve sunucuyu farklı makinelerde hatta farklı ağlarda bile çalıştırsanız konuşma yine de çalışır. Ayrıca, istemci kodu bulutta veya yerel olarak da çalışabilir.

#### <a name="configure-a-client-in-the-appconfig-file"></a>App.config dosyasında istemci yapılandırma
Aşağıdaki kodda, App.config dosyasını kullanarak istemciyi nasıl yapılandıracağınız gösterilir.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Uç nokta tanımları App.config dosyasına taşınır. Daha önceden listelenen kodla aynı olan aşağıdaki örnek, doğrudan altında görünmelidir `<system.serviceModel>` öğesi. Burada, önceki örneklerde olduğu gibi yer tutucuları, geçiş ad alanı ve SAS anahtarınızla değiştirmeniz gerekir.

```xml
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://<namespaceName>.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Sonraki adımlar
Azure geçişi ile ilgili temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* [Azure Geçiş nedir?](relay-what-is-it.md)
* Service Bus örneklerini indirin [Azure örnekleri] [ Azure samples] veya [Service Bus örneklerine genel bakış][overview of Service Bus samples].

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
