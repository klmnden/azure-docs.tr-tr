---
title: SQL vm'lerde (Resource Manager) yönetim görevlerini otomatikleştiren | Microsoft Docs
description: Bu makalede, belirli SQL Server yönetim görevlerini otomatikleştirir SQL Server Aracısı uzantısı yönetmek açıklar. Bunlar otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure anahtar kasası tümleştirmeyi içerir.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/20/2018
ms.author: jroth
ms.openlocfilehash: d9cb4a3bdc5776c4ac70ac376d8b839193e3fc3d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a>SQL Server Aracısı uzantısı (Resource Manager) ile Azure sanal makineler üzerinde yönetim görevlerini otomatik hale getirme
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server Iaas Aracısı uzantısı (SQLIaaSExtension) Azure yönetim görevlerini otomatikleştirmek için sanal makinelerde çalışır. Bu makalede, yükleme, durum ve kaldırma yönergeleri yanı sıra uzantısı tarafından desteklenen hizmetlerine genel bakış sağlar.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu makalede klasik sürümünü görüntülemek için bkz: [SQL Server Aracısı uzantısı için SQL Server sanal makineleri Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Desteklenen hizmetler
SQL Server Iaas Aracısı uzantısı aşağıdaki yönetim görevlerini destekler:

| Yönetim özelliği | Açıklama |
| --- | --- |
| **SQL otomatik yedekleme** |Tüm veritabanları için yedekleme VM'de SQL Server'ın varsayılan örneğinin zamanlama otomatikleştirir. Daha fazla bilgi için bkz: [Azure Virtual Machines'de (Resource Manager) SQL Server için otomatik yedeklemeyi](virtual-machines-windows-sql-automated-backup.md). |
| **SQL otomatik düzeltme eki uygulama** |Güncelleştirmeleri, iş yükü için yoğun saatlerde kaçınmak için hangi sırasında VM önemli Windows güncelleştirmelerini, gerçekleştirilebildiği bir bakım penceresi yapılandırır. Daha fazla bilgi için bkz: [otomatik Azure Virtual Machines'de (Resource Manager) SQL Server için düzeltme eki uygulama](virtual-machines-windows-sql-automated-patching.md). |
| **Azure Anahtar Kasası Tümleştirmesi** |Otomatik olarak yüklemek ve SQL Server VM'nize üzerinde Azure anahtar kasası yapılandırmanıza olanak sağlar. Daha fazla bilgi için bkz: [(Resource Manager) Azure vm'lerinde SQL Server için Azure anahtar kasası tümleştirmeyi yapılandırmak](virtual-machines-windows-ps-sql-keyvault.md). |

Yüklü ve çalışan sonra SQL Server Iaas Aracısı uzantısı bu yönetim özellikleri sanal makinenin Azure portalında ve SQL Server Market görüntüler için Azure PowerShell aracılığıyla ve Azure ile SQL Server panelindeki kullanılabilmesini sağlar PowerShell uzantısı el ile yüklemeleri için. 

## <a name="prerequisites"></a>Önkoşullar
VM üzerinde SQL Server Iaas Aracısı uzantısı kullanmak için gereksinimler:

**İşletim sistemi**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server sürümleri**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [Karşıdan yükle ve en son Azure PowerShell komutlarını yapılandırın](/powershell/azure/overview)

## <a name="installation"></a>Yükleme
SQL Server sanal makineye Galerisi görüntülerden birini sağlamak, SQL Server Iaas Aracısı uzantısı otomatik olarak yüklenir. Uzantı el ile bu SQL Server Vm'lerinin birini yeniden yüklemeniz gerekirse, aşağıdaki PowerShell komutunu kullanın:

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

> [!IMPORTANT]
> Uzantısı yükleme uzantısı zaten yüklü değilse, SQL Server hizmetini yeniden başlatır.

SQL Server Iaas Aracısı uzantısı yalnızca işletim sistemi Windows Server sanal makinede yüklemek mümkündür. Bu, yalnızca bu makinede de el ile SQL Server yüklediyseniz desteklenir. Ardından uzantısını kullanarak el ile aynı yükleyin **kümesi AzureRmVMSqlServerExtension** PowerShell cmdlet'i.

> [!NOTE]
> Bir yalnızca işletim sistemi Windows Server VM üzerinde SQL Server Iaas Aracısı uzantısı el ile yüklerseniz, SQL Server yapılandırma ayarları Azure Portalı aracılığıyla yönetebilirsiniz değil. Bu senaryoda, PowerShell ile tüm değişiklikler yapmanız gerekir.

## <a name="status"></a>Durum
Uzantı'nin yüklü olduğunu doğrulamak için bir Azure portalında aracı durumunu görüntülemek için yoludur. Seçin **tüm ayarları** sanal makine penceresinin ve ardından üzerinde **uzantıları**. Görmeniz gerekir **SQLIaaSExtension** listelenen uzantısı.

![SQL Server Iaas Aracısı uzantısı Azure portalında](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Aynı zamanda **Get-AzureRmVMSqlServerExtension** Azure PowerShell cmdlet'i.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Önceki komutu aracının yüklü olduğundan ve genel durum bilgilerini sağlar onaylar. Aşağıdaki komutlarla otomatik yedekleme ve düzeltme eki ilgili özel durum bilgilerini de alabilirsiniz.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Çıkarma
Azure Portalı'nda üç nokta işaretine tıklayarak uzantısını kaldırabilirsiniz **uzantıları** penceresinde, sanal makine özellikleri. Sonra **Sil**’e tıklayın.

![Azure Portalı'nda SQL Server Iaas Aracısı uzantısını Kaldır](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Aynı zamanda **Kaldır AzureRmVMSqlServerExtension** PowerShell cmdlet'i.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Sonraki adımlar
Genişletme tarafından desteklenen hizmetlerinden birini kullanarak başlayın. Daha fazla ayrıntı için başvurulan makalelerine bakın [desteklenen Hizmetleri](#supported-services) bu makalenin.

SQL Server Azure sanal makinelerde çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelere genel bakış SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md).

