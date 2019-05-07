---
title: Azure mikro hizmetleri için Windows geliştirme ortamı ayarlama | Microsoft Docs
description: Çalışma zamanını, SDK'yı ve araçları yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra Windows üzerinde uygulama derlemek için hazır hale gelirsiniz.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/30/2019
ms.author: aljo
ms.openlocfilehash: 463b05f57ce0c85ebf1732791cb024335103b780
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153615"
---
# <a name="prepare-your-development-environment-on-windows"></a>Windows üzerinde geliştirme ortamınızı hazırlama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

Windows geliştirme makinenizde [Azure Service Fabric uygulamaları][1] derlemek ve çalıştırmak için Service Fabric çalışma zamanını, SDK'yı ve araçları yükleyin. Ayrıca, SDK'da bulunan [Windows PowerShell betiklerinin çalıştırılmasını da etkinleştirmeniz](#enable-powershell-script-execution) gerekir.

## <a name="prerequisites"></a>Önkoşullar
### <a name="supported-operating-system-versions"></a>Desteklenen işletim sistemi sürümleri
Geliştirme için şu işletim sistemi sürümleri desteklenir:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 desteği:
> - Windows 7 varsayılan olarak yalnızca Windows PowerShell 2.0 içerir. Service Fabric PowerShell cmdlet’leri PowerShell 3.0 veya üzerini gerektirir. Microsoft Yükleme Merkezi'nden [Windows PowerShell 5.0'ı indirebilirsiniz][powershell5-download].
> - Windows 7'de Service Fabric Ters Proxy kullanılamaz.
>

## <a name="install-the-sdk-and-tools"></a>SDK'yı ve araçları yükleme
Web Platformu Yükleyicisi (Webpı), SDK ve araçlarını yüklemek için önerilen yoldur. Webpı kullanarak çalışma zamanı hataları alırsanız, belirli bir Service Fabric sürümü için sürüm notları yükleyicileri için doğrudan bağlantılar da bulabilirsiniz. Sürüm Notları çeşitli yayın duyurularda bulunabilir [Service Fabric ekibi blogu](https://blogs.msdn.microsoft.com/azureservicefabric/).

> [!NOTE]
> Yerel Service Fabric geliştirme kümesi yükseltme desteklenmez.

### <a name="to-use-visual-studio-2017"></a>Visual Studio 2017’yi kullanmak için
Service Fabric Araçları, Visual Studio 2017’de Azure Geliştirme iş yükünün parçasıdır. Bu iş yükünü Visual Studio yüklemenizin bir parçası olarak etkinleştirin.
Ayrıca Web Platformu Yükleyicisini kullanarak Microsoft Azure Service Fabric SDK'sını da yüklemeniz gerekir.

* [Microsoft Azure Service Fabric SDK'sını yükleyin][core-sdk]

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>Visual Studio 2015'i kullanmak için (Visual Studio 2015 Güncelleştirme 2 veya üzeri gerekir)
Visual Studio 2015 için Service Fabric araçları Web Platformu Yükleyicisi kullanılarak SDK ile birlikte yüklenir:

* [Microsoft Azure Service Fabric SDK’sını ve Araçları yükleyin][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Yalnızca SDK'yı yükleme
Yalnızca SDK'yı yüklemeniz gerekiyorsa bu paketi yükleyebilirsiniz:
* [Microsoft Azure Service Fabric SDK'sını yükleyin][core-sdk]

Geçerli sürümler şunlardır:
* Service Fabric SDK'sı ve Araçları 3.3.658
* Service Fabric çalışma zamanı 6.4.658
* Service Fabric Tools Pro Visual Studio 2015 2.4.11116.1
* Visual Studio 2017 15.9 2.4.11024.1 Visual Studio için Service Fabric araçlarını içerir 

Desteklenen sürümlerin listesi için bkz. [Service Fabric sürümleri](service-fabric-versions.md)

> [!NOTE]
> Kümeleri (OneBox) kullanılması desteklenmez veya küme için tek makine yükseltmeleri; OneBox kümeyi silin ve bir küme yükseltmesi gerçekleştirin veya uygulama yükseltme gerçekleştirme herhangi bir sorun olması gerekiyorsa bunu oluşturun. 

## <a name="enable-powershell-script-execution"></a>PowerShell betik yürütmesini etkinleştirme
Service Fabric, yerel geliştirme merkezi oluşturmak ve Visual Studio'dan uygulamaları dağıtmak için Windows PowerShell betiklerini kullanır. Varsayılan olarak, Windows bu betiklerin çalışmasını engeller. Betikleri etkinleştirmek için PowerShell yürütme ilkenizi değiştirmeniz gerekir. PowerShell'i yönetici olarak açın ve şu komutu girin:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```
## <a name="install-docker-optional"></a>Docker (isteğe bağlı) yükleme
[Service Fabric, kapsayıcı düzenleyici](service-fabric-containers-overview.md) bir makine kümesindeki mikro hizmetler dağıtmak için. Yerel geliştirme Kümenizde Windows kapsayıcı uygulaması çalıştırmak için Docker için Windows yüklemeniz gerekir. Alma [Docker CE için Windows (stable)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Docker’ı yükleyip başlattıktan sonra tepsi simgesine sağ tıklayıp **Windows kapsayıcılarına geç** öğesini seçin. Bu adım, Windows temelinde Docker görüntülerini çalıştırmak için gereklidir.

## <a name="next-steps"></a>Sonraki adımlar
Artık geliştirme ortamınızı ayarlamayı tamamladığınıza göre, uygulama derlemeye ve çalıştırmaya başlayın.

* [Oluşturmanıza, dağıtmanıza ve uygulamaları yönetme hakkında bilgi edinin](service-fabric-tutorial-create-dotnet-app.md)
* [Programlama modelleri hakkında bilgi edinin: Reliable Services ve Reliable Actors](service-fabric-choose-framework.md)
* [GitHub'da Service Fabric kod örneklerine bakın](https://aka.ms/servicefabricsamples)
* [Service Fabric Explorer kullanarak kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

[1]: https://azure.microsoft.com/campaigns/service-fabric/ "Service Fabric kampanya sayfası"
[2]: https://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI bağlantısı"
[full-bundle-dev15]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI bağlantısı"
[core-sdk]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI bağlantısı"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
