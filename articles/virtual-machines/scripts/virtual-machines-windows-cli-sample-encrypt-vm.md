---
title: Azure CLI Betik Örneği - Windows VM Şifreleme | Microsoft Docs
description: Azure CLI Betik Örneği - Windows VM Şifreleme
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 900dd5e7e9372e36e0831b47e89db5e89b4816d5
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54849577"
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>Azure'da bir Windows sanal makinesi şifreleme

Bu betik güvenli bir Azure Key Vault, şifreleme anahtarları, Azure Active Directory hizmet sorumlusu ve bir Windows sanal makinesi (VM) oluşturur. VM daha sonra Key Vault şifreleme anahtarı ve hizmet sorumlusu kimlik bilgileri kullanılarak şifrelenir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, Azure Key Vault, hizmet sorumlusu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Şifreleme anahtarları gibi güvenli verileri depolamak için bir Azure Key Vault oluşturur. |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key#az_keyvault_key_create) | Key Vault içinde bir şifreleme anahtarı oluşturur. |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp#az_ad_sp_create_for_rbac) | Güvenli bir şekilde kimlik doğrulamak ve şifreleme anahtarlarına erişimi denetlemek üzere bir Azure Active Directory hizmet sorumlusu oluşturur. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Key Vault üzerinde hizmet sorumlusuna şifreleme anahtarları için erişim verecek izinleri ayarlar. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve NSG’ye bağlar. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir.  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption) | Hizmet sorumlusu kimlik bilgilerini ve şifreleme anahtarını kullanarak VM üzerinde şifrelemeyi etkinleştirir. |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_show) | VM şifreleme işleminin durumunu gösterir. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Windows VM belgeleri](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json) içinde bulunabilir.
