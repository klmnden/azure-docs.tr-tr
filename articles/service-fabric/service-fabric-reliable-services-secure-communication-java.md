---
title: Azure Service Fabric Java ile hizmet remoting iletişimler güvenli | Microsoft Docs
description: Bir Azure Service Fabric kümede çalışan Java güvenilir hizmetler için hizmet tabanlı remoting iletişimi güvenli hale getirmek öğrenin.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: cbefb3ede6d0d1fe21065b49c84db9f4db5dd39c
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37020822"
---
# <a name="secure-service-remoting-communications-in-a-java-service"></a>Güvenli bir Java hizmetindeki hizmet remoting iletişim
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

Güvenlik iletişim en önemli yönlerinden birisidir. Güvenilir hizmetler uygulama çerçevesi birkaç önceden oluşturulmuş iletişimi yığınları ve güvenliği geliştirmek için kullanabileceğiniz araçlar sağlar. Bu makalede, bir Java hizmetinde hizmet remoting kullanırken güvenliğini artırmak nasıl anlatılmaktadır. Var olan derlemeler [örnek](service-fabric-reliable-services-communication-remoting-java.md) nasıl uzaktan iletişim için güvenilir hizmetler Java'da yazılmış ayarlanacağı açıklanmaktadır. 

Java hizmetleriyle hizmet remoting kullanırken bir hizmeti güvenli hale getirmek için aşağıdaki adımları izleyin:

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

    Hizmet iletişimi güvenli hale getirmek için kullanmak istediğiniz sertifikayı kümedeki tüm düğümlerde yüklü olduğundan emin olun. Linux üzerinde çalışan hizmetleri için sertifika PEM formmatted dosyası olarak kullanılabilir olması gerekir; ya da bir `.pem` sertifika ve özel anahtarı içeren dosya veya bir `.crt` sertifikayı içeren dosyası ve bir `.key` özel anahtarı içeren dosya. Daha fazla bilgi için bkz: [konumu ve Linux düğümleri X.509 sertifikaları biçimini](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes).
    
    Dinleyici ayarları ve güvenlik kimlik bilgileri sağlayabilir iki yolu vardır:

   1. Kullanarak sağlama bir [yapılandırma paketi](service-fabric-application-and-service-manifests.md):

       Bir adlandırılmış eklemek `TransportSettings` settings.xml dosyasındaki bölümü.

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
