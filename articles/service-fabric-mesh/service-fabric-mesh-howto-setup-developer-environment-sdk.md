---
title: Service Fabric Mesh uygulamaları için Windows geliştirme ortamı ayarlama | Microsoft Docs
description: Service Fabric Mesh uygulaması geliştirebilmek ve bunu Azure Service Fabric Mesh'e dağıtabilmek için Windows geliştirme ortamınızı ayarlayın.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/11/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 96549696013a2dd94741090a0a017b57a3b1e19e
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125170"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-applications"></a>Service Fabric uygulamaları derlemek için Windows geliştirme ortamınızı ayarlayın

Windows geliştirme makinenizde Azure Service Fabric uygulamaları derlemek ve çalıştırmak için Service Fabric çalışma zamanını, SDK'yı ve araçları yükleyin.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Desteklenen işletim sistemi sürümleri

Geliştirme için şu işletim sistemi sürümleri desteklenir:

* Windows 10 (Enterprise, Professional veya Education)
* Windows Server 2016

## <a name="enable-hyper-v"></a>Hyper-V'yi etkinleştirme

Service Fabric uygulamaları oluşturabilmeniz için Hyper-V'niz etkinleştirilmelidir. 

### <a name="windows-10"></a>Windows 10

PowerShell'i yönetici olarak açın ve aşağıdaki komutu çalıştırın:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

Bilgisayarınızı yeniden başlatın. Hyper-V'yi etkinleştirme hakkında daha fazla bilgi için bkz. [Windows 10'da Hyper-V'yi yükleme](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

### <a name="windows-server-2016"></a>Windows Server 2016

PowerShell'i yönetici olarak açın ve aşağıdaki komutu çalıştırın:

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Bilgisayarınızı yeniden başlatın. Hyper-V'yi etkinleştirme hakkında daha fazla bilgi için bkz. [Windows Server 2016'da Hyper-V rolünü yükleme](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server).

## <a name="visual-studio"></a>Visual Studio

Service Fabric uygulamaları dağıtmak için Visual Studio 2017 gereklidir. [Sürüm 15.6.0'ı][download-visual-studio] veya üstünü yükleyin ve aşağıdaki iş yüklerini etkinleştirin:

- ASP.NET ve web geliştirme
- Azure Geliştirme

## <a name="docker"></a>Docker

Service Fabric Mesh tarafından kullanılan kapsayıcılı Service Fabric uygulamalarını desteklemek için Docker'ı yükleyin.

### <a name="windows-10"></a>Windows 10

En son [Windows için Docker Community Edition][download-docker] sürümünü indirin ve yükleyin. 

Yükleme sırasında, istendiğinde **Linux kapsayıcıları yerine Windows kapsayıcılarını kullan**'ı seçin. Bunun ardından oturumunuzu kapatıp yeniden açmanız gerekir. Yeniden oturum açtıktan sonra, Hyper-V'yi daha önce etkinleştirmediyseniz etkinleştirmeniz istenebilir. Hyper-V etkinleştirmeli ve sonra bilgisayarınızı yeniden başlatmalısınız.

Bilgisayarınız yeniden başlatıldıktan sonra, Docker sizden **Kapsayıcılar** özelliğini etkinleştirmenizi ister. Etkinleştirin ve bilgisayarınızı yeniden başlatın.

### <a name="windows-server-2016"></a>Windows Server 2016

Docker'ı yüklemek için aşağıdaki PowerShell komutlarını kullanın. Daha fazla bilgi için bkz. [Windows Server için Docker Enterprise Edition][download-docker-server].

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Bilgisayarınızı yeniden başlatın.

## <a name="sdk-and-tools"></a>SDK ve araçlar

Bağımlı bir sırayla Service Fabric Mesh çalışma zamanını, SDK'yı ve araçları yükleyin.

1. Web Platformu Yükleyicisi'ni kullanarak [Service Fabric Mesh SDK'sını][download-sdkmesh] yükleyin. Bu işlem Microsoft Azure Service Fabric SDK’sını ve çalışma zamanını da yükler.
2. Visual Studio Market'ten [Visual Studio Service Fabric Araçları (önizleme) uzantısını][download-tools] yükleyin.

## <a name="build-a-cluster"></a>Küme oluşturma

Service Fabric uygulamalarını oluşturur ve çalıştırırken hata ayıklama işleminde en iyi performansı elde etmek için, tek düğümlü bir yerel geliştirme kümesi oluşturmanızı öneririz. Service Fabric Mesh projesini her dağıttığınızda veya projenin hatalarını ayıkladığınızda bu küme çalıştırılmalıdır.

Küme oluşturabilmeniz için Docker'in çalışıyor olması **gerekir**. Terminal penceresi açarak ve hata oluşup oluşmadığını görmek için `docker ps` komutunu çalıştırarak Docker'ı çalışmasını test edin. Yanıt bir hata göstermiyorsa, Docker çalışıyor ve siz de küme oluşturmaya hazırsınız demektir.

Çalışma zamanını, SDK'ları ve Visual Studio araçlarını yükledikten sonra geliştirme kümesini oluşturun.

1. PowerShell pencerenizi kapatın.
2. Yönetici olarak yeni, yükseltilmiş bir PowerShell penceresi açın. Yüklediğiniz Service Fabric modüllerinin açılması için bu adım gereklidir.
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

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.ServiceFabricMesh
[download-visual-studio]: https://www.visualstudio.com/downloads/
