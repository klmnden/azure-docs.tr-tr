---
title: Windows Server için Azure Service Fabric tek başına paketin | Microsoft Docs
description: Açıklama ve Windows Server için Azure Service Fabric tek başına paketin içeriği.
services: service-fabric
documentationcenter: .net
author: maburlik
manager: chackdan
editor: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: maburlik
ms.openlocfilehash: facdcd162826e6f77ace098391459cba00061c4f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661624"
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Windows Server için Service Fabric tek başına paketin içeriği
İçinde [indirilen](https://go.microsoft.com/fwlink/?LinkId=730690) Service Fabric tek başına paketinin aşağıdaki dosyaları göreceksiniz:

| **Dosya adı** | **Kısa açıklama** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |İçinde ClusterConfig.json ayarlarla kümeyi oluşturur bir PowerShell Betiği. |
| RemoveServiceFabricCluster.ps1 |İçinde ClusterConfig.json ayarları kullanarak bir küme kaldıran bir PowerShell Betiği. |
| AddNode.ps1 |Varolan bir düğüm eklemek için bir PowerShell Betiği, geçerli makine bir kümede dağıtılan. |
| RemoveNode.ps1 |Varolan bir düğüm kaldırmak için bir PowerShell Betiği, geçerli makine bir kümeden dağıtıldı. |
| CleanFabric.ps1 |Tek başına Service Fabric yükleme geçerli makine kapalı temizlemek için bir PowerShell Betiği. Kendi ilişkili uninstallers kullanarak önceki MSI yüklemeleri kaldırılması gerekir. |
| TestConfiguration.ps1 |Altyapı Cluster.json belirtildiği gibi analiz etmek için bir PowerShell Betiği. |
| DownloadServiceFabricRuntimePackage.ps1 |Burada dağıtma makine internet'e bağlı olmayan senaryolar için bir bant dışı en son çalışma zamanı paketini yüklemek için kullanılan bir PowerShell Betiği. |
| DeploymentComponentsAutoextractor.exe |Tek başına paketin betikler tarafından kullanılan dağıtım bileşenlerini içeren kendi kendine ayıklanan arşiv. |
| EULA_ENU.txt |Microsoft Azure Service Fabric tek başına Windows Server paketi kullanımı için lisans koşulları. Yapabilecekleriniz [EULA'yı bir kopyasını indirin](https://go.microsoft.com/fwlink/?LinkID=733084) şimdi. |
| Readme.txt |Sürüm Notları ve temel yükleme yönergeleri bağlantısı. Bu belgedeki yönergeleri bir alt kümesidir. |
| ThirdPartyNotice.rtf |Paketteki üçüncü taraf yazılım bildirimi. |
| Tools\Microsoft.Azure.ServiceFabric.windowsserver.SupportPackage.zip |Toplama ve izleme günlükleri Microsoft'a destek amaçla yükleme için isteğe bağlı olarak çalıştır StandaloneLogCollector.exe. |
| Tools\ServiceFabricUpdateService.zip |Otomatik kod yükseltmeyi internet erişimi yoksa kümeleri için etkinleştirmek için kullanılan araç. Daha fazla ayrıntı bulunabilir [burada](service-fabric-cluster-upgrade-windows-server.md)|

**Şablonlar** 

| **Dosya adı** | **Kısa açıklama** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |Kümedeki her düğüm için bilgileri dahil olmak üzere güvenli olmayan, üç düğümlü tek makineli (veya sanal makine) geliştirme kümesi için ayarları içeren bir küme yapılandırması örnek dosyası. |
| ClusterConfig.Unsecure.MultiMachine.json |Kümedeki her makine için bilgileri dahil olmak üzere bir güvenli, çok makineli (veya sanal makine) küme ayarlarını içeren bir küme yapılandırması örnek dosyası. |
| ClusterConfig.Windows.DevCluster.json |Bilgi için kümedeki her düğüme dahil olmak üzere bir güvenli, üç düğümlü tek makineli (veya sanal makine) geliştirme kümesi, tüm ayarlarını içeren bir küme yapılandırma örnek dosyası. Küme kullanarak güvenli hale [Windows kimlikleri](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.Windows.MultiMachine.json |Güvenli kümesinde bulunan her makine için bilgiler dahil olmak üzere, Windows güvenliğini kullanarak güvenli, çok makineli (veya sanal makine) küme için tüm ayarları içeren bir küme yapılandırma örnek dosyası. Küme kullanarak güvenli hale [Windows kimlikleri](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.x509.DevCluster.json |Kümedeki her düğüm için bilgileri dahil olmak üzere bir güvenli, üç düğümlü tek makineli (veya sanal makine) geliştirme kümesi, tüm ayarlarını içeren bir küme yapılandırma örnek dosyası. Küme x509 kullanarak güvenli hale sertifikaları. |
| ClusterConfig.x509.MultiMachine.json |Güvenli kümedeki her düğüm için bilgileri dahil olmak üzere güvenli, çok makineli (veya sanal makine) küme için tüm ayarları içeren bir küme yapılandırması örnek dosyası. Küme x509 kullanarak güvenli hale sertifikaları. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |Güvenli kümedeki her düğüm için bilgileri dahil olmak üzere güvenli, çok makineli (veya sanal makine) küme için tüm ayarları içeren bir küme yapılandırması örnek dosyası. Küme kullanarak güvenli hale [Grup yönetilen hizmet hesapları](https://technet.microsoft.com/library/jj128431(v=ws.11).aspx). |

## <a name="cluster-configuration-samples"></a>Küme yapılandırma örnekleri
Küme yapılandırması şablonlarının en son sürümlerine GitHub sayfasında bulunabilir: [Tek başına küme yapılandırması örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

## <a name="independent-runtime-package"></a>Bağımsız çalışma zamanı paketi
En son çalışma zamanı paketi Küme dağıtımı sırasında otomatik olarak indirilir [indirme bağlantısı - Service Fabric çalışma zamanı - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).

## <a name="related"></a>İlgili
* [Tek başına Azure Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* [Service Fabric kümesi güvenlik senaryoları](service-fabric-windows-cluster-windows-security.md)
