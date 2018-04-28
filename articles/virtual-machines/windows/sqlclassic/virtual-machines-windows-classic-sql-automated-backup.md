---
title: Otomatik yedekleme SQL Server sanal makineleri (Klasik) | Microsoft Docs
description: 'Resource Manager kullanarak Azure sanal makineleri olarak çalışan SQL Server için otomatik yedekleme özelliğini açıklar. '
services: virtual-machines-windows
documentationcenter: na
author: rothja
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
ms.author: jroth
ms.openlocfilehash: 3bca1c6c357527a32de499ac9207b1bb734dad7b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Azure sanal makinelerde (Klasik) SQL Server için otomatik yedekleme
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-backup.md)
> * [Klasik](../classic/sql-automated-backup.md)
> 
> 

Otomatik yedekleme otomatik olarak yapılandırır [yönetilen Microsoft Azure yedekleme](https://msdn.microsoft.com/library/dn449496.aspx) SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM üzerindeki tüm mevcut ve yeni veritabanları için. Bu, dayanıklı Azure blob depolama alanını normal veritabanı yedeklemeleri yapılandırmanıza olanak sağlar. Otomatik yedekleme bağımlı [SQL Server Iaas Aracısı uzantısı](../classic/sql-server-agent-extension.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu makalede Resource Manager sürümünü görüntülemek için bkz: [otomatik yedekleme SQL Server Azure sanal makineleri Kaynak Yöneticisi'nde](../sql/virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Önkoşullar
Otomatik yedekleme kullanmak için aşağıdaki önkoşulları göz önünde bulundurun:

**İşletim sistemi**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server sürümü/yayını**:

* SQL Server 2014 Standard
* SQL Server 2014 Enterprise

> [!NOTE]
> Otomatik yedekleme SQL Server 2016 için Resource Manager sanal makineleri ile desteklenir. Daha fazla bilgi için bkz: [otomatik yedekleme v2 için SQL Server 2016 Azure sanal makineleri (Resource Manager)](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2).

**Veritabanı yapılandırması**:

* Hedef veritabanlarına tam kurtarma modeli kullanmanız gerekir.

**Azure PowerShell**:

* [En son Azure PowerShell komutlarını yüklemek](/powershell/azure/overview).

**SQL Server Iaas uzantısı**:

* [SQL Server Iaas uzantısını yükleyin](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda otomatik yedekleme için yapılandırılmış seçenekler açıklanmaktadır. Klasik VM'ler için bu ayarları yapılandırmak için PowerShell kullanmanız gerekir.

| Ayar | Aralık (varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** |Etkinleştir/devre dışı bırak (devre dışı) |Etkinleştirir veya SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM için otomatik yedekleme devre dışı bırakır. |
| **Saklama süresi** |1-30 gün (30 gün) |Bir yedekleme saklanacağı gün sayısı. |
| **Depolama Hesabı** |Azure depolama hesabı (belirtilen VM için oluşturulan depolama hesabı) |Blob depolama alanına otomatik yedekleme dosyalarını depolamak için kullanılacak bir Azure depolama hesabı. Bir kapsayıcı tüm yedekleme dosyalarını depolamak için bu konumda oluşturulur. Yedekleme dosyası adlandırma kuralı, tarih, saat ve makine adını içerir. |
| **Şifreleme** |Etkinleştir/devre dışı bırak (devre dışı) |Etkinleştirir veya şifreleme devre dışı bırakır. Şifreleme etkinleştirildiğinde, yedekleme geri yüklemek için kullanılan sertifikaları belirtilen depolama hesabının aynı adlandırma kuralını kullanarak aynı automaticbackup kapsayıcısında bulunur. Parola değişirse, bu parola ile yeni bir sertifika oluşturulur, ancak önceki yedekleri geri yüklemek için eski sertifika kalır. |
| **Parola** |Parola metin (yok) |Şifreleme anahtarları için bir parola. Yalnızca budur şifreleme etkin olup olmadığını gerekli. Şifrelenmiş bir yedeklemeyi geri yüklemek için doğru parolayı ve yedeğin alındığı anda kullanılan ilgili sertifika olması gerekir. | **Yedekleme sistem veritabanları** | Etkinleştir/devre dışı bırak (devre dışı) | Master, Model ve MSDB tam yedekleme gerçekleştirin |
| **Yedekleme zamanlaması yapılandırma** | El ile otomatik / (otomatik) | Seçin **otomatik** otomatik olarak tam alıp Günlük büyüme üzerinde bağlı olarak yedeklemeler oturum. Seçin **el ile** tam zamanlama belirtmek için ve günlük yedekleri. |

## <a name="configuration-with-powershell"></a>PowerShell ile yapılandırma
Aşağıdaki PowerShell örnekte otomatik yedekleme için mevcut bir SQL Server 2014 VM'yi yapılandırılır. **Yeni AzureVMSqlServerAutoBackupConfig** komut $storageaccount değişkeni tarafından belirtilen Azure depolama hesabındaki yedeklemelerini depolamak için otomatik yedekleme ayarlarını yapılandırır. Bu yedeklemeler 10 gün boyunca saklanır. **Kümesi AzureVMSqlServerExtension** komutu bu ayarlarla belirtilen Azure VM güncelleştirir.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Yüklemek ve SQL Server Iaas aracısı yapılandırmak için birkaç dakika sürebilir.

Şifrelemeyi etkinleştirmek için EnableEncryption parametresi CertificatePassword parametresi için bir parola (güvenli dize) ile birlikte geçirmek için önceki komut dosyasını değiştirin. Aşağıdaki komut dosyası, önceki örnekte otomatik yedekleme ayarlarını etkinleştirir ve şifreleme ekler.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Otomatik yedekleme devre dışı bırakmak için olmadan aynı komut dosyasını Çalıştır **-etkinleştirmek** parametresi **yeni AzureVMSqlServerAutoBackupConfig**. Yüklemesine gibi otomatik yedekleme devre dışı bırakmak için birkaç dakika sürebilir.

> [!NOTE]
> Önceden yapılandırılmış yönetilen Yedekleme ayarlarını devre dışı bırakma ve SQL Server Iaas aracısı kaldırma kaldırmaz. Devre dışı bırakma veya SQL Server Iaas Aracısı kaldırmadan önce otomatik yedekleme devre dışı bırakmalısınız.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Otomatik yedekleme yönetilen yedekleme Azure Vm'lerinde yapılandırır. İçin önemlidir [yönetilen yedekleme belgelerini gözden](https://msdn.microsoft.com/library/dn449496.aspx) davranışı ve etkilerini anlamak için.

Ek yedekleme bulmak ve Azure vm'lerinde SQL Server Kılavuzu şu konuda geri yükleyebilirsiniz: [yedekleme ve geri yükleme Azure Virtual Machines'de SQL Server için](../sql/virtual-machines-windows-sql-backup-recovery.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Diğer kullanılabilir Otomasyon görevler hakkında daha fazla bilgi için bkz: [SQL Server Iaas Aracısı uzantısı](../classic/sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelere genel bakış SQL Server'da](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

