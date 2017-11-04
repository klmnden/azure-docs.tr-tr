---
title: "Azure CLI komut dosyası örneği - yeniden başlatma VM'ler | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - yeniden başlatma VM'ler etiketi ve kimliği"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: ea114f484c774573b7d219cff9102a7308af356e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="restart-vms"></a>Sanal makineleri yeniden başlatın

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Bu örnek, çeşitli şekillerde bazı sanal makineleri almak ve bunları yeniden başlatmak için göstermektedir.

İlk kaynak grubunda yer alan tüm sanal makineleri yeniden başlatır.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

İkinci kullanarak etiketli VM'ler alır `az resouce list` filtreler VM'ler kaynakları ve bu sanal makineleri yeniden başlatır.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Bu örnek, bir Bash kabuğunda çalışır. Windows istemcisi üzerinde Azure CLI betikleri çalıştırma seçenekleri için bkz [Windows Azure CLI çalıştıran](../windows/cli-options.md).


## <a name="sample-script"></a>Örnek komut dosyası

Örnek, üç komut dosyasına sahiptir.
İlk sanal makineleri sağlar.
Her VM sağlanacak beklemeden komutu döndürecek şekilde Hayır bekleme seçeneği kullanır.
İkinci tamamen sağlanması VM'ler için bekler.
Üçüncü komut sağlanan ve ardından hemen tüm sanal makineleri yeniden etiketli VM'ler.

### <a name="provision-the-vms"></a>Sanal makineleri sağlama

Bu komut dosyasını bir kaynak grubu ve yeniden başlatmak için üç VM'ler oluşturur.
İki tanesi etiketlenir.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]

### <a name="wait"></a>Wait

Bu komut dosyası, tüm üç VM'ler sağlanır veya bunlardan birini sağlamak başarısız olana kadar 20 dakikada sağlama durumunu denetler.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]

### <a name="restart-the-vms"></a>Sanal makineleri yeniden başlatın

Bu komut dosyası kaynak grubundaki tüm sanal makineleri yeniden başlatır ve yalnızca etiketli VM'ler yeniden başlatır.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Komut dosyası örneği çalıştırdıktan sonra kaynak grupları, sanal makineleri ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu kullanılabilir.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Sanal makineler oluşturur.  |
| [az vm listesi](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | İle kullanılan `--query` VM'ler sağlanan bunları yeniden başlatmadan önce emin olmak için ve ardından sanal makineleri yeniden başlatmayı kimliklerini almak için. |
| [az kaynak listesi](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | İle kullanılan `--query` etiketini kullanarak sanal makineleri kimliklerini almak için. |
| [az vm yeniden başlatma](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Sanal makineleri yeniden başlatır. |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
