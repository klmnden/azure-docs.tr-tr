---
title: "Yerel Service Fabric Küme kurulumu sorunlarını giderme | Microsoft Docs"
description: "Bu makalede, yerel geliştirme kümeniz sorun giderme önerileri bir dizi kapsar"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: aa393f884b564cee81fcf75cc2eff895efea9471
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Yerel geliştirme Küme kurulumu sorunlarını giderme
Yerel Azure Service Fabric geliştirme kümenizle etkileşim sırasında bir sorun çalıştırırsanız, olası çözümler için aşağıdaki önerileri gözden geçirin.

## <a name="cluster-setup-failures"></a>Küme kurulumu hataları
### <a name="cannot-clean-up-service-fabric-logs"></a>Service Fabric günlüklerini temizleyemiyor
#### <a name="problem"></a>Sorun
DevClusterSetup komut dosyası çalıştırılırken, bu gibi bir hata görürsünüz:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatın ve yönetici olarak yeni bir PowerShell penceresi açın. Artık başarıyla komut dosyasını çalıştırmak mümkün olması gerekir.

## <a name="cluster-connection-failures"></a>Küme bağlantı hataları
### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Service Fabric PowerShell cmdlet'leri Azure PowerShell'de tanınmıyor.
#### <a name="problem"></a>Sorun
Service Fabric PowerShell cmdlet'lerinden herhangi birini gibi çalıştırmayı denerseniz `Connect-ServiceFabricCluster` bir Azure PowerShell penceresinde, cmdlet tanınmıyor belirten başarısız olur. Service Fabric cmdlet'leri yalnızca 64-bit ortamlarında çalışır ancak bunun nedeni Azure PowerShell 32 bit sürümünde Windows PowerShell (hatta 64-bit işletim sistemi sürümleri), kullanmasıdır.

#### <a name="solution"></a>Çözüm
Her zaman Windows Powershell'den doğrudan Service Fabric cmdlet'lerini çalıştırın.

> [!NOTE]
> Bu artık gerçekleşmesi gereken şekilde Azure PowerShell'in en son sürümünü özel bir kısayol oluşturmaz.
> 
> 

### <a name="type-initialization-exception"></a>Tür başlatma özel durumu oluştu
#### <a name="problem"></a>Sorun
PowerShell'de kümeye bağlanırken, System.Fabric.Common.AppTrace için Typeınitializationexception hatasına bakın.

#### <a name="solution"></a>Çözüm
Path değişkeni yükleme sırasında düzgün ayarlanmadı. Windows oturumunu kapatmanız ve yeniden oturum açın. Bu yolunuzu yeniler.

### <a name="cluster-connection-fails-with-object-is-closed"></a>"Nesnesi kapalı" Küme bağlantı başarısız
#### <a name="problem"></a>Sorun
Connect-ServiceFabricCluster yapılan bir çağrı şuna benzer bir hata ile başarısız olur:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatın ve yönetici olarak yeni bir PowerShell penceresi açın. Artık başarıyla bağlanabilmek için olması gerekir.

### <a name="fabric-connection-denied-exception"></a>Doku bağlantı reddedildi özel durumu
#### <a name="problem"></a>Sorun
Visual Studio'da hata ayıklama sırasında bir FabricConnectionDeniedException hatası alırsınız.

#### <a name="solution"></a>Çözüm
Bu hata genellikle başlatmak, Service Fabric çalışma zamanını izin vermek yerine bir hizmet ana bilgisayar işlemi el ile başlatmayı deneyin oluşur.

Çözümünüzdeki başlangıç projesi olarak ayarla tüm hizmet projeleri yok emin olun. Yalnızca Service Fabric uygulaması projeleri başlangıç projesi ayarlanmalıdır.

> [!TIP]
> Kurulum aşağıdaki olduğunda yerel kümenizdeki anormal olarak davranacak şekilde başlar, yerel Küme Yöneticisi sistemi Tepsisi uygulaması kullanarak sıfırlayabilirsiniz. Bu, yeni bir tane ayarlama ve mevcut küme kaldırır. Tüm dağıtılan uygulamaları ve ilişkili veriler kaldırılır unutmayın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Anlama ve sistem durumu raporlarını kümenizle sorun giderme](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)

