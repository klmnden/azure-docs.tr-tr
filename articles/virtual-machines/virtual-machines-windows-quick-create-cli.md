---
title: "Azure Hızlı Başlangıç - Windows VM CLI oluşturma | Microsoft Belgeleri"
description: "Azure CLI ile Windows sanal makinesi oluşturmayı hızlı bir şekilde öğrenin."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/20/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 0f9ff487e289eadb857508134b7e08b00360fdd3
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-cli"></a>Azure CLI ile Windows sanal makinesi oluşturma

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuzda Windows Server 2016 çalıştıran bir sanal makineyi Azure CLI kullanarak dağıtma işleminin ayrıntıları verilmektedir.

Başlamadan önce Azure CLI’nin yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure CLI yükleme kılavuzu](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm#create) ile bir VM oluşturun. 

Aşağıdaki örnekte `myVM` adlı bir VM oluşturulur. Bu örnekte yönetici kullanıcı için `azureuser` kullanıcı adı ve ` myPassword12` parolası kullanılır. Bu değerleri ortamınız için uygun olan bir değerle güncelleştirin. Bu değerler, sanal makine ile bağlantı oluştururken gereklidir.

```azurecli
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
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

Sanal makine bir uzak masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin genel IP adresi ile değiştirin. İstendiğinde, sanal makine oluşturulurken kullanılan kimlik bilgilerini girin.

```bash 
mstsc /v:<Public IP Address>
```

## <a name="delete-virtual-machine"></a>Sanal makineyi silme

Kaynak Grubu, VM ve tüm ilgili kaynaklar artık gerekli olmadığında aşağıdaki komut kullanılarak kaldırılabilir.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Rol yükleme ve güvenlik duvarı yapılandırma öğreticisi](./virtual-machines-windows-hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[VM dağıtımı CLI örneklerini keşfedin](./virtual-machines-windows-cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
