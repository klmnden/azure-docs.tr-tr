---
title: Azure CLI 1.0 ile bir kopyasını bir Linux VM oluşturma | Microsoft Docs
description: Resource Manager dağıtım modelinde Azure CLI 1.0 ile Azure Linux sanal makinenin bir kopyasını oluşturma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: bb350f8d14ad451ad3ff7cd617ca3f90967aaa4b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30906948"
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-the-azure-cli-10"></a>Azure CLI 1.0 ile Azure üzerinde çalışan bir Linux sanal makine bir kopyasını oluşturun
Bu makalede Resource Manager dağıtım modelini kullanarak Linux çalıştıran, Azure sanal makine (VM) bir kopyasını oluşturulacağını gösterir. Önce işletim sistemi ve veri diskleri üzerinde yeni bir kapsayıcı kopyalayın sonra ağ kaynakları ayarlamak ve yeni bir sanal makine oluşturun.

Ayrıca [karşıya yükleme ve özel disk görüntüsünden bir VM oluşturma](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- Azure CLI 1.0: Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız (bu makale)
- [Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="before-you-begin"></a>Başlamadan önce
Adımları başlamadan önce aşağıdaki önkoşulları karşıladığından emin olun:

* Sahip olduğunuz [Azure CLI](../../cli-install-nodejs.md) makinenizde yüklenip. 
* Ayrıca, mevcut bir Azure Linux VM'i hakkında bazı bilgiler gerekir:

| Kaynak VM bilgileri | Nereden edinilir |
| --- | --- |
| VM adı |`azure vm list` |
| Kaynak grubu adı |`azure vm list` |
| Konum |`azure vm list` |
| Depolama hesabı adı |`azure storage account list -g <resourceGroup>` |
| Kapsayıcı adı |`azure storage container list -a <sourcestorageaccountname>` |
| Kaynak VM VHD dosya adı |`azure storage blob list --container <containerName>` |

* Yeni VM hakkında bazı seçim yapmanız gerekir:    <br> -Kapsayıcı adı    <br> -VM adı    <br> -VM boyutu    <br> -vNet adı    <br> -Alt ağ adı    <br> -IP adı    <br> -NIC adı

## <a name="login-and-set-your-subscription"></a>Oturum açma ve aboneliğinizi ayarlayın
1. CLI oturum açın.

    ```azurecli
    azure login
    ```
2. Kaynak Yöneticisi modunda olduğundan emin olun.

    ```azurecli
    azure config mode arm
    ```
3. Doğru aboneliğin ayarlayın. 'Azure hesabı listesi' kullanabileceğiniz tüm aboneliklerinizi görmek için.

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-the-vm"></a>VM’yi durdurma
Durdurun ve kaynak VM serbest bırakma. 'Azure vm listesi' kullanabileceğiniz Grup adları tüm aboneliğinizi ve bunların kaynak VM'lerin bir listesini almak için.

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-the-vhd"></a>VHD'yi kopyalayın
Kullanarak hedef kaynak depolama biriminden VHD kopyalayabilirsiniz `azure storage blob copy start`. Bu örnekte, biz VHD aynı depolama hesabındaki ancak farklı bir kapsayıcı kopyalamak için adımıdır.

Aynı depolama hesabındaki başka bir kapsayıcıya VHD kopyalamak için şunu yazın:

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Yeni VM için sanal ağı ayarlama
Yeni VM için bir sanal ağ ve NIC ayarlayın. 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-the-new-vm"></a>Yeni VM oluşturma
Karşıya yüklenen sanal diskten bir VM artık oluşturabilirsiniz [resource manager şablonu kullanarak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) veya yazarak, kopyalanan diski URI'si belirterek CLI aracılığıyla:

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a>Sonraki adımlar
Yeni bir sanal makine yönetmek için Azure CLI kullanmayı öğrenmek için bkz: [Azure CLI komutları için Azure Resource Manager](../azure-cli-arm-commands.md).

