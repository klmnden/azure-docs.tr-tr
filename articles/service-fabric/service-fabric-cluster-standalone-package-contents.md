---
title: Windows Server için Azure Service Fabric tek başına paketi | Microsoft Docs
description: Açıklama ve Windows Server için Azure Service Fabric tek başına yükleme paketinin içeriğini.
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: maburlik
ms.openlocfilehash: dccdd6518dd97299150892a5629809ea7f708838
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209364"
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Windows Server için Service Fabric tek başına paketinin içeriği
İçinde [indirilen](http://go.microsoft.com/fwlink/?LinkId=730690) Service Fabric tek başına paket, aşağıdaki dosyaları bulacaksınız:

| **Dosya adı** | **Kısa açıklama** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |PowerShell Betiği ClusterConfig.json dosyasında ayarları kullanarak küme oluşturur. |
| RemoveServiceFabricCluster.ps1 |ClusterConfig.json içinde ayarları kullanarak bir küme kaldıran bir PowerShell komut dosyası. |
| AddNode.ps1 |Var olan bir düğüm eklemek için bir PowerShell Betiği küme geçerli makineye dağıtılır. |
| RemoveNode.ps1 |Mevcut bir düğüm kaldırmak için bir PowerShell Betiği geçerli makine kümeden dağıtıldı. |
| CleanFabric.ps1 |Tek başına Service Fabric yüklemesini geçerli makineye devre dışı Temizleme için bir PowerShell Betiği. Kendi ilişkili uninstallers kullanarak önceki MSI yüklemeleri kaldırılması gerekir. |
| TestConfiguration.ps1 |Cluster.json belirtildiği gibi altyapı çözümleme için bir PowerShell Betiği. |
| DownloadServiceFabricRuntimePackage.ps1 |Burada dağıtma makine internet'e bağlı senaryoları için bant dışı son çalışma zamanı paketini karşıdan yüklemek için kullanılan bir PowerShell Betiği. |
| DeploymentComponentsAutoextractor.exe |Tek başına paketi komut dosyaları tarafından kullanılan dağıtım bileşenlerini içeren kendi kendine ayıklanan arşiv. |
| EULA_ENU.txt |Microsoft Azure Service Fabric tek başına Windows Server paketini kullanmak için lisans koşulları. Yapabilecekleriniz [EULA'yı bir kopyasını indirin](http://go.microsoft.com/fwlink/?LinkID=733084) şimdi. |
| Readme.txt |Sürüm Notları ve temel yükleme yönergeleri bir bağlantı. Bu belgedeki yönergeleri alt kümesidir. |
| ThirdPartyNotice.rtf |Paketteki üçüncü taraf yazılım dikkat edin. |
| Tools\Microsoft.Azure.ServiceFabric.windowsserver.SupportPackage.zip |İsteğe bağlı olarak toplamak ve izleme günlükleri Microsoft'a destek amaçla karşıya yüklemek için çalıştırıldığında StandaloneLogCollector.exe. |
| Tools\ServiceFabricUpdateService.zip |İnternet erişimi olmayan kümeler için otomatik kod yükseltmeyi etkinleştirmek için kullanılan araç. Daha fazla ayrıntı bulunabilir [burada](service-fabric-cluster-upgrade-windows-server.md)|

**Şablonlar** 
| **Dosya adı** | **Kısa açıklama** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |Kümedeki her düğüm için bilgiler dahil olmak üzere bir güvenli, üç düğümlü tek makineli (veya sanal makine) geliştirme kümesi için ayarları içeren bir küme yapılandırması örnek dosyası. |
| ClusterConfig.Unsecure.MultiMachine.json |Kümedeki her makine için bilgiler dahil olmak üzere bir güvenli, çok makineli (veya sanal makine) küme ayarlarını içeren bir küme yapılandırması örnek dosyası. |
| ClusterConfig.Windows.DevCluster.json |Kümedeki her bir düğüm için bilgiler dahil olmak üzere güvenli, üç düğümlü tek makineli (veya sanal makine) geliştirme kümesi için tüm ayarları içeren bir küme yapılandırması örnek dosyası. Küme kullanılarak güvenli [Windows kimliklerini](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.Windows.MultiMachine.json |Güvenli kümedeki her makine için bilgileri de dahil, Windows güvenliği kullanarak güvenli, çok makineli (veya sanal makine) küme için tüm ayarları içeren bir küme yapılandırması örnek dosyası. Küme kullanılarak güvenli [Windows kimliklerini](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.x509.DevCluster.json |Kümedeki her düğüm için bilgiler dahil olmak üzere güvenli, üç düğümlü tek makineli (veya sanal makine) geliştirme kümesi için tüm ayarları içeren bir küme yapılandırması örnek dosyası. Küme güvenliğinin x509 ile sertifikalar. |
| ClusterConfig.x509.MultiMachine.json |Güvenli kümedeki her düğüm için bilgiler dahil olmak üzere güvenli, çok makineli (veya sanal makine) küme için tüm ayarları içeren bir küme yapılandırması örnek dosyası. Küme güvenliğinin x509 ile sertifikalar. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |Güvenli kümedeki her düğüm için bilgiler dahil olmak üzere güvenli, çok makineli (veya sanal makine) küme için tüm ayarları içeren bir küme yapılandırması örnek dosyası. Küme kullanarak güvenliği [Grup yönetilen hizmet hesapları](https://technet.microsoft.com/library/jj128431(v=ws.11).aspx). |

## <a name="cluster-configuration-samples"></a>Küme yapılandırma örnekleri
Küme yapılandırması şablonları en son sürümlerini GitHub sayfasında bulunabilir: [tek başına küme yapılandırma örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

## <a name="independent-runtime-package"></a>Bağımsız çalışma zamanı paketi
En son çalışma zamanı paketini otomatik olarak Küme dağıtımı sırasında yüklenen [karşıdan yükleme bağlantısı - Service Fabric çalışma zamanı - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).

## <a name="related"></a>İlgili
* [Tek başına Azure Service Fabric kümesi oluştur](service-fabric-cluster-creation-for-windows-server.md)
* [Service Fabric kümesi güvenlik senaryoları](service-fabric-windows-cluster-windows-security.md)
