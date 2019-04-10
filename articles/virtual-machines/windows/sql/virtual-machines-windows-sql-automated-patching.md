---
title: Otomatik (Resource Manager) SQL Server Vm'leri için düzeltme eki uygulama | Microsoft Docs
description: Otomatik düzeltme eki uygulama özelliği için SQL Sunucusu Kaynak Yöneticisi'ni kullanarak Azure'da çalışan sanal makineler açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/07/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 348979a53bff76c85e6d1531bd16cd695145e21b
ms.sourcegitcommit: ef20235daa0eb98a468576899b590c0bc1a38394
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59425994"
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Azure Virtual Machines’de (Resource Manager) SQL Server için Otomatik Düzeltme Eki Uygulama
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-sql-automated-patching.md)

Otomatik düzeltme eki uygulama, bir Azure sanal makinesinde SQL Server çalıştıran bir bakım penceresi oluşturur. Otomatik Güncelleştirmeler yalnızca bu bakım penceresi sırasında yüklenebilir. SQL Server için bu kısıtlama, sistem güncelleştirmelerinin ve ilişkili tüm yeniden başlatmaların veritabanı için mümkün olan en uygun zamanda yapılmasını sağlar. 

> [!IMPORTANT]
> Yalnızca **Önemli** olarak işaretlenmiş Windows güncelleştirmeleri yüklenir. Toplu Güncelleştirmeler gibi diğer SQL Server güncelleştirmelerinin el ile yüklenmesi gerekir. 

Otomatik Yama Uygulama [SQL Server IaaS Aracı Uzantısı](virtual-machines-windows-sql-server-agent-extension.md)'na bağımlıdır.

## <a name="prerequisites"></a>Önkoşullar
Otomatik düzeltme eki uygulama kullanmak için aşağıdaki önkoşulları göz önünde bulundurun:

**İşletim sistemi**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server sürümü**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [En son Azure PowerShell komutlarını yükleme](/powershell/azure/overview) PowerShell ile otomatik düzeltme eki uygulama'yı yapılandırmayı planlıyorsanız.

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

> [!NOTE]
> Otomatik düzeltme eki uygulama, SQL Server Iaas Aracısı uzantısını kullanır. Geçerli SQL sanal makine galeri görüntüleri, varsayılan olarak bu uzantı ekleyin. Daha fazla bilgi için [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda, otomatik düzeltme eki uygulama için yapılandırılmış seçenekler açıklanmaktadır. Gerçek yapılandırma adımları, Azure portal veya Azure Windows PowerShell komutlarını kullanmadığınıza bağlı olarak değişir.

| Ayar | Olası değerler | Açıklama |
| --- | --- | --- |
| **Otomatik Düzeltme Eki Uygulama** |Etkinleştir/devre dışı bırak (devre dışı) |Etkinleştirir veya bir Azure sanal makinesi için otomatik düzeltme eki uygulama devre dışı bırakır. |
| **Bakım zamanlaması** |Her gün, Pazartesi, Salı, Çarşamba, Thursday, Friday, Cumartesi, Pazar |İndirme ve sanal makineniz için Windows, SQL Server ve Microsoft güncelleştirmelerini yükleme zamanlamasını. |
| **Bakım başlangıç saati** |0-24 |Sanal makineyi güncelleştirmek için yerel başlangıç zamanı. |
| **Bakım penceresi süresine** |30-180 |İndirme ve güncelleştirmelerin yüklenmesini tamamlamak için izin verilen dakika sayısı. |
| **Düzeltme eki kategorisi** |Önemli | Windows Güncelleştirmeleri indirmek ve yüklemek için kategori.|

## <a name="configuration-in-the-portal"></a>Portalda yapılandırma
Sağlama sırasında veya var olan sanal makineler için otomatik düzeltme eki uygulama yapılandırmak için Azure portalını kullanabilirsiniz.

### <a name="new-vms"></a>Yeni VM'ler
Otomatik düzeltme eki uygulama Resource Manager dağıtım modelinde yeni bir SQL Server sanal makine oluşturduğunuzda yapılandırmak için Azure portalını kullanın.

İçinde **SQL Server ayarları** dikey penceresinde **otomatik düzeltme eki uygulama**. Aşağıdaki Azure portalı ekran görüntüsü gösterildiği **SQL otomatik düzeltme eki uygulama** dikey penceresi.

![SQL otomatik düzeltme eki Azure portalında](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Bağlam için tam üzerinde konusuna [azure'da bir SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Varolan Vm'leri
Mevcut SQL Server sanal makineleri için SQL Server sanal makinenizi seçin. Ardından **SQL Server Yapılandırması** bölümünü **ayarları** dikey penceresi.

![SQL otomatik düzeltme eki var olan VM'ler için](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

İçinde **SQL Server Yapılandırması** dikey penceresinde tıklayın **Düzenle** bölüm düzeltme eki uygulama otomatik olarak düğmesi.

![SQL otomatik düzeltme eki uygulama, var olan VM'ler için yapılandırma](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

İşiniz bittiğinde tıklayın **Tamam** altındaki düğmesine **SQL Server Yapılandırması** yaptığınız değişiklikleri kaydetmek için dikey.

İlk kez otomatik düzeltme eki uygulama etkinleştiriyorsanız, Azure SQL Server Iaas Aracısı arka planda yapılandırır. Bu süre boyunca, Azure portalında, otomatik düzeltme eki uygulama yapılandırıldığını göstermeyebilir. Aracının yüklü için yapılandırılmış birkaç dakika bekleyin. Bundan sonra Azure portalında yeni ayarlarını yansıtır.

## <a name="configuration-with-powershell"></a>PowerShell ile yapılandırma
SQL VM'nizi sağladıktan sonra otomatik düzeltme eki uygulama yapılandırmak için PowerShell kullanın.

Aşağıdaki örnekte, PowerShell, otomatik düzeltme eki uygulama, var olan bir SQL Server sanal makinesinde yapılandırmak için kullanılır. **Yeni AzVMSqlServerAutoPatchingConfig** komut, otomatik güncelleştirmeler için yeni bir bakım penceresi yapılandırır.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = New-AzVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

> [!IMPORTANT]
> Uzantı zaten yüklü değilse, uzantının yüklenmesi, SQL Server hizmetini yeniden başlatır.

Aşağıdaki tabloda, bu örneği temel alarak, hedef Azure VM'si pratik etkisi açıklanmaktadır:

| Parametre | Etki |
| --- | --- |
| **dayOfWeek** |Her Perşembe düzeltme ekleri yüklenmemiş. |
| **MaintenanceWindowStartingHour** |Başlangıç 11: 00'da güncelleştirir. |
| **MaintenanceWindowsDuration** |Düzeltme ekleri 120 dakika içinde yüklü olması gerekir. Başlangıç zamanı temel alınarak, 13: 00'te tarafından tamamlamalıdır. |
| **PatchCategory** |Bu parametre için yalnızca olası ayarı **önemli**. Bu, önemli olarak işaretlenmiş bir Windows güncelleştirmesi yüklenir; Bu kategoride yer almayan tüm SQL Server güncelleştirmelerini yüklemez. |

Bu, yüklemek ve SQL Server Iaas Aracısı'nı yapılandırmak için birkaç dakika sürebilir.

Otomatik düzeltme eki uygulama devre dışı bırakmak için olmadan aynı betiği çalıştırın. **-etkinleştirme** parametresi **yeni AzVMSqlServerAutoPatchingConfig**. Olmaması **-etkinleştirme** parametresi sinyalleri özelliğini devre dışı bırakma komutu.

## <a name="next-steps"></a>Sonraki adımlar
Diğer kullanılabilir otomasyon görevleri hakkında daha fazla bilgi için bkz. [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).

