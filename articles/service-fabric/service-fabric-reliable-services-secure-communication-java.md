---
title: Güvenli Azure Service fabric'te Java ile Service uzaktan iletişim | Microsoft Docs
description: Bir Azure Service Fabric kümesinde çalıştırılan Java reliable services için hizmet uzaktan iletişim tabanlı iletişimi güvenli hale getirmeyi öğrenin.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: chackdan
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: b465ab602a14285f8cf40b24ce1dfa9c763fecb8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60773358"
---
# <a name="secure-service-remoting-communications-in-a-java-service"></a>Güvenli bir Java hizmetinde hizmet uzaktan iletişim
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-secure-communication.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-secure-communication-java.md)
>
>

Güvenlik iletişim en önemli yönlerinden birisidir. Reliable Services uygulaması çerçevesi, birkaç önceden oluşturulmuş iletişim yığınlarını ve güvenliği geliştirmek için kullanabileceğiniz araçlar sağlar. Bu makalede bir Java hizmetinde hizmet uzaktan iletişimini kullanırken güvenliğini artırmak nasıl açıklanmaktadır. Var olan bir derleme [örnek](service-fabric-reliable-services-communication-remoting-java.md) , Java dilinde yazılmış reliable services için uzaktan iletişim ayarlamak amacıyla açıklanmaktadır. 

Uzaktan hizmet iletişimini Java Hizmetleri ile kullanırken bir hizmeti güvenli hale getirmek için şu adımları izleyin:

1. Bir arabirim oluşturma `HelloWorldStateless`, hizmetinizde bir uzak yordam çağrısı için kullanılabilecek yöntemleri tanımlar. Hizmetinizi kullanacak `FabricTransportServiceRemotingListener`, içinde bildirilen `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` paket. Bu bir `CommunicationListener` remoting özellikleri sağlayan uygulama.

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
2. Dinleyici ayarları ve güvenlik kimlik bilgilerini ekleyin.

    Hizmeti iletişiminizin güvenliğini sağlamak için kullanmak istediğiniz sertifikayı kümedeki tüm düğümlerde yüklü olduğundan emin olun. Linux üzerinde çalışan hizmetler için sertifika formmatted PEM dosyası olarak bulunmalıdır; ya da bir `.pem` sertifikayı ve özel anahtarı içeren bir dosya veya `.crt` sertifikasını içeren dosya ve `.key` özel anahtarı içeren dosya. Daha fazla bilgi için bkz. [konumu ve X.509 sertifikaları Linux düğümlerinde biçimini](./service-fabric-configure-certificates-linux.md#location-and-format-of-x509-certificates-on-linux-nodes).
    
    Dinleyici ayarları ve güvenlik kimlik bilgilerini sağlayabilir iki yolu vardır:

   1. Bunları kullanarak sağlama bir [yapılandırma paketi](service-fabric-application-and-service-manifests.md):

       Adlandırılmış ekleme `TransportSettings` settings.xml dosyasının bir bölümünde.

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

       Bu durumda, `createServiceInstanceListeners` yöntemi şu şekilde görünür:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Eklerseniz bir `TransportSettings` herhangi bir önek olmadan settings.xml dosyasının bir bölümünde `FabricTransportListenerSettings` tüm ayarları sayfasından Bu bölüm, varsayılan olarak yüklenir.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        Bu durumda, `CreateServiceInstanceListeners` yöntemi şu şekilde görünür:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. Çağırdığınızda yöntemleri güvenli bir hizmette kullanmak yerine uzaktan iletişim yığını kullanarak `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` hizmeti proxy oluşturma, kullanma, sınıf `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    İstemci kodu bir hizmetin parçası olarak çalışıyorsa, yükleyebilir `FabricTransportSettings` settings.xml dosyasından. Daha önce gösterildiği gibi hizmet koduna benzer TransportSettings bir bölüm oluşturun. İstemci kodu için aşağıdaki değişiklikleri yapın:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
