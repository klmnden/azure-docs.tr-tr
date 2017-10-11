---
title: "Otomatik düzeltme eki uygulama SQL Server Vm'lerinin (Klasik) | Microsoft Docs"
description: "Otomatik düzeltme eki uygulama özelliği için SQL Server Klasik dağıtım modunu kullanarak Azure'da çalışan sanal makineler açıklanmıştır."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 1959871141f196ba80ffd7b37e62e5ea5b42dba3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Otomatik Azure sanal makinelerde (Klasik) SQL Server için düzeltme eki uygulama
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [Klasik](../classic/sql-automated-patching.md)
> 
> 

Otomatik düzeltme eki bir Azure SQL Server çalıştıran sanal makine için bir bakım penceresi oluşturur. Otomatik Güncelleştirmeler, yalnızca bu bakım penceresi sırasında yüklenebilir. SQL Server için bu sistem güncelleştirmelerini ve ilişkili tüm yeniden başlatmalar veritabanı için olası en iyi zaman gerçekleşmesini sağlar. Otomatik düzeltme eki bağımlı [SQL Server Iaas Aracısı uzantısı](../classic/sql-server-agent-extension.md).

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalede Resource Manager sürümünü görüntülemek için bkz: [otomatik düzeltme eki uygulama SQL Server Azure sanal makineleri Kaynak Yöneticisi'nde](../sql/virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Ön koşullar
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

* [En son Azure PowerShell komutlarını yüklemek](/powershell/azure/overview).

**SQL Server Iaas uzantısı**:

* [SQL Server Iaas uzantısını yükleyin](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda, otomatik düzeltme eki uygulama için yapılandırılabilir seçenekler açıklanmaktadır. Klasik VM'ler için bu ayarları yapılandırmak için PowerShell kullanmanız gerekir.

| Ayar | Olası değerler | Açıklama |
| --- | --- | --- |
| **Otomatik Düzeltme Eki Uygulama** |Etkinleştir/devre dışı bırak (devre dışı) |Etkinleştirir veya bir Azure sanal makine için otomatik düzeltme eki uygulamayı devre dışı bırakır. |
| **Bakım zamanlaması** |Her gün, Pazartesi, Salı, Çarşamba, Perşembe, Cuma, Cumartesi Pazar |Sanal makineniz için Windows, SQL Server ve Microsoft güncelleştirmeler indiriliyor ve yükleniyor zamanlamasını. |
| **Bakım başlangıç saati** |0-24 |Sanal makineyi güncelleştirmek için yerel başlangıç saati. |
| **Bakım penceresinin süresi** |30-180 |İndirme ve güncelleştirmeleri yüklemesini tamamlamak için izin verilen dakika sayısı. |
| **Düzeltme eki kategorisi** |Önemli |Güncelleştirmeleri indirmek ve yüklemek için kategorisi. |

## <a name="configuration-with-powershell"></a>PowerShell ile yapılandırma
Aşağıdaki örnekte, PowerShell otomatik düzeltme eki uygulama üzerinde mevcut bir SQL Server VM'yi yapılandırmak için kullanılır. **Yeni AzureVMSqlServerAutoPatchingConfig** komut, otomatik güncelleştirmeler için yeni bir bakım penceresi yapılandırır.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Aşağıdaki tabloda, bu örneği temel alarak, hedef Azure VM pratik etkisi açıklanmaktadır:

| Parametre | Etki |
| --- | --- |
| **DayOfWeek** |Her Perşembe düzeltme ekleri yüklenmemiş. |
| **MaintenanceWindowStartingHour** |Begin 11: 00'da güncelleştirir. |
| **MaintenanceWindowsDuration** |Düzeltme ekleri 120 dakika içinde yüklü olması gerekir. Başlangıç zamanı temel alınarak, 1:00 pm tarafından tamamlamalıdır. |
| **PatchCategory** |Bu parametre için yalnızca olası ayar "Önemli" dir. |

Yüklemek ve SQL Server Iaas aracısı yapılandırmak için birkaç dakika sürebilir.

Otomatik düzeltme eki uygulamayı devre dışı bırakmak için aynı Çalıştır komut dosyası yeni AzureVMSqlServerAutoPatchingConfig Enable parametresi olmadan. Yüklemesiyle olduğu gibi otomatik düzeltme eki uygulamayı devre dışı bırakmak için birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Diğer kullanılabilir Otomasyon görevler hakkında daha fazla bilgi için bkz: [SQL Server Iaas Aracısı uzantısı](../classic/sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelere genel bakış SQL Server'da](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

