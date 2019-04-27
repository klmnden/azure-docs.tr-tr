---
title: Yerel, Azure Service Fabric Küme kurulumu sorunlarını giderme | Microsoft Docs
description: Bu makalede, yerel geliştirme kümenizin sorun giderme önerileri kümesi sağlanmıştır.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: chackdan
editor: ''
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 8bb32b2bded061bd19bcd7cfda4ef259a75b0626
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60864448"
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Yerel geliştirme kümesi Kurulumu sorunlarını giderme
Yerel, Azure Service Fabric geliştirme kümesi ile etkileşim sırasında bir sorunla çalıştırırsanız, olası çözümler için aşağıdaki önerileri gözden geçirin.

## <a name="cluster-setup-failures"></a>Küme ayarlama hatalarıyla
### <a name="cannot-clean-up-service-fabric-logs"></a>Service Fabric günlüklerini temizleme olamaz
#### <a name="problem"></a>Sorun
DevClusterSetup betiği çalıştırırken, aşağıdaki hatayı görürsünüz:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatmak ve yönetici olarak yeni bir PowerShell penceresi açın. Artık betik başarılı bir şekilde çalıştırabilirsiniz.

## <a name="cluster-connection-failures"></a>Küme bağlantı hataları

### <a name="type-initialization-exception"></a>Türü başlatma özel durumu
#### <a name="problem"></a>Sorun
PowerShell kümeye bağlanırken, hata Typeınitializationexception System.Fabric.Common.AppTrace için bkz.

#### <a name="solution"></a>Çözüm
Yol değişkeninize yükleme sırasında düzgün ayarlanmadı. Windows dışı oturum açın ve yeniden oturum açın. Bu, path yeniler.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Küme bağlantısı "Nesnesi kapalı ile" başarısız olur.
#### <a name="problem"></a>Sorun
Connect-ServiceFabricCluster yapılan bu gibi bir hata ile başarısız olur:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatmak ve yönetici olarak yeni bir PowerShell penceresi açın.

### <a name="fabric-connection-denied-exception"></a>Fabric bağlantısı reddedildi özel durumu
#### <a name="problem"></a>Sorun
Visual Studio'dan hata ayıklama için FabricConnectionDeniedException hata alıyorum.

#### <a name="solution"></a>Çözüm
Bu hata genellikle bir hizmet ana bilgisayarı işlemi el ile başlatmayı denerseniz oluşur.

Herhangi hizmet projelerini çözümünüzdeki başlangıç projesi olarak ayarla olmadığından emin olun. Yalnızca Service Fabric uygulaması projeleri, başlangıç projesi ayarlamanız gerekir.

> [!TIP]
> Aşağıdaki Kurulum, anormal bir şekilde davranmasına yerel kümenize başlar, yerel Küme Yöneticisi sistem tepsisine kullanarak sıfırlayabilirsiniz. Bu, yeni bir tane ayarlama ve mevcut küme kaldırır. Dağıtılan tüm uygulamalar ve ilişkili verilerin kaldırıldığını unutmayın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Anlama ve sistem durumu raporlarını kümenizle sorun giderme](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)

