---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 64290aad2d9f98006a715b480be8cb96965abbaf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188379"
---
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm) komutuyla bir sanal makine oluşturun. 

Aşağıdaki örnekte *myVM* adlı bir VM oluşturulur ve varsayılan anahtar konumunda henüz yoksa SSH anahtarları oluşturulur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Komut ayrıca *azureuser* yönetici kullanıcı adını belirler. Bu adı daha sonra VM'ye bağlanmak için kullanacaksınız. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir. `publicIpAddress` değerini not edin. Sonraki adımlarda bu adres, VM’ye erişmek için kullanılır.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```



## <a name="open-port-80-for-web-traffic"></a>Web trafiği için 80 numaralı bağlantı noktasını açın 

Varsayılan olarak, Azure’a dağıtılmış Linux VM'lerde yalnızca SSH bağlantılarına izin verilir. Bu VM bir web sunucusu olacağı için, İnternet’ten 80 numaralı bağlantı noktasını açmanız gerekir. İstediğiniz bağlantı noktasını açmak için [az vm open-port](/cli/azure/vm) komutunu kullanın.  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a>VM’ye SSH uygulama


VM’nizin genel IP adresini bilmiyorsanız, [az network public-ip list](/cli/azure/network/public-ip) komutunu çalıştırın. Bu IP adresine sonraki adımlarda ihtiyacınız olacak.


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. Sanal makinenizin doğru genel IP adresi ile değiştirdiğinizden emin olun. Bu örnekte IP adresi *40.68.254.142*’dir. *azureuser*, VM'yi oluşturduğunuzda belirlenen yönetici kullanıcı adıdır.

```bash
ssh azureuser@40.68.254.142
```

