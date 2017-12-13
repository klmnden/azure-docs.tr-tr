---
title: "Azure PowerShell Betiği örnek - Windows VM şifrelemek | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - Windows VM şifrele"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: iainfou
ms.openlocfilehash: f405cdaf61d6aaafa8568a9d7f21614071285c17
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a>Azure PowerShell ile Windows sanal makine şifrele

Bu komut dosyasını güvenli bir Azure anahtar kasası, şifreleme anahtarları, Azure Active Directory Hizmet sorumlusu ve bir Windows sanal makine (VM) oluşturur. VM şifreleme anahtarı anahtar kasası ve hizmet asıl kimlik bilgileri kullanılarak şifrelenir.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

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
| [Yeni-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) | Şifreleme anahtarları gibi güvenli veri depolamak için bir Azure anahtar kasası oluşturur. |
| [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | Bir şifreleme anahtarı anahtar kasası oluşturur. |
| [AzureRmADServicePrincipal yeni](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Bir Azure Active Directory güvenli bir şekilde kimlik doğrulaması ve şifreleme anahtarlarının erişimi denetlemek için hizmet sorumlusu oluşturur. |
| [Set AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | Şifreleme anahtarları için hizmet asıl erişim vermek için bu anahtar kasası üzerinde izinlerini ayarlar. |
| [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve ağ güvenlik grubu bağlanır. Bu komut ayrıca 80 numaralı bağlantı noktasını açar ve yönetici kimlik bilgilerini ayarlar. |
| [Get-AzureRmKeyVault](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | Anahtar kasası hakkında bilgi alır gereken |
| [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | Hizmet asıl kimlik bilgilerini ve şifreleme anahtarı kullanarak bir VM üzerinde şifrelemeyi etkinleştirir. |
| [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | VM şifreleme işleminin durumunu gösterir. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
