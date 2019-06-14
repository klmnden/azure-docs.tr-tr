---
title: Azure Service Fabric hizmetleriniz Windows üzerinde kapsayıcılı hale getirme
description: Service Fabric Reliable Services ve Reliable Actors hizmetleriniz Windows üzerinde kapsayıcılı hale getirme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: anmolah
editor: roroutra
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/23/2018
ms.author: aljo, anmola
ms.openlocfilehash: caed77234646654d151b64d2c80b7231342f6d8c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050481"
---
# <a name="containerize-your-service-fabric-reliable-services-and-reliable-actors-on-windows"></a>Service Fabric güvenilir hizmetler ve Windows üzerinde Reliable Actors kapsayıcılı hale getirme

Service Fabric, kapsayıcılı hale getirmek Service Fabric mikro hizmetler (Reliable Services ve Reliable Actor bağlı hizmetler) destekler. Daha fazla bilgi için [service fabric kapsayıcıları](service-fabric-containers-overview.md).

Bu belgede, bir Windows kapsayıcısının içinde çalışan hizmetinizi almak için yönergeler verilmektedir.

> [!NOTE]
> Şu anda bu özellik yalnızca Windows için çalışır. Kapsayıcıları çalıştırmak için kümenin kapsayıcılar ile Windows Server 2016 çalıştırmalıdır.

## <a name="steps-to-containerize-your-service-fabric-application"></a>Service Fabric uygulamanızı kapsayıcılı hale getirme adımları

1. Service Fabric uygulamanızı Visual Studio'da açın.

2. Sınıf ekleme [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) projenize. Bu sınıfın kodu, doğru kapsayıcısının içinde çalışan uygulamanız içinde Service Fabric çalışma zamanı ikili yükleme yardımcıdır.

3. Kapsayıcılı hale getirme, yükleyici programı girişi sırasında başlatmak istediğiniz her kod paketi için bu seçeneği işaretleyin. Programın giriş noktası dosyanıza aşağıdaki kod parçacığında gösterilen statik oluşturucuyu ekleyin.

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

4. Derleme ve [paket](service-fabric-package-apps.md#Package-App) projenizi. Derlemek ve bir paket oluşturmak için Çözüm Gezgini'nde uygulama projesine sağ tıklayın ve seçin **paket** komutu.

5. Her kod paketi için kapsayıcılı hale getirme, PowerShell betiğini çalıştırmak istediğiniz [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1). Kullanım aşağıdaki gibidir:

    Tam .NET
      ```powershell
        $codePackagePath = 'Path to the code package to containerize.'
        $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
        $applicationExeName = 'Name of the Code package executable.'
        CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
      ```
    .NET Core
      ```powershell
        $codePackagePath = 'Path to the code package to containerize.'
        $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
        $dotnetCoreDllName = 'Name of the Code package dotnet Core Dll.'
        CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -DotnetCoreDllName $dotnetCoreDllName
      ```
      Betik $dockerPackageOutputDirectoryPath Docker yapıtlarla bir klasör oluşturur. Değiştirmek için oluşturulan Dockerfile `expose` vb. Kurulum betikleri çalıştırma tüm bağlantı noktaları. gereksinimlerinize göre.

6. Sonraki için ihtiyacınız [derleme](service-fabric-get-started-containers.md#Build-Containers) ve [anında iletme](service-fabric-get-started-containers.md#Push-Containers) Docker kapsayıcı paketinize deponuzu.

7. ServiceManifest.xml ve ApplicationManifest.xml kapsayıcı görüntüsü, depo bilgilerini, kayıt defteri kimlik doğrulaması ve bağlantı noktası ana bilgisayar eşlemesi eklemek için değiştirin. Bildirimleri değiştirmek için bkz: [bir Azure Service Fabric kapsayıcı uygulaması oluşturma](service-fabric-get-started-containers.md). Hizmet bildirimindeki kod paket tanımı, karşılık gelen kapsayıcı görüntüsü ile değiştirilmesi gerekir. EntryPoint ContainerHost türüne değiştirdiğinizden emin olun.

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

8. Yineleyici ve hizmet uç noktası için bağlantı noktası ana bilgisayar eşlemesi ekleyin. Bu her iki bağlantı noktası, Service Fabric tarafından çalışma zamanında atanmış olduğundan ContainerPort eşleme için atanan bağlantı noktasını kullanmak için sıfır olarak ayarlanır.

   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
   </ContainerHostPolicies>
   </Policies>
   ```

9. Kapsayıcı yalıtım modunu yapılandırmak için bkz: [yapılandırma yalıtım modu]( https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-containers#configure-isolation-mode). Windows, kapsayıcılar için iki yalıtım modunu destekler: İşlem ve Hyper-V. Aşağıdaki kod parçacıkları, uygulama bildirimi dosyasında yalıtım modunun nasıl belirtildiğine gösterir.

   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code" Isolation="process">
   ...
   </ContainerHostPolicies>
   </Policies>
   ```
   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
   ...
   </ContainerHostPolicies>
   </Policies>
   ```

> [!NOTE] 
> Varsayılan olarak, Service Fabric uygulamaları Service Fabric çalışma zamanı, uygulamaya özgü isteklerini kabul eden bir uç nokta biçiminde erişimi. Uygulama, güvenilmeyen kod barındırdığında bu erişimi devre dışı bırakmayı düşünün. Daha fazla bilgi için lütfen bkz [en iyi güvenlik uygulamaları Service fabric'te](service-fabric-best-practices-security.md#platform-isolation). Service Fabric çalışma zamanı erişimi devre dışı bırakmak için aşağıdaki gibi içeri aktarılan hizmet bildirimi için karşılık gelen uygulama bildiriminin ilkeler bölümünde aşağıdaki ayarı ekleyin:
>
```xml
  <Policies>
      <ServiceFabricRuntimeAccessPolicy RemoveServiceFabricRuntimeAccess="true"/>
  </Policies>
```
>

10. Bu uygulamayı test etmek için 5.7 veya üzeri sürümü çalıştıran bir kümeye dağıtmak gerekir. Çalışma zamanı sürüm 6.1 veya daha düşük, düzenleyin ve bu önizleme özelliğini etkinleştirmek için küme ayarları güncelleştirmeniz gerekir. Bu adımları izleyerek [makale](service-fabric-cluster-fabric-settings.md) sonraki gösterilen ayarı eklemek için.
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

11. Sonraki [dağıtma](service-fabric-deploy-remove-applications.md) bu kümeye düzenlenen uygulama paketi.

Artık kümenize çalışan kapsayıcılı bir Service Fabric uygulaması olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric’te kapsayıcı](service-fabric-get-started-containers.md) çalıştırma hakkında daha fazla bilgi edinin.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
