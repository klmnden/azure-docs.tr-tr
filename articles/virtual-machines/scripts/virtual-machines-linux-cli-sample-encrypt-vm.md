---
title: "Azure CLI örnek komut dosyası - bir Linux VM şifrelemek | Microsoft Docs"
description: "Azure CLI örnek komut dosyası - bir Linux VM şifrele"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5f5c57a9c5a20e6e6a514b5b4c9d2e040d504983
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Azure'da bir Linux sanal makine şifrele

Bu komut dosyasını güvenli bir Azure anahtar kasası, şifreleme anahtarları, Azure Active Directory hizmet asıl ve Linux sanal makine (VM) oluşturur. VM şifreleme anahtarı anahtar kasası ve hizmet asıl kimlik bilgileri kullanılarak şifrelenir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

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

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
