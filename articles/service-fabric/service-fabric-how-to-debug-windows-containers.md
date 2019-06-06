---
title: Windows kapsayıcıları Service Fabric ve VS hata ayıklama | Microsoft Docs
description: Visual Studio 2019'ı kullanarak Azure Service fabric'te Windows kapsayıcılarını hata ayıklama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/14/2019
ms.author: aljo, mikhegn
ms.openlocfilehash: 15f288d5400b49ec05c9ffb936fd2097cc61bae8
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428155"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2019"></a>Nasıl yapılır: Visual Studio 2019'ı kullanarak Azure Service fabric'te Windows kapsayıcılarını hata ayıklama

Visual Studio 2019'ile Service Fabric Hizmetleri kapsayıcılar içindeki .NET uygulamalarına ayıklayabilirsiniz. Bu makalede, ortamınızı yapılandırın ve ardından yerel bir Service Fabric kümesinde çalışan bir kapsayıcı bir .NET uygulamasında hata ayıklama işlemini göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

* Bu Hızlı Başlangıç için Windows 10'da izleyin [Windows kapsayıcılarını çalıştırmaya yönelik Windows 10 yapılandırma](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* Bu Hızlı Başlangıç için Windows Server 2016'da izleyin [Windows kapsayıcılarını çalıştırmaya yönelik Windows 2016'yı yapılandırma](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* Yerel Service Fabric ortamınızı izleyerek ayarlama [Windows üzerinde geliştirme ortamınızı hazırlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Kapsayıcıları hata ayıklamak için geliştirme ortamınızı yapılandırma

1. Docker Windows hizmeti için bir sonraki adımla devam etmeden önce çalışır durumda olduğundan emin olun.

1. Kapsayıcılar arasında DNS çözümlemesi desteklemek için kendi yerel geliştirme kümesi ayarlama makine adını kullanarak gerekir. Bu adımlar, ayrıca ters proxy adresi hizmetleriyle istiyorsanız gereklidir.
   1. Yönetici olarak PowerShell'i açın
   2. Genellikle SDK'sı küme kurulum klasörüne gidin `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`.
   3. Betiği çalıştırın `DevClusterSetup.ps1`

      ``` PowerShell
        C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1
      ```

      > [!NOTE]
      > Kullanabileceğiniz `-CreateOneNodeCluster` tek düğümlü bir Küme kurulumu için. Varsayılan yerel beş düğümlü bir küme oluşturur.
      >

      Service Fabric DNS hizmeti hakkında daha fazla bilgi için bkz. [Azure Service Fabric DNS hizmetinde](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice). Service Fabric kullanma hakkında daha fazla ters proxy kapsayıcıda çalıştırmaya hizmetlerinden bilgi edinmek için [kapsayıcılarda çalıştırılan Hizmetleri için ters proxy özel işlem](service-fabric-reverseproxy.md#special-handling-for-services-running-in-containers).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Service Fabric'teki kapsayıcıları hata ayıklama sırasında bilinen sınırlamalar

Hata ayıklama kapsayıcıları Service Fabric ve olası çözümleri ile bilinen kısıtlamaların listesi aşağıdadır:

* Localhost için ClusterFQDNorIP kullanarak DNS çözümlemesi kapsayıcılarda desteklemeyeceği.
    * Çözüm: Makine adı (yukarıya bakın) kullanarak yerel küme oluşturma
* Windows 10 çalıştıran bir sanal makinede DNS yanıtı geri kapsayıcı elde etmezsiniz.
    * Çözüm: Sanal makineler NIC'de IPv4 için UDP Sağlama boşaltması devre dışı bırak
    * Windows 10 çalıştıran makinede ağ performansı bozar.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Docker Compose kullanarak uygulamayı dağıtıldıysa DNS kullanarak aynı uygulama Hizmetleri'nde çözümleme hizmet adını Windows 10 üzerinde çalışmıyor
    * Çözüm: Hizmet uç noktaları çözümlenecek ServiceName.ApplicationName kullanın
    * https://github.com/Azure/service-fabric-issues/issues/1062
* IP adresi için ClusterFQDNorIP kullanıyorsanız, konaktaki birincil IP değiştirme DNS işlevselliğinin keser.
    * Çözüm: Konakta yeni birincil IP kullanarak bir küme oluşturun veya makine adı kullanın. Bu arıza tasarım gereğidir.
* DNS, ağ üzerinde çözümlenebilen FQDN kümenin oluşturulduğu değilse, başarısız olur.
    * Çözüm: Ana bilgisayarın birincil IP kullanarak yerel kümeye yeniden oluşturun. Bu hata, tasarım gereğidir.
* Bir kapsayıcı hata ayıklarken docker günlükleri yalnızca Service Fabric Explorer dahil olmak üzere Service Fabric API'leri üzerinden değil Visual Studio çıktı penceresinde kullanılabilir

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Docker kapsayıcıları Service Fabric üzerinde çalışan bir .NET uygulamasında hata ayıklama

1. Visual Studio'yu yönetici olarak çalıştırın.

1. Mevcut bir .NET uygulamasını açın veya yeni bir tane oluşturun.

1. Projeye sağ tıklayıp **Ekle -> kapsayıcı Düzenleyicisi desteği Service Fabric ->**

1. Tuşuna **F5** uygulamada hata ayıklamaya başlayın.

    Visual Studio, .NET ve .NET Core konsol ve ASP.NET proje türleri destekler.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kapsayıcıları ve özellikleri hakkında daha fazla bilgi edinmek için Service Fabric kapsayıcıları overview](service-fabric-containers-overview.md) bakın.
