---
title: Azure Backup için Azure Resource Manager şablonları
description: Azure Backup PowerShell Örnekleri
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 01/31/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: bed2c002cacdac1e47852e6fa3181885af6733d2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236796"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Azure Backup için Azure Resource Manager şablonları

Aşağıdaki tabloda Kurtarma Hizmetleri kasaları ve Azure Backup özellikleri ile birlikte kullanılacak Azure Resource Manager şablonlarının bağlantıları bulunur. JSON söz dizimi ve özellikler hakkında bilgi edinmek için [Microsoft.RecoveryServices kaynak türleri](/azure/templates/microsoft.recoveryservices/allversions).

|   |   |
|---|---|
|**Kurtarma Hizmetleri kasası** | |
| [Kurtarma Hizmetleri kasası oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Kurtarma Hizmetleri kasası oluşturun. Kasa Azure Backup ve Azure Site Recovery için kullanılabilir. |
|**Sanal makineleri yedekleme**| |
| [Kaynak Yöneticisi VM’lerini yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Aynı kaynak grubundaki Kaynak Yöneticisi sanal makinelerini yedeklemek için kaynak var olan Kurtarma Hizmetleri kasasını ve Yedekleme ilkesini kullanın.|
| [IaaS sanal makinelerini Kurtarma Hizmetleri kasasına yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Klasik ve Kaynak Yöneticisi sanal makinelerini yedekleme şablonu. |
| [IaaS sanal makineleri için Haftalık Yedekleme ilkesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | Şablon, Kurtarma Hizmetleri kasası ve klasik ve Resource Manager sanal makinelerini yedeklemek için kullanılan haftalık bir yedekleme ilkesi oluşturur.|
| [IaaS sanal makineleri için Günlük Yedekleme ilkesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | Şablon, Kurtarma Hizmetleri kasası ve klasik ve Resource Manager sanal makinelerini yedeklemek için kullanılan günlük bir yedekleme ilkesi oluşturur.|
| [Yedekleme özelliği etkin Windows Server VM dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | Şablon, varsayılan yedekleme ilkesi etkinken bir Windows Server VM ve Kurtarma Hizmetleri kasası oluşturur.|
|**Yedekleme işlerini izleme** |  |
| [Azure İzleyici günlüklerine Azure Backup ile kullanma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | Şablonu Azure Yedekleme'yle yedeklemeyi izleme ve işler, Yedekleme uyarılarını ve kurtarma Hizmetleri Kasalarınızda kullanılan bulut depolama geri yüklemenize olanak sağlayan, Azure İzleyici günlüklerine dağıtır.|  
|**Azure VM'de SQL Server Yedekleme** |  |
| [Azure VM'de SQL Server Yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vm-workload-backup) | Şablon, Kurtarma Hizmetleri kasası ve iş yükü belirli yedekleme ilkesi oluşturur. VM, Azure Backup hizmeti ve söz konusu VM'deki yapılandırır koruma ile kaydeder. Şu anda yalnızca SQL galeri görüntüleri için çalışır. |
|   |   |
