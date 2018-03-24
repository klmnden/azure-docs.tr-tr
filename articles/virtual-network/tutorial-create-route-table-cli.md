---
title: Ağ trafiği - Azure CLI | Microsoft Docs
description: Azure CLI kullanarak bir yol tablosu ile ağ trafiğini yönlendirmek öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 871b562fa12b93d1b65e23ca58615d35ef6bb34b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Azure CLI kullanarak bir yol tablosu ile ağ trafiği yönlendirme

Azure otomatik olarak yollar varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında trafiği. Azure'nın geçersiz kılmak için kendi Rota oluşturabilmeniz için varsayılan yönlendirme. Örneğin, bir ağ sanal gereç (NVA) aracılığıyla alt ağlar arasında trafiği yönlendirmek istiyorsanız, özel yollar oluşturma olanağı yararlıdır. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Rota tablosu oluşturma
> * Bir yol oluşturma
> * Birden çok alt ağı ile bir sanal ağ oluşturma
> * Bir alt ağ için bir yol tablosu ilişkilendirme
> * Trafiğini yönlendiren bir NVA oluşturma
> * Sanal makineler (VM) farklı alt dağıtma
> * Bir NVA aracılığıyla başka bir yolu trafiğini bir alt ağdan

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI Sürüm 2.0.28 çalıştırıyorsanız bu hızlı başlangıç yükleyip CLI yerel olarak kullanmak seçerseniz, gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Rota tablosu oluşturma

Bir yol tablosu oluşturmadan önce bir kaynak grubuyla oluşturmanız [az grubu oluşturma](/cli/azure/group#az_group_create) bu makalede oluşturulan tüm kaynaklar için. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
``` 

Bir yol tablosu ile oluşturma [az ağ yol tablosu oluşturmanız](/cli/azure/network/route#az_network_route_table_create). Aşağıdaki örnek, adında bir yol tablosu oluşturur *myRouteTablePublic*. 

```azurecli-interactive 
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Bir yol oluşturma

Rota tablosunda bir yol oluşturma [az ağ yol tablosu yol oluşturma](/cli/azure/network/route-table/route#az_network_route_table_route_create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

## <a name="associate-a-route-table-to-a-subnet"></a>Bir alt ağ için bir yol tablosu ilişkilendirme

Bir alt ağ için bir yol tablosu ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmanız gerekir. Bir alt ağ ile birlikte bir sanal ağ oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

İki ek alt ağlar ile oluşturma [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create).

```azurecli-interactive
# Create a private subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24

# Create a DMZ subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name DMZ \
  --address-prefix 10.0.2.0/24
```

İlişkilendirme *myRouteTablePublic* yol tablosuna *ortak* alt ağ ile [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

## <a name="create-an-nva"></a>Bir NVA oluşturma

Bir NVA yönlendirme, saldırısından veya WAN iyileştirmesi gibi bir ağ işlevi gerçekleştiren bir VM'dir.

Bir NVA oluşturma *DMZ* alt ağ ile [az vm oluşturma](/cli/azure/vm#az_vm_create). Bir VM oluşturduğunuzda, Azure oluşturur ve varsayılan olarak VM, bir ortak IP adresi atar. `--public-ip-address ""` Parametresi oluşturun ve VM için Internet'ten bağlı olması gerektiğinden değil VM, bir ortak IP adresi atamak için Azure'a bildirir. SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse komutu bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azure-cli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmNva \
  --image UbuntuLTS \
  --public-ip-address "" \
  --subnet DMZ \
  --vnet-name myVirtualNetwork \
  --generate-ssh-keys
```

VM oluşturmak için birkaç dakika sürer. Azure VM oluşturduktan ve VM hakkında çıktıyı döndürür kadar sonraki adıma devam etmeyin. 

Kendi IP adresi için hedeflendiği değil, kendisine gönderilen ağ trafiği iletmek bir ağ arabirimi için IP iletme ağ arabirimi için etkinleştirilmesi gerekir. Ağ arabirimi için IP iletimini etkinleştirme [az ağ NIC güncelleştirmesi](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

VM dahilinde işletim sistemi ya da VM içinde çalışan bir uygulama aynı zamanda ağ trafiğini iletebilir olması gerekir. Sanal makinenin işletim sistemi ile içinde IP iletimini etkinleştirmeniz [az vm uzantısı kümesi](/cli/azure/vm/extension#az_vm_extension_set):

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
Komutu yürütmek için bir dakika sürebilir.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu trafiğinden doğrulamak için iki VM sanal ağ oluşturma *ortak* alt ağ için yönlendirilir *özel* sonraki adımda nva'nın alt ağa. 

Bir VM oluşturma *ortak* alt ağ ile [az vm oluşturma](/cli/azure/vm#az_vm_create). `--no-wait` Parametresi bir sonraki komuta devam edebilmek için arka planda komutu çalıştırmak Azure sağlar. Bu makalede kolaylaştırmak için bir parola kullanılır. Anahtarlar genellikle üretim dağıtımında kullanılır. Anahtarları kullanıyorsanız, SSH aracı iletmeyi yapılandırmanız gerekir. Daha fazla bilgi için SSH istemcisi belgelerine bakın. Değiştir `<replace-with-your-password>` seçtiğiniz bir parola ile aşağıdaki komutu.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Bir VM oluşturma *özel* alt ağ.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

VM oluşturmak için birkaç dakika sürer. VM oluşturulduktan sonra Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPrivate",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Not edin **Publicıpaddress**. Bu adres, sonraki adımda Internet'ten VM erişmek için kullanılır.

## <a name="route-traffic-through-an-nva"></a>Bir NVA arasında trafiği yönlendirme

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVmPrivate* VM. Değiştir *<publicIpAddress>* vm'nizin ortak IP adresine sahip. Yukarıdaki örnekte, IP adresidir *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

İçin bir parola istendiğinde, seçtiğiniz parolayı girin [sanal makineler oluşturma](#create-virtual-machines).

İzleme yolu yüklemek için aşağıdaki komutu kullanın *myVmPrivate* VM:

```bash 
sudo apt-get install traceroute
```

Ağ trafiği için yönlendirme test etmek için aşağıdaki komutu kullanın *myVmPublic* VM'den *myVmPrivate* VM.

```bash
traceroute myVmPublic
```

Yanıt aşağıdaki örneğe benzer:

```bash
traceroute to myVmPublic (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Trafik, doğrudan yönlendirilir görebilirsiniz *myVmPrivate* VM *myVmPublic* VM. Azure'nın varsayılan yollar, doğrudan alt ağlar arasında trafiği yönlendirme. 

SSH için aşağıdaki komutu kullanın *myVmPublic* VM'den *myVmPrivate* VM:

```bash 
ssh azureuser@myVmPublic
```

İzleme yolu yüklemek için aşağıdaki komutu kullanın *myVmPublic* VM:

```bash 
sudo apt-get install traceroute
```

Ağ trafiği için yönlendirme test etmek için aşağıdaki komutu kullanın *myVmPrivate* VM'den *myVmPublic* VM.

```bash
traceroute myVmPrivate
```

Yanıt aşağıdaki örneğe benzer:

```bash
traceroute to myVmPrivate (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
İlk atlama NVA'ın özel IP adresi 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama 10.0.1.4, özel IP adresi olan *myVmPrivate* VM. Eklenen rota *myRouteTablePublic* rota tablosu ve ilişkili *ortak* alt neden NVA aracılığıyla yerine doğrudan trafiği yönlendirmek Azure *özel* alt ağ.

Her ikisi için SSH oturumları kapatmak *myVmPublic* ve *myVmPrivate* VM'ler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yol tablosu oluşturulur ve bir alt ağa ilişkilendirilmiş. Ortak bir alt ağ trafiği için özel bir alt ağa yönlendirilmiş basit bir NVA oluşturuldu. Güvenlik Duvarı ve WAN iyileştirme dışında gibi ağ işlevleri gerçekleştirmek önceden yapılandırılmış NVAs çeşitli dağıtmak [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Yönlendirme tabloları üretim kullanımı için dağıtmadan önce baştan sona ile öğrenmeniz olduğunu önerilir [Azure'da yönlendirme](virtual-networks-udr-overview.md), [Yönet yol tablolarını](manage-route-table.md), ve [Azuresınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Bir sanal ağ içinde birçok Azure kaynakları dağıtabilirsiniz, ancak bazı Azure PaaS hizmetler için kaynaklar sanal bir ağa dağıtılamıyor. Hala erişimi bazı Azure PaaS Hizmetleri'nden trafik için yalnızca bir sanal ağ alt kaynaklara yine de kısıtlayabilirsiniz. Ağ erişimi Azure PaaS kaynaklarına erişimi kısıtlayabilir öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Ağ erişimi PaaS kaynaklarına erişimi kısıtlayabilir](tutorial-restrict-network-access-to-resources-cli.md)
