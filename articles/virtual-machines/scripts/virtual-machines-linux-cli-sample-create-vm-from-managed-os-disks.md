---
title: "Azure CLI betik örnek - yönetilen bir disk işletim sistemi diski olarak ekleyerek bir VM oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - yönetilen bir disk işletim sistemi diski olarak ekleyerek bir VM oluşturma"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 2141ea4fd25dfc69ada02c54c4f6b6b717b8e7db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a>Mevcut yönetilen işletim sistemi diski ile CLI kullanarak bir sanal makine oluşturun

Bu komut dosyasını varolan yönetilen disk işletim sistemi diski olarak ekleyerek bir sanal makine oluşturur. Senaryolar önceki içinde bu komut dosyasını kullanın:
* Yönetilen bir diskten farklı Abonelikteki kopyalanan var olan bir diskten yönetilen işletim sistemi bir VM oluşturma
* Özel bir VHD dosyasından oluşturulmuş var olan bir diskten yönetilen bir VM oluşturma 
* Bir anlık görüntüden oluşturulmuş var olan bir diskten yönetilen işletim sistemi bir VM oluşturma 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, yönetilen disk özelliklerini alma, yönetilen bir diske yeni bir VM'e ve bir VM oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her öğe.

| Komut | Notlar |
|---|---|
| [az disk Göster](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Disk adı ve kaynak grubu adı kullanarak yönetilen disk özelliklerini alır. ID özelliği, yönetilen bir diske yeni bir VM'e için kullanılır |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Yönetilen bir işletim sistemi diski kullanarak VM oluşturur |
## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
