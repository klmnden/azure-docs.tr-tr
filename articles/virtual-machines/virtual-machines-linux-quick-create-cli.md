---
title: "Azure Hızlı Başlangıç - VM CLI oluşturma | Microsoft Belgeleri"
description: "Azure CLI ile sanal makine oluşturmayı hızlı bir şekilde öğrenin."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/10/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: c1d7cfe614ab4e677e7fff989e79eb09acb3feed
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a>Azure CLI ile Linux sanal makinesi oluşturma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuzda Ubuntu 16.04 LTS çalıştıran bir sanal makineyi Azure CLI kullanarak dağıtma işleminin ayrıntıları verilmektedir.

Başlamadan önce Azure CLI’nin yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure CLI yükleme kılavuzu](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm#create) komutuyla bir sanal makine oluşturun. 

Aşağıdaki örnekte `myVM` adlı bir VM oluşturulur ve varsayılan anahtar konumunda henüz yoksa SSH anahtarları oluşturulur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

VM oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir. Genel IP adresini not alın. Bu adres, VM’ye erişmek için kullanılır.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westeurope",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin genel IP adresi ile değiştirin.

```bash 
ssh <Public IP Address>
```

## <a name="delete-virtual-machine"></a>Sanal makineyi silme

Kaynak Grubu, VM ve tüm ilgili kaynaklar artık gerekli olmadığında aşağıdaki komut kullanılarak kaldırılabilir.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek oranda kullanılabilir sanal makine oluşturma öğreticisi](./virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[VM dağıtımı CLI örneklerini keşfedin](./virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

