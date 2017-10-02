---
title: "Azure mikro hizmetleri için Windows geliştirme ortamı ayarlama | Microsoft Docs"
description: "Çalışma zamanını, SDK'yı ve araçları yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra Windows üzerinde uygulama derlemek için hazır hale gelirsiniz."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2017
ms.author: ryanwi, mikhegn
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 0691f26168feacf290b732afd7dfd680a2537179
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---
# <a name="prepare-your-development-environment-on-windows"></a>Windows üzerinde geliştirme ortamınızı hazırlama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Windows geliştirme makinenizde [Azure Service Fabric uygulamaları][1] derlemek ve çalıştırmak için çalışma zamanını, SDK'yı ve araçları yükleyin. Ayrıca, SDK'da bulunan Windows PowerShell betiklerinin çalıştırılmasını da etkinleştirmeniz gerekir.

## <a name="prerequisites"></a>Önkoşullar
### <a name="supported-operating-system-versions"></a>Desteklenen işletim sistemi sürümleri
Geliştirme için şu işletim sistemi sürümleri desteklenir:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 varsayılan olarak yalnızca Windows PowerShell 2.0 içerir. Service Fabric PowerShell cmdlet’leri PowerShell 3.0 veya üzerini gerektirir. Microsoft Yükleme Merkezi'nden [Windows PowerShell 5.0'ı indirebilirsiniz][powershell5-download].
> 
> 

## <a name="install-the-sdk-and-tools"></a>SDK'yı ve araçları yükleme
### <a name="to-use-visual-studio-2017"></a>Visual Studio 2017’yi kullanmak için
Service Fabric Araçları, Visual Studio 2017’de Azure Geliştirme ve Yönetim iş yükünün parçasıdır. Bu iş yükünü Visual Studio yüklemenizin bir parçası olarak etkinleştirin.
Ayrıca Web Platformu Yükleyicisini kullanarak Microsoft Azure Service Fabric SDK'sını da yüklemeniz gerekir.

* [Microsoft Azure Service Fabric SDK'sını yükleyin][core-sdk]

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>Visual Studio 2015'i kullanmak için (Visual Studio 2015 Güncelleştirme 2 veya üzeri gerekir)
Visual Studio 2015 için Service Fabric araçları Web Platformu Yükleyicisi kullanılarak SDK ile birlikte yüklenir:

* [Microsoft Azure Service Fabric SDK’sını ve Araçları yükleyin][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Yalnızca SDK'yı yükleme
Yalnızca SDK'yı yüklemeniz gerekiyorsa bu paketi yükleyebilirsiniz:
* [Microsoft Azure Service Fabric SDK'sını yükleyin][core-sdk]

Geçerli sürümler şunlardır:
* Service Fabric SDK'sı 2.8.211
* Service Fabric çalışma zamanı 6.0.211
* Visual Studio 2015 1.7.50721 için Service Fabric Araçları
* Visual Studio 2017 Güncelleştirme 3, Visual Studio 1.7.20170817 için Service Fabric Araçlarını içerir
* Visual Studio 2017 Güncelleştirme 4 Önizleme 1, (15.4.0 Önizleme 1.0) Visual Studio 1.7.20170721 için Service Fabric Araçlarını içerir

Desteklenen sürümlerin listesi için bkz. [Service Fabric desteği](service-fabric-support.md)

## <a name="enable-powershell-script-execution"></a>PowerShell betik yürütmesini etkinleştirme
Service Fabric, yerel geliştirme merkezi oluşturmak ve Visual Studio'dan uygulamaları dağıtmak için Windows PowerShell betiklerini kullanır. Varsayılan olarak, Windows bu betiklerin çalışmasını engeller. Betikleri etkinleştirmek için PowerShell yürütme ilkenizi değiştirmeniz gerekir. PowerShell'i yönetici olarak açın ve şu komutu girin:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Sonraki adımlar
Artık geliştirme ortamınızı ayarlamayı tamamladığınıza göre, uygulama derlemeye ve çalıştırmaya başlayın.

* [Visual Studio'da ilk Service Fabric uygulamanızı oluşturma](service-fabric-create-your-first-application-in-visual-studio.md)
* [Yerel kümenizde uygulamaları dağıtmayı ve yönetmeyi öğrenin](service-fabric-get-started-with-a-local-cluster.md)
* [Programlama modelleri hakkında bilgi edinin: Reliable Services ve Reliable Actors](service-fabric-choose-framework.md)
* [GitHub'da Service Fabric kod örneklerine bakın](https://aka.ms/servicefabricsamples)
* [Service Fabric Explorer kullanarak kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)
* [Platforma yönelik daha geniş kapsamlı bilgi edinmek için Service Fabric öğrenme yolunu uygulayın](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric kampanya sayfası"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI bağlantısı"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI bağlantısı"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI bağlantısı"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395

