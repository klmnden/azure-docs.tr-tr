---
title: SQL Server sanal makineleri (Klasik) için otomatik yedekleme | Microsoft Docs
description: "Kaynak Yöneticisi'ni kullanarak Azure sanal makinelerinde çalışan SQL Server için otomatik yedekleme özelliğini açıklar. "
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 3333e830-8a60-42f5-9f44-8e02e9868d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/23/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: aeb97d661d330ed6afb3ca5e5e1eb924dacc4024
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60607710"
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Azure sanal makineler (Klasik) SQL Server için otomatik yedekleme
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [Klasik](../classic/sql-automated-backup.md)
> 
> 

Otomatik yedekleme otomatik olarak yapılandırır [yönetilen Microsoft Azure yedekleme](https://msdn.microsoft.com/library/dn449496.aspx) Azure VM'deki SQL Server 2014 Standard veya Enterprise çalıştıran tüm mevcut ve yeni veritabanları için. Bu, kalıcı Azure blob depolama kullanan normal veritabanı yedeklemelerini yapılandırma sağlar. Otomatik yedekleme bağlıdır [SQL Server Iaas Aracısı uzantısı](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalede Resource Manager sürümünü görüntülemek için bkz: [Azure sanal makineler Kaynak Yöneticisi'nde SQL Server için otomatik yedekleme](../sql/virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Önkoşullar
Otomatik yedekleme kullanmak için aşağıdaki önkoşulları göz önünde bulundurun:

**İşletim sistemi**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server sürümü**:

* SQL Server 2014 Standard
* SQL Server 2014 Enterprise

> [!NOTE]
> SQL Server 2016 için otomatik yedekleme ile Resource Manager sanal makinelerinde desteklenir. Daha fazla bilgi için [otomatik yedekleme v2 için SQL Server 2016 Azure Virtual Machines'de (Resource Manager)](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2).

**Veritabanı yapılandırması**:

* Hedef veritabanlarının tam kurtarma modelini kullanmanız gerekir.

**Azure PowerShell**:

* [En son Azure PowerShell komutlarını yükleme](/powershell/azure/overview).

**SQL Server Iaas uzantısı**:

* [SQL Server Iaas uzantısı yükleme](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda, otomatik yedekleme için yapılandırılmış seçenekler açıklanmaktadır. Klasik VM'ler için bu ayarları yapılandırmak için PowerShell kullanmanız gerekir.

| Ayar | Aralığı (varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** |Etkinleştir/devre dışı bırak (devre dışı) |Etkinleştirir veya SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM için otomatik yedekleme devre dışı bırakır. |
| **Saklama dönemi** |1-30 gün (30 gün) |Bir yedekleme saklanacağı gün sayısı. |
| **Depolama Hesabı** |Azure depolama hesabı (belirtilen VM'si için oluşturulan depolama hesabı) |Otomatik yedekleme dosyalarını blob storage'da depolamak için kullanılacak bir Azure depolama hesabı. Tüm yedekleme dosyalarını depolamak için bu konumda bir kapsayıcı oluşturulur. Yedekleme dosyası adlandırma kuralı, tarih, saat ve makine adı içerir. |
| **Şifreleme** |Etkinleştir/devre dışı bırak (devre dışı) |Etkinleştirir veya şifreleme devre dışı bırakır. Şifreleme etkin olduğunda, bir yedeklemeyi geri yüklemek için kullanılan sertifikaları aynı automaticbackup kapsayıcı adlandırma kuralını kullanarak belirtilen depolama hesabında yer alır. Parola değişirse, bu parolayla yeni bir sertifika oluşturulur, ancak önceki yedeklemeleri geri yüklemek için eski sertifika kalır. |
| **Parola** |Parola metin (hiçbiri) |Şifreleme anahtarları parolası. Bu sadece, şifreleme etkinse gereken. Şifrelenmiş bir yedeklemeyi geri yüklemek için doğru parolayı ve yedeğin alındığı anda kullanıldı ilgili sertifika olmalıdır. |
| **Yedekleme sistemi veritabanları** | Etkinleştir/devre dışı bırak (devre dışı) | Tam Master, Model ve MSDB yedekleri alın |
| **Yedekleme zamanlamasını yapılandırma** | El ile otomatik / (otomatik) | Seçin **otomatik** otomatik olarak tam olarak yararlanın ve günlük yedeklemelerine Günlük büyüme üzerinde temel. Seçin **el ile** tam zamanlama belirtmek için ve günlük yedeklemeler. |

## <a name="configuration-with-powershell"></a>PowerShell ile yapılandırma
Aşağıdaki PowerShell örneği, mevcut bir SQL Server 2014 VM için otomatik yedekleme yapılandırılır. **Yeni AzureVMSqlServerAutoBackupConfig** komut $storageaccount değişkeni tarafından belirtilen Azure depolama hesabında yedeklemeleri depolamak için otomatik yedekleme ayarlarını yapılandırır. Bu yedeklemeler 10 gün boyunca saklanır. **Kümesi AzureVMSqlServerExtension** komutu bu ayarlarla belirtilen Azure sanal Makineyi güncelleştirir.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Bu, yüklemek ve SQL Server Iaas Aracısı'nı yapılandırmak için birkaç dakika sürebilir.

Şifrelemeyi etkinleştirmek için EnableEncryption parametre CertificatePassword parametresi için bir parola (güvenli dize) birlikte geçirmek için önceki komut dosyasını değiştirin. Aşağıdaki betik, önceki örnekte otomatik yedekleme ayarlarını etkinleştirir ve şifreleme ekler.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Otomatik yedekleme devre dışı bırakmak için aynı betiği olmadan çalıştırın **-etkinleştirme** parametresi **yeni AzureVMSqlServerAutoBackupConfig**. Yükleme gibi ile otomatik yedekleme devre dışı bırakmak için birkaç dakika sürebilir.

> [!NOTE]
> Daha önce yapılandırılan yönetilen yedekleme ayarları devre dışı bırakma ve SQL Server Iaas aracısı kaldırma kaldırmaz. Devre dışı bırakma veya SQL Server Iaas Aracısı kaldırmadan önce otomatik yedekleme devre dışı bırakmalısınız.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Otomatik yedekleme, yedekleme yönetilen Azure Vm'lerinde yapılandırır. Bu nedenle için önemlidir [yönetilen yedekleme için belgeleri gözden](https://msdn.microsoft.com/library/dn449496.aspx) davranışını ve etkilerini anlamak için.

Ek bir yedek bulmak ve Azure vm'lerde SQL Server için yönergeler de şu konudaki geri yükleyebilirsiniz: [Yedekleme ve Azure sanal makineler'de SQL Server için geri yükleme](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Diğer kullanılabilir otomasyon görevleri hakkında daha fazla bilgi için bkz. [SQL Server Iaas Aracısı uzantısı](../classic/sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

