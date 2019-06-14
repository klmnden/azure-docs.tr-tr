---
title: Azure CLI kullanarak Linux VM kopyalama | Microsoft Docs
description: Azure CLI ve yönetilen diskleri kullanarak Azure Linux VM bir kopyasını oluşturmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/17/2018
ms.author: cynthn
ms.openlocfilehash: abc8c09a51104c81b827afb7055531df98691714
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60328757"
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-and-managed-disks"></a>Azure CLI ve yönetilen diskler kullanarak bir Linux VM bir kopyasını oluşturun

Bu makalede Azure CLI ve Azure Resource Manager dağıtım modeli kullanarak Linux çalıştıran, Azure sanal makine (VM) bir kopyasını oluşturma işlemini gösterir. 

Ayrıca [bir VHD'den VM oluşturma ve karşıya yükleme](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Önkoşullar

-   [Azure CLI](/cli/azure/install-az-cli2)’yi yükleyin.

-   İle bir Azure hesabında oturum [az login](/cli/azure/reference-index#az-login).

-   İçin bir kopyalama kaynağı olarak kullanmak için bir Azure VM var.

## <a name="stop-the-source-vm"></a>Kaynak VM durdurma

Kaynak VM kullanarak serbest [az vm deallocate](/cli/azure/vm#az-vm-deallocate).
Aşağıdaki örnekte adlı VM serbest bırakılır *myVM* kaynak grubundaki *myResourceGroup*:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

## <a name="copy-the-source-vm"></a>Kaynak VM kopyalama

VM kopyalama için temel alınan sanal sabit disk bir kopyasını oluşturun. Bu işlem, kaynak VM aynı yapılandırma ve ayarlar içeren yönetilen Disk olarak özel bir sanal sabit disk (VHD) oluşturur.

Azure Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../windows/managed-disks-overview.md). 

1.  Her VM ve adı, işletim sistemi diski ile liste [az vm listesini](/cli/azure/vm#az-vm-list). Aşağıdaki örnekte adlı kaynak grubundaki tüm Vm'leri listeler *myResourceGroup*:
    
    ```azurecli
    az vm list -g myResourceGroup \
         --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' \
         --output table
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Yeni bir yönetilen disk oluşturma ve kullanarak disk kopyalama [az disk oluşturma](/cli/azure/disk#az-disk-create). Aşağıdaki örnekte adlı bir disk oluşturur *myCopiedDisk* adlı bir yönetilen diskten *myDisk*:

    ```azurecli
    az disk create --resource-group myResourceGroup \
         --name myCopiedDisk --source myDisk
    ``` 

1.  Artık kaynak grubunuzda bir yönetilen diskleri kullanarak doğrulama [az disk listesi](/cli/azure/disk#az-disk-list). Aşağıdaki örnekte adlı kaynak grubunda yönetilen diskler listelenmiştir *myResourceGroup*:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```


## <a name="set-up-a-virtual-network"></a>Bir virtual Network ayarlama

Aşağıdaki isteğe bağlı adımlar, yeni sanal ağ, alt ağ, genel IP adresi ve sanal ağ arabirim kartı (NIC) oluşturur.

Sorun giderme, amacıyla veya başka dağıtımlar için bir VM kopyalıyorsanız, var olan bir sanal ağda VM kullanılacak istemeyebilirsiniz.

Kopyalanan sanal makineleriniz için bir sanal ağ altyapısı oluşturmak istiyorsanız, sonraki birkaç adımı izleyin. Sanal ağ oluşturmak istemiyorsanız, atlamak [VM oluşturma](#create-a-vm).

1.  Kullanarak sanal ağ oluşturma [az ağ sanal ağ oluşturma](/cli/azure/network/vnet#az-network-vnet-create). Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVnet* ve adlı bir alt ağ *mySubnet*:

    ```azurecli
    az network vnet create --resource-group myResourceGroup \
        --location eastus --name myVnet \
        --address-prefix 192.168.0.0/16 \
        --subnet-name mySubnet \
        --subnet-prefix 192.168.1.0/24
    ```

1.  Kullanarak bir genel IP oluşturma [az network public-IP oluşturma](/cli/azure/network/public-ip#az-network-public-ip-create). Aşağıdaki örnekte adlı bir genel IP oluşturur *Mypublicıp* DNS adıyla *mypublicdns*. (DNS adı benzersiz olması gerektiğinden, benzersiz bir ad belirtin.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup \
        --location eastus --name myPublicIP --dns-name mypublicdns \
        --allocation-method static --idle-timeout 4
    ```

1.  Kullanarak bir NIC oluşturup [az ağ NIC oluşturup](/cli/azure/network/nic#az-network-nic-create).
    Aşağıdaki örnekte adlı bir NIC oluşturur *Mynıc* için bağlı *mySubnet* alt ağı:

    ```azurecli
    az network nic create --resource-group myResourceGroup \
        --location eastus --name myNic \
        --vnet-name myVnet --subnet mySubnet \
        --public-ip-address myPublicIP
    ```

## <a name="create-a-vm"></a>VM oluşturma

Kullanarak bir VM oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create).

Kopyalanan yönetilen diski işletim sistemi diski olarak kullanma belirtin (`--attach-os-disk`) aşağıdaki gibi:

```azurecli
az vm create --resource-group myResourceGroup \
    --name myCopiedVM --nics myNic \
    --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Sonraki adımlar

Yeni sanal makinenize yönetmek için Azure CLI kullanma konusunda bilgi almak için bkz: [Azure CLI komutları için Azure Resource Manager](../azure-cli-arm-commands.md).
