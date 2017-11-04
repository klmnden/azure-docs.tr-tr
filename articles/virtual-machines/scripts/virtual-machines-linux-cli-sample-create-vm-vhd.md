---
title: "Azure CLI örnek komut dosyası - VHD ile bir VM oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - bir sanal sabit disk kullanan bir VM oluşturma."
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
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 6234473d9f7f0eb18ea85e52273eb82a9ce04da5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Bir sanal sabit disk ile bir VM oluşturma

Bu örnek, bir VHD'yi kullanarak bir sanal makine oluşturur.
Bir kaynak grubu, bir depolama hesabı ve kapsayıcı oluşturur ve ardından VHD'yi kapsayıcıya karşıya yükleyerek bir VM oluşturur.
Yerini ssh ortak anahtar, ortak anahtar ile bu nedenle, VM erişebilirsiniz.

Önyüklenebilir bir VHD gerekir.
Https://azclisamples.blob.core.windows.net/vhds/sample.vhd kullandık VHD indirin veya kendi VHD kullanın. Komut dosyası arar `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı listesi](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_list) | Listeleri depolama hesapları |
| [az depolama hesabı onay-adı](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_check_name) | Depolama hesabı adı geçerli olduğunu ve önceden var olmayan denetler |
| [az depolama hesabı anahtarları listesi](https://docs.microsoft.com/cli/azure/storage/account/keys#az_storage_account_keys_list) | Depolama hesapları için anahtarları listeler |
| [az depolama blob var.](https://docs.microsoft.com/cli/azure/storage/blob#az_storage_blob_exists) | Blob mevcut olup olmadığını denetler |
| [az depolama kapsayıcısı oluşturma](https://docs.microsoft.com/cli/azure/storage/container#az_storage_container_create) | Bir depolama hesabında bir kapsayıcı oluşturur. |
| [az depolama blob karşıya yükleme](https://docs.microsoft.com/cli/azure/storage/blob#az_storage_blob_upload) | Bir blob kapsayıcısında VHD yükleyerek oluşturur. |
| [az vm listesi](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | İle kullanılan `--query` VM adı kullanımda olup olmadığını denetleyin. | 
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Sanal makineler oluşturur. |
| [az vm kümesi linux kullanıcıya erişim](https://docs.microsoft.com/cli/azure/vm/access#az_vm_access_set_linux_user) | VM geçerli kullanıcı erişimi vermek için SSH anahtarı sıfırlar. |
| [az vm-IP-adresleri listesi](https://docs.microsoft.com/cli/azure/vm#az_vm_list-ip-addresses) | Oluşturulan VM IP adresini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
