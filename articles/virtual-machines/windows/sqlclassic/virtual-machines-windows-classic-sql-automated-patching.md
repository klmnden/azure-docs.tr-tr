---
title: Otomatik SQL Server Vm'leri (Klasik) için düzeltme eki uygulama | Microsoft Docs
description: Otomatik düzeltme eki uygulama özelliği için SQL Server Klasik dağıtım modunu kullanarak Azure'da çalışan sanal makineler açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/07/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: aa912e3eb76d72e7a79c83d7e51d493310bd36b3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60362144"
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Otomatik düzeltme eki uygulama SQL Server Azure sanal makineler'de (Klasik)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [Klasik](../classic/sql-automated-patching.md)
> 
> 

Otomatik düzeltme eki uygulama, bir Azure sanal makinesinde SQL Server çalıştıran bir bakım penceresi oluşturur. Otomatik Güncelleştirmeler yalnızca bu bakım penceresi sırasında yüklenebilir. SQL Server için sistem güncelleştirmelerini ve ilişkili tüm yeniden başlatmalar veritabanı için en iyi olası zaman gerçekleşmesini sağlar. 

> [!IMPORTANT]
> Yalnızca **Önemli** olarak işaretlenmiş Windows güncelleştirmeleri yüklenir. Toplu Güncelleştirmeler gibi diğer SQL Server güncelleştirmelerinin el ile yüklenmesi gerekir. 

Otomatik Yama Uygulama [SQL Server IaaS Aracı Uzantısı](../classic/sql-server-agent-extension.md)'na bağımlıdır.

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalede Resource Manager sürümünü görüntülemek için bkz: [otomatik düzeltme eki uygulama SQL Server Azure sanal makineler Kaynak Yöneticisi'nde](../sql/virtual-machines-windows-sql-automated-patching.md).

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

* [En son Azure PowerShell komutlarını yükleme](/powershell/azure/overview).

**SQL Server Iaas uzantısı**:

* [SQL Server Iaas uzantısı yükleme](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda, otomatik düzeltme eki uygulama için yapılandırılmış seçenekler açıklanmaktadır. Klasik VM'ler için bu ayarları yapılandırmak için PowerShell kullanmanız gerekir.

| Ayar | Olası değerler | Açıklama |
| --- | --- | --- |
| **Otomatik Düzeltme Eki Uygulama** |Etkinleştir/devre dışı bırak (devre dışı) |Etkinleştirir veya bir Azure sanal makinesi için otomatik düzeltme eki uygulama devre dışı bırakır. |
| **Bakım zamanlaması** |Her gün, Pazartesi, Salı, Çarşamba, Thursday, Friday, Cumartesi, Pazar |İndirme ve sanal makineniz için Windows, SQL Server ve Microsoft güncelleştirmelerini yükleme zamanlamasını. |
| **Bakım başlangıç saati** |0-24 |Sanal makineyi güncelleştirmek için yerel başlangıç zamanı. |
| **Bakım penceresi süresine** |30-180 |İndirme ve güncelleştirmelerin yüklenmesini tamamlamak için izin verilen dakika sayısı. |
| **Düzeltme eki kategorisi** |Önemli |Güncelleştirmeleri indirmek ve yüklemek için kategori. |

## <a name="configuration-with-powershell"></a>PowerShell ile yapılandırma
Aşağıdaki örnekte, PowerShell, otomatik düzeltme eki uygulama, var olan bir SQL Server sanal makinesinde yapılandırmak için kullanılır. **Yeni AzureVMSqlServerAutoPatchingConfig** komut, otomatik güncelleştirmeler için yeni bir bakım penceresi yapılandırır.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Aşağıdaki tabloda, bu örneği temel alarak, hedef Azure VM'si pratik etkisi açıklanmaktadır:

| Parametre | Etki |
| --- | --- |
| **dayOfWeek** |Her Perşembe düzeltme ekleri yüklenmemiş. |
| **MaintenanceWindowStartingHour** |Başlangıç 11: 00'da güncelleştirir. |
| **MaintenanceWindowDuration** |Düzeltme ekleri 120 dakika içinde yüklü olması gerekir. Başlangıç zamanı temel alınarak, 13: 00'te tarafından tamamlamalıdır. |
| **PatchCategory** |Bu parametre yalnızca olası ayarı "Önemli" dir. |

Bu, yüklemek ve SQL Server Iaas Aracısı'nı yapılandırmak için birkaç dakika sürebilir.

Otomatik düzeltme eki uygulama devre dışı bırakmak için aynı çalıştırın New-AzureVMSqlServerAutoPatchingConfig etkinleştirme parametresi olmadan komut. Yüklemesiyle olduğu gibi otomatik düzeltme eki uygulama devre dışı bırakmak için birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Diğer kullanılabilir otomasyon görevleri hakkında daha fazla bilgi için bkz. [SQL Server Iaas Aracısı uzantısı](../classic/sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

