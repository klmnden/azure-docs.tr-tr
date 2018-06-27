---
title: İle C# ile Azure Service Fabric hizmeti remoting iletişimler güvenli | Microsoft Docs
description: Bir Azure Service Fabric kümesi çalıştıran C# güvenilir hizmetler için hizmet tabanlı remoting iletişimi güvenli hale getirmek öğrenin.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: be5dab7b9714f13a4bd30e6ab33a5a0e2016212d
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37020028"
---
# <a name="secure-service-remoting-communications-in-a-c-service"></a>C# hizmetindeki hizmet remoting iletişimleri güvenli
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

Güvenlik iletişim en önemli yönlerinden birisidir. Güvenilir hizmetler uygulama çerçevesi birkaç önceden oluşturulmuş iletişimi yığınları ve güvenliği geliştirmek için kullanabileceğiniz araçlar sağlar. Bu makalede, C# hizmetinde hizmet remoting kullanırken güvenliğini artırmak nasıl anlatılmaktadır. Var olan derlemeler [örnek](service-fabric-reliable-services-communication-remoting.md) , C# ile yazılmış güvenilir hizmetler için uzaktan iletişim ayarlamak açıklanmaktadır. 

C# hizmetleriyle hizmet remoting kullanırken bir hizmeti güvenli hale getirmek için aşağıdaki adımları izleyin:

1. Bir arabirim oluşturmak `IHelloWorldStateful`, hizmetiniz üzerinde uzaktan yordam çağrısı için kullanılabilecek yöntemleri tanımlar. Hizmetinizi kullanacak `FabricTransportServiceRemotingListener`, içinde bildirilen `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` ad alanı. Bu bir `ICommunicationListener` remoting özellikleri sağlayan uygulama.

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
2. Dinleyici ayarları ve güvenlik kimlik bilgileri ekleyin.

    Hizmet iletişimi güvenli hale getirmek için kullanmak istediğiniz sertifikayı kümedeki tüm düğümlerde yüklü olduğundan emin olun. 
    
    > [!NOTE]
    > Linux düğümleri üzerinde sertifika PEM biçimli dosya olarak mevcut olmalıdır */var/lib/sfcerts* dizini. Daha fazla bilgi için bkz: [konumu ve Linux düğümleri X.509 sertifikaları biçimini](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes). 

    Dinleyici ayarları ve güvenlik kimlik bilgileri sağlayabilir iki yolu vardır:

   1. Bunları doğrudan hizmet kodunda sağlar:

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
   2. Kullanarak sağlama bir [yapılandırma paketi](service-fabric-application-and-service-manifests.md):

       Bir adlandırılmış eklemek `TransportSettings` settings.xml dosyasındaki bölümü.

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

       Bu durumda, `CreateServiceReplicaListeners` yöntemi şöyle görünür:

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

        Eklerseniz bir `TransportSettings` settings.xml dosyasındaki bölümünde `FabricTransportRemotingListenerSettings ` tüm ayarları varsayılan olarak bu bölümünden yüklenir.

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Bu durumda, `CreateServiceReplicaListeners` yöntemi şöyle görünür:

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
3. Çağırdığınızda yöntemler güvenli bir hizmette kullanmak yerine uzaktan iletişim yığını kullanarak `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` hizmeti proxy'si oluşturmak, kullanmak için sınıf `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Geçirin `FabricTransportRemotingSettings`, içeren `SecurityCredentials`.

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

    İstemci kodu bir hizmetin bir parçası çalışıyorsa, yükleyebilir `FabricTransportRemotingSettings` settings.xml dosyasından. Daha önce gösterildiği gibi hizmet koduna benzer HelloWorldClientTransportSettings bir bölüm oluşturun. İstemci kodu aşağıdaki değişiklikleri yapın:

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    İstemci bir hizmetin bir parçası çalışır durumda değilse, client_name.exe olduğu aynı konumda client_name.settings.xml dosyası oluşturabilirsiniz. Ardından bu dosyada bir TransportSettings bölüm oluşturun.

    Hizmete benzer şekilde, eklerseniz bir `TransportSettings` istemci settings.xml/client_name.settings.xml bölümünde `FabricTransportRemotingSettings` tüm ayarları varsayılan olarak bu bölümünden yükler.

    Bu durumda, önceki kod daha Basitleştirilmiş:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```


Sonraki adım olarak, okuma [Reliable Services OWIN ile Web API](service-fabric-reliable-services-communication-webapi.md).
