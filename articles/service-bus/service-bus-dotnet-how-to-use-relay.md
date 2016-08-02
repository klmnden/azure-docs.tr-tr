<properties
    pageTitle=".NET içeren Service Bus geçişini kullanma | Microsoft Azure"
    description="Farklı konumlarda barındırılan iki uygulamayı bağlamak için Azure Service Bus geçişini kullanmayı öğrenin."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="05/06/2016"
    ms.author="sethm"/>


# Azure Service Bus geçişi hizmetini kullanma

Bu makale, Service Bus geçişi hizmetini kullanmayı açıklar. Bu örnekler, C# dilinde yazılmıştır ve Service Bus derlemesinde bulundurulan uzantıları içeren Windows Communication Foundation (WCF) API'sini kullanır. Service Bus geçişi hakkında daha fazla bilgi edinmek için [Service Bus geçişli mesajlaşmaya](service-bus-relay-overview.md) genel bakış bölümüne bakın.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## Service Bus geçişi nedir?

[Service Bus *geçişi* hizmeti](service-bus-relay-overview.md), hem Azure veri merkezinde hem de şirket içi kurumsal ortamınızda çalışan karma uygulamalar oluşturmanıza olanak sağlar. Service Bus geçişi bu işlemi, güvenlik duvarı bağlantısını açmanıza veya kurumsal ağ altyapısına müdahale eden değişiklikler yapmanıza gerek kalmadan kurumsal işletme ağında bulunan Windows Communication Foundation (WCF) hizmetlerini genel bulutta kullanıma sunmanızı sağlayarak gerçekleştirir.

![Geçiş Kavramları](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Service Bus geçişi, WCF hizmetlerini var olan kuruluş ortamınızda barındırmanıza olanak sağlar. Bu durumda, WCF hizmetlerine yönelik gelen oturum ve istekleri dinleme işlemini Azure'da çalışan Service Bus hizmetine devredebilirsiniz. Böylece hizmetleri Azure'da çalışan uygulama koduna, mobil çalışanlara veya extranet iş ortağı ortamlarına yönelik olarak kullanıma sunabilirsiniz. Service Bus, güvenli bir şekilde bu hizmetlere kimin erişebileceğini ayrıntılı olarak kontrol etmenize olanak sağlar. Var olan kuruluş çözümlerinizin verilerini ve uygulama işlevselliğini kullanmanız ve bunlardan bulutta faydalanmanız için güçlü ve güvenli bir yol sunar.

Bu makalede, iki taraf arasında güvenli bir konuşma sağlayan TCP kanal bağlaması kullanılarak kullanıma sunulan WCF web hizmeti oluşturmak için Service Bus geçişinin nasıl kullanılacağı gösterilir.

## Hizmet ad alanı oluşturma

Azure'da Service Bus geçişini kullanmaya başlamak için öncelikle bir ad alanı oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Hizmet ad alanı oluşturmak için:

1.  [Klasik Azure portalında][] oturum açın.

2.  Portalın sol gezinti bölmesinde **Service Bus** hizmetine tıklayın.

3.  Portalın alt bölmesinde **Oluştur**'a tıklayın.

    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-13.png)

4.  **Yeni bir ad alanı ekle** iletişim kutusunda, ad alanına ad girin.
    Adın kullanılabilirliği sistem tarafından hemen kontrol edilir.

    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-04.png)

5.  Ad alanındaki adın kullanılabilirliğinden emin olduktan sonra, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin (işlem kaynaklarınızın dağıtıldığı ülkeyle aynı ülkeyi/bölgeyi kullandığınızdan emin olun).

    > [AZURE.IMPORTANT] Uygulamanızın dağıtılması için seçmeyi planladığınız *aynı bölgeyi* seçin. Bunu uygulayarak en iyi performansı alırsınız.

6.  İletişim kutusundaki diğer alanları varsayılan değerlerinde bırakın (**Mesajlaşma** ve **Standart** katman), ardından onay işaretine tıklayın. Ardından sistem, ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-27.png)

    Ardından oluşturduğunuz ad alanı portalda görünür ve bir dakika içinde etkinleştirilir. Devam etmek için durumun **Etkin** olmasını bekleyin.

## Ad alanı için varsayılan yönetim kimlik bilgilerini elde etme

Yeni ad alanında geçiş bağlantısı oluşturmak gibi yönetim işlemlerini gerçekleştirmek için ad alanına yönelik Paylaşılan Erişim İmzası (SAS) kimlik doğrulama kuralını yapılandırmanız gerekir. SAS hakkında daha fazla bilgi için bkz. [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması][].

1.  Kullanılabilir ad alanlarının listesini görüntülemek için sol gezinti bölmesinde **Service Bus** düğümüne tıklayın.
    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-13.png)

2.  Görüntülenen listede az önce oluşturduğunuz ad alanı adına çift tıklayın.
    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-09.png)

3.  Sayfanın üst tarafındaki **Yapılandır** sekmesine tıklayın.

4.  Service Bus ad alanı sağlandığında, **RootManageSharedAccessKey** olarak ayarlanan **KeyName** içeren bir **SharedAccessAuthorizationRule** varsayılan olarak oluşturulur. Bu sayfada, oluşturulan anahtarın yanı sıra varsayılan kurala yönelik birincil ve ikincil anahtarlar da görüntülenir.

## Service Bus NuGet paketi alma

[Service Bus NuGet paketi](https://www.nuget.org/packages/WindowsAzure.ServiceBus), Service Bus API'sini almanın ve uygulamanızı tüm Service Bus bağımlılıklarıyla yapılandırmanın en kolay yoludur. Uygulamanıza NuGet paketini yüklemek için aşağıdakileri yapın:

1.  Çözüm Gezgini'nde, **Başvurular**'a sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
2.  "Service Bus" için arama yapın ve **Microsoft Azure Service Bus** öğesini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından aşağıdaki iletişim kutusunu kapatın.

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)


## TCP içeren bir SOAP web hizmetini kullanmak ve kullanıma sunmak için Service Bus hizmetini kullanma

Var olan WCF SOAP web hizmetini dışarıdan kullanıma açmak için hizmet bağlamalarında ve adreslerinde değişiklik yapmanız gerekir. Bu işlem, WCF hizmetlerinizi nasıl yapılandırdığınıza veya ayarladığınıza bağlı olarak yapılandırma dosyanızda ve kodda değişiklikler yapmanızı gerektirebilir. WCF aynı hizmet üzerinden birden çok ağ uç noktasına sahip olmanıza olanak sağladığından, dışarıdan erişim için Service Bus uç noktaları eklerken aynı anda var olan dahili uç noktalarını da koruyabileceğinizi unutmayın.

Bu görevde, basit bir WCF hizmeti oluşturup bu hizmete bir Service Bus dinleyicisi ekleyeceksiniz. Bu alıştırma Visual Studio'ya aşina olduğunuzu varsayar ve proje oluşturmanın tüm ayrıntılarını içermez. Bunun yerine, kod üzerine odaklanır.

Aşağıda yer alan adımlarla işe koyulmadan önce ortamınızı oluşturmak için aşağıdaki prosedürü tamamlayın:

1.  Visual Studio'da, çözüm içinde "İstemci" ve "Hizmet" olmak üzere iki proje içeren bir konsol uygulaması oluşturun.
2.  Her iki projeye de Microsoft Azure Service Bus NuGet paketini ekleyin. Bu işlem gerekli olan tüm derleme başvurularını projelerinize ekler.

### Hizmet oluşturma

Hizmetin kendisini oluşturmak ilk adımdır. Tüm WCF hizmetleri en az üç farklı bölümden oluşur:

-   Değiştirilen mesajları ve çağrılan işlemleri açıklayan sözleşme tanımı.
-   Geçerli sözleşmenin uygulaması.
-   WCF hizmetini barındıran ve birkaç uç noktayı kullanıma sokan ana bilgisayar.

Bu bölümdeki kod örnekleri, bu bileşenlerden tümüne yöneliktir.

Sözleşme, iki sayı ekleyen ve sonuç döndüren tek bir işlemi (`AddNumbers`) açıklar. `IProblemSolverChannel` arabirimi, istemcinin ara sunucu ömrünü kolayca yönetmesine olanak sağlar. Böyle bir arabirim oluşturmak en iyi uygulama olarak değerlendirilir. Hem "İstemci" hem de "Hizmet" projenizden sözleşme tanımına başvurabilmeniz için bu dosyayı farklı bir dosyaya koymak faydalıdır. Ayrıca, kodu her iki projeye de kopyalayabilirsiniz.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Sözleşme mevcutsa uygulama kısmı oldukça kolaydır.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### Hizmet ana bilgisayarını programlamayla yapılandırma

Sözleşme ve uygulama elverişli olduğunda artık hizmeti barındırabilirsiniz. Barındırma işlemi, hizmetin yönetim örneklerinin sorumluluğunu üstlenen ve iletiler için dinleme yapan uç noktalarını barındıran [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) nesnesinde gerçekleşir. Aşağıdaki kod, dahili ve dış uç noktaların yan yana görünmesini sağlamak için hizmeti hem bir Service Bus uç noktası hem de normal yerel uç noktayla birlikte yapılandırır. *yourKey* dizesini önceki kurulum adımında elde ettiğiniz SAS anahtarıyla ve *namespace* dizesini de ad alanındaki adla değiştirin.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "yourKey")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

Verilen örnekte, aynı sözleşme uygulamasında bulunan iki uç nokta oluşturursunuz. Biri yerel uç noktadır, diğeri ise Service Bus aracılığıyla yansıtılır. Bu iki uç nokta arasındaki temel farklılıklar bağlamalar ve adreslerdir. Yerel uç nokta için [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) ve Service Bus uç noktası için [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) bağlaması kullanılır. Yerel uç nokta, farklı bir bağlantı noktası içeren yerel ağ adresine sahiptir. Service Bus uç noktası ise `sb` dizesi, ad alanı adı ve "solver" yolundan oluşan bir uç nokta adresine sahiptir. Bu durum hizmet uç noktasını, tam olarak belirtilen dış DNS adını içeren Service Bus TCP uç noktası olarak tanımlayan URI `sb://[serviceNamespace].servicebus.windows.net/solver` ile sonuçlanır. **Hizmet** uygulamasının yukarıda açıklanan `Main` işlevinde yer tutucuları kaldırıp kodları yerleştirirseniz işlevsel bir hizmet elde edersiniz. Hizmetinizin yalnızca Service Bus hizmetinde dinleme yapmasını istiyorsanız yerel uç nokta bildirimini kaldırın.

### App.config dosyasında bir hizmet ana bilgisayarı yapılandırma

App.config dosyasını kullanarak da ana bilgisayarı yapılandırabilirsiniz. Bu durumda kullanılan hizmet barındırma kodu bir sonraki örnekte gösterilir.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Uç nokta tanımları App.config dosyasına taşınır. Service Bus hizmetine yönelik uzantıların yapılandırılması sırasında gerektiğinden, NuGet paketinin App.config dosyasına halihazırda bir dizi tanım eklediğini unutmayın. Önceki kodun birebir eş değeri olan aşağıdaki örnek, doğrudan **system.serviceModel** ögesinin altında görünmelidir. Bu kod örneği, projenizin C# ad alanının **Hizmet** olarak adlandırıldığını varsayar.
Yer tutucuları, Service Bus hizmeti ad alanınız ve SAS anahtarınızla değiştirin.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Bu değişiklikleri yaptıktan sonra, uygulama önceden olduğu gibi ancak biri yerel ve biri bulutta dinleyen olmak üzere iki dinamik uç noktayla birlikte başlar.

### İstemci oluşturma

#### Programlama ile istemci yapılandırma

Hizmeti kullanmak için [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) nesnesini kullanan bir WCF istemcisi oluşturabilirsiniz. Service Bus, SAS kullanılarak uygulanan belirteç tabanlı bir güvenlik modeli kullanır. [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) sınıfı, bazı iyi bilinen belirteç sağlayıcılarını döndüren, fabrikada yerleştirilen yöntemleri içeren bir güvenlik belirteci sağlayıcısı sunar. Aşağıdaki örnek, uygun SAS belirtecini edinmeyi ele alırken [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) yöntemini kullanır. Ad ve anahtar, önceki bölümde açıklanan şekilde portaldan alınanlardır.

İlk olarak, `IProblemSolver` sözleşmesi koduna hizmetten başvurun veya bu kodu hizmetten alarak istemci projenize kopyalayın.

Ardından, istemcinin `Main` yöntemindeki kodu değiştirin. Bu değiştirme işlemini yer tutucu metnini Service Bus ad alanı ve SAS anahtarıyla değiştirerek gerçekleştirin.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","yourKey") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Artık istemciyi ve hizmeti oluşturup çalıştırabilirsiniz (önce hizmeti çalıştırın) ve istemci hizmeti çağırıp **9** yazdırır. İstemciyi ve sunucuyu farklı makinelerde hatta farklı ağlarda bile çalıştırsanız konuşma yine de çalışır. Ayrıca, istemci kodu bulutta veya yerel olarak da çalışabilir.

#### App.config dosyasında istemci yapılandırma

Aşağıdaki kodda, App.config dosyasını kullanarak istemciyi nasıl yapılandıracağınız gösterilir.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Uç nokta tanımları App.config dosyasına taşınır. Önceden listelenen kodla aynı olan aşağıdaki örnek, doğrudan **system.serviceModel** ögesinin altında görünmelidir. Burada, önceden yaptığınız gibi yer tutucuları Service Bus ad alanınız ve SAS anahtarınızla değiştirmeniz gerekir.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## Sonraki adımlar

Artık Service Bus geçişi hizmetine ilişkin temel bilgileri öğrendiniz, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

- [Service Bus geçişli mesajlaşmaya genel bakış](service-bus-relay-overview.md)
- [Azure Service Bus mimarisine genel bakış](service-bus-fundamentals-hybrid-solutions.md)
- [Azure örneklerinden][] Service Bus örneklerini indirin veya [Service Bus örneklerine genel bakış][] bölümüne bakın.

  [Klasik Azure portalında]: http://manage.windowsazure.com
  [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması]: service-bus-shared-access-signature-authentication.md
  [Azure örneklerinden]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Service Bus örneklerine genel bakış]: service-bus-samples.md


<!---HONumber=Jun16_HO2-->


