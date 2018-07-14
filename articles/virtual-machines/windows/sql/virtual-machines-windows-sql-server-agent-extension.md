---
title: (Resource Manager) SQL vm'lerdeki yönetim görevlerini otomatik hale getirin | Microsoft Docs
description: Bu makalede belirli SQL Server yönetim görevlerini otomatikleştiren SQL Server Aracısı uzantısı yönetmek açıklar. Bunlar, otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure anahtar kasası tümleştirmeyi içerir.
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
ms.date: 07/12/2018
ms.author: jroth
ms.openlocfilehash: c663aec02d4d1808426a9f05a6674d5504563a63
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009409"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a>SQL Server Aracısı uzantısı (Resource Manager) ile Azure sanal Makineler'de yönetim görevlerini otomatikleştirin
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server Iaas Aracısı uzantısı (SQLIaaSExtension), Azure yönetim görevlerini otomatikleştirmek için sanal makinelerde çalıştırır. Bu makalede, yükleme, durum ve kaldırma yönergeleri yanı sıra uzantı tarafından desteklenen hizmetleri genel bir bakış sağlar.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu makalede klasik sürümünü görüntülemek için bkz: [SQL Server Aracısı uzantısı için SQL Server Vm'leri Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Desteklenen hizmetler
SQL Server Iaas Aracısı uzantısı, aşağıdaki yönetim görevlerini destekler:

| Yönetim özelliği | Açıklama |
| --- | --- |
| **SQL otomatik yedekleme** |Tüm veritabanları için yedekleme VM'de SQL Server'ın varsayılan örneğinin zamanlama otomatikleştirir. Daha fazla bilgi için [Azure Virtual Machines'de (Resource Manager) SQL Server için otomatik yedekleme](virtual-machines-windows-sql-automated-backup.md). |
| **SQL otomatik düzeltme eki uygulama** |İş yükünüz için yoğun saatlerde güncelleştirmeleri kurtulabilirsiniz aşamasında vm'niz önemli Windows güncelleştirmelerini, gerçekleştirilebildiği bir bakım penceresi yapılandırır. Daha fazla bilgi için [otomatik Azure Virtual Machines'de (Resource Manager) SQL Server için düzeltme eki uygulama](virtual-machines-windows-sql-automated-patching.md). |
| **Azure Anahtar Kasası Tümleştirmesi** |Otomatik olarak yüklemeniz ve SQL Server sanal makinenizde Azure Key Vault yapılandırma sağlar. Daha fazla bilgi için [(Resource Manager) Azure vm'lerde SQL Server için Azure anahtar kasası tümleştirmeyi yapılandırma](virtual-machines-windows-ps-sql-keyvault.md). |

Yüklü ve çalışır sonra SQL Server Iaas Aracısı uzantısı bu yönetim özelliklerini Azure portalında ve SQL Server Market görüntüleri için Azure PowerShell ve Azure aracılığıyla sanal makine SQL Server panosunda kullanılabilmesini sağlar Uzantıyı el ile yüklemelerinde PowerShell. 

## <a name="prerequisites"></a>Önkoşullar
Sanal makinenizde SQL Server Iaas Aracısı uzantısı kullanmak için gereksinimler:

**İşletim sistemi**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server sürümleri**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [İndirme ve en son Azure PowerShell komutlarını yapılandırma](/powershell/azure/overview)

> [!IMPORTANT]
> Şu anda [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md) azure'da SQL Server FCI için desteklenmiyor. İçinde bir FCI katılan vm'lerden uzantı kaldırmanızı öneririz. Aracı kaldırıldıktan sonra uzantı tarafından desteklenen özellikler SQL Vm'lerine kullanılamaz.

## <a name="installation"></a>Yükleme
SQL Server sanal makine galeri görüntüleri sağlarken, SQL Server Iaas Aracısı uzantısı otomatik olarak yüklenir. Bu SQL Server Vm'leri birinde uzantıyı el ile yeniden gerekiyorsa, aşağıdaki PowerShell komutunu kullanın:

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

> [!IMPORTANT]
> Uzantı zaten yüklü değilse, uzantının yüklenmesi, SQL Server hizmetini yeniden başlatır.

> [!NOTE]
> SQL Server Iaas Aracısı uzantısı yalnızca desteklenir [SQL Server sanal makine galeri görüntüleri](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms) (Kullandıkça Öde veya Getir-kendi lisansını). SQL Server bir yalnızca işletim sistemi Windows Server sanal makinesi üzerinde el ile yüklerseniz veya özelleştirilmiş bir SQL Server VM VHD dağıtırsanız desteklenmiyor. Bu gibi durumlarda, yüklemek ve PowerShell kullanarak uzantıyı el ile yönetmek mümkün olabilir, ancak Azure portalında SQL Server yapılandırma ayarlarını almaz. Ancak, bunun yerine bir SQL Server VM galeri görüntüsü yükleyin ve ardından özelleştirmek için önerilir.

## <a name="status"></a>Durum
Uzantı yüklendiğini doğrulamak için bir aracı durumunu Azure portalında görüntülemek için yoludur. Seçin **tüm ayarlar** sanal makine penceresinde ve ardından şirket **uzantıları**. Görmelisiniz **SQLIaaSExtension** listelenen uzantısı.

![Azure portalında SQL Server Iaas Aracısı uzantısı](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Ayrıca **Get-AzureRmVMSqlServerExtension** Azure PowerShell cmdlet'i.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Önceki komut, aracının yüklü olduğundan ve genel durum bilgilerini sağlar onaylar. Aşağıdaki komutlarla otomatik yedekleme ve düzeltme eki uygulama hakkında özel durum bilgilerini de alabilirsiniz.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Çıkarma
Azure Portalı'nda üç nokta simgesine tıklayarak uzantıyı kaldırabilirsiniz **uzantıları** , sanal makine özellikleri penceresi. Sonra **Sil**’e tıklayın.

![Azure portalında SQL Server Iaas Aracısı uzantısı kaldırma](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Ayrıca **Remove-AzureRmVMSqlServerExtension** PowerShell cmdlet'i.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Sonraki adımlar
Uzantı tarafından desteklenen hizmetlerinden birini kullanmaya başlayın. Başvurulan makaleleri daha fazla ayrıntı için bkz. [desteklenen Hizmetleri](#supported-services) bu makalenin.

Azure sanal Makineler'de SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).

