---
title: "Güvenli iletişim için Azure Service Fabric Hizmetleri'nde Yardım | Microsoft Docs"
description: "Güvenilir hizmetler için iletişimi güvenli hale getirmek nasıl genel bakış çalıştıran bir Azure Service Fabric kümesi."
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: c4634e3d8efb1745fffcfe3e647e43d867038716
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Güvenli iletişim Azure Service Fabric hizmetler için Yardım
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Hizmet remoting kullanırken hizmet korunmasına yardımcı olma
Biz varolan kullanmaya başlayacağınız [örnek](service-fabric-reliable-services-communication-remoting-java.md) nasıl uzaktan iletişim için güvenilir hizmetler ayarlanacağı açıklanmaktadır. Hizmet remoting kullanırken bir hizmeti güvenli hale getirmek için aşağıdaki adımları izleyin:

1. Bir arabirim oluşturmak `HelloWorldStateless`, hizmetiniz üzerinde uzaktan yordam çağrısı için kullanılabilecek yöntemleri tanımlar. Hizmetinizi kullanacak `FabricTransportServiceRemotingListener`, içinde bildirilen `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` paket. Bu bir `CommunicationListener` remoting özellikleri sağlayan uygulama.

    ```java
    public interface HelloWorldStateless extends Service {
        CompletableFuture<String> getHelloWorld();
    }

    class HelloWorldStatelessImpl extends StatelessService implements HelloWorldStateless {
        @Override
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
        return listeners;
        }

        public CompletableFuture<String> getHelloWorld() {
            return CompletableFuture.completedFuture("Hello World!");
        }
    }
    ```
2. Dinleyici ayarları ve güvenlik kimlik bilgileri ekleyin.

    Hizmet iletişimi güvenli hale getirmek için kullanmak istediğiniz sertifikayı kümedeki tüm düğümlerde yüklü olduğundan emin olun. Dinleyici ayarları ve güvenlik kimlik bilgileri sağlayabilir iki yolu vardır:

   1. Kullanarak sağlama bir [yapılandırma paketi](service-fabric-application-model.md):

       Ekleme bir `TransportSettings` settings.xml dosyasındaki bölümü.

       ```xml
       <!--Section name should always end with "TransportSettings".-->
       <!--Here we are using a prefix "HelloWorldStateless".-->
        <Section Name="HelloWorldStatelessTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509_2" />
            <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
            <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
        </Section>

       ```

       Bu durumda, `createServiceInstanceListeners` yöntemi şöyle görünür:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Eklerseniz bir `TransportSettings` herhangi öneki olmadan settings.xml dosyasındaki bölümünü `FabricTransportListenerSettings` tüm ayarları varsayılan olarak bu bölümünden yüklenir.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Bu durumda, `CreateServiceInstanceListeners` yöntemi şöyle görünür:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Çağırdığınızda yöntemler güvenli bir hizmette kullanmak yerine uzaktan iletişim yığını kullanarak `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` hizmeti proxy'si oluşturmak, kullanmak için sınıf `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    İstemci kodu bir hizmetin bir parçası çalışıyorsa, yükleyebilir `FabricTransportSettings` settings.xml dosyasından. Daha önce gösterildiği gibi hizmet koduna benzer TransportSettings bir bölüm oluşturun. İstemci kodu aşağıdaki değişiklikleri yapın:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
