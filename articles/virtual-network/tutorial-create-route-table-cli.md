---
title: Ağ trafiği - Azure CLI | Microsoft Docs
description: Bu makalede, Azure CLI kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme hakkında bilgi edinin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: ff5897766bb56b76a34940ecd786773fd844a336
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683110"
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Azure CLI kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirme

Azure varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında gerçekleşen trafiği otomatik olarak yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Örneğin, bir ağ sanal gereci üzerinden alt ağlar arasındaki trafiği yönlendirmek isteyebilirsiniz. Bu makalede şunları öğreneceksiniz:

* Yönlendirme tablosu oluşturma
* Yönlendirme oluşturma
* Birden fazla alt ağa sahip bir sanal ağ oluşturma
* Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
* Trafiği yönlendiren bir NVA oluşturma
* Sanal makineleri (VM) farklı alt ağlara dağıtma
* NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Bir yol tablosu oluşturabilmeniz için önce bir kaynak grubu oluşturun [az grubu oluşturma](/cli/azure/group) bu makalede oluşturulan tüm kaynaklar için. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
``` 

Yönlendirme tablosu ile oluşturma [az ağ yönlendirme tablosu oluşturma](/cli/azure/network/route-table#az-network-route-table-create). Aşağıdaki örnekte adlı bir rota tablosu oluşturur *myRouteTablePublic*. 

```azurecli-interactive 
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Yönlendirme oluşturma

Bir rota ile yol tablosundaki oluşturma [az ağ route-table route oluşturma](/cli/azure/network/route-table/route#az-network-route-table-route-create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir yönlendirme tablosunu bir alt ağ ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmanız gerekir. Bir alt ağ ile sanal ağ oluşturma [az ağ sanal ağ oluşturma](/cli/azure/network/vnet).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

İki ek alt ağlar ile oluşturmak [az ağ sanal ağ alt ağı oluşturma](/cli/azure/network/vnet/subnet).

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

İlişkilendirme *myRouteTablePublic* yol tablosuna *genel* alt ağ ile [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

## <a name="create-an-nva"></a>NVA oluşturma

NVA; yönlendirme, güvenlik duvarı oluşturma veya WAN iyileştirmesi gibi ağ işlevlerini gerçekleştiren bir VM'dir.

Bir NVA oluşturma *DMZ* alt ağ ile [az vm oluşturma](/cli/azure/vm). Bir VM oluşturduğunuzda, Azure oluşturur ve varsayılan olarak VM, genel bir IP adresi atar. `--public-ip-address ""` Parametresi oluşturun ve olduğundan VM, internet'ten bağlı gerekmez VM, genel bir IP adresi atamak için Azure bildirir. SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

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

Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM oluşturma işlemini tamamlayıp VM hakkında daha fazla çıkış döndürür kadar sonraki adıma geçmeyin. 

Bir ağ arabiriminin kendi IP adresini hedeflemeden kendisine gönderilen ağ trafiğini iletebilmesi için, ağ arabiriminde IP iletme özelliğinin etkinleştirilmiş olması gerekir. Ağ arabirimi için IP iletimini etkinleştirmeniz [az ağ NIC güncelleştirme](/cli/azure/network/nic).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

VM içindeki işletim sistemi veya VM içinde çalışan bir uygulama da ağ trafiğini iletebilmelidir. Sanal makinenin işletim sistemi içinde IP iletimini etkinleştirmeniz [az vm uzantısı kümesi](/cli/azure/vm/extension):

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
Komutu yürütmek için bir dakika kadar sürebilir.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Gelen trafiğin doğrulayabilmek sanal ağda iki VM oluşturma *genel* alt yönlendirileceğini *özel* sonraki bir adımda NVA aracılığıyla alt ağ. 

Bir VM oluşturma *genel* alt ağ ile [az vm oluşturma](/cli/azure/vm). `--no-wait` Parametresi sonraki komuta devam edebilmesi arka planda komutu yürütmek Azure sağlar. Bu makalede kolaylaştırmak için bir parola kullanılır. Anahtarlar genellikle üretim dağıtımında kullanılır. Anahtarları kullanıyorsanız, SSH aracı iletmeyi yapılandırmanız da gerekir. Daha fazla bilgi için SSH istemcinizin belgelerine bakın. Değiştirin `<replace-with-your-password>` seçtiğiniz parolayla aşağıdaki komutta.

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

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra Azure CLI'yı bilgiler aşağıdaki örneğe benzer gösterir: 

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
**publicIpAddress** değerini not alın. Bu adres, bir sonraki adımda internet'ten sanal Makineye erişmek için kullanılır.

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVmPrivate* VM. Değiştirin  *\<Publicıpaddress >* sanal makinenizin genel IP adresiyle. Yukarıdaki örnekte, IP adresidir *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Bir parola istendiğinde, seçtiğiniz parolayı girin [sanal makineler oluşturma](#create-virtual-machines).

İzleme yönlendirmesi yüklemek için aşağıdaki komutu kullanın *myVmPrivate* VM:

```bash 
sudo apt-get install traceroute
```

Ağ trafiği için yönlendirmeyi test etmek için aşağıdaki komutu kullanın *myVmPublic* VM'den *myVmPrivate* VM.

```bash
traceroute myVmPublic
```

Yanıt aşağıdaki örneğe benzer:

```bash
traceroute to myVmPublic (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Trafiğin *myVmPrivate* sanal makinesinden *myVmPublic* sanal makinesine doğrudan yönlendirildiğini görebilirsiniz. Azure'nın varsayılan yollarını doğrudan alt ağlar arasında trafiği yönlendirme. 

SSH oturumu açmak için aşağıdaki komutu kullanın *myVmPublic* VM'den *myVmPrivate* VM:

```bash 
ssh azureuser@myVmPublic
```

İzleme yönlendirmesi yüklemek için aşağıdaki komutu kullanın *myVmPublic* VM:

```bash 
sudo apt-get install traceroute
```

Ağ trafiği için yönlendirmeyi test etmek için aşağıdaki komutu kullanın *myVmPrivate* VM'den *myVmPublic* VM.

```bash
traceroute myVmPrivate
```

Yanıt aşağıdaki örneğe benzer:

```bash
traceroute to myVmPrivate (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
İlk atlamanın, NVA özel IP adresi olan 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama ise *myVmPrivate* sanal makinesinin özel IP adresi olan 10.0.1.4’tür. *myRouteTablePublic* yönlendirme tablosuna eklenip *Genel* alt ağ ile ilişkilendirilen yönlendirme, Azure’un trafiği doğrudan *Özel* alt ağına yönlendirmek yerine NVA aracılığıyla yönlendirmesine neden olmuştur.

SSH oturumları hem de kapatmak *myVmPublic* ve *myVmPrivate* VM'ler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [az grubu Sil](/cli/azure/group) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yönlendirme tablosu oluşturup bir alt ağ ile ilişkilendirilmiş. Bir genel alt ağdan özel alt ağa trafiği yönlendiren basit bir NVA oluşturdunuz. [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking)’ten güvenlik duvarı ve WAN iyileştirme gibi ağ işlevleri gerçekleştiren, önceden yapılandırılmış çeşitli NVA’lar dağıtın. Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Bir sanal ağ içinde çok sayıda Azure kaynağına dağıtabilmenize karşın, bazı Azure PaaS hizmetlerinin kaynakları bir sanal ağa dağıtılamaz. Yine de, bazı Azure PaaS hizmetlerinin kaynaklarına erişimi yalnızca bir sanal ağ alt ağından gelecek trafikle kısıtlayabilirsiniz. Bilgi edinmek için bkz. nasıl [PaaS kaynaklarına ağ erişimini kısıtlama](tutorial-restrict-network-access-to-resources-cli.md).
