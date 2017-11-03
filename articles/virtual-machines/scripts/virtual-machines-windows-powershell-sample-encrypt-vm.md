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
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 9279fea482fcd8716bcd996985e10f500a4775ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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
| [AzureRmVirtualNetworkSubnetConfig yeni](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma sanal ağ oluşturma işlemine kullanılır. |
| [Yeni-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Sanal ağ oluşturur. |
| [AzureRmPublicIpAddress yeni](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Bir ortak IP adresi oluşturur. |
| [AzureRmNetworkSecurityRuleConfig yeni](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Bir ağ güvenlik grubu kural yapılandırması oluşturur. Bu yapılandırma, NSG oluşturulduğunda bir NSG kuralı oluşturmak için kullanılır. |
| [AzureRmNetworkSecurityGroup yeni](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Bir ağ güvenlik grubu oluşturur. |
| [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | Alt ağ bilgilerini alır. Bu bilgiler, bir ağ arabirimi oluşturulurken kullanılır. |
| [AzureRmNetworkInterface yeni](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Bir ağ arabirimi oluşturur. |
| [AzureRmVMConfig yeni](/powershell/module/azurerm.compute/new-azurermvmconfig) | Bir VM yapılandırması oluşturur. Bu yapılandırma VM adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma VM oluşturma sırasında kullanılır. |
| [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Bir sanal makine oluşturun. |
| [Get-AzureRmKeyVault](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | Anahtar kasası hakkında bilgi alır gereken |
| [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | Hizmet asıl kimlik bilgilerini ve şifreleme anahtarı kullanarak bir VM üzerinde şifrelemeyi etkinleştirir. |
| [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | VM şifreleme işleminin durumunu gösterir. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
