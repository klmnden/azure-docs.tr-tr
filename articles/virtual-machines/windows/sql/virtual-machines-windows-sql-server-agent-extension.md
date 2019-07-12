---
title: SQL Server Iaas Aracısı uzantısı ile Azure sanal Makineler'de yönetim görevlerini otomatik hale getirin | Microsoft Docs
description: Bu makalede belirli SQL Server yönetim görevlerini otomatikleştiren SQL Server Aracısı uzantısı yönetmek açıklar. Bunlar, otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure anahtar kasası tümleştirmeyi içerir.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 41023103dc30d16f599e847f9d324bc7bb4be11c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798046"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-iaas-agent-extension"></a>SQL Server Iaas Aracısı uzantısı ile Azure sanal Makineler'de yönetim görevlerini otomatikleştirin
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS Aracı Uzantısı Extension (SqlIaasExtension) Azure sanal makinelerinde yönetim görevlerini otomatikleştirmek için çalıştırılır. Bu makalede, bir genel bakış ve yükleme, durum ve kaldırma yönergeleri yanı sıra uzantı tarafından desteklenen hizmetleri sağlar.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu makalede klasik sürümünü görüntülemek için bkz: [SQL Server Aracısı uzantısı için SQL Server Vm'leri Klasik](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

SQL Iaas uzantısı üç SQL yönetilebilirlik mod vardır: **Tam**, **basit**, ve **NoAgent**. 

- **Tam** modu tüm işlevselliği sunar, ancak SQL Server ve SA izinleri yeniden başlatılmasını gerektirir. Varsayılan olarak yüklenir ve tek bir örneği ile bir SQL Server VM'si yönetmek için kullanılması gereken seçenek budur. 

- **Basit** SQL Server'ın bir yeniden başlatma gerektirmez, ancak yalnızca SQL Server sürümü ve lisans türünü değiştirmeyi destekler. Bu seçenek, birden çok örnek ile SQL Server Vm'leri için kullanılan veya bir yük devretme kümesi örneği (FCI), katılımcı olması gerekir. 

- **NoAgent** adanmış SQL Server 2008 ve Windows Server 2008'de SQL Server 2008 R2. Kullanan bilgi `NoAgent` görmek için Windows Server 2008 görüntü modu [Windows Server 2008 kayıt](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008r2-on-windows-server-2008-vms). 

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

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**SQL Server sürümleri**:

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell**:

* [İndirme ve en son Azure PowerShell komutlarını yapılandırma](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]


## <a name="change-management-modes"></a>Değişiklik Yönetimi modları

PowerShell kullanarak, SQL Iaas Aracısı geçerli modunu görüntüleyebilirsiniz: 

  ```powershell-interactive
     //Get the SqlVirtualMachine
     $sqlvm = Get-AzResource -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName  -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines
     $sqlvm.Properties.sqlManagement
  ```

SQL Server Vm'leri için *NoAgent* veya *basit* Iaas uzantısı, yüklü moduna yükseltebilirsiniz *tam* Azure portalını kullanarak. -SQL Iaas uzantısı tamamen kaldırıp yeniden yüklemeniz gerekecek bunun yüklemek mümkün değildir. 

Yükseltme Aracı modu *tam*, aşağıdakileri yapın: 

1. [Azure portal](https://portal.azure.com) oturum açın.
1. Gidin, [SQL sanal makineleri](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) kaynak. 
1. SQL Server sanal makinenizi seçip **genel bakış**. 
1. SQL Vm'lerini kullanmaya için *NoAgent* veya *basit* Iaas modları için iletiyi seçin **yalnızca lisans türü ve sürümü Güncelleştirmeler ile SQL Iaas uzantısı kullanılabilir**.

    ![Modu değiştirme Portalı'ndan Başlat](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Kabul **SQL Server hizmetini başlatın** onay kutusunu seçerek ve ardından **Onayla** Iaas modunuzu 'Full' yükseltmek için. 

    ![Iaas uzantısı için tam yönetimini etkinleştirme](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

##  <a name="installation"></a>Yükleme
SQL Server VM'nize ile kaydettiğinizde SQL Iaas uzantısı yüklü [SQL VM kaynak sağlayıcısı](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider). Ancak, gerekirse SQL Iaas Aracısı'nı da kullanarak el ile yüklenebilir *tam* veya *basit* modu yükleme. 

*Tam* SQL Server Iaas Aracısı uzantısı Azure portalını kullanarak SQL Server sanal makine galeri görüntüleri sağladığınızda otomatik olarak yüklenir. 

### <a name="full-mode-installation"></a>Tam modda yükleme
*Tam* SQL Iaas uzantısı, SQL Server VM üzerinde tek bir örnek için tam yönetilebilirlik sunar. Varsayılan bir örnek ise, sonra uzantıyı varsayılan bir örnek ile çalışır ve diğer örneklerin yönetiminde desteklemez. Varsayılan örneği yok, ancak yalnızca bir adlandırılmış örnek varsa, adlandırılmış örnek yönetecektir. Varsayılan örnek yok ve birden fazla adlandırılmış örneklerde, uzantıyı yüklemek başarısız olur. 

Yükleme *tam* SQL Iaas modu, SQL Server hizmetini yeniden başlatılacak. SQL Server hizmetini yeniden başlatmayı önlemek için yükleme *basit* moduyla yönetilebilirlik yerine sınırlı. 

SQL Iaas Aracısı ile *tam* modunu PowerShell kullanarak:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Full' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='Full'}  
  
  ```

| Parametre | Kabul edilebilir değerler                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `'AHUB'`, veya `'PAYG'`     |
| &nbsp;             | &nbsp;                          |


> [!WARNING]
> - Uzantı zaten yüklü değilse, yükleme **tam** uzantısı, SQL Server hizmetini yeniden başlatır. Kullanım **basit** modu, SQL Server hizmetini yeniden başlatılmasını önlemek için. 
> - SQL Iaas uzantısı güncelleştiriliyor, SQL Server hizmetini yeniden başlatmaz. 

#### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>Tek bir adlandırılmış SQL Server örneği ile bir VM üzerinde yükleme
SQL Iaas uzantısı varsayılan örneği kaldırılır ve Iaas uzantısı yeniden yüklerseniz, bir SQL Server'da adlandırılmış bir örnekle çalışır.

Adlandırılmış bir SQL Server örneğini kullanmak için aşağıdakileri yapın:
   1. Market'ten bir SQL Server VM dağıtın. 
   1. Iaas uzantısı içinden kaldırma [Azure portalında](https://portal.azure.com).
   1. SQL Server SQL Server VM içinde tamamen kaldırın.
   1. SQL Server, SQL Server VM içinde adlandırılmış bir örnekle yükleyin. 
   1. Azure portalındaki Iaas uzantısını yükleyin.  


### <a name="install-in-lightweight-mode"></a>Basit modda yükleme
Basit mod, SQL Server hizmetini yeniden başlatmaz, ancak sınırlı işlevsellik sunar. 

SQL Iaas Aracısı ile *basit* modunu PowerShell kullanarak:


  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

| Parametre | Kabul edilebilir değerler                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `'AHUB'`, veya `'PAYG'`     |
| &nbsp;             | &nbsp;                          |


## <a name="get-status-of-sql-iaas-extension"></a>SQL Iaas uzantısı durumunu alın
Uzantı yüklendiğini doğrulamak için bir aracı durumunu Azure portalında görüntülemek için yoludur. Seçin **tüm ayarlar** sanal makine penceresinde ve ardından şirket **uzantıları**. Görmelisiniz **SqlIaasExtension** listelenen uzantısı.

![Azure portalında SQL Server Iaas Aracısı uzantısı](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Ayrıca **Get-AzVMSqlServerExtension** Azure PowerShell cmdlet'i.

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

Önceki komut, aracının yüklü olduğundan ve genel durum bilgilerini sağlar onaylar. Aşağıdaki komutlarla otomatik yedekleme ve düzeltme eki uygulama hakkında özel durum bilgilerini de alabilirsiniz.

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Temizleme
Azure portalında, üç nokta simgesine tıklayarak uzantıyı kaldırabilirsiniz **uzantıları** , sanal makine özellikleri penceresi. Sonra **Sil**’e tıklayın.

![Azure portalında SQL Server Iaas Aracısı uzantısı kaldırma](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Ayrıca **Remove-AzVMSqlServerExtension** PowerShell cmdlet'i.

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Sonraki adımlar
Uzantı tarafından desteklenen hizmetlerinden birini kullanmaya başlayın. Daha fazla bilgi için başvurulan makalelere bakın [desteklenen Hizmetleri](#supported-services) bu makalenin.

Azure sanal Makineler'de SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).

