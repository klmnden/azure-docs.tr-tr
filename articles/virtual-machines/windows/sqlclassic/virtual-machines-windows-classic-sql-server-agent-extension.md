---
title: SQL Vm'leri (Klasik) yönetim görevlerini otomatik hale getirin | Microsoft Docs
description: Bu konuda, belirli SQL Server yönetim görevlerini otomatikleştiren SQL Server Aracısı uzantısı yönetme işlemi açıklanır. Bunlar, otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure anahtar kasası tümleştirmeyi içerir. Bu konuda, Klasik dağıtım modunu kullanır.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 2b719185aabd39cd70b9cb890a9599aa06ca4ff4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60334854"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-classic"></a>SQL Server Aracısı uzantısı (Klasik) ile Azure sanal Makineler'de yönetim görevlerini otomatikleştirin
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasik](../classic/sql-server-agent-extension.md)
> 
>
 
SQL Server Iaas Aracısı uzantısı (Sqlıaasagent), Azure yönetim görevlerini otomatikleştirmek için sanal makinelerde çalıştırır. Bu konu, yükleme, durum ve kaldırma yönergeleri yanı sıra uzantı tarafından desteklenen hizmetleri genel bir bakış sağlar.

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalede Resource Manager sürümünü görüntülemek için bkz: [SQL Server Aracısı uzantısı için SQL Server Vm'leri Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Desteklenen hizmetler
SQL Server Iaas Aracısı uzantısı, aşağıdaki yönetim görevlerini destekler:

| Yönetim özelliği | Açıklama |
| --- | --- |
| **SQL otomatik yedekleme** |Tüm veritabanları için yedekleme VM'de SQL Server'ın varsayılan örneğinin zamanlama otomatikleştirir. Daha fazla bilgi için [Azure Virtual Machines'de (Klasik) SQL Server için otomatik yedekleme](../classic/sql-automated-backup.md). |
| **SQL otomatik düzeltme eki uygulama** |İş yükünüz için yoğun saatlerde güncelleştirmeleri kurtulabilirsiniz aşamasında vm'niz önemli Windows güncelleştirmelerini, gerçekleştirilebildiği bir bakım penceresi yapılandırır. Daha fazla bilgi için [otomatik SQL Server Azure sanal makineler (Klasik) içinde düzeltme eki](../classic/sql-automated-patching.md). |
| **Azure Anahtar Kasası Tümleştirmesi** |Otomatik olarak yüklemeniz ve SQL Server sanal makinenizde Azure Key Vault yapılandırma sağlar. Daha fazla bilgi için [(Klasik) Azure vm'lerde SQL Server için Azure anahtar kasası tümleştirmeyi yapılandırma](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Önkoşullar
Sanal makinenizde SQL Server Iaas Aracısı uzantısı kullanmak için gereksinimler:

### <a name="operating-system"></a>İşletim Sistemi:
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

### <a name="sql-server-versions"></a>SQL Server sürümleri:
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:
[İndirme ve yapılandırma en son Azure PowerShell komutlarını](/powershell/azure/overview).

Windows PowerShell'i başlatın ve Azure aboneliğinize bağlayın **Add-AzureAccount** komutu.

    Add-AzureAccount

Birden çok aboneliğiniz varsa, kullanmak **Select-AzureSubscription** hedef içeren aboneliği seçmek için Klasik VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Bu noktada, Klasik sanal makineleri ve ile ilişkili hizmet adları listesini alabilirsiniz **Get-AzureVM** komutu.

    Get-AzureVM

## <a name="installation"></a>Yükleme
Klasik VM'ler için SQL Server Iaas Aracısı uzantısı yükleyin ve bununla ilişkili hizmetlere yapılandırmak için PowerShell kullanmanız gerekir. Kullanım **kümesi AzureVMSqlServerExtension** uzantıyı yüklemek için PowerShell cmdlet'i. Örneğin, aşağıdaki komut, bir Windows Server VM (Klasik) uzantıyı yükleyen ve "SQLIaaSExtension" adları.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

SQL Iaas Aracısı uzantısı'nın en son sürüme güncelleştirme, uzantı güncelleştirildikten sonra sanal makinenizi yeniden başlatmanız gerekir.

> [!NOTE]
> Klasik sanal makineleri yükleme ve SQL Iaas Aracısı uzantısı portal üzerinden yapılandırma seçeneği yoktur.

> [!NOTE]
> SQL Server Iaas Aracısı uzantısı yalnızca desteklenir [SQL Server sanal makine galeri görüntüleri](../sql/virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms) (Kullandıkça Öde veya Getir-kendi lisansını). SQL Server bir yalnızca işletim sistemi Windows Server sanal makinesi üzerinde el ile yüklerseniz veya özelleştirilmiş bir SQL Server VM VHD dağıtırsanız desteklenmiyor. Bu gibi durumlarda, yüklemek ve PowerShell kullanarak uzantıyı el ile yönetmek mümkün olabilir, ancak bunun yerine bir SQL Server VM galeri görüntüsü yükleyin ve ardından özelleştirmek için önemle tavsiye edilir.

## <a name="status"></a>Durum
Uzantı yüklendiğini doğrulamak için bir Azure Portalı'nda aracı durumunu görüntülemek için yoludur. Sanal makine dikey pencerede listelenen sanal makine seçin ve ardından **uzantıları**. Görmelisiniz **Sqlıaasagent** listelenen uzantısı.

![Azure portalında SQL Server Iaas Aracısı uzantısı](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Ayrıca **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet'i.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Temizleme
Azure Portalı'nda üç nokta simgesine tıklayarak uzantıyı kaldırabilirsiniz **uzantıları** dikey penceresinde, sanal makine özellikleri. Ardından **kaldırma**.

![Azure portalında SQL Server Iaas Aracısı uzantısını kaldırma](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Ayrıca **Remove-AzureVMSqlServerExtension** Powershell cmdlet'i.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Sonraki Adımlar
Uzantı tarafından desteklenen hizmetlerinden birini kullanmaya başlayın. Daha fazla ayrıntı için bkz: başvurulan konular [desteklenen Hizmetleri](#supported-services) bu makalenin.

Azure sanal Makineler'de SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

