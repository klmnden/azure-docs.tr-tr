---
title: "Otomatik düzeltme eki uygulama SQL Server Vm'lerinin (Resource Manager) | Microsoft Docs"
description: "Otomatik düzeltme eki uygulama özelliği için SQL Server Kaynak Yöneticisi'ni kullanarak Azure'da çalışan sanal makineler açıklanmıştır."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/05/2018
ms.author: jroth
ms.openlocfilehash: c1cdf03133d765f7726d16378b042de8e04b2cfc
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Azure Virtual Machines’de (Resource Manager) SQL Server için Otomatik Düzeltme Eki Uygulama
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-sql-automated-patching.md)

Otomatik düzeltme eki bir Azure SQL Server çalıştıran sanal makine için bir bakım penceresi oluşturur. Otomatik Güncelleştirmeler, yalnızca bu bakım penceresi sırasında yüklenebilir. SQL Server için bu rescriction sistem güncelleştirmelerini ve ilişkili tüm yeniden başlatmalar veritabanı için olası en iyi zaman gerçekleşmesini sağlar. Otomatik düzeltme eki bağımlı [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu makalede klasik sürümünü görüntülemek için bkz: [otomatik düzeltme eki uygulama için Azure sanal makineleri Klasik SQL Server'da](../sqlclassic/virtual-machines-windows-classic-sql-automated-patching.md).

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

* [En son Azure PowerShell komutlarını yüklemek](/powershell/azure/overview) otomatik düzeltme eki uygulama PowerShell ile yapılandırmayı planlıyorsanız.

> [!NOTE]
> Otomatik düzeltme eki SQL Server Iaas Aracısı uzantısını kullanır. Geçerli SQL sanal makineye Galerisi görüntülerini varsayılan olarak bu uzantı ekleyin. Daha fazla bilgi için bkz: [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda, otomatik düzeltme eki uygulama için yapılandırılabilir seçenekler açıklanmaktadır. Gerçek yapılandırma adımları Azure portalında veya Azure Windows PowerShell komutlarını kullanmadığınıza bağlı olarak farklılık gösterir.

| Ayar | Olası değerler | Açıklama |
| --- | --- | --- |
| **Otomatik Düzeltme Eki Uygulama** |Etkinleştir/devre dışı bırak (devre dışı) |Etkinleştirir veya bir Azure sanal makine için otomatik düzeltme eki uygulamayı devre dışı bırakır. |
| **Bakım zamanlaması** |Her gün, Pazartesi, Salı, Çarşamba, Perşembe, Cuma, Cumartesi Pazar |Sanal makineniz için Windows, SQL Server ve Microsoft güncelleştirmeler indiriliyor ve yükleniyor zamanlamasını. |
| **Bakım başlangıç saati** |0-24 |Sanal makineyi güncelleştirmek için yerel başlangıç saati. |
| **Bakım penceresinin süresi** |30-180 |İndirme ve güncelleştirmeleri yüklemesini tamamlamak için izin verilen dakika sayısı. |
| **Düzeltme eki kategorisi** |Önemli |Güncelleştirmeleri indirmek ve yüklemek için kategorisi. |

## <a name="configuration-in-the-portal"></a>Portal Yapılandırması
Otomatik düzeltme eki uygulama sağlama sırasında veya var olan VM'ler için yapılandırmak için Azure portalını kullanabilirsiniz.

### <a name="new-vms"></a>Yeni sanal makineleri
Resource Manager dağıtım modelinde yeni bir SQL Server sanal makine oluşturduğunuzda, otomatik düzeltme eki uygulama yapılandırmak için Azure Portalı'nı kullanın.

İçinde **SQL Server ayarları** dikey penceresinde, select **otomatik düzeltme eki uygulama**. Aşağıdaki Azure portal ekran görüntüsü gösterildiği **SQL otomatik düzeltme eki uygulama** dikey.

![SQL otomatik düzeltme eki Azure portalında](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Bağlam için tam üzerinde konusuna [azure'da bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Var olan sanal makineleri
Var olan SQL Server sanal makineler için SQL Server sanal makine seçin. Ardından **SQL Server yapılandırma** bölümünü **ayarları** dikey.

![SQL otomatik düzeltme eki uygulama var olan VM'ler için](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

İçinde **SQL Server yapılandırma** dikey penceresinde tıklatın **Düzenle** bölüm düzeltme eki uygulama otomatik düğmesini.

![SQL otomatik düzeltme eki uygulama var olan VM'ler için yapılandırma](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Tamamlandığında, tıklatın **Tamam** alt düğmesinde **SQL Server yapılandırma** yaptığınız değişiklikleri kaydetmek için dikey.

Otomatik düzeltme eki uygulama ilk kez etkinleştiriyorsanız Azure SQL Server Iaas Aracısı arka planda yapılandırır. Bu süre boyunca, Azure portalında otomatik düzeltme eki uygulama yapılandırıldığını gösterilmeyebilir. Aracının yüklü, yapılandırılmış birkaç dakika bekleyin. Bundan sonra Azure portalını yeni ayarlarını yansıtır.

> [!NOTE]
> Bir şablon kullanarak otomatik düzeltme eki uygulama da yapılandırabilirsiniz. Daha fazla bilgi için bkz: [otomatik düzeltme eki uygulama için Azure Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).
> 
> 

## <a name="configuration-with-powershell"></a>PowerShell ile yapılandırma
SQL VM'nizi sağladıktan sonra otomatik düzeltme eki uygulama yapılandırmak için PowerShell kullanın.

Aşağıdaki örnekte, PowerShell otomatik düzeltme eki uygulama üzerinde mevcut bir SQL Server VM'yi yapılandırmak için kullanılır. **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** komut, otomatik güncelleştirmeler için yeni bir bakım penceresi yapılandırır.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

> [!IMPORTANT]
> Uzantısı yükleme uzantısı zaten yüklü değilse, SQL Server hizmetini yeniden başlatır.

Aşağıdaki tabloda, bu örneği temel alarak, hedef Azure VM pratik etkisi açıklanmaktadır:

| Parametre | Etki |
| --- | --- |
| **DayOfWeek** |Her Perşembe düzeltme ekleri yüklenmemiş. |
| **MaintenanceWindowStartingHour** |Begin 11: 00'da güncelleştirir. |
| **MaintenanceWindowsDuration** |Düzeltme ekleri 120 dakika içinde yüklü olması gerekir. Başlangıç zamanı temel alınarak, 1:00 pm tarafından tamamlamalıdır. |
| **PatchCategory** |Bu parametre yalnızca olası ayarı **önemli**. |

Yüklemek ve SQL Server Iaas aracısı yapılandırmak için birkaç dakika sürebilir.

Otomatik düzeltme eki uygulamayı devre dışı bırakmak için olmadan aynı komut dosyasını Çalıştır **-etkinleştirmek** parametresi **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Yokluğu **-etkinleştirmek** parametresi sinyalleri özelliğini devre dışı bırakmak için komutu.

## <a name="next-steps"></a>Sonraki adımlar
Diğer kullanılabilir Otomasyon görevler hakkında daha fazla bilgi için bkz: [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelere genel bakış SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md).

