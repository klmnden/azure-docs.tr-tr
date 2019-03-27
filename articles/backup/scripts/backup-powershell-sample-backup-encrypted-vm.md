---
title: Azure PowerShell Betiği örneği - bir Azure sanal makine yedekleme | Microsoft Docs
description: Azure PowerShell betik örneği - bir Azure sanal makinesini yedekleme
services: backup
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 03/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: bc54832b300bf7a70d067f07b9eb7cc67404f2e7
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58496812"
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>PowerShell ile şifrelenmiş bir Azure sanal makineyi yedekleme

Bu betik, şifrelenmiş bir Azure sanal makinesi için coğrafi olarak yedekli depolama (GRS) ile bir kurtarma Hizmetleri kasası oluşturur. Varsayılan koruma ilke kasaya uygulanır. İlke, sanal makine için günlük bir yedekleme oluşturur ve her yedekleme 30 gün boyunca tutar. Betik ayrıca sanal makine için ilk kurtarma noktasını tetikler ve bu kurtarma noktasını 365 gün boyunca tutar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/backup/backup-encrypted-vm/backup-encrypted-vm.ps1 "Back up encrypted virtual machine")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.


| Komut | Notlar | 
|---|---| 
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. | 
| [Yeni AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesvault) | Yedeklemeleri depolamak için bir kurtarma Hizmetleri kasası oluşturur. | 
| [Set-AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperties) | Kurtarma Hizmetleri kasasında depolama özellikleri kümeleri yedekleme. | 
| [Yeni AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)| Kurtarma Hizmetleri Kasası'nda, zamanlama ilkesi kullanarak koruma İlkesi ve bekletme ilkesi oluşturur. | 
| [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) | Key Vault üzerinde hizmet sorumlusuna şifreleme anahtarları için erişim verecek izinleri ayarlar. | 
| [AzRecoveryServicesBackupProtection etkinleştir](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection) | Belirtilen bir yedekleme koruma ilkesi olan bir öğe için yedekleme sağlar. | 
| [Set-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)| Mevcut bir yedekleme koruma ilkesini değiştirir. | 
| [Yedekleme AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem) | Yedekleme zamanlaması için bağlanmayan korumalı bir Azure yedekleme öğesi için bir yedekleme işini başlatır. |
| [Bekleme AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) | Bir Azure yedekleme işini tamamlamak bekler. | 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. | 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

