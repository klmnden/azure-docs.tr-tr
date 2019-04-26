---
title: Azure CLI ile ayrılmış bir Linux VM oluşturma | Microsoft Docs
description: Azure CLI ile bir kullanılabilirlik alanında Linux VM oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: cdd9910bfef96f56cfa8c8e81363ff9bdb40f444
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60328740"
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Azure CLI ile bir kullanılabilirlik alanında Linux sanal makinesi oluşturma

Azure kullanılabilirlik alanında Linux VM oluşturmak için Azure CLI aracılığıyla bu makalede adımları. [Kullanılabilirlik alanı](../../availability-zones/az-overview.md), bir Azure bölgesinde fiziksel olarak ayrılmış bir alandır. Uygulamalarınızı beklenmeyen hatalardan veya tüm veri merkezinin kaybedilmesinden korumak için kullanılabilirlik alanlarından yararlanın.

Kullanılabilirlik alanı kullanmak için, [desteklenen bir Azure bölgesinde](../../availability-zones/az-overview.md#services-support-by-region) sanal makinenizi oluşturun.

En son yüklediğinizden emin olun [Azure CLI](/cli/azure/install-az-cli2) ile bir Azure hesabında oturum açtığınızdan [az login](/cli/azure/reference-index).


## <a name="check-vm-sku-availability"></a>VM SKU kullanılabilirliğini denetleme
VM boyutları veya SKU'ların kullanılabilirliği, bölge ve alanlara göre farklılık gösterebilir. Kullanılabilirlik Alanları kullanımını planlamanıza yardımcı olmak üzere, kullanılabilir VM SKU'larını Azure bölgesine ve alana göre listeleyebilirsiniz. Bu özellik, uygun bir VM boyutu seçmenizi ve alanlar arasında istenen dayanıklılığı elde etmenizi sağlar. Farklı VM türleri ve boyutları hakkında daha fazla bilgi için bkz. [VM Boyutlarına genel bakış](sizes.md).

Kullanılabilir VM SKU'larına görüntüleyebileceğiniz [az vm listesi-skus](/cli/azure/vm) komutu. Aşağıdaki örnekte kullanılabilir VM SKU'ları *eastus2* bölgesinde içinde listelenmiştir:

```azurecli
az vm list-skus --location eastus2 --output table
```

Çıktı, her VM boyutunun kullanılabilir olduğu Kullanılabilir Alanlarını gösteren aşağıdaki sıkıştırılmış örneğe benzer:

```azurecli
ResourceType      Locations  Name               [...]    Tier       Size     Zones
----------------  ---------  -----------------           ---------  -------  -------
virtualMachines   eastus2    Standard_DS1_v2             Standard   DS1_v2   1,2,3
virtualMachines   eastus2    Standard_DS2_v2             Standard   DS2_v2   1,2,3
[...]
virtualMachines   eastus2    Standard_F1s                Standard   F1s      1,2,3
virtualMachines   eastus2    Standard_F2s                Standard   F2s      1,2,3
[...]
virtualMachines   eastus2    Standard_D2s_v3             Standard   D2_v3    1,2,3
virtualMachines   eastus2    Standard_D4s_v3             Standard   D4_v3    1,2,3
[...]
virtualMachines   eastus2    Standard_E2_v3              Standard   E2_v3    1,2,3
virtualMachines   eastus2    Standard_E4_v3              Standard   E4_v3    1,2,3
```


## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun.  

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makineden önce bir kaynak grubu oluşturulmalıdır. Bu örnekte, adlı bir kaynak grubu *Eastus* oluşturulur *eastus2* bölge. Doğu ABD 2, kullanılabilirlik alanlarını destekleyen Azure bölgelerinden biridir.

```azurecli 
az group create --name myResourceGroupVM --location eastus2
```

Kaynak grubu oluştururken veya değiştirirken Bu makale boyunca görülebilir bir VM belirtilir.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm) komutuyla bir sanal makine oluşturun. 

Bir sanal makine oluştururken, işletim sistemi görüntüsü, disk boyutlandırma ve yönetici kimlik bilgileri gibi çeşitli seçenekler bulunur. Bu örnekte, Ubuntu Server çalıştıran *myVM* adlı bir sanal makine oluşturulmuştur. Sanal makine kullanılabilirlik alanında oluşturulur *1*. Varsayılan olarak, VM oluşturulduktan *Standard_DS1_v2* boyutu.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --location eastus2 --image UbuntuLTS --generate-ssh-keys --zone 1
```

VM’nin oluşturulması birkaç dakika sürebilir. VM oluşturulduktan sonra, Azure CLI VM hakkında bilgi çıkışı sağlar. Not `zones` VM çalışıyor kullanılabilirlik alanı belirten değer. 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus2",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM",
  "zones": "1"
}
```

## <a name="confirm-zone-for-managed-disk-and-ip-address"></a>Yönetilen disk ve IP adresi için bölge onaylayın

VM için yönetilen disk, sanal Makineyi bir kullanılabilirlik alanında dağıtıldığında, aynı kullanılabilirlik alanında oluşturulur. Varsayılan olarak, bir genel IP adresi de bu bölgede oluşturulur. Aşağıdaki örnekler, bu kaynaklar hakkında bilgi alın.

Yönetilen disk sanal makinenin kullanılabilirlik alanında olduğunu doğrulamak için [az vm show](/cli/azure/vm) disk kimliğini döndürmek için komutu Bu örnekte disk kimliği sonraki adımlardan birinde kullanılan bir değişkende depolanır. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
Artık yönetilen disk hakkında bilgi edinebilirsiniz:

```azurecli-interactive
az disk show --resource-group myResourceGroupVM --name $osdiskname
```

Çıktı, yönetilen diskin VM ile aynı kullanılabilirlik alanında olduğunu gösterir:

```azurecli
{
  "creationData": {
    "createOption": "FromImage",
    "imageReference": {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westeurope/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/latest",
      "lun": null
    },
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/disks/osdisk_761c570dab",
  "location": "eastus2",
  "managedBy": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "name": "myVM_osdisk_761c570dab",
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupVM",
  "sku": {
    "name": "Premium_LRS",
    "tier": "Premium"
  },
  "tags": {},
  "timeCreated": "2018-03-05T22:16:06.892752+00:00",
  "type": "Microsoft.Compute/disks",
  "zones": [
    "1"
  ]
}
```

Kullanım [az vm-IP-adreslerini](/cli/azure/vm) genel IP adresi kaynağı adını döndürmek için komutu *myVM*. Bu örnekte, bir sonraki adımda kullanılan bir değişken adı depolanır.

```azurecli
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

Artık IP adresi hakkında bilgi edinebilirsiniz:

```azurecli
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

Çıktı, IP adresi VM ile aynı kullanılabilirlik alanında olduğunu gösterir:

```azurecli
{
  "dnsSettings": null,
  "etag": "W/\"b7ad25eb-3191-4c8f-9cec-c5e4a3a37d35\"",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "52.174.34.95",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "myResourceGroupVM",
    "subnet": null
  },
  "location": "eastUS2",
  "name": "myVMPublicIP",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "myResourceGroupVM",
  "resourceGuid": "8c70a073-09be-4504-0000-000000000000",
  "tags": {},
  "type": "Microsoft.Network/publicIPAddresses",
  "zones": [
    "1"
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kullanılabilirlik alanında nasıl sanal makine oluşturulacağını öğrendiniz. Azure VM’leri için [bölgeler ve kullanılabilirlik](regions-and-availability.md) hakkında daha fazla bilgi edinin.




