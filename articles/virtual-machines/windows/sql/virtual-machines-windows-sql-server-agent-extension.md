---
title: (Resource Manager) SQL vm'lerdeki yönetim görevlerini otomatik hale getirin | Microsoft Docs
description: Bu makalede belirli SQL Server yönetim görevlerini otomatikleştiren SQL Server Aracısı uzantısı yönetmek açıklar. Bunlar, otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure anahtar kasası tümleştirmeyi içerir.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
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
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 71878d5d033f0005d2c8c36d9f59799e125a19dd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58762710"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a>SQL Server Aracısı uzantısı (Resource Manager) ile Azure sanal Makineler'de yönetim görevlerini otomatikleştirin
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS Aracı Uzantısı Extension (SqlIaasExtension) Azure sanal makinelerinde yönetim görevlerini otomatikleştirmek için çalıştırılır. Bu makalede, yükleme, durum ve kaldırma yönergeleri yanı sıra uzantı tarafından desteklenen hizmetleri genel bir bakış sağlar.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu makalede klasik sürümünü görüntülemek için bkz: [SQL Server Aracısı uzantısı için SQL Server Vm'leri Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Desteklenen hizmetler
SQL Server Iaas Aracısı uzantısı, aşağıdaki yönetim görevlerini destekler:

| Yönetim özelliği | Açıklama |
| --- | --- |
| **SQL otomatik yedekleme** |Tüm veritabanları için yedekleme için varsayılan örnek zamanlama otomatikleştirir ya da bir [yüklendiğinden](virtual-machines-windows-sql-server-iaas-faq.md#administration) adlı VM üzerinde SQL Server örneği. Daha fazla bilgi için [Azure Virtual Machines'de (Resource Manager) SQL Server için otomatik yedekleme](virtual-machines-windows-sql-automated-backup.md). |
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

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> Şu anda [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md) azure'da SQL Server FCI için desteklenmiyor. İçinde bir FCI katılan vm'lerden uzantı kaldırmanızı öneririz. Aracı kaldırıldıktan sonra uzantı tarafından desteklenen özellikler SQL Vm'lerine kullanılamaz.

## <a name="installation"></a>Yükleme
SQL Server sanal makine galeri görüntüleri sağlarken, SQL Server Iaas Aracısı uzantısı otomatik olarak yüklenir. SQL Iaas uzantısı için tek bir SQL Server VM örneğinde yönetilebilirlik sunar. Varsayılan bir örnek ise, sonra uzantıyı varsayılan bir örnek ile çalışır ve diğer örneklerin yönetiminde desteklemez. Varsayılan örneği yok, ancak yalnızca bir adlandırılmış örnek varsa, adlandırılmış örnek yönetecektir. Varsayılan örnek yok ve birden fazla adlandırılmış örneklerde, uzantıyı yüklemek başarısız olur. 



Bu SQL Server Vm'leri birinde uzantıyı el ile yeniden gerekiyorsa, aşağıdaki PowerShell komutunu kullanın:

```powershell
Set-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension" -Version "2.0" -Location "East US 2"
```

> [!WARNING]
> Uzantı zaten yüklü değilse, uzantının yüklenmesi, SQL Server hizmetini yeniden başlatır. Ancak, SQL Iaas uzantısı güncelleştirme SQL Server hizmetini yeniden başlatmaz. 

> [!NOTE]
> SQL Server Iaas Aracısı uzantısı için özel bir SQL Server görüntüleri yüklemek mümkün olsa da, şu anda sınırlı işlevdir [lisans türü değiştirme](virtual-machines-windows-sql-ahb.md). SQL Iaas uzantısı tarafından sunulan diğer özellikleri yalnızca çalışır [SQL Server sanal makine galeri görüntüleri](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms) (Kullandıkça Öde veya Getir-kendi lisansını).

### <a name="use-a-single-named-instance"></a>Tek bir adlandırılmış örnek kullanın
SQL Iaas uzantısı adlandırılmış bir örnekle, varsayılan örneği düzgün kaldırdıysanız ve Iaas uzantısı yüklenirse bir SQL Server görüntüsü üzerinde çalışır.

Adlandırılmış bir SQL Server örneğini kullanmak için aşağıdakileri yapın:
   1. Market'ten bir SQL Server VM dağıtın. 
   1. Iaas uzantısı içinden kaldırma [Azure portalında](https://portal.azure.com).
   1. SQL Server SQL Server VM içinde tamamen kaldırın.
   1. SQL Server, SQL Server VM içinde adlandırılmış bir örnekle yükleyin. 
   1. Azure portalındaki Iaas uzantısını yükleyin.  

## <a name="status"></a>Durum
Uzantı yüklendiğini doğrulamak için bir aracı durumunu Azure portalında görüntülemek için yoludur. Seçin **tüm ayarlar** sanal makine penceresinde ve ardından şirket **uzantıları**. Görmelisiniz **SqlIaasExtension** listelenen uzantısı.

![Azure portalında SQL Server Iaas Aracısı uzantısı](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Ayrıca **Get-AzVMSqlServerExtension** Azure PowerShell cmdlet'i.

    Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Önceki komut, aracının yüklü olduğundan ve genel durum bilgilerini sağlar onaylar. Aşağıdaki komutlarla otomatik yedekleme ve düzeltme eki uygulama hakkında özel durum bilgilerini de alabilirsiniz.

    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Çıkarma
Azure portalında, üç nokta simgesine tıklayarak uzantıyı kaldırabilirsiniz **uzantıları** , sanal makine özellikleri penceresi. Sonra **Sil**’e tıklayın.

![Azure portalında SQL Server Iaas Aracısı uzantısı kaldırma](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Ayrıca **Remove-AzVMSqlServerExtension** PowerShell cmdlet'i.

    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"

## <a name="next-steps"></a>Sonraki adımlar
Uzantı tarafından desteklenen hizmetlerinden birini kullanmaya başlayın. Başvurulan makaleleri daha fazla ayrıntı için bkz. [desteklenen Hizmetleri](#supported-services) bu makalenin.

Azure sanal Makineler'de SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).

