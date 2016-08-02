<properties 
    pageTitle="Service Bus geçişli mesajlaşma öğreticisi | Microsoft Azure"
    description="Service Bus geçişli mesajlaşma hizmetini kullanarak bir Service Bus istemci uygulaması ve hizmeti derleyin."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="05/17/2016"
    ms.author="sethm" />

# Service Bus geçişli mesajlaşma öğreticisi

Bu öğreticide, Service Bus "geçiş" özelliklerini kullanarak basit bir Service Bus istemci uygulamasının ve hizmetinin nasıl derleneceği açıklanır. Service Bus [aracılı mesajlaşma](service-bus-messaging-overview.md#Brokered-messaging) hizmetinin kullanıldığı benzer bir öğretici için [Service Bus Aracılı Mesajlaşama .NET öğreticisi](service-bus-brokered-tutorial-dotnet.md) başlığına bakın.

Bu öğreticide yapacağınız çalışmalar, Service Bus istemcisi ve hizmet uygulaması oluşturmak için gereken adımlarını anlamanıza olanak sağlar. WCF hizmetindeki benzerlerinde olduğu gibi, bir hizmet bir veya birden çok uç noktayı kullanıma sunan bir yapıdır. Aynı zamanda, uç noktaların her biri de bir veya daha fazla hizmet işlemini kullanıma sunar. Bir hizmetin uç noktası hizmetin bulunabileceği bir adres, istemcinin hizmetle iletişiminde paylaşması gereken bilgileri içeren bir bağlama ve hizmet tarafından istemcilerine sağlanan işlevselliği tanımlayan bir sözleşme belirtir. WCF ile Service Bus hizmeti arasındaki temel farklılık, uç noktanın yerel olarak bilgisayarınızda değil de bulutta kullanıma sunulmasıdır.

Bu öğreticideki konu başlıklarını sırasıyla anlayıp uyguladıktan sonra çalışan bir hizmetiniz ve hizmetin işlemlerini çağırabilen bir istemciniz olacaktır. İlk konu başlığında nasıl hesap ayarlanacağı açıklanmıştır. Sonraki adımlarda sözleşme kullanan bir hizmet tanımlama, hizmeti uygulama ve koddaki hizmeti yapılandırma açıklanır. Ayrıca, hizmeti çalıştırma ve barındırma da açıklanır. Oluşturan hizmet kendiliğinden barındırılır ve aynı bilgisayarda çalışır. Hizmeti bir kod veya yapılandırma dosyası kullanarak yapılandırabilirsiniz.

Son üç adımda istemci uygulaması oluşturma, istemci uygulamasını yapılandırma ve ana bilgisayarın işlevselliğine erişebilen bir istemci oluşturma ile bu istemciyi kullanma açıklanır.

Bu bölümdeki tüm konu başlıklarınde, geliştirme ortamı olarak Visual Studio'yu kullandığınız varsayılmıştır.

## Hesap için kaydolma

İlk adım bir Service Bus hizmeti ad alanı oluşturmak ve Paylaşılan Erişim İmzası (SAS) anahtarı edinmektir. Ad alanı, Service Bus tarafından kullanıma sunulan her uygulama için bir uygulama sınırı sağlar. Ad alanı ve SAS anahtarı birleşimi ile Service Bus hizmetinin bir uygulamaya erişim kimliğini doğrulayan kimlik bilgisi sağlanır.

1. Ad alanı oluşturmak için [klasik Azure portalını][] ziyaret edin. Sol taraftaki **Service Bus** hizmetine ve ardından **Oluştur**'a tıklayın. Ad alanınız için bir ad yazın, diğer değerler için varsayılanları kabul edin ve ardından Tamam onay işaretine tıklayın.

    >[AZURE.NOTE] İstemci ve hizmet uygulamalarının ikisi için de aynı ad alanını kullanmak zorunda değilsiniz.

    ![][4]

1. Portalın ana penceresinde, önceki adımda oluşturduğunuz ad alanı adına tıklayın.

2. Ad alanınıza yönelik varsayılan paylaşılan erişim ilkelerini ve anahtarlarını görüntülemek için **Yapılandır** sekmesine tıklayın.

    ![][1]

3. **RootManageSharedAccessKey** ilkesinin birincil anahtarını not edin veya panoya kopyalayın. Bu değeri daha sonra bu öğreticide kullanacaksınız.

## Service Bus ile kullanmak için bir WCF sözleşmesi tanımlayın.

Hizmet sözleşmesi, hizmetin hangi işlemleri (yöntemler ve işlevlere yönelik Web hizmeti terminolojisi) desteklediğini belirtir. Sözleşmeler; C++, C# veya Visual Basic arabirimi tanımlamasıyla oluşturulur. Arabirimdeki her yöntem belirli bir hizmet işlemine karşılık gelir. Her arabirimde [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) özniteliğinin ve her işlemde de [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) özniteliğinin uygulanmış olması gerekir. [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) özniteliğini içeren bir arabirimdeki yöntem [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) özniteliğine sahip değilse bu yöntem kullanıma sunulmaz. Bu görevlere ilişkin kod, aşağıdaki yordamın altındaki örnekte sağlanır. Sözleşmeler ve hizmetlere ilişkin daha fazla bilgi için WCF belgelerinde bulunan [Hizmetleri Tasarlama ve Uygulama](https://msdn.microsoft.com/library/ms729746.aspx) başlığına bakın.

### Arabirim içeren bir Service Bus sözleşmesi oluşturma

1. **Başlat** menüsünde programa sağ tıklayıp **Yönetici olarak çalıştır** seçeneğini belirleyip Visual Studio'yu yönetici olarak açın.

2. Yeni bir konsol uygulama projesi oluşturun. **Dosya** menüsüne tıklayın, **Yeni** seçeneği belirleyin ve ardından **Proje**'ye tıklayın. **Yeni Proje** iletişim kutusunda, **Visual C#** öğesine tıklayın (**Visual C#** görünmezse **Diğer Diller** bölümüne bakın). **Konsol Uygulaması** şablonuna tıklayıp **EchoService** olarak adlandırın. Projeyi oluşturmak için **Tamam**'a tıklayın.

    ![][2]

3. Service Bus NuGet paketini yükleyin. Bu paket otomatik olarak Service Bus kitaplıklarının yanı sıra WCF **System.ServiceModel** öğesine de başvurular ekler. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx), WCF'nin temel özelliklerine programlamayla erişmenizi sağlayan ad alanıdır. Service Bus, hizmet sözleşmelerini tanımlamak için WCF'nin birçok nesnesini ve özniteliklerini kullanır.

    Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'e tıklayın. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus` için arama yapın. **Sürüm(ler)** kutusunda proje adının seçili olduğundan emin olun. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.

    ![][3]

3. Çözüm Gezgini'nde, zaten açılmamışsa Program.cs dosyasına çift tıklayarak dosyayı düzenleyicide açın.

4. Aşağıdaki using deyimlerini dosyanın üst tarafına ekleyin:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Ad alanındaki varsayılan ad olan **EchoService**'i **Microsoft.ServiceBus.Samples** olarak değiştirin.

    >[AZURE.IMPORTANT] Bu öğretici, [WCF istemcisini yapılandır](#configure-the-wcf-client) adımındaki yapılandırma dosyasında kullanılan sözleşme yönetimli türün ad alanı olan C# **Microsoft.ServiceBus.Samples** ad alanını kullanır. Bu örneği derlemek istediğinizde herhangi bir ad alanını kullanabilirsiniz ancak uygulama yapılandırma dosyasında sözleşmenin ve hizmetin ad alanlarını uygun şekilde değiştirmezseniz bu öğretici çalışmaz. App.config dosyasında belirtilen ad alanının C# dosyalarınızda belirtilen ad alanıyla aynı olması gerekir.

1. `Microsoft.ServiceBus.Samples` ad alanı bildiriminden hemen sonra ancak yine de ad alanı içinde olacak şekilde `IEchoContract` adlı yeni arabirimi tanımlayın ve **http://samples.microsoft.com/ServiceModel/Relay/** ad alanı değerini içeren arabirime `ServiceContractAttribute` özniteliğini uygulayın. Kodunuzun kapsamında kullandığınız ad alanı ile ad alanı değeri farklılık gösterir. Ad alanı değeri bu sözleşme için benzersiz bir tanımlayıcı olarak kullanılır. Ad alanını açıkça belirlemek, varsayılan ad alanı değerinin sözleşme adına eklenmesini engeller.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Genellikle, hizmet sözleşmesi ad alanı sürüm bilgilerini barındıran bir adlandırma şeması içerir. Sürüm bilgilerini hizmet sözleşmesi ad alanına dahil etmek, hizmetlerin yeni bir ad alanı içeren yeni bir hizmet sözleşmesi tanımlayarak ve bu sözleşmeyi yeni bir uç noktada kullanıma sunarak büyük değişiklikleri yalıtmalarına olanak sağlar. Bu şekilde, istemciler güncelleştirmeye gerek kalmadan eski hizmet sözleşmelerini kullanmaya devam edebilir. Sürüm bilgileri, bir tarihten veya bir derleme numarasından oluşabilir. Daha fazla bilgi için bkz. [Hizmet Sürümü Oluşturma](http://go.microsoft.com/fwlink/?LinkID=180498). Bu öğreticinin amaçları doğrultusunda, hizmet sözleşmesi ad alanının adlandırma şeması sürüm bilgilerini içermez.

1. `IEchoContract` arabirimi içinde, `IEchoContract` sözleşmesinin arabirimde kullanıma sunduğu tek bir işlem için bir yöntem bildirin ve genel Service Bus sözleşmesinin bir parçası olarak kullanıma sunmak istediğiniz yönteme `OperationContractAttribute` özniteliğini uygulayın.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. `IEchoContract` arabirimi tanımından hemen sonra, aşağıda belirtildiği şekilde `IEchoContract` ve `IClientChannel` arabirimlerinden devralma işlemini gerçekleştiren bir kanal bildirin:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanal, ana bilgisayar ve istemcinin bilgileri birbirlerine göndermek için kullandıkları WCF nesnesidir. Daha sonra, iki uygulama arasındaki bilgileri yansıtmak üzere kanalda kod yazacaksınız.

1. Çalışmanızın o ana kadarki doğruluğunu onaylamak için **Derle** menüsünde **Çözümü Derle**'ye tıklayın veya **Ctrl+Shift+B**'ye basın.

### Örnek

Aşağıda verilen kodda, Service Bus sözleşmesini tanımlayan temel bir arabirim gösterilir.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
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

## Service Bus hizmetini kullanmak için WCF sözleşmesi uygulama

Service Bus geçişi oluşturmak için öncelikle bir arabirim kullanılarak tanımlanan sözleşmeyi oluşturmanız gerekir. Arabirimi oluşturma hakkında daha fazla bilgi edinmek için önceki adıma bakın. Bir sonraki adım ise bu arabirimi uygulamaktır. Bu adımda kullanıcı tanımlı `IEchoContract` arabirimini uygulayan `EchoService` adlı bir sınıf oluşturursunuz. Arabirimi uyguladıktan sonra, bir App.config yapılandırma dosyası kullanarak arabirimi yapılandırırsınız. Yapılandırma dosyasında hizmetin adı, sözleşmenin adı, Service Bus ile iletişim kurmak için kullanılan protokol türü gibi uygulama için gerekli bilgiler bulunur. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır. Hizmet sözleşmesini uygulama konusunda daha genel bilgiler için WCF belgelerinde [Hizmet Sözleşmelerini Uygulama](https://msdn.microsoft.com/library/ms733764.aspx) konusuna bakın.

1. `IEchoContract` arabiriminin tanımından hemen sonra `EchoService` adlı yeni bir sınıf oluşturun. `EchoService` sınıfı, `IEchoContract` arabirimini uygular. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Diğer arabirim uygulamalarına benzer şekilde, tanımı farklı bir dosyada uygulayabilirsiniz. Ancak bu öğreticide uygulama, arabirim tanımı ve `Main` yöntemiyle aynı dosyadadır.

1. `IEchoContract` arabirimine [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) özniteliğini uygulayın. Öznitelik, hizmet adını ve ad alanını belirtir. Bunu yaptıktan sonra `EchoService` sınıfı şu şekilde görünür:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. `EchoService` sınıfındaki `IEchoContract` arabiriminde tanımlanan `Echo` yöntemini uygulayın. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Çalışmanızın doğruluğunu onaylamak için **Derle** menüsünde **Çözümü Derle**'ye tıklayın.

### Hizmet ana bilgisayarı yapılandırmasını tanımlama

1. Yapılandırma dosyası, WCF yapılandırma dosyasına büyük ölçüde benzer. Hizmet adını, uç noktayı (Service Bus tarafından istemcilerin ve ana bilgisayarların birbirleriyle iletişim kurması için kullanıma sunulan konum) ve bağlamayı (iletişim kurmak için kullanılan protokol türü) içerir. Yapılandırma dosyasının temel farkı, bu durumda yapılandırılan hizmet uç noktasının .NET Framework'ün bir parçası olmayan [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) bağlamasına başvuru sağlamasıdır. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx), Service Bus tarafından tanımlanan bağlamalardan biridir.

1. **Çözüm Gezgini**'nde, App.config dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın.

2. `<appSettings>` öğesinde, yer tutucuları hizmet ad alanınızdaki adla ve önceki adımların birinde kopyaladığınız SAS anahtarı ile değiştirin. 

1. `<system.serviceModel>` etiketleri içinde, bir `<services>` öğesi ekleyin. Tek bir yapılandırma dosyasında birden çok Service Bus uygulaması tanımlayabilirsiniz. Ancak bu öğreticide yalnızca bir adet tanımlanır.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. `<services>` öğesi içinde, hizmetin adını tanımlamak için `<service>` öğesi ekleyin.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. `<service>` öğesi içinde, uç nokta sözleşmesinin konumunu ve uç noktaya yönelik bağlama türünü tanımlayın.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Uç nokta, istemcinin ana bilgisayar uygulamasını nerede arayacağını tanımlar. Öğreticinin daha sonraki kısımlarında bu adım, Service Bus aracılığıyla ana bilgisayarı tamamen kullanıma sunan bir URI oluşturmak için kullanılır. Bağlama, Service Bus ile iletişim kurmak için protokol olarak TCP kullandığımızı bildirir.

1. Çalışmanızın doğruluğunu onaylamak için **Derle** menüsüne ve **Çözümü Derle**'ye tıklayın.

### Örnek

Aşağıdaki kod, hizmet sözleşmesinin uygulamasını gösterir.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

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

```
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

## Service Bus'a kaydolmak için temel bir Web hizmeti barındırma ve çalıştırma

Bu adımda, temel bir Service Bus hizmetinin nasıl çalıştırılacağı açıklanır.

### Service Bus kimlik bilgilerini oluşturma

1. `Main()` içinde, konsol penceresinden okunan ad alanını ve SAS anahtarını depolayabileceğiniz iki değişken oluşturun.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    SAS anahtarı daha sonra Service Bus projenize erişmek için kullanılacaktır. Ad alanı, Hizmet URI'si oluşturmak için `CreateServiceUri` öğesine bir parametre olarak geçirilir.

4. [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) nesnesi kullanarak, kimlik bilgisi türü için SAS anahtarı kullanacağınızı bildirin. Aşağıdaki kodu son adımda eklenen koddan hemen sonra ekleyin.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### Hizmet için taban adresi oluşturma

1. Son adımda eklediğiniz koddan sonra hizmetin taban adresi için bir `Uri` örneği oluşturun. Bu URI; Service Bus şemasını, ad alanını ve hizmet arabiriminin yolunu belirtir.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "sb", Service Bus şeması için kullanılan bir kısaltmadır ve protokol olarak TCP'yi kullandığımızı belirtir. Ayrıca bu durum, [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) bağlama olarak sunulduğunda da yapılandırma dosyasında belirtilir.
    
    Bu öğretici için URI şudur: `sb://putServiceNamespaceHere.windows.net/EchoService`

### Hizmet ana bilgisayarını oluşturma ve yapılandırma

1. Bağlantı modunu `AutoDetect` olarak ayarlayın.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Bağlantı modu, hizmetin Service Bus ile iletişim kurmak için kullandığı protokolü açıklar (HTTP veya TCP). Hizmet `AutoDetect` varsayılan ayarı yoluyla , TCP kullanılabilir durumdaysa TCP, kullanılabilir durumda değilse HTTP üzerinden Service Bus ile bağlantı kurmayı dener. Bu durumun hizmetin istemci iletişimi için belirttiği protokole göre değişebileceğini unutmayın. Bu protokol, kullanılan bağlamaya göre belirlenir. Örneğin, bir hizmet istemcilerin HTTP üzerinden iletişim kurduğu uç noktayı (Service Bus hizmetinden kullanıma sunulur) belirten [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) bağlamasını kullanabilir. Aynı hizmet **ConnectivityMode.AutoDetect** bağlamasını da seçebilirdi. Bu durumda hizmet Service Bus ile TCP üzerinden iletişim kurar.

1. Bu bölümün önceki kısımlarında oluşturduğunuz URI'yi kullanarak hizmet ana bilgisayarını oluşturun.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Hizmet ana bilgisayarı, hizmetin örneğini oluşturan WCF nesnesidir. Burada, hizmet ana bilgisayarını oluşturmak istediğiniz hizmet türüne (bir `EchoService` türü) ve hizmeti kullanıma sunmak istediğiniz adrese geçirirsiniz.

1. Program.cs dosyasının üst kısmında, [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) ve [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx) öğelerine başvurular ekleyin.

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. `Main()` öğesine geri dönüp, genel erişimi etkinleştirmek için uç noktayı yapılandırın.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Bu adımda, uygulamanızın projenize yönelik ATOM beslemesi incelenerek herkes tarafından bulunabileceği Service Bus'a bildirilir. **DiscoveryType** kısmını **özel** olarak ayarlarsanız bir istemci yine de hizmete erişebilir. Ancak Service Bus ad alanı aratıldığında hizmet görünmez. Bunun yerine, istemcinin uç nokta yolunu önceden bilmesi gerekir.

1. App.config dosyasında açıklanan hizmet uç noktalarına hizmet kimlik bilgilerini uygulayın:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Bir önceki adımda da belirtildiği üzere, yapılandırma dosyasında birden çok hizmet ve uç nokta bildirebilirsiniz. Birden çok hizmet ve uç nokta bildirirseniz bu kod yapılandırma dosyasına çapraz geçiş yapar ve kimlik bilgilerinin uygulanacağı tüm uç noktalara yönelik arama yapar. Ancak bu öğreticide yapılandırma dosyasının yalnızca bir uç noktası vardır.

### Hizmet ana bilgisayarını açma

1. Hizmeti açın.

    ```
    host.Open();
    ```

1. Kullanıcıyı hizmetin çalıştığı konusunda bilgilendirin ve hizmetin nasıl kapatılacağını açıklayın.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Bu işlemi bitirdiğinizde, hizmet ana bilgisayarını kapatın.

    ```
    host.Close();
    ```

1. Projeyi derlemek için **Ctrl+Shift+B**'ye basın.

### Örnek

Aşağıdaki örnek, hizmet sözleşmesini ve bu öğreticinin önceki bölümlerinde yer alan uygulamayı içerir ve hizmeti bir konsol uygulamasında barındırır. Aşağıdakileri EchoService.exe adlı bir yürütülebilir dosya içinde derleyin.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
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

            // Add the Service Bus credentials to all endpoints specified in configuration.
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

## Hizmet sözleşmesi için bir WCF istemcisi oluşturma

Bir sonraki adım temel bir Service Bus istemci uygulaması oluşturmak ve sonraki adımlarda kullanacağınız hizmet sözleşmesini tanımlamaktır. Bu adımların birçoğunun hizmet oluşturmak için kullanılan adımlara benzer olduğunu unutmayın: sözleşme tanımlama, App.config dosyası düzenleme, Service Bus hizmetine bağlanmak için kimlik bilgileri kullanma vb. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

1. Aşağıdaki işlemleri gerçekleştirerek geçerli Visual Studio çözümünde istemci için yeni bir proje oluşturun:
    1. Çözüm Gezgini'nde, hizmetin barındıran çözümde bulunan geçerli çözüme (projeye değil) sağ tıklayın ve **Ekle**'ye tıklayın. Ardından **Yeni Proje**'ye tıklayın.
    2. **Yeni Proje Ekle** iletişim kutusunda, **Visual C#** öğesine tıklayıp (**Visual C#** görüntülenmiyorsa **Diğer Diller** kısmına bakın), **Konsol Uygulaması** şablonunu seçin, ardından **EchoClient** olarak adlandırın.
    3. **Tamam**'a tıklayın.
<br />

1. Çözüm Gezgini'nde, **EchoClient** projesindeki Program.cs dosyasına çift tıklayarak düzenleyicide açın (zaten açılmış durumda değilse).

1. `EchoClient` olan varsayılan ad alanı adını `Microsoft.ServiceBus.Samples` olarak değiştirin.

1. [Service Bus NuGet paketini](https://www.nuget.org/packages/WindowsAzure.ServiceBus) yükleyin. Çözüm Gezgini'nde, **EchoClient** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus` için arama yapın. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.

    ![][3]

1. Program.cs dosyasındaki [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) ad alanı için `using` deyimini ekleyin. 

    ```
    using System.ServiceModel;
    ```

1. Hizmet sözleşmesi tanımını ad alanına aşağıdaki örnekte gösterilen şekilde ekleyin. Bu tanımın **Service** projesinde kullanılan tanımla aynı olduğunu unutmayın. Bu kodu `Microsoft.ServiceBus.Samples` ad alanının üstüne eklemeniz gerekir.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. İstemciyi derlemek için **Ctrl+Shift+B**'ye basın.

### Örnek

Aşağıdaki kod EchoClient projesindeki Program.cs dosyasının geçerli durumunu gösterir.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
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

## WCF istemcisini yapılandırma

Bu adımda, daha önce bu öğreticide oluşturduğunuz hizmete erişen temel istemci uygulamasına yönelik bir App.config dosyası oluşturursunuz. Bu App.config dosyası sözleşmeyi, bağlamayı ve uç noktanın adını tanımlar. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

1. Çözüm Gezgini'nde, **EchoClient** projesinde bulunan **App.config** dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın.

2. `<appSettings>` öğesinde, yer tutucuları hizmet ad alanınızdaki adla ve önceki adımların birinde kopyaladığınız SAS anahtarı ile değiştirin.

1. system.serviceModel öğesi içinde `<client>` öğesini ekleyin.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Bu adım ile WCF stilinde istemci uygulaması tanımladığınızı bildirirsiniz.

1. `client` öğesi içinde adı, sözleşmeyi ve uç noktaya yönelik bağlama türünü tanımlayın.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Bu adımda uç noktanın adı, hizmette tanımlanan sözleşme ve istemci uygulamanın Service Bus ile iletişim kurmak için TCP kullandığı tanımlanır. Uç nokta adı, bir sonraki adımda bu uç nokta yapılandırmasını hizmet URI'si ile bağlamak için kullanılır.

1. **Dosya**'ya ve **Tümünü Kaydet**'e tıklayın.

## Örnek

Aşağıdaki kod, Echo istemciye yönelik App.config dosyasını gösterir.

```
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

## Service Bus hizmetini çağırmak için WCF istemcisini uygulama

Bu adımda, daha önce bu öğreticide oluşturduğunuz hizmete erişen bir temel istemci uygulaması yürütürsünüz. Hizmete benzer şekilde, istemci de Service Bus'a erişmek için aynı işlemlerin çoğunu gerçekleştir:

1. Bağlantı modunu ayarlar.
1. Ana bilgisayar hizmetinin yer aldığı URI'yi oluşturur.
1. Güvenlik kimlik bilgilerini tanımlar.
1. Bağlantıya kimlik bilgilerini uygular.
1. Bağlantıyı açar.
1. Uygulamaya özgü görevleri gerçekleştirir.
1. Bağlantıyı kapatır.

Temel farklardan biri, istemci uygulaması Service Bus'a bağlanmak için kanal kullanırken, hizmetin **ServiceHost** çağrısını gerçekleştirmesidir. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

### İstemci uygulamasını gerçekleştirme

1. Bağlantı modunu **AutoDetect** olarak ayarlayın. Aşağıdaki kodu **EchoClient** uygulamasının `Main()` yöntemine ekleyin.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Konsoldan okunan hizmet ad alanı ve SAS anahtarına yönelik değerleri tutması için değişkenleri tanımlayın.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Service Bus projenizdeki ana bilgisayarın konumunu tanımlayan URI'yi oluşturun.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Hizmet ad alanı uç noktanız için kimlik bilgileri nesnesini oluşturun.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. App.config dosyasında tanımlanan yapılandırmayı yükleyen kanal fabrikasını oluşturun.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Kanal fabrikası, hizmet ve istemcinin iletişim kurmak için kullandığı bir kanal oluşturan WCF nesnesidir.

1. Service Bus kimlik bilgilerini uygulama

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Kanalı oluşturup hizmet tarafından kullanılması için açın.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Yankı için işlevselliği ve temel kullanıcı arabirimini yazın.

    ```
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

1. Kanalı ve fabrikayı kapatın.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## Uygulamaları çalıştırmak için

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

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Hizmet uygulaması, aşağıdaki örnekte görüldüğü şekilde konsol penceresini dinlediği adrese yazdırır.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. **EchoClient** konsol penceresinde, az önce hizmet uygulaması için girdiğiniz bilgileri girin. İstemci uygulaması için aynı hizmet ad alanı ve SAS anahtarı değerlerini girmek üzere önceki adımları uygulayın.

11. Bu değerleri girdikten sonra istemci hizmete yönelik bir kanal açar ve aşağıdaki konsol çıktısı örneğinde görüldüğü şekilde bazı metinler girmenizi ister.

    `Enter text to echo (or [Enter] to exit):` 

    Hizmet uygulamasına gönderilecek metinleri girin ve Enter tuşuna basın. Bu metin, Echo hizmet işlemi aracılığıyla hizmete gönderilir ve aşağıdaki örnek çıktıda görüldüğü şekilde hizmet konsol penceresinde görünür.

    `Echoing: My sample text`

    İstemci uygulaması, `Echo` işleminin dönüş değerini, diğer bir deyişle orijinal metni alır ve konsol penceresine yazdırır. Aşağıda, istemci konsol penceresinden örnek çıktı verilmiştir.

    `Server echoed: My sample text`

12. Bu şekilde istemciden hizmete metin iletileri göndermeye devam edebilirsiniz. İşiniz bittiğinde her iki uygulamayı da sonlandırmak için istemci ve hizmet konsolu pencerelerinde Enter tuşuna basın.

## Örnek

Aşağıdaki örnekte istemci uygulaması oluşturma, hizmet işlemlerini çağırma ve işlem çağrısı sonlandıktan sonra istemciyi kapatma işlemlerinin nasıl gerçekleştirileceği gösterilir.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
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

## Sonraki adımlar

Bu öğreticide Service Bus "geçiş" özellikleri kullanılarak Service Bus istemci uygulamasının ve hizmetinin nasıl derleneceği gösterilmiştir. Service Bus [aracılı mesajlaşma](service-bus-messaging-overview.md#Brokered-messaging) hizmetini kullanan benzer bir öğretici için [Service Bus Aracılı Mesajlaşama .NET öğreticisi](service-bus-brokered-tutorial-dotnet.md) başlığına bakın.

Service Bus hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

- [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
- [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus mimarisi](service-bus-architecture.md)

[klasik Azure portalını]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png



<!---HONumber=Jun16_HO2-->


