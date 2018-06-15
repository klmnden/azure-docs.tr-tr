---
title: Azure Backup için Azure Resource Manager şablonları
description: Azure Backup PowerShell Örnekleri
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 04/18/2018
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: 0aac49be397f5e1c86fa834d341399775fd71cfa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607081"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Azure Backup için Azure Resource Manager şablonları

Aşağıdaki tabloda Kurtarma Hizmetleri kasaları ve Azure Backup özellikleri ile birlikte kullanılacak Azure Resource Manager şablonlarının bağlantıları bulunur.

|   |   |
|---|---|
|**Kurtarma Hizmetleri kasası** | |
| [Kurtarma Hizmetleri kasası oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Kurtarma Hizmetleri kasası oluşturun. Kasa Azure Backup ve Azure Site Recovery için kullanılabilir. |
|**Sanal makineleri yedekleme**| |
| [Kaynak Yöneticisi VM’lerini yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Aynı kaynak grubundaki Kaynak Yöneticisi sanal makinelerini yedeklemek için kaynak var olan Kurtarma Hizmetleri kasasını ve Yedekleme ilkesini kullanın.|
| [IaaS sanal makinelerini Kurtarma Hizmetleri kasasına yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Klasik ve Kaynak Yöneticisi sanal makinelerini yedekleme şablonu. |
| [IaaS sanal makineleri için Haftalık Yedekleme ilkesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | Şablon, Kurtarma Hizmetleri kasası ve klasik ile Kaynak Yöneticisi sanal makinelerini yedeklemek için kullanılan haftalık bir yedekleme ilkesi oluşturur.|
| [IaaS sanal makineleri için Günlük Yedekleme ilkesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | Şablon, Kurtarma Hizmetleri kasası ve klasik ile Kaynak Yöneticisi sanal makinelerini yedeklemek için kullanılan günlük bir yedekleme ilkesi oluşturur.|
| [Yedekleme özelliği etkin Windows Server VM dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | Şablon, varsayılan yedekleme ilkesi etkinken bir Windows Server VM ve Kurtarma Hizmetleri kasası oluşturur.|
|**Yedekleme işlerini izleme** |  |
| [OMS Log Analytics kullanarak Azure Backup izleme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | Şablon, Azure Backup için OMS İzlemeyi dağıtarak yedekleme ve geri yükleme işlerini, yedekleme uyarılarını ve Kurtarma Hizmetleri kasalarınızda kullanılan Bulut depolama alanını izlemenize olanak tanır.|  
|   |   |

