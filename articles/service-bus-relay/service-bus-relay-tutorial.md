---
title: Azure WCF geçişini kullanarak dış istemci için bir şirket içi WCF REST hizmeti kullanıma sunma | Microsoft Docs
description: WCF geçişi kullanarak bir istemci ve hizmet uygulaması oluşturun.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: db73363a05734db5d7e3375a5755a807eb7ce2a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60790087"
---
# <a name="expose-an-on-premises-wcf-rest-service-to-external-client-by-using-azure-wcf-relay"></a>Azure WCF geçişini kullanarak dış istemci için bir şirket içi WCF REST hizmeti kullanıma sunma

Bu öğreticide basit bir WCF geçişi istemci uygulaması ve Azure Geçişi'ni kullanarak hizmet oluşturma açıklanır. Kullanıldığı benzer bir öğretici için [Service Bus mesajlaşması](../service-bus-messaging/service-bus-messaging-overview.md), bkz: [Service Bus kuyrukları ile çalışmaya başlama](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

Bu öğreticide, bir WCF geçişi istemci ve hizmet uygulaması oluşturmak için gerekli olan adımları bir anlayış verir. Özgün WCF hizmetindeki benzerlerinde gibi her biri bir veya daha fazla hizmet işlemlerini kullanıma sunan bir veya daha fazla uç noktayı kullanıma sokan bir yapısı olan bir hizmettir. Bir hizmetin uç noktası hizmetin bulunabileceği bir adres, istemcinin hizmetle iletişiminde paylaşması gereken bilgileri içeren bir bağlama ve hizmet tarafından istemcilerine sağlanan işlevselliği tanımlayan bir sözleşme belirtir. WCF ve WCF geçişi arasındaki temel fark, uç nokta yerel olarak bilgisayarınızda yerine bulutta kullanıma sunulduğunu ' dir.

Bu öğreticideki konu başlıklarını sırasıyla anlayıp uyguladıktan sonra çalışan bir hizmetiniz ve hizmetin işlemlerini çağırabilen bir istemciniz olacaktır. İlk konu başlığında nasıl hesap ayarlanacağı açıklanmıştır. Sonraki adımlarda sözleşme kullanan bir hizmet tanımlama, hizmeti uygulama ve koddaki hizmeti yapılandırma açıklanır. Ayrıca, hizmeti çalıştırma ve barındırma da açıklanır. Oluşturan hizmet kendiliğinden barındırılır ve aynı bilgisayarda çalışır. Hizmeti bir kod veya yapılandırma dosyası kullanarak yapılandırabilirsiniz.

Son üç adımda istemci uygulaması oluşturma, istemci uygulamasını yapılandırma ve ana bilgisayarın işlevselliğine erişebilen bir istemci oluşturma ile bu istemciyi kullanma açıklanır.

Bu öğreticide aşağıdaki adımları uygulayın:

> [!div class="checklist"]
> * Bir geçiş ad alanı oluşturun.
> * Bir WCF Hizmeti sözleşmesi oluşturma
> * WCF sözleşmesi uygulama
> * Geçiş hizmeti ile kaydetmek için WCF Hizmeti barındırma ve çalıştırma
> * Hizmet sözleşmesi için bir WCF istemcisi oluşturma
> * WCF istemcisini yapılandırma
> * WCF istemcisini uygulama
> * Uygulamalar çalıştırın. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- [Visual Studio 2015 veya üzeri](https://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
- .NET için Azure SDK. Buradan yükleyin [SDK indirme sayfasını](https://azure.microsoft.com/downloads/).

## <a name="create-a-relay-namespace"></a>Bir geçiş ad alanı oluşturma
Ad alanı oluşturma ve edinmek için ilk adım olan bir [paylaşılan erişim imzası (SAS)](../service-bus-messaging/service-bus-sas.md) anahtarı. Bir ad alanı aracılığıyla geçiş hizmetine tarafından kullanıma sunulan her uygulama için bir uygulama sınırı sağlar. Hizmet ad alanı oluşturulduğunda sistem tarafından otomatik olarak bir SAS anahtarı oluşturulur. Hizmet ad alanı ve SAS anahtarı birleşimi, Azure'a bir uygulamaya erişim kimliğini doğrulayan kimlik bilgisi sağlanır.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract"></a>Bir WCF hizmet sözleşmesini tanımlama
Hizmet sözleşmesi hangi işlemleri belirtir (yöntemler ve işlevlere yönelik web hizmeti terminolojisi) hizmetini destekler. Sözleşmeler; C++, C# veya Visual Basic arabirimi tanımlamasıyla oluşturulur. Arabirimdeki her yöntem belirli bir hizmet işlemine karşılık gelir. Her arabirimde [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) özniteliğinin ve her işlemde de [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) özniteliğinin uygulanmış olması gerekir. [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) özniteliğini içeren bir arabirimdeki yöntem [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) özniteliğine sahip değilse bu yöntem kullanıma sunulmaz. Bu görevlere ilişkin kod, aşağıdaki yordamın altındaki örnekte sağlanır. Sözleşmeler ve hizmetlere ilişkin daha fazla bilgi için WCF belgelerinde bulunan [Hizmetleri Tasarlama ve Uygulama](https://msdn.microsoft.com/library/ms729746.aspx) başlığına bakın.

### <a name="create-a-relay-contract-with-an-interface"></a>Geçiş sözleşme ile bir arabirim oluşturma

1. **Başlat** menüsünde programa sağ tıklayıp **Yönetici olarak çalıştır** seçeneğini belirleyip Visual Studio'yu yönetici olarak açın.
2. Yeni bir konsol uygulama projesi oluşturun. **Dosya** menüsüne tıklayın, **Yeni** seçeneği belirleyin ve ardından **Proje**'ye tıklayın. **Yeni Proje** iletişim kutusunda, **Visual C#** öğesine tıklayın (**Visual C#** görünmezse **Diğer Diller** bölümüne bakın). Tıklayın **konsol uygulaması (.NET Framework)** şablonunu ve adlandırın **EchoService**. Projeyi oluşturmak için **Tamam**'a tıklayın.

    ![Konsol uygulaması oluşturma][2]

3. Service Bus NuGet paketini yükleyin. Bu paket otomatik olarak Service Bus kitaplıklarının yanı sıra WCF **System.ServiceModel** öğesine de başvurular ekler. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx), WCF'nin temel özelliklerine programlamayla erişmenizi sağlayan ad alanıdır. Service Bus, hizmet sözleşmelerini tanımlamak için WCF'nin birçok nesnesini ve özniteliklerini kullanır.

    Çözüm Gezgini'nde projeye sağ tıklayın ve ardından **NuGet paketlerini Yönet...** . Gözat sekmesine tıklayıp **WindowsAzure.ServiceBus** için arama yapın. **Sürüm(ler)** kutusunda proje adının seçili olduğundan emin olun. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.

    ![Service Bus paket][3]
4. Çözüm Gezgini'nde, zaten açılmamışsa Program.cs dosyasına çift tıklayarak dosyayı düzenleyicide açın.
5. Aşağıdaki using deyimlerini dosyanın üst tarafına ekleyin:

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. Ad alanındaki varsayılan ad olan **EchoService**'i **Microsoft.ServiceBus.Samples** olarak değiştirin.

   > [!IMPORTANT]
   > Bu öğreticide C# ad alanı **Microsoft.ServiceBus.Samples**, sözleşme tabanlı ad olduğu yönetilen yapılandırma dosyasında kullanılan türü [WCF istemcisini yapılandırma](#configure-the-wcf-client) adım. Bu örneği derlemek istediğinizde herhangi bir ad alanını kullanabilirsiniz ancak uygulama yapılandırma dosyasında sözleşmenin ve hizmetin ad alanlarını uygun şekilde değiştirmezseniz bu öğretici çalışmaz. App.config dosyasında belirtilen ad alanının C# dosyalarınızda belirtilen ad alanıyla aynı olması gerekir.
   >
   >
7. Metodundan hemen sonra `Microsoft.ServiceBus.Samples` ad alanı bildirimi, ancak ad alanı içinde adlı yeni arabirimi tanımlayın `IEchoContract` ve uygulama `ServiceContractAttribute` öznitelik ad alanı değerini içeren arabirime `https://samples.microsoft.com/ServiceModel/Relay/`. Kodunuzun kapsamında kullandığınız ad alanı ile ad alanı değeri farklılık gösterir. Ad alanı değeri bu sözleşme için benzersiz bir tanımlayıcı olarak kullanılır. Ad alanını açıkça belirlemek, varsayılan ad alanı değerinin sözleşme adına eklenmesini engeller. Ad alanı bildiriminden sonra aşağıdaki kodu yapıştırın:

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > Genellikle, hizmet sözleşmesi ad alanı sürüm bilgilerini barındıran bir adlandırma şeması içerir. Sürüm bilgilerini hizmet sözleşmesi ad alanına dahil etmek, hizmetlerin yeni bir ad alanı içeren yeni bir hizmet sözleşmesi tanımlayarak ve bu sözleşmeyi yeni bir uç noktada kullanıma sunarak büyük değişiklikleri yalıtmalarına olanak sağlar. Bu şekilde, istemcilerin güncelleştirilmesine gerek kalmadan eski hizmet sözleşmelerini kullanmaya devam edebilirsiniz. Sürüm bilgileri, bir tarihten veya bir derleme numarasından oluşabilir. Daha fazla bilgi için bkz. [Hizmet Sürümü Oluşturma](https://go.microsoft.com/fwlink/?LinkID=180498). Bu öğreticinin amaçları doğrultusunda, hizmet sözleşmesi ad alanının adlandırma şeması sürüm bilgilerini içermez.
   >
   >
8. İçinde `IEchoContract` arabirim, tek bir işlem için bir yöntem bildirin `IEchoContract` sözleşme arabirimde kullanıma sunduğu ve uygulama `OperationContractAttribute` gibi genel WCF geçişi sözleşmenin bir parçası olarak kullanıma sunmak istediğiniz yönteme öznitelik:

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. `IEchoContract` arabirimi tanımından hemen sonra, aşağıda belirtildiği şekilde `IEchoContract` ve `IClientChannel` arabirimlerinden devralma işlemini gerçekleştiren bir kanal bildirin:

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanal, ana bilgisayar ve istemcinin bilgileri birbirlerine göndermek için kullandıkları WCF nesnesidir. Daha sonra, iki uygulama arasındaki bilgileri yansıtmak üzere kanalda kod yazacaksınız.
10. Çalışmanızın o ana kadarki doğruluğunu onaylamak için **Derle** menüsünde **Çözümü Derle**'ye tıklayın veya **Ctrl+Shift+B**'ye basın.

### <a name="example"></a>Örnek

Aşağıdaki kod, bir WCF geçişi sözleşmesini tanımlayan temel bir arabirimi gösterir.

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Oluşturulması tamamlandığına göre arabirimi uygulayabilirsiniz.

## <a name="implement-the-wcf-contract"></a>WCF sözleşmesi uygulama

Bir Azure geçişi oluşturmak için öncelikle bir arabirim kullanılarak tanımlanan sözleşmeyi oluşturmanız gerekir. Arabirimi oluşturma hakkında daha fazla bilgi edinmek için önceki adıma bakın. Bir sonraki adım ise bu arabirimi uygulamaktır. Bu adımda kullanıcı tanımlı `IEchoContract` arabirimini uygulayan `EchoService` adlı bir sınıf oluşturursunuz. Arabirimi uyguladıktan sonra, bir App.config yapılandırma dosyası kullanarak arabirimi yapılandırırsınız. Yapılandırma dosyasında hizmetin adı, sözleşmeyi ve geçiş hizmeti ile iletişim kurmak için kullanılan protokol türü adı gibi bir uygulama için gerekli bilgileri içerir. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır. Hizmet sözleşmesini uygulama konusunda daha genel bilgiler için WCF belgelerinde [Hizmet Sözleşmelerini Uygulama](https://msdn.microsoft.com/library/ms733764.aspx) konusuna bakın.

1. `IEchoContract` arabiriminin tanımından hemen sonra `EchoService` adlı yeni bir sınıf oluşturun. `EchoService` sınıfı, `IEchoContract` arabirimini uygular.

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    Diğer arabirim uygulamalarına benzer şekilde, tanımı farklı bir dosyada uygulayabilirsiniz. Ancak bu öğreticide uygulama, arabirim tanımı ve `Main` yöntemiyle aynı dosyadadır.
2. `IEchoContract` arabirimine [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) özniteliğini uygulayın. Öznitelik, hizmet adını ve ad alanını belirtir. Bunu yaptıktan sonra `EchoService` sınıfı şu şekilde görünür:

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. `EchoService` sınıfındaki `IEchoContract` arabiriminde tanımlanan `Echo` yöntemini uygulayın.

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. Çalışmanızın doğruluğunu onaylamak için **Derle** menüsünde **Çözümü Derle**'ye tıklayın.

### <a name="define-the-configuration-for-the-service-host"></a>Hizmet ana bilgisayarı yapılandırmasını tanımlama

1. Yapılandırma dosyası, WCF yapılandırma dosyasına büyük ölçüde benzer. İçerdiği hizmet adını, uç noktayı (diğer bir deyişle, istemcilerin ve ana bilgisayarların birbirleriyle iletişim kurmak Azure geçişi sunan konum) ve bağlamayı (iletişim kurmak için kullanılan protokol türü). Yapılandırma dosyasının temel farkı, bu durumda yapılandırılan hizmet uç noktasının .NET Framework'ün bir parçası olmayan [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) bağlamasına başvuru sağlamasıdır. [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) hizmet tarafından tanımlanan bağlamalardan biridir.
2. **Çözüm Gezgini**'nde, App.config dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın.
3. `<appSettings>` öğesinde, yer tutucuları hizmet ad alanınızdaki adla ve önceki adımların birinde kopyaladığınız SAS anahtarı ile değiştirin.
4. `<system.serviceModel>` etiketleri içinde, bir `<services>` öğesi ekleyin. Bir tek bir yapılandırma dosyasında birden çok geçiş uygulama tanımlayabilirsiniz. Ancak bu öğreticide yalnızca bir adet tanımlanır.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. `<services>` öğesi içinde, hizmetin adını tanımlamak için `<service>` öğesi ekleyin.

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. `<service>` öğesi içinde, uç nokta sözleşmesinin konumunu ve uç noktaya yönelik bağlama türünü tanımlayın.

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Uç nokta, istemcinin ana bilgisayar uygulamasını nerede arayacağını tanımlar. Daha sonra öğreticide Bu adım tam olarak ana bilgisayar Azure geçişi üzerinden kullanıma sunan bir URI oluşturmak için kullanılır. Bağlama, TCP protokolü olarak geçiş hizmeti ile iletişim kurmak için kullanıyoruz olduğunu bildirir.
7. Çalışmanızın doğruluğunu onaylamak için **Derle** menüsüne ve **Çözümü Derle**'ye tıklayın.

### <a name="example"></a>Örnek

Aşağıdaki kod, hizmet sözleşmesinin uygulamasını gösterir.

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Aşağıdaki kod, hizmet ana bilgisayarı ile ilişkilendirilen App.config dosyasının temel biçimini gösterir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-the-wcf-service-to-register-with-the-relay-service"></a>Geçiş hizmeti ile kaydetmek için WCF Hizmeti barındırma ve çalıştırma

Bu adım, bir Azure geçişi hizmetini çalıştırma işlemi açıklanır.

### <a name="create-the-relay-credentials"></a>Geçiş kimlik bilgileri oluşturma

1. `Main()` içinde, konsol penceresinden okunan ad alanını ve SAS anahtarını depolayabileceğiniz iki değişken oluşturun.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    SAS anahtarı daha sonra projenize erişmek için kullanılır. Ad alanı, Hizmet URI'si oluşturmak için `CreateServiceUri` öğesine bir parametre olarak geçirilir.
2. [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) nesnesi kullanarak, kimlik bilgisi türü için SAS anahtarı kullanacağınızı bildirin. Aşağıdaki kodu son adımda eklenen koddan hemen sonra ekleyin.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-the-service"></a>Hizmetin taban adresi oluşturma

Son adımda eklediğiniz kodun hemen ardından, oluşturun bir `Uri` hizmetin taban adresine örneği. Bu URI; Service Bus şemasını, ad alanını ve hizmet arabiriminin yolunu belirtir.

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

"sb", Service Bus şeması için kullanılan bir kısaltmadır ve protokol olarak TCP'yi kullandığımızı belirtir. Ayrıca bu durum, [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) bağlama olarak sunulduğunda da yapılandırma dosyasında belirtilir.

Bu öğretici için URI şudur: `sb://putServiceNamespaceHere.windows.net/EchoService`

### <a name="create-and-configure-the-service-host"></a>Oluşturma ve hizmet ana bilgisayarı yapılandırma

1. Bağlantı modunu `AutoDetect` olarak ayarlayın.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Bağlantı modunu hizmet geçiş hizmeti ile iletişim kurmak için kullandığı protokolü açıklar; HTTP veya TCP. Varsayılan ayarı kullanarak `AutoDetect`, TCP kullanılabilir değilse, Azure geçişi kullanılabilir durumdaysa TCP ve HTTP üzerinden bağlanmak hizmet çalışır. Bu durumun hizmetin istemci iletişimi için belirttiği protokole göre değişebileceğini unutmayın. Bu protokol, kullanılan bağlamaya göre belirlenir. Örneğin, bir hizmeti kullanım [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) bitim istemciler HTTP üzerinden iletişim kurduğu bağlama. Aynı hizmet **ConnectivityMode.AutoDetect** böylece hizmet, Azure geçişi ile TCP üzerinden iletişim kurar.
2. Bu bölümün önceki kısımlarında oluşturduğunuz URI'yi kullanarak hizmet ana bilgisayarını oluşturun.

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Hizmet ana bilgisayarı, hizmetin örneğini oluşturan WCF nesnesidir. Burada, hizmet ana bilgisayarını oluşturmak istediğiniz hizmet türüne (bir `EchoService` türü) ve hizmeti kullanıma sunmak istediğiniz adrese geçirirsiniz.
3. Program.cs dosyasının üst kısmında, [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) ve [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description) öğelerine başvurular ekleyin.

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. `Main()` öğesine geri dönüp, genel erişimi etkinleştirmek için uç noktayı yapılandırın.

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Bu adım, uygulamanızın projenize yönelik ATOM inceleyerek genel olarak bulunabilir geçiş hizmeti bildirir. **DiscoveryType** kısmını **özel** olarak ayarlarsanız bir istemci yine de hizmete erişebilir. Ancak, geçiş ad alanı aratıldığında hizmet görünmez. Bunun yerine, istemcinin uç nokta yolunu önceden bilmesi gerekir.
5. App.config dosyasında açıklanan hizmet uç noktalarına hizmet kimlik bilgilerini uygulayın:

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Bir önceki adımda da belirtildiği üzere, yapılandırma dosyasında birden çok hizmet ve uç nokta bildirebilirsiniz. Birden çok hizmet ve uç nokta bildirirseniz bu kod yapılandırma dosyasına çapraz geçiş yapar ve kimlik bilgilerinin uygulanacağı tüm uç noktalara yönelik arama yapar. Ancak bu öğreticide yapılandırma dosyasının yalnızca bir uç noktası vardır.

### <a name="open-the-service-host"></a>Hizmet ana bilgisayarını açma

1. Hizmeti açın.

    ```csharp
    host.Open();
    ```
2. Kullanıcıyı hizmetin çalıştığı konusunda bilgilendirin ve hizmetin nasıl kapatılacağını açıklayın.

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. Bu işlemi bitirdiğinizde, hizmet ana bilgisayarını kapatın.

    ```csharp
    host.Close();
    ```
4. Projeyi derlemek için **Ctrl+Shift+B**'ye basın.

### <a name="example"></a>Örnek

Tamamlanmış hizmet kodunuzu aşağıdaki gibi görünmelidir. Kod, hizmet sözleşmesini ve önceki adımdan öğreticide içerir ve hizmeti bir konsol uygulamasında barındırır.

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Hizmet sözleşmesi için bir WCF istemcisi oluşturma

Sonraki adım, bir istemci uygulaması oluşturma ve sonraki adımları uygulayacaksınız hizmet sözleşmesini tanımlama oluşturmaktır. Bu adımların birçoğunun hizmet oluşturmak için kullanılan adımlara benzer olduğunu unutmayın: Sözleşme App.config düzenleme tanımlama alan geçiş hizmetine bağlanmak için kimlik bilgilerini kullanarak, dosya ve benzeri. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

1. Aşağıdaki işlemleri gerçekleştirerek geçerli Visual Studio çözümünde istemci için yeni bir proje oluşturun:

   1. Çözüm Gezgini'nde, hizmetin barındıran çözümde bulunan geçerli çözüme (projeye değil) sağ tıklayın ve **Ekle**'ye tıklayın. Ardından **Yeni Proje**'ye tıklayın.
   2. İçinde **Yeni Proje Ekle** iletişim kutusu, tıklayın **Visual C#** (varsa **Visual C#** görüntülenmiyorsa, altına bakın **diğer diller**) seçin **Konsol uygulaması (.NET Framework)** şablonunu ve adlandırın **EchoClient**.
   3. **Tamam** düğmesine tıklayın.
      <br />
2. Çözüm Gezgini'nde, **EchoClient** projesindeki Program.cs dosyasına çift tıklayarak düzenleyicide açın (zaten açılmış durumda değilse).
3. `EchoClient` olan varsayılan ad alanı adını `Microsoft.ServiceBus.Samples` olarak değiştirin.
4. Yükleme [Service Bus NuGet paketini](https://www.nuget.org/packages/WindowsAzure.ServiceBus): Çözüm Gezgini'nde sağ **EchoClient** proje ve ardından **NuGet paketlerini Yönet**. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus` için arama yapın. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.

    ![][3]
5. Program.cs dosyasındaki [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) ad alanı için `using` deyimini ekleyin.

    ```csharp
    using System.ServiceModel;
    ```
6. Hizmet sözleşmesi tanımını ad alanına aşağıdaki örnekte gösterilen şekilde ekleyin. Bu tanımın **Service** projesinde kullanılan tanımla aynı olduğunu unutmayın. Bu kodu `Microsoft.ServiceBus.Samples` ad alanının üstüne eklemeniz gerekir.

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. İstemciyi derlemek için **Ctrl+Shift+B**'ye basın.

### <a name="example"></a>Örnek

Aşağıdaki kodu Program.cs dosyasında geçerli durumunu gösterir **EchoClient** proje.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>WCF istemcisini yapılandırma

Bu adımda, daha önce bu öğreticide oluşturduğunuz hizmete erişen temel istemci uygulamasına yönelik bir App.config dosyası oluşturursunuz. Bu App.config dosyası sözleşmeyi, bağlamayı ve uç noktanın adını tanımlar. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

1. Çözüm Gezgini'nde, **EchoClient** projesinde bulunan **App.config** dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın.
2. `<appSettings>` öğesinde, yer tutucuları hizmet ad alanınızdaki adla ve önceki adımların birinde kopyaladığınız SAS anahtarı ile değiştirin.
3. system.serviceModel öğesi içinde `<client>` öğesini ekleyin.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Bu adım ile WCF stilinde istemci uygulaması tanımladığınızı bildirirsiniz.
4. `client` öğesi içinde adı, sözleşmeyi ve uç noktaya yönelik bağlama türünü tanımlayın.

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Bu adım, hizmet ve istemci uygulaması, Azure geçişi ile iletişim kurmak için TCP kullandığı olgu tanımlanan sözleşme bitiş noktası adını tanımlar. Uç nokta adı, bir sonraki adımda bu uç nokta yapılandırmasını hizmet URI'si ile bağlamak için kullanılır.
5. Tıklayın **dosya**, ardından **Tümünü Kaydet**.

### <a name="example"></a>Örnek

Aşağıdaki kod, Echo istemciye yönelik App.config dosyasını gösterir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client"></a>WCF istemcisini uygulama
Bu adımda, daha önce bu öğreticide oluşturduğunuz hizmete erişen bir temel istemci uygulaması yürütürsünüz. Hizmete benzer şekilde, istemci Azure geçişi erişmek için aynı işlemlerin çoğunu gerçekleştirir:

1. Bağlantı modunu ayarlar.
2. Ana bilgisayar hizmetinin yer aldığı URI'yi oluşturur.
3. Güvenlik kimlik bilgilerini tanımlar.
4. Bağlantıya kimlik bilgilerini uygular.
5. Bağlantıyı açar.
6. Uygulamaya özgü görevleri gerçekleştirir.
7. Bağlantıyı kapatır.

Ancak, bir çağrı hizmeti kullanır ancak istemci uygulamanın kanal alan geçiş hizmetine bağlanmak için kullandığı ana farklardan biri olan **ServiceHost**. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

### <a name="implement-a-client-application"></a>Bir istemci uygulama
1. Bağlantı modunu **AutoDetect** olarak ayarlayın. Aşağıdaki kodu **EchoClient** uygulamasının `Main()` yöntemine ekleyin.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. Konsoldan okunan hizmet ad alanı ve SAS anahtarına yönelik değerleri tutması için değişkenleri tanımlayın.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. Geçiş projenizdeki ana bilgisayarın konumunu tanımlayan URI'yi oluşturun.

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. Hizmet ad alanı uç noktanız için kimlik bilgileri nesnesini oluşturun.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. App.config dosyasında tanımlanan yapılandırmayı yükleyen kanal fabrikasını oluşturun.

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Kanal fabrikası, hizmet ve istemcinin iletişim kurmak için kullandığı bir kanal oluşturan WCF nesnesidir.
6. Kimlik bilgileri geçerli.

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. Kanalı oluşturup hizmet tarafından kullanılması için açın.

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. Yankı için işlevselliği ve temel kullanıcı arabirimini yazın.

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Kanal nesnesi örneğinin kod tarafından hizmete yönelik bir ara sunucu olarak kullanıldığını unutmayın.
9. Kanalı ve fabrikayı kapatın.

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

### <a name="example"></a>Örnek

Tamamlanan kodunuzu aşağıdaki gibi görünmelidir, istemci uygulamasının nasıl oluşturulacağını, hizmet işlemlerini çağırma ve işlem çağrısından sonra istemciyi kapatma işlemlerinin nasıl gösteren tamamlanmıştır.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

1. Çözümü derlemek için **Ctrl+Shift+B**'ye basın. Bu işlem, daha önceki adımlarda oluşturduğunuz istemci ve hizmet projelerini derler.
2. İstemci uygulamasını çalıştırmadan önce hizmet uygulamasının çalıştığından emin olmanız gerekir. Visual Studio'da bulunan Çözüm Gezgini'nde, **EchoService** çözümüne sağ tıklayın ve ardından **Özellikler**'e tıklayın.
3. Çözüm özellikleri iletişim kutusunda, **Başlangıç Projesi**'ne ve **Birden çok başlangıç projesi** düğmesine tıklayın. **EchoService** çözümünün liste başında olduğundan emin olun.
4. **EchoService** ve **EchoClient** projeleri için **Eylem** kutusunu **Başlat** olarak ayarlayın.

    ![][5]
5. **Proje Bağımlılıkları**'na tıklayın. **Projeler** kutusunda **EchoClient** projesini seçin. **Bağımlıdır** kutusunda **EchoService** projesinin seçildiğinden emin olun.

    ![][6]
6. **Özellikler** iletişim kutusunu kapatmak için **Tamam**'a tıklayın.
7. Her iki projeyi de çalıştırmak için **F5**'e basın.
8. Her iki konsol penceresi de açılır ve sizden ad alanı adı ister. Öncelikle hizmetin çalıştırılması gerekir. **EchoService** konsol penceresinde ad alanına ad girin ve **Enter** tuşuna basın.
9. Ardından SAS anahtarınız istenir. SAS anahtarını girin ve ENTER tuşuna basın.

    Konsol penceresinden örnek çıktı sunulur. Burada sağlanan değerlerin yalnızca örnek verme amacıyla oluşturulduğunu unutmayın.

    `Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`

    Hizmet uygulaması, aşağıdaki örnekte görüldüğü şekilde konsol penceresini dinlediği adrese yazdırır.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`
10. **EchoClient** konsol penceresinde, az önce hizmet uygulaması için girdiğiniz bilgileri girin. İstemci uygulaması için aynı hizmet ad alanı ve SAS anahtarı değerlerini girmek üzere önceki adımları uygulayın.
11. Bu değerleri girdikten sonra istemci hizmete yönelik bir kanal açar ve aşağıdaki konsol çıktısı örneğinde görüldüğü şekilde bazı metinler girmenizi ister.

    `Enter text to echo (or [Enter] to exit):`

    Hizmet uygulamasına gönderilecek metinleri girin ve Enter tuşuna basın. Bu metin, Echo hizmet işlemi aracılığıyla hizmete gönderilir ve aşağıdaki örnek çıktıda görüldüğü şekilde hizmet konsol penceresinde görünür.

    `Echoing: My sample text`

    İstemci uygulaması, `Echo` işleminin dönüş değerini, diğer bir deyişle orijinal metni alır ve konsol penceresine yazdırır. Aşağıda, istemci konsol penceresinden örnek çıktı verilmiştir.

    `Server echoed: My sample text`
12. Bu şekilde istemciden hizmete metin iletileri göndermeye devam edebilirsiniz. İşiniz bittiğinde her iki uygulamayı da sonlandırmak için istemci ve hizmet konsolu pencerelerinde Enter tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar
Şu öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Ağınızın dışından bir istemci için bir şirket içi WCF REST hizmeti kullanıma sunma](service-bus-relay-rest-tutorial.md)

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
