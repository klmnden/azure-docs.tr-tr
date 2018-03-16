---
title: "Azure Service Fabric mikro (Önizleme) containerize nasıl"
description: "Azure Service Fabric, Service Fabric mikro containerize yeni bir işlevsellik ekledi. Bu özellik şu anda önizleme sürümündedir."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: anmolah
editor: anmolah
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/04/2017
ms.author: anmola
ms.openlocfilehash: e66e488d8e547e828c014b105a816a14726e5005
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="how-to-containerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a>Service Fabric Reliable Services ve Reliable Actors (Önizleme) containerize nasıl

Service Fabric containerizing Service Fabric mikro (Reliable Services ve güvenilir dayalı aktör Hizmetleri) destekler. Daha fazla bilgi için bkz: [service fabric kapsayıcıları](service-fabric-containers-overview.md).

Bu özellik Önizleme sürümünde olduğu ve bu makalede bir kapsayıcı içinde çalışan hizmetiniz için çeşitli adımlar sağlar.  

> [!NOTE]
> Bu özellik Önizleme aşamasındadır ve üretimde desteklenmiyor. Şu anda bu özellik yalnızca Windows için çalışır. Kapsayıcıları çalıştırmak için küme Windows Server 2016 kapsayıcılarla çalıştırması gerekir.

## <a name="steps-to-containerize-your-service-fabric-application"></a>Service Fabric uygulamanızı containerize adımları

1. Service Fabric uygulamanızı Visual Studio'da açın.

2. Sınıf ekleme [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) projenize. Bu sınıftaki Service Fabric çalışma zamanı ikili dosyaları, uygulamanızın içinde bir kapsayıcı içinde çalışırken doğru bir şekilde yüklemek için bir yardımcı kodudur.

3. Containerize, yükleyici programı girişi adresindeki başlatmak istediğiniz her kod paketi noktası. Program giriş noktası dosyanıza aşağıdaki kod parçacığında gösterildiği statik oluşturucu ekleyin.

  ```csharp
  namespace MyApplication
  {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is the entry point of the service host process.
          /// </summary>
          private static void Main()
          {
  ```

4. Derleme ve [paket](service-fabric-package-apps.md#Package-App) projenizi. Derleme ve bir paket oluşturmak için Çözüm Gezgini'nde uygulama projesine sağ tıklayın ve seçin **paket** komutu.

5. Her kod paketi için containerize, PowerShell betiğini çalıştırmak istediğiniz [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1). Kullanım aşağıdaki gibidir:
  ```powershell
    $codePackagePath = 'Path to the code package to containerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
    $applicationExeName = 'Name of the ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  Komut dosyası, Docker yapıtlarla $dockerPackageOutputDirectoryPath adresindeki bir klasör oluşturur. Tüm bağlantı noktalarını kullanıma, gereksinimlerinize göre vb. Kurulum betikleri çalıştırmak için oluşturulan Dockerfile değiştirin.

6. Sonra [yapı](service-fabric-get-started-containers.md#Build-Containers) ve [itme](service-fabric-get-started-containers.md#Push-Containers) Docker kapsayıcısı paketinizi deponuza.

7. ApplicationManifest.xml ve ServiceManifest.xml kapsayıcı görüntü, havuz bilgileri, kayıt defteri kimlik doğrulaması ve bağlantı noktası ana bilgisayar eşleme eklemek için değiştirin. Bildirimleri değiştirmek için bkz: [Azure Service Fabric kapsayıcı uygulama oluşturma](service-fabric-get-started-containers.md). Hizmet bildirimi kod paketi tanımında karşılık gelen kapsayıcı görüntüsüyle değiştirilmesi gerekiyor. EntryPoint ContainerHost türüne değiştirdiğinizden emin olun.

  ```xml
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
  </EntryPoint>
  <!-- Pass environment variables to your container: -->    
</CodePackage>
  ```

8. Yineleyici ve hizmet uç noktası için bağlantı noktası ana bilgisayar eşlemesi ekleyin. Her iki, bu bağlantı noktalarına çalışma zamanında Service Fabric tarafından atanmış olduğundan, ContainerPort eşleme için atanan bağlantı noktası kullanmak için sıfır olarak ayarlanır.

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. Bu uygulamayı test etmek için 5.7 veya daha yüksek sürümünü çalıştıran bir kümeye dağıtmak için gerekir. Ayrıca, düzenlemek ve bu önizleme özelliği etkinleştirmek için küme ayarlarını güncelleştirmek gerekir. Bu adımları [makale](service-fabric-cluster-fabric-settings.md) sonraki gösterilen ayarı eklemek için.
```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
```
10. Sonraki [dağıtmak](service-fabric-deploy-remove-applications.md) bu kümeye düzenlenen uygulama paketi.

Şimdi, küme çalışmayı kapsayıcılı Service Fabric uygulaması olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric’te kapsayıcı](service-fabric-get-started-containers.md) çalıştırma hakkında daha fazla bilgi edinin.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
