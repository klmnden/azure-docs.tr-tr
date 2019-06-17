---
title: Service Fabric Mesh uygulamalarını derlemek için Windows geliştirme ortamı ayarlama | Microsoft Docs
description: Service Fabric Mesh uygulaması geliştirebilmek ve bunu Azure Service Fabric Mesh'e dağıtabilmek için Windows geliştirme ortamınızı ayarlayın.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: 5ab817c65ab562f37b456cc3589624c1876084f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428205"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Service Fabric Mesh uygulamalarını derlemek için Windows geliştirme ortamınızı ayarlayın

Windows geliştirme makinenizde Azure Service Fabric Örgü uygulamalar geliştirip çalıştırmanızı için gerekir:

* Docker
* Visual Studio 2017 veya üstü
* Service Fabric Mesh çalışma zamanı
* Mesh Service Fabric SDK ve araçları.

Ve Windows'ın aşağıdaki sürümlerinden biri:

* Windows 10 1709 (Fall Creators update) ya da 1803 (Enterprise, Professional veya eğitim) sürümleri (Windows 10 Nisan 2018 güncelleştirmesi)
* Windows Server 1709 sürümü
* Windows Server sürümü 1803

Yüklü olan her şeyi size yardım alarak çalıştırdığınız Windows sürümü üzerinde aşağıdaki yönergeleri sağlar.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="visual-studio"></a>Visual Studio

Service Fabric Mesh uygulama dağıtmak için Visual Studio 2017 veya üstü gereklidir. [Sürüm 15.6.0'ı][download-visual-studio] veya üstünü yükleyin ve aşağıdaki iş yüklerini etkinleştirin:

* ASP.NET ve web geliştirme
* Azure geliştirme

## <a name="install-docker"></a>Docker'ı yükleme

Docker'ın yüklü zaten varsa, en son sürüm olduğundan emin olun. Yeni bir sürümü kullanıma bağlıdır, ancak el ile en son sürümü kullandığınızdan emin olun, docker, isteyebilir.

#### <a name="install-docker-on-windows-10"></a>Windows 10 Docker'ı yükleyin

Service Fabric Mesh tarafından kullanılan kapsayıcılı Service Fabric uygulamalarını desteklemek için en yeni [Docker Community Edition for Windows][download-docker] sürümünü indirin ve yükleyin.

Yükleme sırasında, istendiğinde **Linux kapsayıcıları yerine Windows kapsayıcılarını kullan**'ı seçin.

Hyper-V makinenizde etkin değilse, bunu etkinleştirmek Docker'ın yükleyicisi sunar. Bu seçenek sunulursa **Tamam**'a tıklayın.

#### <a name="install-docker-on-windows-server-2016"></a>Windows Server 2016'da Docker'ı yükleyin

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

> [!IMPORTANT]
> Küme oluşturabilmeniz için Docker'in çalışıyor olması **gerekir**.
> Terminal penceresi açarak ve hata oluşup oluşmadığını görmek için `docker ps` komutunu çalıştırarak Docker'ı çalışmasını test edin. Yanıt bir hata göstermiyorsa, Docker çalışıyor ve siz de küme oluşturmaya hazırsınız demektir.

> [!Note]
> Üzerinde geliştiriyorsanız, Windows Fall Creators update (1709 sürümü) makine, yalnızca Windows sürüm 1709 docker görüntülerini kullanabilirsiniz.
> Windows üzerinde geliştirme yapıyorsanız 10 Nisan 2018 Güncelleştirmesi (sürüm 1803) makine, ya da Windows sürüm 1709 veya 1803 docker görüntülerini kullanabilirsiniz.

Visual Studio kullanıyorsanız, yoksa, Visual Studio yerel bir küme sizin için oluşturur çünkü bu bölümü atlayabilirsiniz.

Oluştururken veya aynı anda tek bir Service Fabric uygulama çalışan hata ayıklama performansı için en iyi bir tek düğümlü yerel geliştirme kümesi oluşturun. Aynı anda birden çok uygulama çalıştırıyorsanız, beş düğümlü yerel geliştirme kümesi oluşturun. Kümeyi dağıtmak veya bir Service Fabric Mesh projede hata ayıklaması çalıştırılması gerekir.

Artık çalışma zamanı, SDK, Visual Studio Araçları, Docker'ı yükleme ve Docker çalışmasını sonra bir geliştirme kümesi oluşturun.

1. PowerShell pencerenizi kapatın.
2. Yönetici olarak yeni, yükseltilmiş bir PowerShell penceresi açın. Yüklediğiniz en son yüklenen Service Fabric modüllerinin açılması için bu adım gereklidir.
3. Aşağıdaki PowerShell komutunu çalıştırarak geliştirme kümesini oluşturun:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateMeshCluster -CreateOneNodeCluster
    ```
4. Yerel küme yöneticisi aracını başlatmak için, aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```
5. Küme hizmeti sonra Yöneticisi Aracı (, sistem tepsisinde görünür) çalışıyor olduğundan, sağ tıklayın ve tıklayın **yerel kümeyi başlatın**.

![Şekil 1 - başlangıç yerel kümedeki](./media/service-fabric-mesh-howto-setup-developer-environment-sdk/start-local-cluster.png)

Artık Service Fabric Mesh uygulamaları oluşturmaya hazırsınız!

## <a name="next-steps"></a>Sonraki adımlar

[Azure Service Fabric uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md) öğreticisini okuyun.

Bul yanıtlar [sık sorulan sorular ve bilinen sorunların](service-fabric-mesh-faq.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://aka.ms/sfmesh_vs2017tools
[download-visual-studio]: https://www.visualstudio.com/downloads/