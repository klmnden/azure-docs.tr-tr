---
title: Azure PowerShell Betiği örneği - bir Azure sanal makine yedekleme | Microsoft Docs
description: Azure PowerShell betik örneği - bir Azure sanal makinesini yedekleme
services: backup
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 01/31/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 915c5f6c8e8de1b5a7a7590ba41125cbff7b8f36
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497647"
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>PowerShell ile şifrelenmiş bir Azure sanal makineyi yedekleme

Bu betik, şifrelenmiş bir Azure sanal makinesi için coğrafi olarak yedekli depolama (GRS) ile bir kurtarma Hizmetleri kasası oluşturur. Varsayılan koruma ilke kasaya uygulanır. İlke, sanal makine için günlük bir yedekleme oluşturur ve her yedekleme 30 gün boyunca tutar. Betik ayrıca sanal makine için ilk kurtarma noktasını tetikler ve bu kurtarma noktasını 365 gün boyunca tutar. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/backup/backup-encrypted-vm/backup-encrypted-vm.ps1 "Back up encrypted virtual machine")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.


| Komut | Notlar | 
|---|---| 
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. | 
| [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/New-AzureRmRecoveryServicesVault) | Yedeklemeleri depolamak için bir kurtarma Hizmetleri kasası oluşturur. | 
| [Set-AzureRmRecoveryServicesBackupProperties](/powershell/module/azurerm.recoveryservices/Set-AzureRmRecoveryServicesBackupProperties) | Kurtarma Hizmetleri kasasında depolama özellikleri kümeleri yedekleme. | 
| [Yeni-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)| Kurtarma Hizmetleri Kasası'nda, zamanlama ilkesi kullanarak koruma İlkesi ve bekletme ilkesi oluşturur. | 
| [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Key Vault üzerinde hizmet sorumlusuna şifreleme anahtarları için erişim verecek izinleri ayarlar. | 
| [Enable-AzureRmRecoveryServicesBackupProtection](/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection) | Belirtilen bir yedekleme koruma ilkesi olan bir öğe için yedekleme sağlar. | 
| [Set-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy)| Mevcut bir yedekleme koruma ilkesini değiştirir. | 
| [Backup-Azurermrecoveryservicesbackupıtem](/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem) | Yedekleme zamanlaması için bağlanmayan korumalı bir Azure yedekleme öğesi için bir yedekleme işini başlatır. |
| [Wait-AzureRmRecoveryServicesBackupJob](/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob) | Bir Azure yedekleme işini tamamlamak bekler. | 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. | 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

