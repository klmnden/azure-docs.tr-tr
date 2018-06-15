---
title: Yerel Azure Service Fabric Küme kurulumu sorunlarını giderme | Microsoft Docs
description: Bu makalede, yerel geliştirme kümeniz sorun giderme önerileri bir dizi kapsar
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: a7f58914fd6e498e717e19bfea11c9e3fcfc0399
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212026"
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Yerel geliştirme Küme kurulumu sorunlarını giderme
Yerel Azure Service Fabric geliştirme kümenizle etkileşim sırasında bir sorun çalıştırırsanız, olası çözümler için aşağıdaki önerileri gözden geçirin.

## <a name="cluster-setup-failures"></a>Küme kurulumu hataları
### <a name="cannot-clean-up-service-fabric-logs"></a>Service Fabric günlüklerini temizleyemiyor
#### <a name="problem"></a>Sorun
DevClusterSetup komut dosyası çalıştırılırken, aşağıdaki hatayı görürsünüz:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatın ve yönetici olarak yeni bir PowerShell penceresi açın. Komut dosyası artık başarılı bir şekilde çalıştırabilirsiniz.

## <a name="cluster-connection-failures"></a>Küme bağlantı hataları

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
Geçerli PowerShell penceresini kapatın ve yönetici olarak yeni bir PowerShell penceresi açın.

### <a name="fabric-connection-denied-exception"></a>Doku bağlantı reddedildi özel durumu
#### <a name="problem"></a>Sorun
Visual Studio'da hata ayıklama sırasında bir FabricConnectionDeniedException hatası alırsınız.

#### <a name="solution"></a>Çözüm
Bu hata, genellikle bir hizmet ana bilgisayar işlemi el ile başlatmayı denerseniz oluşur.

Çözümünüzdeki başlangıç projesi olarak ayarla tüm hizmet projeleri yok emin olun. Yalnızca Service Fabric uygulaması projeleri başlangıç projesi ayarlanmalıdır.

> [!TIP]
> Kurulum aşağıdaki olduğunda yerel kümenizdeki anormal olarak davranacak şekilde başlar, yerel Küme Yöneticisi sistemi Tepsisi uygulaması kullanarak sıfırlayabilirsiniz. Bu, yeni bir tane ayarlama ve mevcut küme kaldırır. Tüm dağıtılan uygulamaları ve ilişkili veriler kaldırılır unutmayın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Anlama ve sistem durumu raporlarını kümenizle sorun giderme](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)

