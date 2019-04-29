---
title: Azure Geçişi'ni kullanarak REST Öğreticisi | Microsoft Docs
description: REST tabanlı bir arabirimi kullanıma sunan basit bir Azure Service Bus geçişi ana bilgisayar uygulaması oluşturun.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/06/2018
ms.author: spelluru
ms.openlocfilehash: 4ed45e1ed18ad630831772997b1fc150882731bd
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123408"
---
# <a name="azure-wcf-relay-rest-tutorial"></a>Azure WCF geçişi REST Öğreticisi
Bu öğretici, REST tabanlı bir arabirimi kullanıma sunan basit bir Azure geçişi ana bilgisayar uygulaması derlemeyi açıklar. REST, HTTP istekleri üzerinden Service Bus API'lerine erişmek için web tarayıcısı gibi bir web istemcisi sunar.

Bu öğreticide, Azure geçişi üzerinde bir REST hizmeti oluşturmak için programlama modeli Windows Communication Foundation (WCF) REST kullanılır. Daha fazla bilgi için WCF belgelerinde [WCF REST Programlama Modeli](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) ve [Hizmetleri Tasarlama ve Uygulama](/dotnet/framework/wcf/designing-and-implementing-services) konularına bakın.

Bu öğreticide aşağıdaki adımları uygulayın:

> [!div class="checklist"]
> * Bir geçiş ad alanı oluşturun.
> * REST tabanlı WCF hizmet sözleşmesini tanımlama
> * REST tabanlı WCF sözleşmesi uygulama
> * REST tabanlı WCF Hizmeti barındırma ve çalıştırma
> * Çalıştırma ve test etme hizmeti

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- [Visual Studio 2015 veya üzeri](https://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
- .NET için Azure SDK. Buradan yükleyin [SDK indirme sayfasını](https://azure.microsoft.com/downloads/).

## <a name="create-a-relay-namespace"></a>Bir geçiş ad alanı oluşturma

Azure'da geçiş özelliklerini kullanmaya başlamak için öncelikle bir hizmet ad alanı oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Azure kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar. [Buradaki yönergeleri](relay-create-namespace-portal.md) izleyerek bir Geçiş ad alanı oluşturun.

## <a name="define-a-rest-based-wcf-service-contract-to-use-with-azure-relay"></a>Azure geçişi ile kullanmak için REST tabanlı WCF hizmet sözleşmesini tanımlama

WCF REST stilinde bir hizmet oluşturduğunuzda sözleşme tanımlamanız gerekir. Sözleşmede ana bilgisayarın hangi işlemleri desteklediği belirtilir. Bir hizmet işlemi, web hizmeti yöntemi olarak düşünülebilir. Sözleşmeler; C++, C# veya Visual Basic arabirimi tanımlamasıyla oluşturulur. Arabirimdeki her yöntem belirli bir hizmet işlemine karşılık gelir.  [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) özniteliğinin her arabirime ve [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) özniteliğinin her işleme uygulanması gerekir. Arabirimdeki bir yöntem [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) özniteliğine sahip olup [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) özniteliğine sahip olmazsa bu yöntem kullanıma sunulmaz. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte gösterilir.

Bir WCF sözleşmesi ve REST stili sözleşmesi arasındaki birincil fark özellik ektir [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute): [WebGetAttribute](/dotnet/api/system.servicemodel.web.webgetattribute). Bu özellik sayesinde arabiriminizdeki bir yöntem ile arabirimin diğer tarafındaki bir yöntemi eşleyebilirsiniz. Bu örnekte [WebGetAttribute](/dotnet/api/system.servicemodel.web.webgetattribute) HTTP GET isteğine bir yöntemi bağlamak için özniteliği. Bu, Service Bus'ın doğru şekilde almak ve arabirime gönderilen komutları yorumlamasını sağlar.

### <a name="to-create-a-contract-with-an-interface"></a>Bir arabirim ile bir sözleşme oluşturmak için

1. Visual Studio'yu yönetici olarak açın: **Başlangıç** menüsünde programa sağ tıklayıp **Yönetici olarak çalıştır**'a tıklayın.
2. Yeni bir konsol uygulama projesi oluşturun. **Dosya** menüsüne tıklayın ve **Yeni** seçeneğini belirleyip **Proje** seçimini yapın. **Yeni Proje** iletişim kutusunda **Visual C#** seçeneğine tıklayıp **Konsol Uygulaması** şablonuna tıklayın ve **ImageListener** olarak adlandırın. Varsayılan **Konum**'u kullanın. Projeyi oluşturmak için **Tamam**'a tıklayın.
3. Visual Studio, bir C# projesi için `Program.cs` dosyası oluşturur. Bu sınıf, bir konsol uygulaması projesinin doğru şekilde derlenmesi için gerekli olan boş `Main()` yöntemi içerir.
4. Service Bus hizmetine başvurular ekleyin ve Service Bus NuGet paketini yükleyerek projede **System.ServiceModel.dll** öğesini ekleyin. Bu paket otomatik olarak Service Bus kitaplıklarının yanı sıra WCF **System.ServiceModel** öğesine de başvurular ekler. Çözüm Gezgini'nde **ImageListener** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus` için arama yapın. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.
5. Projede **System.ServiceModel.Web.dll** öğesine açık olarak bir başvuru eklemeniz gerekir:
   
    a. Çözüm Gezgini'nde, **Başvurular** klasörüne sağ tıklayın ve proje klasöründe **Başvuru Ekle**'ye tıklayın.
   
    b. **Başvuru Ekle** iletişim kutusunda, sol taraftaki **Altyapı** sekmesine tıklayın ve **Ara** kutusuna **System.ServiceModel.Web** yazın. **System.ServiceModel.Web** öğesinin onay kutusunu işaretleyin ve **Tamam**'a tıklayın.
6. Aşağıdaki `using` deyimlerini Program.cs dosyasının üstüne ekleyin.
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    [System.ServiceModel](/dotnet/api/system.servicemodel), WCF'nin temel özelliklerine programlama erişimi sağlayan ad alanıdır. WCF geçişi birçok nesnesini ve özniteliklerini WCF hizmet sözleşmelerini tanımlamak için kullanır. Geçiş uygulamalarınızın çoğunda bu ad alanı kullanırsınız. Benzer şekilde, [System.ServiceModel.Channels](/dotnet/api/system.servicemodel.channels) yardımcı iletişim Azure geçişi ve istemci web tarayıcısı ile nesne olan kanalı tanımlayın. Son olarak [System.ServiceModel.Web](/dotnet/api/system.servicemodel.web), web tabanlı uygulamalar oluşturmanıza olanak sağlayan türleri içerir.
7. `ImageListener` ad alanını **Microsoft.ServiceBus.Samples** olarak yeniden adlandırın.
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. Ad alanı bildiriminin büyük ayracını açtıktan hemen sonra **IImageContract** adlı yeni bir arabirim tanımlayın ve `https://samples.microsoft.com/ServiceModel/Relay/` değerini içeren arabirime **ServiceContractAttribute** özniteliğini uygulayın. Kodunuzun kapsamında kullandığınız ad alanı ile ad alanı değeri farklılık gösterir. Ad alanı değeri bu sözleşme için benzersiz bir tanımlayıcı olarak kullanılır ve sürüm bilgisini içermelidir. Daha fazla bilgi için bkz. [Hizmet Sürümü Oluşturma](https://go.microsoft.com/fwlink/?LinkID=180498). Ad alanını açıkça belirlemek, varsayılan ad alanı değerinin sözleşme adına eklenmesini engeller.
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. `IImageContract` arabirimi içinde, `IImageContract` sözleşmesinin arabirimde kullanıma sunduğu tek bir işlem için bir yöntem bildirin ve genel Service Bus sözleşmesinin bir parçası olarak kullanıma sunmak istediğiniz yönteme `OperationContractAttribute` özniteliğini uygulayın.
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. **OperationContract** özniteliği içinde **WebGet** değerini ekleyin.
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    Bunu etkinleştirir alan geçiş hizmetine HTTP GET istekleri yapan `GetImage`ve dönüş değerlerini çevrilecek `GetImage` HTTP GetResponse YANITINA çevirmesine içine. Bu öğreticinin sonraki bölümlerinde, bu yönteme erişmek ve tarayıcıdaki görüntüyü görüntülemek için bir web tarayıcısı kullanacaksınız.
11. `IImageContract` tanımından hemen sonra, `IImageContract` ve `IClientChannel` arabirimlerinden devralma işlemini gerçekleştiren bir kanal bildirin.
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    Kanal, hizmet ve istemcilerin bilgileri birbirlerine göndermek için kullandıkları WCF nesnesidir. Daha sonra ana bilgisayar uygulamanızda kanal oluşturun. Azure geçişi ardından kullanır bu kanalı HTTP GET isteklerini tarayıcıya geçirmek için **Getımage** uygulaması. Geçiş kanalı almak için de kullanır **Getımage** dönüş değeri ve istemci tarayıcısı için HTTP GetResponse çevir.
12. Çalışmanızın o ana kadarki doğruluğunu onaylamak için **Derle** menüsünde **Çözümü Derle**'ye tıklayın.

### <a name="example"></a>Örnek
Aşağıdaki kod, bir WCF geçişi sözleşmesini tanımlayan temel bir arabirimi gösterir.

```csharp
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

    [ServiceContract(Name = "IImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
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

## <a name="implement-the-rest-based-wcf-service-contract"></a>REST tabanlı WCF hizmet sözleşmesini uygulama
REST stilinde WCF geçişi hizmetini oluşturmak için öncelikle bir arabirim kullanılarak tanımlanan sözleşmeyi oluşturmanız gerekir. Bir sonraki adım ise bu arabirimi uygulamaktır. Bu adımda kullanıcı tanımlı **IImageContract** arabirimini uygulayan **ImageService** adlı bir sınıf oluşturursunuz. Sözleşmeyi uyguladıktan sonra, App.config dosyası kullanarak arabirimi yapılandırırsınız. Yapılandırma dosyasında hizmetin adı, sözleşmeyi ve geçiş hizmeti ile iletişim kurmak için kullanılan protokol türü adı gibi bir uygulama için gerekli bilgileri içerir. Bu görevler için kullanılan kod, aşağıdaki yordamın altındaki örnekte sağlanır.

Önceki adımlarda olduğu gibi bir REST stilinde sözleşme ve bir WCF geçişi sözleşmesi uygulama arasında çok az bir fark yoktur.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>REST stilinde Service Bus sözleşmesini uygulama
1. **IImageContract** arabiriminin tanımından hemen sonra **ImageService** adlı yeni bir sınıf oluşturun. **ImageService** sınıfı, **IImageContract** arabirimini uygular.
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    Diğer arabirim uygulamalarına benzer şekilde, tanımı farklı bir dosyada uygulayabilirsiniz. Ancak bu öğreticide uygulama, arabirim tanımı ve `Main()` yöntemiyle aynı dosyada görünmelidir.
2. Sınıfın WCF sözleşmesinin bir uygulaması olduğunu belirtmek için **IImageService** sınıfına [ServiceBehaviorAttribute](/dotnet/api/system.servicemodel.servicebehaviorattribute) özniteliğini uygulayın.
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    Önceden belirtildiği gibi bu ad alanı, geleneksel bir ad alanı değildir. Bunun yerine, ad alanı sözleşmeyi tanımlayan WCF mimarisinin bir parçasıdır. Daha fazla bilgi için [veri sözleşmesi adları](https://msdn.microsoft.com/library/ms731045.aspx) makale için WCF belgelerinde bulunan.
3. Projenize bir .jpg görüntüsü ekleyin.  
   
    Bu resim, hizmetin alıcı tarayıcıda gösterdiği resimdir. Projenize sağ tıklayın ve ardından **Ekle**'ye tıklayın. Ardından **Var Olan Öğe**'ye tıklayın. Uygun bir .jpg aramak için **Var Olan Öğeyi Ekle** iletişim kutusunu kullanın ve **Ekle**'ye tıklayın.
   
    Dosya eklerken **Dosya adı:** alanının yanındaki açılır listede **Tüm Dosyalar** seçeneğinin belirlendiğinden emin olun. Bu öğreticinin bundan sonraki bölümlerinde görüntü adının "image.jpg" olduğu varsayılır. Farklı bir dosyanız varsa görüntüyü tekrar adlandırmanız veya dengelemek için kodunu değiştirmeniz gerekir.
4. Çalışan hizmetin görüntü dosyasını bulabileceğinden emin olmak için **Çözüm Gezgini**'nde görüntü dosyasına sağ tıklayın ve **Özellikler**'e tıklayın. **Özellikler** bölmesinde, **Çıktı Dizinine Kopyala** seçeneğini **Daha yeniyse kopyala** olarak ayarlayın.
5. Projede **System.Drawing.dll** derlemesine başvuru ekleyin ve aşağıdaki ilişkili `using` deyimlerini ekleyin.  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. **ImageService** sınıfında, bit eşlemi yükleyen ve istemci tarayıcıya göndermek için hazırlayan aşağıdaki oluşturucuyu ekleyin.
   
    ```csharp
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
   
    ```csharp
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

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Service Bus üzerinde web hizmetini çalıştırmak için yapılandırma tanımlama
1. **Çözüm Gezgini**'nde, **App.config** dosyasına çift tıklayarak dosyayı Visual Studio düzenleyicisinde açın.
   
    **App.config** dosya içeren hizmet adını, uç noktayı (diğer bir deyişle, istemcilerin ve ana bilgisayarların birbirleriyle iletişim kurmak Azure geçişi sunulan konum) ve bağlamayı (iletişim kurmak için kullanılan protokol türü). Ana fark burada yapılandırılan hizmet uç noktasının ise bir [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) bağlama.
2. `<system.serviceModel>` XML öğesi, bir veya birden çok hizmeti tanımlayan WCF öğesidir. Burada ise hizmet adını ve uç noktayı tanımlamak için kullanılır. `<system.serviceModel>` öğesinin alt kısmına (ancak hala `<system.serviceModel>` içinde) aşağıdaki içeriğe sahip olan `<bindings>` öğesini ekleyin. Bu işlem, uygulamada kullanılan bağlamaları tanımlar. Birden çok bağlama tanımlayabilirsiniz ancak bu öğreticide yalnızca bir tane tanımlayacaksınız.
   
    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```
   
    WCF geçişi önceki kodu tanımlar [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) ile bağlama **öğesinin** kümesine **hiçbiri**. Bu ayar, yukarıdaki bağlamayı kullanan bir uç noktanın istemci kimlik bilgilerini gerektirmeyeceğini belirtir.
3. `<bindings>` öğesinden sonra `<services>` öğesini ekleyin. Bağlamalara benzer şekilde tek bir yapılandırma dosyasında birden çok hizmet tanımlayabilirsiniz. Ancak bu öğreticide yalnızca bir tane tanımlayacaksınız.
   
    ```xml
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
4. Sonra `<services>` öğesi oluşturmak bir `<behaviors>` "SAS_KEY" ile değiştirerek aşağıdaki içeriğe sahip öğe *paylaşılan erişim imzası* (SAS) anahtarı, elde ettiğiniz [Azureportalı] [Azure portal].
   
    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
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
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. Çözümün tamamını derlemek için **Derle** menüsünde **Çözümü Derle** seçeneğine tıklayın.

### <a name="example"></a>Örnek
Aşağıdaki kodda, **WebHttpRelayBinding** bağlamasını kullanan Service Bus üzerinde çalışan REST tabanlı hizmete yönelik sözleşme ve hizmet uygulaması gösterilir.

```csharp
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


    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
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

```xml
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
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
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
        <!-- Service Bus specific app settings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY>"/>
    </appSettings>
</configuration>
```

## <a name="host-the-rest-based-wcf-service-to-use-azure-relay"></a>Azure Geçişi'ni kullanmak için REST tabanlı WCF Hizmeti barındırma
Bu adımda, WCF geçişi ile bir konsol uygulaması kullanarak bir web hizmetinin nasıl çalıştırılacağı açıklanır. Bu adımda yazılan kodların tam listesi yordamdan sonraki örnekte verilmiştir.

### <a name="to-create-a-base-address-for-the-service"></a>Hizmet için taban adresi oluşturma
1. İçinde `Main()` işlevi bildiriminde, projenizin ad depolamak için bir değişken oluşturun. Değiştirdiğinizden emin olun `yourNamespace` daha önce oluşturduğunuz bir geçiş ad alanı adı.
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    Service Bus, benzersiz bir URI oluşturmak için ad alanınızdaki adı kullanır.
2. Ad alanını temel alan hizmetin taban adresine yönelik `Uri` örneğini oluşturun.
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Web hizmeti ana bilgisayarını oluşturma ve yapılandırma
* Bu bölümün önceki kısımlarında oluşturulan URI adresini kullanarak web hizmeti ana bilgisayarını oluşturun.
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Hizmet ana bilgisayarı, ana bilgisayar uygulamasının örneğini oluşturan WCF nesnesidir. Bu örnek, hizmet ana bilgisayarını oluşturmak istediğiniz ana bilgisayar türüne (bir **ImageService**) ve ana bilgisayar uygulamasını kullanıma sunmak istediğiniz adrese geçirir.

### <a name="to-run-the-web-service-host"></a>Web hizmeti ana bilgisayarını çalıştırma
1. Hizmeti açın.
   
    ```csharp
    host.Open();
    ```
    Hizmet artık çalışır durumdadır.
2. Hizmetin çalıştığını ve nasıl durdurulacağını belirten bir ileti görüntüleyin.
   
    ```csharp
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. Bu işlemi bitirdiğinizde, hizmet ana bilgisayarını kapatın.
   
    ```csharp
    host.Close();
    ```

### <a name="example"></a>Örnek
Aşağıdaki örnek, hizmet sözleşmesini ve bu öğreticinin önceki kısımlarında yer alan uygulamayı içerir ve hizmeti bir konsol uygulamasında barındırır. Aşağıdaki kodu ImageListener.exe adlı bir yürütülebilir dosyada derleyin.

```csharp
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

    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
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

## <a name="run-and-test-the-service"></a>Çalıştırma ve test etme hizmeti
Çözümü derledikten sonra uygulamayı çalıştırmak için şunları yapın:

1. Hizmeti çalıştırmak için **F5**'e basın veya yürütülebilir dosya konumuna gözatın (ImageListener\bin\Debug\ImageListener.exe). Uygulamayı çalışır durumda tutmak için bir sonraki adımı gerçekleştirmeniz gerekir.
2. Görüntüye bakmak için komut istemindeki adresi kopyalayıp bir tarayıcıya yapıştırın.
3. İşiniz bittiğinde uygulamayı kapatmak için komut istemi penceresinde **Enter**'a basın.

## <a name="next-steps"></a>Sonraki adımlar
Azure geçişi hizmetini kullanan bir uygulama oluşturduğunuza göre daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Geçiş’e genel bakış](relay-what-is-it.md)
* [.NET ile WCF geçişi hizmetini kullanma](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
