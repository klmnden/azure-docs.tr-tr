---
title: "Azure CLI örnek komut dosyası - VM Windows şifrelemek | Microsoft Docs"
description: "Azure CLI örnek komut dosyası - VM Windows şifrele"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 7e1e9a69d226b03e6722c2d19b7a091cfe4e71c7
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>Azure'da Windows sanal makine şifrele

Bu komut dosyasını güvenli bir Azure anahtar kasası, şifreleme anahtarları, Azure Active Directory Hizmet sorumlusu ve bir Windows sanal makine (VM) oluşturur. VM şifreleme anahtarı anahtar kasası ve hizmet asıl kimlik bilgileri kullanılarak şifrelenir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, Azure anahtar kasası, hizmet sorumlusu, sanal makine ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az keyvault oluşturma](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Şifreleme anahtarları gibi güvenli veri depolamak için bir Azure anahtar kasası oluşturur. |
| [az keyvault anahtarı oluşturma](https://docs.microsoft.com/cli/azure/keyvault/key#az_keyvault_key_create) | Bir şifreleme anahtarı anahtar kasası oluşturur. |
| [az ad sp oluşturma-için-rbac](https://docs.microsoft.com/cli/azure/ad/sp#az_ad_sp_create_for_rbac) | Bir Azure Active Directory güvenli bir şekilde kimlik doğrulaması ve şifreleme anahtarlarının erişimi denetlemek için hizmet sorumlusu oluşturur. |
| [az keyvault-ilkesini ayarlama](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Şifreleme anahtarları için hizmet asıl erişim vermek için bu anahtar kasası üzerinde izinlerini ayarlar. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve NSG bağlanır. Bu komut ayrıca kullanılacak sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir.  |
| [az vm şifrelemeyi etkinleştir](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_enable) | Hizmet asıl kimlik bilgilerini ve şifreleme anahtarı kullanarak bir VM üzerinde şifrelemeyi etkinleştirir. |
| [az vm şifreleme Göster](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_show) | VM şifreleme işleminin durumunu gösterir. |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).
