---
title: SQL vm'lerde (Klasik) yönetim görevlerini otomatikleştiren | Microsoft Docs
description: Bu konuda, belirli SQL Server yönetim görevlerini otomatikleştirir SQL Server Aracısı uzantısı yönetmek açıklar. Bunlar otomatik yedekleme, otomatik düzeltme eki uygulama ve Azure anahtar kasası tümleştirmeyi içerir. Bu konu, Klasik dağıtım modunu kullanır.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/07/2018
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3ff9a8b91b0359c57fae5b1a01b5d895ab9a1685
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
ms.locfileid: "29851767"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-classic"></a>SQL Server Aracısı uzantısı (Klasik) ile Azure sanal makineler üzerinde yönetim görevlerini otomatik hale getirme
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasik](../classic/sql-server-agent-extension.md)
> 
>
 
SQL Server Iaas Aracısı uzantısı (Sqlıaasagent) Azure yönetim görevlerini otomatikleştirmek için sanal makinelerde çalışır. Bu konu, yükleme, durum ve kaldırma yönergeleri yanı sıra uzantısı tarafından desteklenen hizmetlerine genel bakış sağlar.

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalede Resource Manager sürümünü görüntülemek için bkz: [SQL Server Aracısı uzantısı için SQL Server VM'ler Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Desteklenen hizmetler
SQL Server Iaas Aracısı uzantısı aşağıdaki yönetim görevlerini destekler:

| Yönetim özelliği | Açıklama |
| --- | --- |
| **SQL otomatik yedekleme** |Tüm veritabanları için yedekleme VM'de SQL Server'ın varsayılan örneğinin zamanlama otomatikleştirir. Daha fazla bilgi için bkz: [Azure Virtual Machines'de (Klasik) SQL Server için otomatik yedeklemeyi](../classic/sql-automated-backup.md). |
| **SQL otomatik düzeltme eki uygulama** |Güncelleştirmeleri, iş yükü için yoğun saatlerde kaçınmak için hangi sırasında VM önemli Windows güncelleştirmelerini, gerçekleştirilebildiği bir bakım penceresi yapılandırır. Daha fazla bilgi için bkz: [otomatik Azure Virtual Machines'de (Klasik) SQL Server için düzeltme eki uygulama](../classic/sql-automated-patching.md). |
| **Azure Anahtar Kasası Tümleştirmesi** |Otomatik olarak yüklemek ve SQL Server VM'nize üzerinde Azure anahtar kasası yapılandırmanıza olanak sağlar. Daha fazla bilgi için bkz: [(Klasik) Azure vm'lerinde SQL Server için Azure anahtar kasası tümleştirmeyi yapılandırmak](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Önkoşullar
VM üzerinde SQL Server Iaas Aracısı uzantısı kullanmak için gereksinimler:

### <a name="operating-system"></a>İşletim Sistemi:
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

### <a name="sql-server-versions"></a>SQL Server sürümleri:
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:
[İndirme ve en son Azure PowerShell komutlarının yapılandırma](/powershell/azure/overview).

Windows PowerShell'i başlatın ve Azure aboneliğinizle bağlanmak **Add-AzureAccount** komutu.

    Add-AzureAccount

Birden çok aboneliğiniz varsa, kullanmak **Select-AzureSubscription** hedef içeren aboneliği seçmek için Klasik VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Bu noktada, Klasik sanal makineleri ve ilişkili hizmet adlarını listesini almak **Get-AzureVM** komutu.

    Get-AzureVM

## <a name="installation"></a>Yükleme
Klasik VM'ler için SQL Server Iaas Aracısı uzantısını yükleyin ve ilişkili hizmetlerini yapılandırmak için PowerShell kullanmanız gerekir. Kullanım **kümesi AzureVMSqlServerExtension** uzantıyı yüklemek için PowerShell cmdlet. Örneğin, aşağıdaki komutu bir Windows Server VM'ye (Klasik) uzantıyı yükleyen ve "SQLIaaSExtension" adları.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

SQL Iaas Aracısı uzantısı'nın en son sürüm güncelleştirirseniz, uzantı güncelleştirdikten sonra sanal makinenizi yeniden başlatmanız gerekir.

> [!NOTE]
> Klasik sanal makineleri yükleme ve portal üzerinden SQL Iaas Aracısı uzantısı yapılandırma seçeneği yok.
> 
> 

## <a name="status"></a>Durum
Uzantı'nin yüklü olduğunu doğrulamak için bir Azure Portalı'nda aracı durumunu görüntülemek için yoludur. Sanal makine dikey penceresinde listelenen bir sanal makineyi seçin ve ardından tıklatın **uzantıları**. Görmeniz gerekir **Sqlıaasagent** listelenen uzantısı.

![SQL Server Iaas Aracısı uzantısı Azure portalında](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Aynı zamanda **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet'i.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Çıkarma
Azure Portalı'nda üç nokta işaretine tıklayarak uzantısını kaldırabilirsiniz **uzantıları** dikey penceresinde, sanal makine özellikleri. Ardından **kaldırma**.

![Azure Portal'ın SQL Server Iaas Aracısı uzantısını Kaldır](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Aynı zamanda **Kaldır AzureVMSqlServerExtension** Powershell cmdlet'i.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Sonraki Adımlar
Genişletme tarafından desteklenen hizmetlerinden birini kullanarak başlayın. Daha fazla ayrıntı için bkz: başvuru konular [desteklenen Hizmetleri](#supported-services) bu makalenin.

SQL Server Azure sanal makinelerde çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelere genel bakış SQL Server'da](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

