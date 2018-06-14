---
title: Azure CLI 2.0 kullanarak bir Linux VM Kopyala | Microsoft Docs
description: Azure CLI 2.0 ve yönetilen diskleri kullanarak, Azure Linux VM kopyasını oluşturmayı öğrenin.
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
ms.date: 09/25/2017
ms.author: cynthn
ms.openlocfilehash: 66f2789d717816f5be3fd8b298819825f8cd87f7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30905019"
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a>Azure CLI 2.0 ve yönetilen diskleri kullanarak bir Linux VM bir kopyasını oluşturun


Bu makalede Azure CLI 2.0 ve Azure Resource Manager dağıtım modeli kullanarak Linux çalıştıran, Azure sanal makine (VM) bir kopyasını oluşturulacağını gösterir. Bu adımları [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile de gerçekleştirebilirsiniz.

Ayrıca [karşıya yükleyin ve bir VHD'den bir VM oluşturma](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Önkoşullar


-   Yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2)

-   Oturum açmak için bir Azure hesabı [az oturum açma](/cli/azure/reference-index#az_login).

-   Kopya için kaynak olarak kullanılacak bir Azure VM vardır.

## <a name="step-1-stop-the-source-vm"></a>1. adım: kaynak VM'yi Durdur


Kaynak VM kullanarak ayırması [az vm serbest bırakma](/cli/azure/vm#az_vm_deallocate).
Aşağıdaki örnek adlı VM kaldırır **myVM** kaynak grubunda **myResourceGroup**:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

## <a name="step-2-copy-the-source-vm"></a>2. adım: kaynak VM'yi kopyalama


Bir VM kopyalamak için alttaki sanal sabit diskin bir kopyasını oluşturun. Bu işlem, yönetilen aynı yapılandırması ve ayarları VM kaynağı olarak içeren bir Disk olarak özel bir VHD oluşturur.

Azure Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../windows/managed-disks-overview.md). 

1.  Disk her VM ve kendi işletim sistemi adı listesi [az vm listesi](/cli/azure/vm#az_vm_list). Aşağıdaki örnek adlı kaynak grubundaki tüm sanal makineleri listeler **myResourceGroup**:
    
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

1.  Kopya oluşturarak yeni bir disk yönetilen diski kullanarak [az disketi](/cli/azure/disk#az_disk_create). Aşağıdaki örnek adlı bir disk oluşturur **myCopiedDisk** adlı Yönetilen diskten **myDisk**:

    ```azurecli
    az disk create --resource-group myResourceGroup \
         --name myCopiedDisk --source myDisk
    ``` 

1.  Artık kaynak grubunuzdaki yönetilen diskleri kullanarak doğrulayın [az disk listesi](/cli/azure/disk#az_disk_list). Aşağıdaki örnek adlı kaynak grubunda yönetilen diskleri listeler **myResourceGroup**:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```


## <a name="step-3-set-up-a-virtual-network"></a>3. adım: Sanal ağ ayarlama


Aşağıdaki isteğe bağlı adımlar yeni bir sanal ağ, alt ağ, genel IP adresi ve sanal ağ arabirim kartı (NIC) oluşturur.

Bir VM amacıyla ya da başka dağıtımlar sorun giderme için kopyalıyorsanız VM var olan bir sanal ağda kullanmak istemeyebilirsiniz.

Kopyalanan Vm'leriniz için bir sanal ağ altyapısı oluşturmak istiyorsanız, sonraki birkaç adımda izleyin. Bir sanal ağ oluşturmak istemiyorsanız, geçin [4. adım: bir VM oluşturma](#step-4-create-a-vm).

1.  Kullanarak sanal ağ oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create). Aşağıdaki örnek adlı bir sanal ağ oluşturur **myVnet** ve adlı bir alt ağ **mySubnet**:

    ```azurecli
    az network vnet create --resource-group myResourceGroup \
        --location eastus --name myVnet \
        --address-prefix 192.168.0.0/16 \
        --subnet-name mySubnet \
        --subnet-prefix 192.168.1.0/24
    ```

1.  Kullanarak bir genel IP oluşturun [az ağ genel IP oluşturun](/cli/azure/network/public-ip#az_network_public_ip_create). Aşağıdaki örnek adlı ortak IP oluşturur **myPublicIP** DNS adı ile **mypublicdns**. (DNS adı gerekir benzersiz olmalıdır, böylece benzersiz bir ad sağlayın.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup \
        --location eastus --name myPublicIP --dns-name mypublicdns \
        --allocation-method static --idle-timeout 4
    ```

1.  NIC kullanarak oluşturduğunuz [az ağ NIC oluşturma](/cli/azure/network/nic#az_network_nic_create).
    Aşağıdaki örnek, adlandırılmış bir NIC oluşturur **myNic** için bağlı **mySubnet** alt ağ:

    ```azurecli
    az network nic create --resource-group myResourceGroup \
        --location eastus --name myNic \
        --vnet-name myVnet --subnet mySubnet \
        --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a>4. adım: bir VM oluşturma

Kullanarak bir VM şimdi oluşturabilirsiniz [az vm oluşturma](/cli/azure/vm#az_vm_create).

İşletim sistemi diski olarak kullanmak için kopyalanan yönetilen diski belirtin (--attach-os-disk) aşağıdaki gibi:

```azurecli
az vm create --resource-group myResourceGroup \
    --name myCopiedVM --nics myNic \
    --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Sonraki adımlar

Yeni VM'nin yönetmek için Azure CLI kullanmayı öğrenmek için bkz: [Azure CLI komutları için Azure Resource Manager](../azure-cli-arm-commands.md).
