---
title: Azure CLI ile zoned bir Linux VM oluşturma | Microsoft Docs
description: Azure CLI ile bir kullanılabilirlik bölgesindeki bir Linux VM oluşturma
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
ms.date: 03/27/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: cfc3fbf5432108222ee7941d92d78e49d3eaed78
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Azure CLI ile bir kullanılabilirlik bölgesindeki bir Linux sanal makine oluşturun

Bir Azure kullanılabilirlik bölgesinde bir Linux VM oluşturmak için Azure CLI kullanarak aracılığıyla bu makalede adımlar. [Kullanılabilirlik alanı](../../availability-zones/az-overview.md), bir Azure bölgesinde fiziksel olarak ayrılmış bir alandır. Uygulamalarınızı beklenmeyen hatalardan veya tüm veri merkezinin kaybedilmesinden korumak için kullanılabilirlik alanlarından yararlanın.

Bir kullanılabilirlik bölgeyi kullanmak için sanal makinenizde oluşturmanız bir [Azure bölgesi desteklenen](../../availability-zones/az-overview.md#regions-that-support-availability-zones).

En son yüklediğinizden emin olun [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı için oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).


## <a name="check-vm-sku-availability"></a>VM SKU kullanılabilirliğini denetleme
VM boyutları veya SKU'ların kullanılabilirliği, bölge ve alanlara göre farklılık gösterebilir. Kullanılabilirlik Alanları kullanımını planlamanıza yardımcı olmak üzere, kullanılabilir VM SKU'larını Azure bölgesine ve alana göre listeleyebilirsiniz. Bu özellik, uygun bir VM boyutu seçmenizi ve alanlar arasında istenen dayanıklılığı elde etmenizi sağlar. Farklı VM türleri ve boyutları hakkında daha fazla bilgi için bkz. [VM Boyutlarına genel bakış](sizes.md).

Kullanılabilir VM SKU'ları ile görüntüleyebilirsiniz [az vm listesi-SKU'ları](/cli/azure/vm#az_vm_list_skus) komutu. Aşağıdaki örnekte kullanılabilir VM SKU'ları *eastus2* bölgesinde içinde listelenmiştir:

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

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun.  

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makineden önce bir kaynak grubu oluşturulmalıdır. Bu örnekte, bir kaynak grubu adında *myResourceGroupVM* oluşturulan *eastus2* bölge. Doğu ABD 2 kullanılabilirlik bölgeyi destekleyen Azure bölgeleri biridir.

```azurecli 
az group create --name myResourceGroupVM --location eastus2
```

Kaynak grubu oluştururken veya değiştirirken Bu makale görülebilir bir VM belirtilir.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm#az_vm_create) komutuyla bir sanal makine oluşturun. 

Bir sanal makine oluştururken, işletim sistemi görüntüsü, disk boyutlandırma ve yönetici kimlik bilgileri gibi çeşitli seçenekler bulunur. Bu örnekte, Ubuntu Server çalıştıran *myVM* adlı bir sanal makine oluşturulmuştur. VM kullanılabilirlik bölgede oluşturulan *1*. Varsayılan olarak, VM oluşturulan *Standard_DS1_v2* boyutu.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys --zone 1
```

VM’nin oluşturulması birkaç dakika sürebilir. VM oluşturulduktan sonra, Azure CLI VM hakkında bilgi çıkışı sağlar. Not edin `zones` VM çalıştığı kullanılabilirlik bölge belirten değer. 

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

VM bir kullanılabilirlik bölgesinde dağıtıldığında, yönetilen bir disk VM için aynı kullanılabilirlik bölgede oluşturulur. Varsayılan olarak, bir ortak IP adresi da bu bölgede oluşturulur. Aşağıdaki örnekler, bu kaynakları hakkında bilgi alın.

VM'ın yönetilen disk kullanılabilirlik bölgesinde olduğunu doğrulamak için kullanılan [az vm Göster](/cli/azure/vm#az_vm_show) disk kimliği döndürülecek komutu. Bu örnekte, disk kimliği, bir sonraki adımda kullanılan bir değişkende depolanır. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
Şimdi, yönetilen disk hakkında bilgi alabilirsiniz:

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

Kullanım [az vm listesi-ip-addresses](/cli/azure/vm#az_vm_list_ip_addresses) ortak IP adresi kaynağında adını döndürmek için komut *myVM*. Bu örnekte, bir sonraki adımda kullanılan bir değişken adı depolanır.

```azurecli
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

Şimdi, IP adresi hakkında bilgi alabilirsiniz:

```azurecli
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

Çıkış IP adresi VM ile aynı kullanılabilirlik bölgedeki olduğunu gösterir:

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

Bu makalede, bir kullanılabilirlik bölgesinde bir VM oluşturma öğrendiniz. Azure VM’leri için [bölgeler ve kullanılabilirlik](regions-and-availability.md) hakkında daha fazla bilgi edinin.




