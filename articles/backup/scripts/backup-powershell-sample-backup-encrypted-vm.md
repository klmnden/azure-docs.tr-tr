---
title: Azure PowerShell Betiği örnek - bir Azure sanal makinesini yedekleme | Microsoft Docs
description: Azure PowerShell Betiği örnek - bir Azure sanal makinesini yedekleme
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
tags: ''
ms.assetid: ''
ms.service: backup
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/07/2017
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: 4376add4a2e51806bd5db228ad2fe2afcf2e4f57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23842653"
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>PowerShell ile şifrelenmiş bir Azure sanal makinesini yedekleme

Bu komut dosyasını bir kurtarma Hizmetleri kasası şifrelenmiş bir Azure sanal makinesi için coğrafi olarak yedekli depolama (GRS) oluşturur. Varsayılan koruma ilke kasaya uygulanır. İlke günlük yedekleme için sanal makine oluşturur ve 30 gün boyunca her yedekleme korur. Komut ayrıca sanal makine için ilk kurtarma noktası tetikler ve bu kurtarma noktası 365 gün boyunca korur. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/backup/backup-encrypted-vm/backup-encrypted-vm.ps1 "Back up encrypted virtual machine")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası dağıtımı oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her öğe.


| Komut | Notlar | 
|---|---| 
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. | 
| [AzureRmRecoveryServicesVault yeni](/powershell/module/azurerm.recoveryservices/New-AzureRmRecoveryServicesVault) | Yedekleri depolamak için bir kurtarma Hizmetleri kasası oluşturur. | 
| [Set-AzureRmRecoveryServicesBackupProperties](/powershell/module/azurerm.recoveryservices/Set-AzureRmRecoveryServicesBackupProperties) | Ayarlar depolama özellikleri kurtarma Hizmetleri kasasına yedekleme. | 
| [AzureRmRecoveryServicesBackupProtectionPolicy yeni](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)| Kurtarma Hizmetleri kasasına zamanlama İlkesi'ni kullanarak koruma İlkesi ve bekletme ilkesi oluşturur. | 
| [Set AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Şifreleme anahtarları için hizmet asıl erişim vermek için bu anahtar kasası üzerinde izinlerini ayarlar. | 
| [Enable-AzureRmRecoveryServicesBackupProtection](/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection) | Belirtilen yedekleme koruma İlkesi'yle bir öğe için yedeklemeyi sağlar. | 
| [Set-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy)| Varolan yedekleme koruma ilkesini değiştirir. | 
| [Yedekleme AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem) | Yedekleme zamanlaması bağlanmayan korumalı bir Azure yedekleme öğesi için bir yedekleme işini başlatır. |
| [Bekleme AzureRmRecoveryServicesBackupJob](/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob) | Bir Azure yedekleme işini tamamlamak bekler. | 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır. | 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

