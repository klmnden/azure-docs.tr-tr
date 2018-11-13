---
title: Service Fabric Mesh uygulamalarını derlemek için Windows geliştirme ortamı ayarlama | Microsoft Docs
description: Service Fabric Mesh uygulaması geliştirebilmek ve bunu Azure Service Fabric Mesh'e dağıtabilmek için Windows geliştirme ortamınızı ayarlayın.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 08/08/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: 0531985cbab9c10b4df8ea3f27ac6c7903790da5
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978239"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Service Fabric Mesh uygulamalarını derlemek için Windows geliştirme ortamınızı ayarlayın

Windows geliştirme makinenizde Azure Service Fabric Mesh uygulamaları derlemek ve çalıştırmak için Service Fabric Mesh çalışma zamanını, SDK'yı ve araçları yükleyin.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Desteklenen işletim sistemi sürümleri

Geliştirme için şu işletim sistemi sürümleri desteklenir:

* Windows 10 (Enterprise, Professional veya Education)
* Windows Server 2016

## <a name="visual-studio"></a>Visual Studio

Service Fabric Mesh uygulamalarını dağıtmak için Visual Studio 2017 gereklidir. [Sürüm 15.6.0'ı][download-visual-studio] veya üstünü yükleyin ve aşağıdaki iş yüklerini etkinleştirin:

* ASP.NET ve web geliştirme
* Azure Geliştirme

## <a name="install-docker"></a>Docker'ı yükleme

#### <a name="windows-10"></a>Windows 10

Service Fabric Mesh tarafından kullanılan kapsayıcılı Service Fabric uygulamalarını desteklemek için en yeni [Docker Community Edition for Windows][download-docker] sürümünü indirin ve yükleyin.

Yükleme sırasında, istendiğinde **Linux kapsayıcıları yerine Windows kapsayıcılarını kullan**'ı seçin.

Hyper-V makinenizde etkin değilse Docker yükleyicisi ile etkinleştirebilirsiniz. Bu seçenek sunulursa **Tamam**'a tıklayın.

#### <a name="windows-server-2016"></a>Windows Server 2016

Hyper-V rolleri etkin değilse PowerShell'i yönetici olarak açın ve aşağıdaki komutu çalıştırarak Hyper-V'yi etkinleştirdikten sonra bilgisayarınızı yeniden başlatın. Daha fazla bilgi için bkz. [Windows Server için Docker Enterprise Edition][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Bilgisayarınızı yeniden başlatın.

Docker'ı yüklemek için PowerShell'i yönetici olarak açın ve aşağıdaki komutu çalıştırın:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sdk-and-tools"></a>SDK ve araçlar

Aşağıdaki sırayla Service Fabric Mesh çalışma zamanını, SDK'yı ve araçları yükleyin.

1. Web Platformu Yükleyicisi'ni kullanarak [Service Fabric Mesh SDK'sını][download-sdkmesh] yükleyin. Bu işlem Microsoft Azure Service Fabric SDK’sını ve çalışma zamanını da yükler.
2. Visual Studio Market'ten [Visual Studio Service Fabric Mesh Araçları (önizleme) uzantısını][download-tools] yükleyin.

## <a name="build-a-cluster"></a>Küme oluşturma

Yerel kümeniz yoksa Visual Studio tarafından oluşturulacağından Visual Studio kullanıyorsanız bu adımı atlayabilirsiniz.

Service Fabric uygulamalarını oluşturur ve çalıştırırken hata ayıklama işleminde en iyi performansı elde etmek için, tek düğümlü bir yerel geliştirme kümesi oluşturmanızı öneririz. Service Fabric Mesh projesini her dağıttığınızda veya projenin hatalarını ayıkladığınızda bu küme çalıştırılmalıdır.

Küme oluşturabilmeniz için Docker'in çalışıyor olması **gerekir**. Terminal penceresi açarak ve hata oluşup oluşmadığını görmek için `docker ps` komutunu çalıştırarak Docker'ı çalışmasını test edin. Yanıt bir hata göstermiyorsa, Docker çalışıyor ve siz de küme oluşturmaya hazırsınız demektir.

Çalışma zamanını, SDK'ları ve Visual Studio araçlarını yükledikten sonra geliştirme kümesini oluşturun.

1. PowerShell pencerenizi kapatın.
2. Yönetici olarak yeni, yükseltilmiş bir PowerShell penceresi açın. Yüklediğiniz en son yüklenen Service Fabric modüllerinin açılması için bu adım gereklidir.
3. Aşağıdaki PowerShell komutunu çalıştırarak geliştirme kümesini oluşturun:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster -UseMachineName
    ```

4. Yerel küme yöneticisi aracını başlatmak için, aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```

Artık Service Fabric Mesh uygulamaları oluşturmaya hazırsınız!

## <a name="next-steps"></a>Sonraki adımlar

[Azure Service Fabric uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md) öğreticisini okuyun.

[Sık sorulan sorulara](service-fabric-mesh-faq.md) yanıtlar bulun.

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.ServiceFabricMesh
[download-visual-studio]: https://www.visualstudio.com/downloads/