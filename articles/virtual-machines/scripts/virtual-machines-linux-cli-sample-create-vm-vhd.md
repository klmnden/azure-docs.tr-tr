---
title: Azure CLI Betik Örneği - VHD ile VM Oluşturma | Microsoft Docs
description: Azure CLI Betik Örneği - Sanal sabit disk kullanarak VM oluşturma.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 414ef43063cc48b7b9ae7be5fbccbb7906ae8c03
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Bir sanal sabit disk ile VM oluşturma

Bu örnek, VHD kullanarak bir sanal makine oluşturur.
Bir kaynak grubu, depolama hesabı ve kapsayıcı oluşturur, ardından VHD'yi kapsayıcıya yükleyerek bir VM oluşturur.
VM’ye erişebilmeniz için ssh ortak anahtarını sizin ortak anahtarınızla değiştirir.

Önyüklenebilir bir VHD gerekir.
https://azclisamples.blob.core.windows.net/vhds/sample.vhd sayfasında kullandığımız VHD’yi indirebilir veya kendi VHD’nizi kullanabilirsiniz. Betik `~/sample.vhd` öğesini arar.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_list) | Depolama hesaplarını listeler |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_check_name) | Depolama hesabı adının geçerli olduğunu ve önceden var olmadığını doğrular |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys#az_storage_account_keys_list) | Depolama hesaplarının anahtarlarını listeler |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob#az_storage_blob_exists) | Blobun mevcut olup olmadığını denetler |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container#az_storage_container_create) | Depolama hesabında bir kapsayıcı oluşturur. |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob#az_storage_blob_upload) | VHD’yi karşıya yükleyerek kapsayıcıda bir blob oluşturur. |
| [az vm list](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | VM adının kullanımda olup olmadığını denetlemek için `--query` ile birlikte kullanılır. | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Sanal makineleri oluşturur. |
| [az vm access set-linux-user](https://docs.microsoft.com/cli/azure/vm/access#az_vm_access_set_linux_user) | Geçerli kullanıcıya VM erişimi vermek için SSH anahtarını sıfırlar. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az_vm_list-ip-addresses) | Oluşturulan VM’nin IP adresini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
