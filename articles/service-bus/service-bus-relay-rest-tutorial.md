<properties
    pageTitle="Geçişli mesajlaşma kullanan Service Bus REST öğreticisi | Microsoft Azure"
    description="REST tabanlı bir arabirimi kullanıma sunan basit bir Service Bus geçişi ana bilgisayar uygulaması derleyin."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="06/03/2016"
    ms.author="sethm" />

# Service Bus REST öğreticisi

Bu öğretici, REST tabanlı bir arabirimi kullanıma sunan basit bir Service Bus ana bilgisayar uygulaması derlemeyi açıklar. REST, HTTP istekleri üzerinden Service Bus API'lerine erişmek için web tarayıcısı gibi bir web istemcisi sunar.

Bu öğreticide, Service Bus üzerinde bir REST hizmeti oluşturmak için Windows Communication Foundation (WCF) REST programlama modeli kullanılır. Daha fazla bilgi için WCF belgelerinde [WCF REST Programlama Modeli](https://msdn.microsoft.com/library/bb412169.aspx) ve [Hizmetleri Tasarlama ve Uygulama](https://msdn.microsoft.com/library/ms729746.aspx) konularına bakın.

## Adım 1: Hizmet ad alanı oluşturma

İlk adım, bir ad alanı oluşturup Paylaşılan Erişim İmzası (SAS) anahtarı almaktır. Ad alanı, Service Bus tarafından kullanıma sunulan her uygulama için bir uygulama sınırı sağlar. Hizmet ad alanı oluşturulduğunda sistem tarafından otomatik olarak bir SAS anahtarı oluşturulur. Hizmetin ad alanı ve SAS anahtarı birleşimi, uygulamaya erişim için kimlik doğrulamaya yönelik olarak Service Bus kimlik bilgilerini oluşturur.

1. Hizmet ad alanı oluşturmak için [klasik Azure portalını][] ziyaret edin. Sol taraftaki **Service Bus** hizmetine ve ardından **Oluştur**'a tıklayın. Ad alanınız için bir ad yazıp onay işaretine tıklayın.

2. [klasik Azure portalını][] ana penceresinde, önceki adımda oluşturduğunuz ad alanı adına tıklayın.

3. **Yapılandır** sekmesine tıklayın.

4. **Paylaşılan erişim anahtarı oluşturucu** bölümünde, **RootManageSharedAccessKey** ilkesiyle ilişkili **Ana Anahtar**'ı not edin veya panoya kopyalayın  Bu değeri daha sonra bu öğreticide kullanacaksınız.

## Adım 2:Service Bus ile kullanmak için bir REST tabanlı WCF hizmet sözleşmesini tanımlama

Diğer Service Bus hizmetlerinde olduğu gibi REST stilinde bir hizmet oluşturduğunuzda sözleşme tanımlamanız gerekir. Sözleşmede ana bilgisayarın hangi işlemleri desteklediği belirtilir. Bir hizmet işlemi, web hizmeti yöntemi olarak düşünülebilir. Sözleşmeler; C++, C# veya Visual Basic arabirimi tanımlamasıyla oluşturulur. Arabirimdeki her yöntem belirli bir hizmet işlemine karşılık gelir.  [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) özniteliğinin her arabirime ve [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) özniteliğinin her işleme uygulanması gerekir. Arabirimdeki bir yöntem [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) özniteliğine sahip olup [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) özniteliğine sahip olmazsa bu yöntem kullanıma sunulmaz. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte gösterilir.

Temel Service Bus sözleşmesi ve REST stili sözleşmesi arasındaki birincil fark, [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) özniteliğine özellik eklenmesidir:[WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Bu özellik sayesinde arabiriminizdeki bir yöntem ile arabirimin diğer tarafındaki bir yöntemi eşleyebilirsiniz. Bu durumda, HTTP GET işlemine bir yöntem bağlamak için [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) özniteliğini kullanacağız. Bu özellik, Service Bus'ın arabirime gönderilen komutları doğru şekilde almasını ve yorumlamasını sağlar.

### Arabirim içeren bir Service Bus sözleşmesi oluşturma

1. Visual Studio'yu yönetici olarak açın: **Başlangıç** menüsünde programa sağ tıklayıp **Yönetici olarak çalıştır**'a tıklayın.

2. Yeni bir konsol uygulama projesi oluşturun. **Dosya** menüsüne tıklayın ve **Yeni** seçeneğini belirleyip **Proje** seçimini yapın. **Yeni Proje** iletişim kutusunda **Visual C#** seçeneğine tıklayıp **Konsol Uygulaması** şablonuna tıklayın ve **ImageListener** olarak adlandırın. Varsayılan **Konum**'u kullanın. Projeyi oluşturmak için **Tamam**'a tıklayın.

3. Visual Studio, bir C# projesi için `Program.cs` dosyası oluşturur. Bu sınıf, bir konsol uygulaması projesinin doğru şekilde derlenmesi için gerekli olan boş `Main()` yöntemi içerir.

4. Service Bus hizmetine başvurular ekleyin ve Service Bus NuGet paketini yükleyerek projede **System.ServiceModel.dll** öğesini ekleyin. Bu paket otomatik olarak Service Bus kitaplıklarının yanı sıra WCF **System.ServiceModel** öğesine de başvurular ekler. Çözüm Gezgini'nde **ImageListener** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus` için arama yapın. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.

5. Projede **System.ServiceModel.Web.dll** öğesine açık olarak bir başvuru eklemeniz gerekir:

    a. Çözüm Gezgini'nde, **Başvurular** klasörüne sağ tıklayın ve proje klasöründe **Başvuru Ekle**'ye tıklayın.

    b. **Başvuru Ekle** iletişim kutusunda, sol taraftaki **Altyapı** sekmesine tıklayın ve **Ara** kutusuna **System.ServiceModel.Web** yazın. **System.ServiceModel.Web** öğesinin onay kutusunu işaretleyin ve **Tamam**'a tıklayın.

6. Aşağıdaki `using` deyimlerini Program.cs dosyasının üstüne ekleyin.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx), WCF'nin temel özelliklerine programlama erişimi sağlayan ad alanıdır. Service Bus, hizmet sözleşmelerini tanımlamak için WCF'nin birçok nesnesini ve özniteliklerini kullanır. Service Bus geçiş uygulamalarınızın çoğunda bu ad alanını kullanacaksınız. Benzer şekilde [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) ise Service Bus ve istemci web tarayıcısı ile iletişim kurduğunuz aracı nesne olan kanalı tanımlamaya yardım eder. Son olarak [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx), web tabanlı uygulamalar oluşturmanıza olanak sağlayan türleri içerir.

7. `ImageListener` ad alanını **Microsoft.ServiceBus.Samples** olarak yeniden adlandırın.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Ad alanı bildiriminin büyük ayracını açtıktan hemen sonra **IImageContract** adlı yeni bir arabirim tanımlayın ve `http://samples.microsoft.com/ServiceModel/Relay/` değerini içeren arabirime **ServiceContractAttribute** özniteliğini uygulayın. Kodunuzun kapsamında kullandığınız ad alanı ile ad alanı değeri farklılık gösterir. Ad alanı değeri bu sözleşme için benzersiz bir tanımlayıcı olarak kullanılır ve sürüm bilgisini içermelidir. Daha fazla bilgi için bkz. [Hizmet Sürümü Oluşturma](http://go.microsoft.com/fwlink/?LinkID=180498). Ad alanını açıkça belirlemek, varsayılan ad alanı değerinin sözleşme adına eklenmesini engeller.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. `IImageContract` arabirimi içinde, `IImageContract` sözleşmesinin arabirimde kullanıma sunduğu tek bir işlem için bir yöntem bildirin ve genel Service Bus sözleşmesinin bir parçası olarak kullanıma sunmak istediğiniz yönteme `OperationContractAttribute` özniteliğini uygulayın.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. **OperationContract** özniteliği içinde **WebGet** değerini ekleyin.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Bu işlemi gerçekleştirmek, Service Bus hizmetinin HTTP GET isteklerini `GetImage` öğesine yönlendirmesine ve `GetImage` dönüş değerini HTTP GETRESPONSE yanıtına çevirmesine olanak sağlar. Bu öğreticinin sonraki bölümlerinde, bu yönteme erişmek ve tarayıcıdaki görüntüyü görüntülemek için bir web tarayıcısı kullanacaksınız.

11. `IImageContract` tanımından hemen sonra, `IImageContract` ve `IClientChannel` arabirimlerinden devralma işlemini gerçekleştiren bir kanal bildirin.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Kanal, hizmet ve istemcilerin bilgileri birbirlerine göndermek için kullandıkları WCF nesnesidir. Daha sonra, ana bilgisayar uygulamanızda kanal oluşturacaksınız. Ardından, Service Bus bu kanalı HTTP GET isteklerini **GetImage** uygulamanızdan tarayıcıya geçirmek için kullanır. Ayrıca kanal, Service Bus tarafından **GetImage** dönüş değerini alıp istemci tarayıcısı için HTTP GETRESPONSE olarak çevirmek için kullanılır.

12. Çalışmanızın o ana kadarki doğruluğunu onaylamak için **Derle** menüsünde **Çözümü Derle**'ye tıklayın.

### Örnek

Aşağıda verilen kodda, Service Bus sözleşmesini tanımlayan temel bir arabirim gösterilir.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## Adım 3:Service Bus hizmetini kullanmak için REST tabanlı WCF hizmeti sözleşmesi uygulama

REST stilinde Service Bus hizmeti oluşturmak için öncelikle bir arabirim tarafından tanımlanan sözleşmeyi oluşturmanız gerekir. Bir sonraki adım ise bu arabirimi uygulamaktır. Bu adımda kullanıcı tanımlı **IImageContract** arabirimini uygulayan **ImageService** adlı bir sınıf oluşturursunuz. Sözleşmeyi uyguladıktan sonra, App.config dosyası kullanarak arabirimi yapılandırırsınız. Yapılandırma dosyasında hizmetin adı, sözleşmenin adı, Service Bus ile iletişim kurmak için kullanılan protokol türü gibi uygulama için gerekli bilgiler bulunur. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

Önceki adımlara benzer şekilde, REST stilinde sözleşme ve temel Service Bus sözleşmesi uygulama arasında çok küçük farklar vardır.

### REST stilinde Service Bus sözleşmesini uygulama

1. **IImageContract** arabiriminin tanımından hemen sonra **ImageService** adlı yeni bir sınıf oluşturun. **ImageService** sınıfı, **IImageContract** arabirimini uygular.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Diğer arabirim uygulamalarına benzer şekilde, tanımı farklı bir dosyada uygulayabilirsiniz. Ancak bu öğreticide uygulama, arabirim tanımı ve `Main()` yöntemiyle aynı dosyada görünmelidir.

2. Sınıfın WCF sözleşmesinin bir uygulaması olduğunu belirtmek için **IImageService** sınıfına [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) özniteliğini uygulayın.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Önceden belirtildiği gibi bu ad alanı, geleneksel bir ad alanı değildir. Bunun yerine, ad alanı sözleşmeyi tanımlayan WCF mimarisinin bir parçasıdır. Daha fazla bilgi için WCF belgelerinde [Veri Sözleşmesi Adları](https://msdn.microsoft.com/library/ms731045.aspx) konu başlığına bakın.

3. Projenize bir .jpg görüntüsü ekleyin.  

    Bu resim, hizmetin alıcı tarayıcıda gösterdiği resimdir. Projenize sağ tıklayın ve ardından **Ekle**'ye tıklayın. Ardından **Var Olan Öğe**'ye tıklayın. Uygun bir .jpg aramak için **Var Olan Öğeyi Ekle** iletişim kutusunu kullanın ve **Ekle**'ye tıklayın.

    Dosya eklerken **Dosya adı:** alanının yanındaki açılır listede **Tüm Dosyalar** seçeneğinin belirlendiğinden emin olun. Bu öğreticinin bundan sonraki bölümlerinde görüntü adının "image.jpg" olduğu varsayılır. Farklı bir dosyanız varsa görüntüyü tekrar adlandırmanız veya dengelemek için kodunu değiştirmeniz gerekir.

4. Çalışan hizmetin görüntü dosyasını bulabileceğinden emin olmak için **Çözüm Gezgini**'nde görüntü dosyasına sağ tıklayın ve **Özellikler**'e tıklayın. **Özellikler** bölmesinde, **Çıktı Dizinine Kopyala** seçeneğini **Daha yeniyse kopyala** olarak ayarlayın.

5. Projede **System.Drawing.dll** derlemesine başvuru ekleyin ve aşağıdaki ilişkili `using` deyimlerini ekleyin.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. **ImageService** sınıfında, bit eşlemi yükleyen ve istemci tarayıcıya göndermek için hazırlayan aşağıdaki oluşturucuyu ekleyin.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Önceki koddan hemen sonra görüntüyü içeren HTTP iletisini döndürmek için **ImageService** sınıfında aşağıdaki **GetImage** yöntemini ekleyin.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Bu uygulamalar, görüntüyü almak ve tarayıcıda akış için hazırlamak üzere **MemoryStream** kullanılır. Akış noktasını sıfırdan başlatır, akış içeriğini .jpeg olarak bildirir ve bilgileri akışa sunar.

8. **Derle** menüsünde **Çözümü Derle**'ye tıklayın.

### Service Bus üzerinde web hizmetini çalıştırmak için yapılandırma tanımlama

1. **Çözüm Gezgini**'nde, **App.config** dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın.

    **App.config** dosyası WCF yapılandırma dosyasına benzer ve hizmet adını, uç noktayı (Service Bus tarafından istemcilerin ve ana bilgisayarların birbirleriyle iletişim kurması için kullanıma sunulan konum) ve bağlamayı (iletişim kurmak için kullanılan protokol türü) içerir. İkisi arasındaki temel fark, buradaki yapılandırılan hizmet uç noktasının .NET Framework'ün bir parçası olmayan [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) bağlamasına başvuru sağlamasıdır.

2. `<system.serviceModel>` XML öğesi, bir veya birden çok hizmeti tanımlayan WCF öğesidir. Burada ise hizmet adını ve uç noktayı tanımlamak için kullanılır. `<system.serviceModel>` öğesinin alt kısmına (ancak hala `<system.serviceModel>` içinde) aşağıdaki içeriğe sahip olan `<bindings>` öğesini ekleyin. Bu işlem, uygulamada kullanılan bağlamaları tanımlar. Birden çok bağlama tanımlayabilirsiniz ancak bu öğreticide yalnızca bir tane tanımlayacaksınız.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Bu adım, **relayClientAuthenticationType** öğesinin **Hiçbiri** olarak ayarlandığı Service Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) bağlamasını tanımlar. Bu ayar, yukarıdaki bağlamayı kullanan bir uç noktanın istemci kimlik bilgilerini gerektirmeyeceğini belirtir.

3. `<bindings>` öğesinden sonra `<services>` öğesini ekleyin. Bağlamalara benzer şekilde tek bir yapılandırma dosyasında birden çok hizmet tanımlayabilirsiniz. Ancak bu öğreticide yalnızca bir tane tanımlayacaksınız.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Bu adımda, önceden tanımlanan varsayılan **webHttpRelayBinding** bağlamasını kullanan bir hizmet yapılandırılır. Ayrıca, bir sonraki adımda tanımlanan varsayılan **sbTokenProvider** öğesi kullanılır.

4. `<services>` öğesinden sonra, "SAS_KEY" alanını 1. Adımda [klasik Azure portalını][] aldığınız *Paylaşılan Erişim İmzası* (SAS) anahtarıyla değiştirerek aşağıdaki içeriğe sahip olan `<behaviors>` öğesini oluşturun.

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Yine App.config dosyasında `<appSettings>` öğesinde, tüm bağlantı dizesi değerini portaldan daha önce edindiğiniz bağlantı dizesiyle değiştirin. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Çözümün tamamını derlemek için **Derle** menüsünde **Çözümü Derle** seçeneğine tıklayın.

### Örnek

Aşağıdaki kodda, **WebHttpRelayBinding** bağlamasını kullanan Service Bus üzerinde çalışan REST tabanlı hizmete yönelik sözleşme ve hizmet uygulaması gösterilir.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Aşağıdaki örnek, hizmetle ilişkilendirilen App.config dosyasını gösterir.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## 4. Adım: Service Bus hizmetini kullanmak için REST tabanlı WCF hizmeti barındırma

Bu adımda Service Bus hizmetinde konsol uygulaması kullanan bir web hizmetini çalıştırma işlemi açıklanır. Bu adımda yazılan kodların tam listesi yordamdan sonraki örnekte verilmiştir.

### Hizmet için taban adresi oluşturma

1. `Main()` işlevi bildiriminde Service Bus projenizin ad alanını depolamak için bir değişken oluşturun. Daha önce oluşturduğunuz hizmet ad alanındaki adı `yourNamespace` ile değiştirdiğinizden emin olun.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Service Bus, benzersiz bir URI oluşturmak için ad alanınızdaki adı kullanır.

2. Ad alanını temel alan hizmetin taban adresine yönelik `Uri` örneğini oluşturun.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### Web hizmeti ana bilgisayarını oluşturma ve yapılandırma

- Bu bölümün önceki kısımlarında oluşturulan URI adresini kullanarak web hizmeti ana bilgisayarını oluşturun.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Hizmet ana bilgisayarı, ana bilgisayar uygulamasının örneğini oluşturan WCF nesnesidir. Bu örnek, hizmet ana bilgisayarını oluşturmak istediğiniz ana bilgisayar türüne (bir **ImageService**) ve ana bilgisayar uygulamasını kullanıma sunmak istediğiniz adrese geçirir.

### Web hizmeti ana bilgisayarını çalıştırma

1. Hizmeti açın.

    ```
    host.Open();
    ```
    Hizmet artık çalışır durumdadır.

2. Hizmetin çalıştığını ve nasıl durdurulacağını belirten bir ileti görüntüleyin.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Bu işlemi bitirdiğinizde, hizmet ana bilgisayarını kapatın.

    ```
    host.Close();
    ```

## Örnek

Aşağıdaki örnek, hizmet sözleşmesini ve bu öğreticinin önceki kısımlarında yer alan uygulamayı içerir ve hizmeti bir konsol uygulamasında barındırır. Aşağıdaki kodu ImageListener.exe adlı bir yürütülebilir dosyada derleyin.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### Kod derleme

Çözümü derledikten sonra uygulamayı çalıştırmak için şunları yapın:

1. Hizmeti çalıştırmak için **F5**'e basın veya yürütülebilir dosya konumuna gözatın (ImageListener\bin\Debug\ImageListener.exe). Uygulamayı çalışır durumda tutmak için bir sonraki adımı gerçekleştirmeniz gerekir.

2. Görüntüye bakmak için komut istemindeki adresi kopyalayıp bir tarayıcıya yapıştırın.

3. İşiniz bittiğinde uygulamayı kapatmak için komut istemi penceresinde **Enter**'a basın.

## Sonraki adımlar

Artık Service Bus geçiş hizmetini kullanan bir uygulama derlediğinize göre geçişli mesajlaşma hakkında daha fazla bil edinmek için aşağıdaki makalelere bakın:

- [Azure Service Bus mimarisine genel bakış](service-bus-fundamentals-hybrid-solutions.md#relays)

- [Service Bus Geçişi Hizmetini Kullanma](service-bus-dotnet-how-to-use-relay.md)

[klasik Azure portalını]: http://manage.windowsazure.com


<!----HONumber=Jun16_HO2-->


