---
title: Hizmet uzaktan iletişimi ile güvenli iletişim C# Azure Service fabric'te | Microsoft Docs
description: Temel uzaktan iletişim için güvenliğini sağlamayı öğrenmek C# bir Azure Service Fabric kümesinde çalışan güvenilir Hizmetleri.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: chackdan
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: b6d4a44a53ba553ab4fd514c81867156192b69f5
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662549"
---
# <a name="secure-service-remoting-communications-in-a-c-service"></a>Hizmet uzaktan iletişim güvenli bir C# hizmeti
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

Güvenlik iletişim en önemli yönlerinden birisidir. Reliable Services uygulaması çerçevesi, birkaç önceden oluşturulmuş iletişim yığınlarını ve güvenliği geliştirmek için kullanabileceğiniz araçlar sağlar. Bu makalede ele alınmaktadır hizmet uzaktan iletişimi kullanırken güvenliğini artırmak nasıl bir C# hizmeti. Var olan bir derleme [örnek](service-fabric-reliable-services-communication-remoting.md) , yazılan reliable services için uzaktan iletişim ayarlamak amacıyla açıklanmaktadır C#. 

Uzaktan hizmet iletişimini ile kullanırken bir hizmeti güvenli hale getirmek için C# Hizmetleri, şu adımları izleyin:

1. Bir arabirim oluşturma `IHelloWorldStateful`, hizmetinizde bir uzak yordam çağrısı için kullanılabilecek yöntemleri tanımlar. Hizmetinizi kullanacak `FabricTransportServiceRemotingListener`, içinde bildirilen `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` ad alanı. Bu bir `ICommunicationListener` remoting özellikleri sağlayan uygulama.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```
2. Dinleyici ayarları ve güvenlik kimlik bilgilerini ekleyin.

    Hizmeti iletişiminizin güvenliğini sağlamak için kullanmak istediğiniz sertifikayı kümedeki tüm düğümlerde yüklü olduğundan emin olun. 
    
    > [!NOTE]
    > Linux düğümlerinde sertifika PEM biçimli dosya olarak mevcut olmalıdır */var/lib/sfcerts* dizin. Daha fazla bilgi için bkz. [konumu ve X.509 sertifikaları Linux düğümlerinde biçimini](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes). 

    Dinleyici ayarları ve güvenlik kimlik bilgilerini sağlayabilir iki yolu vardır:

   1. Bunları doğrudan hizmeti kodunda sağlar:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           FabricTransportRemotingListenerSettings  listenerSettings = new FabricTransportRemotingListenerSettings
           {
               MaxMessageSize = 10000000,
               SecurityCredentials = GetSecurityCredentials()
           };
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
           };
       }

       private static SecurityCredentials GetSecurityCredentials()
       {
           // Provide certificate details.
           var x509Credentials = new X509Credentials
           {
               FindType = X509FindType.FindByThumbprint,
               FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
               StoreLocation = StoreLocation.LocalMachine,
               StoreName = "My",
               ProtectionLevel = ProtectionLevel.EncryptAndSign
           };
           x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
           x509Credentials.RemoteCertThumbprints.Add("9FEF3950642138446CC364A396E1E881DB76B483");
           return x509Credentials;
       }
       ```
   2. Bunları kullanarak sağlama bir [yapılandırma paketi](service-fabric-application-and-service-manifests.md):

       Adlandırılmış ekleme `TransportSettings` settings.xml dosyasının bir bölümünde.

       ```xml
       <Section Name="HelloWorldStatefulTransportSettings">
           <Parameter Name="MaxMessageSize" Value="10000000" />
           <Parameter Name="SecurityCredentialsType" Value="X509" />
           <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
           <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
           <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
           <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
           <Parameter Name="CertificateStoreName" Value="My" />
           <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
           <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
       </Section>
       ```

       Bu durumda, `CreateServiceReplicaListeners` yöntemi şu şekilde görünür:

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(
                       context,this,FabricTransportRemotingListenerSettings .LoadFrom("HelloWorldStatefulTransportSettings")))
           };
       }
       ```

        Eklerseniz bir `TransportSettings` settings.xml dosyasının bir bölümünde `FabricTransportRemotingListenerSettings ` tüm ayarları sayfasından Bu bölüm, varsayılan olarak yüklenir.

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Bu durumda, `CreateServiceReplicaListeners` yöntemi şu şekilde görünür:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                return new[]{
                        new ServiceReplicaListener(
                            (context) => new FabricTransportServiceRemotingListener(context,this))};
            };
        }
        ```
3. Çağırdığınızda yöntemleri güvenli bir hizmette kullanmak yerine uzaktan iletişim yığını kullanarak `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` hizmeti proxy oluşturma, kullanma, sınıf `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Geçirin `FabricTransportRemotingSettings`, içeren `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "9FEF3950642138446CC364A396E1E881DB76B483",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
    x509Credentials.RemoteCertThumbprints.Add("4FEF3950642138446CC364A396E1E881DB76B48C");

    FabricTransportRemotingSettings transportSettings = new FabricTransportRemotingSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    İstemci kodu bir hizmetin parçası olarak çalışıyorsa, yükleyebilir `FabricTransportRemotingSettings` settings.xml dosyasından. Daha önce gösterildiği gibi hizmet koduna benzer HelloWorldClientTransportSettings bir bölüm oluşturun. İstemci kodu için aşağıdaki değişiklikleri yapın:

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    İstemciye bir hizmetin parçası olarak çalışmıyorsa client_name.exe olduğu aynı konumda client_name.settings.xml dosyası oluşturabilirsiniz. Ardından bu dosyada TransportSettings bölümü oluşturun.

    Hizmete benzer şekilde, eklerseniz bir `TransportSettings` istemci settings.xml/client_name.settings.xml bölümünde `FabricTransportRemotingSettings` tüm ayarları sayfasından Bu bölüm, varsayılan olarak yükler.

    Bu durumda, önceki kod daha basitleştirilmiştir:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```


Sonraki adım olarak, okuma [Reliable Services özelliğinde OWIN ile Web API](service-fabric-reliable-services-communication-webapi.md).
