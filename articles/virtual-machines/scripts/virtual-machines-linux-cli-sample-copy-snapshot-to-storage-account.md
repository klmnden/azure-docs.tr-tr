---
title: "Azure CLI komut dosyası örneği - farklı bir bölgeye depolama hesabında VHD olarak verme/kopyalama anlık görüntü | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - bir depolama hesabı aynı veya farklı Abonelikteki VHD olarak verme/kopyalama anlık görüntü"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: dfb78106bc72aacee85f8412032165fdfcfc1ab3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-cli"></a>Dışa aktarma/yönetilen anlık görüntüler VHD CLI ile farklı bir bölgeye depolama hesabında kopyalama

Bu komut dosyasını farklı bir bölgeye depolama hesabında yönetilen bir anlık görüntü verir. İlk anlık görüntü SAS URI'sini oluşturur ve farklı bölgede bulunan bir depolama hesabına kopyalamak için kullanır. Olağanüstü durum kurtarma için farklı bölgede yönetilen disklerinizi yedekleme korumak için bu komut dosyasını kullanın. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası SAS URI'si için yönetilen bir anlık görüntü oluşturmak için komutları kullanır ve anlık görüntü SAS URI'sini kullanarak bir depolama hesabı kopyalar. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [erişim izni ver az anlık görüntüsünü alın](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_grant_access) | Bir depolama hesabına temel VHD dosyasını kopyalayın veya şirket içi indirmek için kullanılan salt okunur SAS oluşturur  |
| [az depolama blob kopyalama Başlat](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#az_storage_blob_copy_start) | Bir blob zaman uyumsuz olarak bir depolama hesabından başka bir konuma kopyalar |

## <a name="next-steps"></a>Sonraki adımlar

[Bir VHD'den yönetilen bir disk oluştur](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Yönetilen bir diskten bir sanal makine oluşturun](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek bir sanal makine ve yönetilen diskleri CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
