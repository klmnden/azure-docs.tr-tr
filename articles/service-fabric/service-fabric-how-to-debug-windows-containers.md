---
title: Service Fabric ve VS ile Windows kapsayıcıları hata ayıklama | Microsoft Docs
description: Windows Azure Service Fabric Visual Studio 2017 kullanarak kapsayıcılarında hata ayıklama öğrenin.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/14/2018
ms.author: mikhegn
ms.openlocfilehash: bca33fe187668d38d4451b2de5b9e54d86e40ba9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655825"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2017"></a>Nasıl yapılır: Windows Azure Service Fabric Visual Studio 2017 kullanarak kapsayıcılarında hata ayıklama

Visual Studio 2017 güncelleştirme 7 (15.7), Service Fabric Hizmetleri olarak kapsayıcıları .NET uygulamalarında ayıklayabilirsiniz. Bu makalede, ortamınızı yapılandırın ve yerel bir Service Fabric kümede çalışan bir kapsayıcı bir .NET uygulamasında hata ayıklama gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Bu Hızlı Başlangıç Windows 10'izleyin [Windows kapsayıcıları çalıştırmak için Windows 10 yapılandırma](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* Windows Server 2016'da, bu hızlı başlangıç izleyin [Windows kapsayıcıları çalıştırmak için Windows 2016'yı yapılandırma](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* İzleyerek yerel Service Fabric ortamınızı ayarlayın [Windows geliştirme ortamınızı hazırlama](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Kapsayıcıları hata ayıklamak için geliştirme ortamınızı yapılandırma

1. Windows hizmeti için Docker sonraki adımla devam etmeden önce çalıştığından emin olun.

1. DNS çözümlemesi kapsayıcılar arasındaki desteklemek için yerel geliştirme kümenizi kurmak makine adını kullanarak gerekir.
    1. Yönetici olarak PowerShell'i açın
    1. Genellikle SDK Küme kurulumu klasörüne gidin `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`
    1. Komut dosyasını çalıştırmak `DevClusterSetup.ps1` parametresi `-UseMachineName`

    ``` PowerShell
      C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1 -UseMachineName
    ```

    > [!NOTE]
    > Kullanabileceğiniz `-CreateOneNodeCluster` tek düğümlü bir Küme kurulumu için. Varsayılan yerel beş düğümlü bir küme oluşturur.
    >

    Service Fabric DNS hizmeti hakkında daha fazla bilgi için bkz: [Azure Service Fabric DNS hizmetinde](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-dnsservice).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Service Fabric kapsayıcılarında hata ayıklama bilinen sınırlamaları

Service Fabric ve olası çözümlerini hata ayıklama kapsayıcılarında bilinen sınırlamalarla listesi aşağıdadır:

* Localhost için ClusterFQDNorIP kullanarak DNS çözümlemesi kapsayıcılarında desteklemez.
    * Çözüm: makine adı (yukarıya bakın) kullanarak yerel küme ayarlama
* Windows10 bir sanal makinede çalışan DNS yanıtı kapsayıcısı için geri alamayacaksınız.
    * Çözüm: UDP Sağlama boşaltması IPv4 NIC üzerinde sanal makineleri için devre dışı bırak
    * Lütfen bu makinedeki ağ performansı bozar unutmayın.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Docker Compose kullanarak uygulama dağıtıldıysa DNS kullanarak aynı uygulama Hizmetleri'nde çözme hizmet adı Windows10 üzerinde çalışmıyor
    * Çözüm: servicename.applicationname hizmet uç noktaları gidermek için kullanın.
    * https://github.com/Azure/service-fabric-issues/issues/1062
* IP adresi için ClusterFQDNorIP kullanıyorsanız, ana bilgisayardaki birincil IP değiştirme DNS işlevselliği çalışmamasına neden olur.
    * Çözüm: ana bilgisayarda yeni birincil IP kullanarak küme oluşturun veya makine adı kullanın. Bu tasarım gereğidir.
* Küme ile oluşturulduğundan FQDN ağda çözümlenebilir değil DNS başarısız olur.
    * Çözüm: yerel küme ana bilgisayarın birincil IP kullanarak yeniden oluşturun. Bu tasarım gereğidir.
* Bir kapsayıcı hata ayıklarken, docker günlükleri yalnızca Service Fabric Explorer dahil olmak üzere hizmet doku API'leri üzerinden değil Visual Studio çıktı penceresinde kullanılabilir

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Docker kapsayıcılarında Service Fabric üzerinde çalışan bir .NET uygulama hata ayıklama

1. Visual Studio Yönetici olarak çalıştırın.

1. Varolan bir .NET uygulama açın veya yeni bir tane oluşturun.

1. Projeye sağ tıklayın ve seçin **Ekle -> kapsayıcı Orchestrator desteği Service Fabric ->**

1. Tuşuna **F5** uygulama hata ayıklama başlatılamıyor.

    Visual Studio .NET ve .NET Core konsol ve ASP.NET proje türleri destekler.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric ve kapsayıcıları özellikleri hakkında daha fazla bilgi için lütfen bu bağlantıyı izleyin: [Service Fabric kapsayıcıları genel bakış](service-fabric-containers-overview.md).