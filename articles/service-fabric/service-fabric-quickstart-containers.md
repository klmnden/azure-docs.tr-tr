---
title: "Azure Service Fabric Windows kapsayıcı uygulaması oluşturma | Microsoft Belgeleri"
description: "Azure Service Fabric üzerinde ilk Windows kapsayıcı uygulamanızı oluşturun."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9d3d15c63055f3eeb0e6cb292d75a8c42b33f7fe
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="deploy-a-service-fabric-windows-container-application-on-azure"></a>Azure’da bir Service Fabric Windows kapsayıcı uygulaması dağıtma
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur. 

Bir Service Fabric kümesindeki Windows kapsayıcısında mevcut olan bir uygulamayı çalıştırmak için uygulamanızda herhangi bir değişiklik yapılması gerekmez. Bu hızlı başlangıç, Service Fabric uygulamasında önceden oluşturulmuş bir Docker kapsayıcısı görüntüsünü dağıtmayı gösterir. İşlemi tamamladığınızda, çalışan bir Windows Server 2016 Nano Server ve IIS kapsayıcısına sahip olacaksınız. Bu hızlı başlangıç, Windows kapsayıcısı dağıtmayı açıklar. Linux kapsayıcısı dağıtmak için [bu Hızlı Başlangıca](service-fabric-quickstart-containers-linux.md) bakın.

![IIS varsayılan web sayfası][iis-default]

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:
> [!div class="checklist"]
> * Docker görüntü kapsayıcısını paketleme
> * İletişimi yapılandırma
> * Service Fabric uygulamasını oluşturma ve paketleme
> * Kapsayıcı uygulamasını Azure’a dağıtma

## <a name="prerequisites"></a>Ön koşullar
* Bir Azure aboneliği ([ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz).
* Şunları çalıştıran bir geliştirme bilgisayarı:
  * Visual Studio 2015 veya Visual Studio 2017.
  * [Service Fabric SDK’sı ve araçları](service-fabric-get-started.md).

## <a name="package-a-docker-image-container-with-visual-studio"></a>Visual Studio ile Docker görüntü kapsayıcısını paketleme
Service Fabric SDK’sı ve araçları, bir kapsayıcıyı Service Fabric kümesine dağıtmanıza yardımcı olan bir hizmet şablonu sağlar.

Visual Studio'yu “Yönetici” olarak başlatın.  **Dosya** > **Yeni** > **Proje**’yi seçin.

**Service Fabric uygulaması**’nı seçin, "MyFirstContainer" olarak adlandırın ve **Tamam**’a tıklayın.

**Hizmet şablonları** listesinden **Kapsayıcı**’yı seçin.

**Görüntü Adı** olarak "microsoft/iis:nanoserver" değerini [Windows Server Nano Server ve IIS temel görüntüsü](https://hub.docker.com/r/microsoft/iis/)'nü seçin. 

Hizmeti "MyContainerService" olarak adlandırın ve **Tamam**’a tıklayın.

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>İletişim ve kapsayıcı bağlantı noktalarıyla konak bağlantı noktalarını eşlemeyi yapılandırın
Hizmetin iletişim sağlayabilmesi için bir uç nokta gerekir.  Şimdi ServiceManifest.xml dosyasındaki `Endpoint` öğesine protokol, bağlantı noktası ve tür ekleyebilirsiniz. Bu hızlı başlangıçta kapsayıcı hizmeti 80 numaralı bağlantı noktasını dinler: 

```xml
<Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
```
`UriScheme` değerinin sağlanması, kapsayıcı uç noktasını bulunabilirlik için Service Fabric Adlandırma hizmetine otomatik olarak kaydeder. Bu makalenin sonunda tam bir ServiceManifest.xml örnek dosyası verilmiştir. 

ApplicationManifest.xml dosyasının `ContainerHostPolicies` kısmında bir `PortBinding` ilkesi belirterek kapsayıcı bağlantı noktasından ana bilgisayar bağlantı noktasına eşleme yapılandırın.  Bu Hızlı Başlangıç için `ContainerPort` 80'dir ve `EndpointRef`, "MyContainerServiceTypeEndpoint" değeridir (hizmet bildiriminde tanımlanan uç noktası).  80 numaralı bağlantı noktasında hizmete gelen istekler, kapsayıcı üzerindeki 80 numaralı bağlantı noktasıyla eşlenir.  

```xml
<ServiceManifestImport>
...
  <ConfigOverrides />
  <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
    </ContainerHostPolicies>
  </Policies>
</ServiceManifestImport>
```

Bu makalenin sonunda tam bir ApplicationManifest.xml örnek dosyası verilmiştir.

## <a name="create-a-cluster"></a>Küme oluşturma
Uygulamayı Azure’daki bir kümeye dağıtmak için kendi kümenizi oluşturabilir veya bir grup kümesi kullanabilirsiniz.

Grup kümeleri, Azure üzerinde barındırılan ve Service Fabric ekibi tarafından sunulan ücretsiz, sınırlı süreli Service Fabric kümeleridir. Bu kümelerde herkes uygulama dağıtabilir ve platform hakkında bilgi edinebilir. Bir grup kümesine erişmek için [yönergeleri takip edin](http://aka.ms/tryservicefabric).  

Kendi kümenizi oluşturma hakkında daha fazla bilgi için bkz. [Azure'da Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

Aşağıdaki adımda kullandığınız bağlantı uç noktasını not edin.  

## <a name="deploy-the-application-to-azure-using-visual-studio"></a>Visual Studio kullanarak uygulamayı Azure’a dağıtma
Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

Çözüm Gezgini'nde **MyFirstContainer**’a sağ tıklayın ve **Yayımla**’yı seçin. Yayımla iletişim kutusu görüntülenir.

![Yayımla İletişim Kutusu](./media/service-fabric-quickstart-dotnet/publish-app.png)

Kümenin bağlantı uç noktasını **Bağlantı Uç Noktası** alanına girin. Grup kümesine kaydolurken bağlantı uç noktası tarayıcıda sağlanır. Örneğin: `winh1x87d1d.westus.cloudapp.azure.com:19000`.  **Yayımla**'ya tıkladığınızda uygulama dağıtılır.

Bir tarayıcı penceresi açıp http://winh1x87d1d.westus.cloudapp.azure.com:80 adresine gidin. IIS varsayılan web sayfasını görmeniz gerekir: ![IIS varsayılan web sayfası][iis-default]

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Tam Service Fabric uygulaması ve hizmet bildirimleri örneği
Bu hızlı başlangıçta kullanılan tam hizmet ve uygulama bildirimleri aşağıda verilmiştir.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>microsoft/iis:nanoserver</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="MyContainerService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="MyContainerService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="[MyContainerService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta şunları öğrendiniz:
> [!div class="checklist"]
> * Docker görüntü kapsayıcısını paketleme
> * İletişimi yapılandırma
> * Service Fabric uygulamasını oluşturma ve paketleme
> * Kapsayıcı uygulamasını Azure’a dağıtma

* [Service Fabric’te kapsayıcı](service-fabric-containers-overview.md) çalıştırma hakkında daha fazla bilgi edinin.
* [Kapsayıcı içinde .NET uygulaması dağıtma](service-fabric-host-app-in-a-container.md) öğreticisini okuyun.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
* GitHub’da [Service Fabric kapsayıcı kod örneklerine](https://github.com/Azure-Samples/service-fabric-containers) bakın.

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png
