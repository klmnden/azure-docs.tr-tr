---
title: "Linux üzerinde Azure Service Fabric kapsayıcı uygulaması oluşturma | Microsoft Docs"
description: "Azure Service Fabric üzerinde ilk Linux kapsayıcı uygulamanızı oluşturun.  Uygulamanızla bir Docker görüntüsü oluşturun, görüntüyü bir kapsayıcı kayıt defterine iletin, bir Service Fabric kapsayıcı uygulaması oluşturup dağıtın."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 78306672e812745fd1902ae264c2adea196ab721
ms.contentlocale: tr-tr
ms.lasthandoff: 09/07/2017

---

# <a name="deploy-a-service-fabric-linux-container-application-on-azure"></a>Azure’da bir Service Fabric Linux kapsayıcı uygulaması dağıtma
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur. 

Bir Service Fabric kümesindeki Linux kapsayıcısında mevcut olan bir uygulamayı çalıştırmak için uygulamanızda herhangi bir değişiklik yapılması gerekmez. Bu hızlı başlangıç, Service Fabric uygulamasında önceden oluşturulmuş bir Docker kapsayıcısı görüntüsünü dağıtmayı gösterir. Hızlı başlangıcı tamamladığınızda, çalışan bir nginx kapsayıcısına sahip olacaksınız.  Bu hızlı başlangıç, Linux kapsayıcısı dağıtmayı açıklar. Windows kapsayıcısı dağıtmak için [bu Hızlı Başlangıca](service-fabric-quickstart-containers.md) bakın.

![Nginx][nginx]

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:
> [!div class="checklist"]
> * Docker görüntü kapsayıcısını paketleme
> * İletişimi yapılandırma
> * Service Fabric uygulamasını oluşturma ve paketleme
> * Kapsayıcı uygulamasını Azure’a dağıtma

## <a name="prerequisites"></a>Ön koşullar
[Service Fabric SDK, Service Fabric CLI ve Service Fabric yeoman şablonu oluşturucularını](service-fabric-get-started-linux.md) yükleme.
  
## <a name="package-a-docker-image-container-with-yeoman"></a>Yeoman ile Docker görüntü kapsayıcısını paketleme
Linux için Service Fabric SDK’sı uygulamanızı oluşturmayı ve kapsayıcı görüntüsü eklemeyi kolaylaştıran bir [Yeoman](http://yeoman.io/) oluşturucu içerir. 

Service Fabric kapsayıcı uygulaması oluşturmak için, terminal penceresini açın ve `yo azuresfcontainer` komutunu çalıştırın.  

Uygulamanıza "MyFirstContainer" adını verin ve uygulama hizmetini "MyContainerService" olarak adlandırın.

Kapsayıcı görüntüsü adı olarak "nginx:latest" (Docker hub'daki [nginx kapsayıcısı görüntüsü](https://hub.docker.com/r/_/nginx/)) değerini kullanın. 

Bu görüntüde tanımlanmış bir iş yükü giriş noktası vardır, bu nedenle giriş komutlarını açıkça belirtmeniz gerekir. 

"1" örnek sayısı belirtin.

![Kapsayıcılar için Service Fabric Yeoman oluşturucusu][sf-yeoman]

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>İletişim ve kapsayıcı bağlantı noktalarıyla konak bağlantı noktalarını eşlemeyi yapılandırın
İstemcilerin hizmetinizle iletişim kurabilmesi için bir HTTP uç noktası yapılandırın.  *./MyFirstContainer/MyContainerServicePkg/ServiceManifest.xml* dosyasını açın ve **ServiceManifest** öğesinde bir uç nokta kaynağını belirtin.  Protokolü, bağlantı noktasını ve adını ekleyin. Bu hızlı başlangıçta hizmet, bağlantı noktası 80’i dinler: 

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
    <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
`UriScheme` değerinin sağlanması, kapsayıcı uç noktasını bulunabilirlik için Service Fabric Adlandırma hizmetine otomatik olarak kaydeder. Bu makalenin sonunda tam bir ServiceManifest.xml örnek dosyası verilmiştir. 

ApplicationManifest.xml dosyasının `ContainerHostPolicies` bölümündeki `PortBinding` ilkesini kullanarak bir kapsayıcı bağlantı noktasını Hizmet `Endpoint` hedefine eşleyin.  Bu hızlı başlangıçta `ContainerPort` değeri 80 (kapsayıcı 80 numaralı bağlantı noktasını açar) ve `EndpointRef` değeri "myserviceTypeEndpoint"’tir (hizmet bildiriminde tanımlanan uç nokta).  80 numaralı bağlantı noktasında hizmete gelen istekler, kapsayıcı üzerindeki 80 numaralı bağlantı noktasıyla eşlenir.  

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a>Service Fabric uygulamasını oluşturma ve paketleme
Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak için kullanabileceğiniz bir [Gradle](https://gradle.org/) derleme betiği içerir. Yaptığınız tüm değişiklikleri kaydedin.  Uygulamayı derlemek ve paketlemek için şu komutu çalıştırın:

```bash
cd MyFirstContainer
gradle
```
## <a name="create-a-cluster"></a>Küme oluşturma
Uygulamayı Azure’daki bir kümeye dağıtmak için kendi kümenizi oluşturabilir veya bir grup kümesi kullanabilirsiniz.

Grup kümeleri, Azure üzerinde barındırılan ve Service Fabric ekibi tarafından sunulan ücretsiz, sınırlı süreli Service Fabric kümeleridir. Bu kümelerde herkes uygulama dağıtabilir ve platform hakkında bilgi edinebilir. Bir grup kümesine erişmek için [yönergeleri takip edin](http://aka.ms/tryservicefabric).  

Kendi kümenizi oluşturma hakkında daha fazla bilgi için bkz. [Azure'da ilk Service Fabric kümenizi oluşturma](service-fabric-get-started-azure-cluster.md).

Aşağıdaki adımda kullandığınız bağlantı uç noktasını not edin.

## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure’a dağıtma
Uygulama oluşturulduktan sonra Service Fabric CLI kullanarak Azure kümesine dağıtabilirsiniz.

Azure’daki Service Fabric kümesine bağlanın.

```bash
sfctl cluster select --endpoint http://lnxt10vkfz6.westus.cloudapp.azure.com:19080
```

Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini kullanın.

```bash
./install.sh
```

Bir tarayıcı penceresi açın ve http://lnxt10vkfz6.westus.cloudapp.azure.com:19080/Explorer adresinden Service Fabric Explorer’a gidin. Uygulamalar düğümünü genişletin ve şu anda uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

![Service Fabric Explorer][sfx]

Çalışan kapsayıcıya bağlanın.  80 numaralı bağlantı noktasında döndürülen IP adresine işaret eden bir web tarayıcısı açın (örneğin "lnxt10vkfz6.westus.cloudapp.azure.com:80"). Tarayıcıda nginx hoş geldiniz sayfasını görmeniz gerekir.

![Nginx][nginx]

## <a name="clean-up"></a>Temizleme
Kümeden uygulama örneğini silmek ve uygulama türünün kaydını silmek için şablonda sağlanan kaldırma betiğini kullanın.

```bash
./uninstall.sh
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Tam Service Fabric uygulaması ve hizmet bildirimleri örneği
Bu hızlı başlangıçta kullanılan tam hizmet ve uygulama bildirimleri aşağıda verilmiştir.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>nginx:latest</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>

```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest  ApplicationTypeName="MyFirstContainerType" ApplicationTypeVersion="1.0.0"
                      xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
   <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
</ServiceManifestImport>
   
   <DefaultServices>
      <Service Name="MyContainerService">
        <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
        cluster, set InstanceCount to -1 for the container service to run on every node in 
        the cluster.
        -->
        <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="1">
            <SingletonPartition />
        </StatelessService>
      </Service>
   </DefaultServices>
   
</ApplicationManifest>

```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:
> [!div class="checklist"]
> * Docker görüntü kapsayıcısını paketleme
> * İletişimi yapılandırma
> * Service Fabric uygulamasını oluşturma ve paketleme
> * Kapsayıcı uygulamasını Azure’a dağıtma

* [Service Fabric’te kapsayıcı](service-fabric-containers-overview.md) çalıştırma hakkında daha fazla bilgi edinin.
* [Kapsayıcı içinde .NET uygulaması dağıtma](service-fabric-host-app-in-a-container.md) öğreticisini okuyun.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
* GitHub’da [Service Fabric kapsayıcı kod örneklerine](https://github.com/Azure-Samples/service-fabric-dotnet-containers) bakın.

[sfx]: ./media/service-fabric-quickstart-containers-linux/SFX.png
[nginx]: ./media/service-fabric-quickstart-containers-linux/nginx.png
[sf-yeoman]: ./media/service-fabric-quickstart-containers-linux/YoSF.png

