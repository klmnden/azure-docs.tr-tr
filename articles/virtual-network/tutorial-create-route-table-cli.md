---
title: "Ağ trafiği - Azure CLI | Microsoft Docs"
description: "Azure CLI kullanarak bir yol tablosu ile ağ trafiğini yönlendirmek öğrenin."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/02/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 3c16c774fa1c8a5c8bf50b4f4f9d0bfb283318e3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Azure CLI kullanarak bir yol tablosu ile ağ trafiği yönlendirme

Azure otomatik olarak yollar varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında trafiği. Azure'nın geçersiz kılmak için kendi Rota oluşturabilmeniz için varsayılan yönlendirme. Örneğin, bir güvenlik duvarı üzerinden alt ağlar arasında trafiği yönlendirmek istiyorsanız, özel yollar oluşturma olanağı yararlıdır. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Rota tablosu oluşturma
> * Bir yol oluşturma
> * Bir sanal ağ alt ağı için bir yol tablosu ilişkilendirme
> * Test yönlendirme
> * Yönlendirme sorunlarını giderme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI Sürüm 2.0.28 çalıştırıyorsanız bu hızlı başlangıç yükleyip CLI yerel olarak kullanmak seçerseniz, gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Rota tablosu oluşturma

Varsayılan olarak bir sanal ağdaki tüm alt ağlar arasında trafiği Azure yollar. Azure'nın varsayılan yollar hakkında daha fazla bilgi için bkz: [sistem yolları](virtual-networks-udr-overview.md). Azure'nın varsayılan yönlendirme geçersiz kılmak için rotaları içeren bir yol tablosu oluşturmanız ve bir sanal ağ alt ağı için yol tablosu ilişkilendirebilirsiniz.

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

Bir rota tablosu sıfır veya daha fazla yol içerir. Rota tablosunda bir yol oluşturma [az ağ yol tablosu yol oluşturma](/cli/azure/network/route-table/route#az_network_route_table_route_create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

Rota 10.0.2.4 IP adresiyle bir ağ sanal gereç aracılığıyla 10.0.1.0/24 adres ön eki giden tüm trafiği yönlendirir. Ağ sanal gereç ve alt ağ belirtilen adres ön eki ile daha sonraki adımlarda oluşturulur. Rota yönlendirme, doğrudan alt ağlar arasında trafiği yönlendirir Azure'nın varsayılan değerini geçersiz kılar. Her yol, bir sonraki atlama türü belirtir. Sonraki atlama türü trafiği yönlendirmek nasıl Azure bildirir. Bu örnekte, sonraki atlama türü olan *değerinin VirtualAppliance*. Kullanılabilir tüm sonraki atlama türlerini Azure ve bunların ne zaman kullanılacağı hakkında daha fazla bilgi için bkz: [türleri'sonraki atlama](virtual-networks-udr-overview.md#custom-routes).

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

Bir yol tablosu sıfır veya daha fazla alt ağlara ilişkilendirebilirsiniz. Bir alt ağ için ilişkili sıfır veya bir yol tablosu olabilir. Bir alt ağdan giden trafik, Azure'nın varsayılan yollar ve bir yol tablosu bir alt ağa ilişkilendirmek için eklediğiniz tüm özel yollar göre yönlendirilir. İlişkilendirme *myRouteTablePublic* yol tablosuna *ortak* alt ağ ile [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

Yönlendirme tabloları üretim kullanımı için dağıtmadan önce baştan sona ile öğrenmeniz olduğunu önerilir [Azure'da yönlendirme](virtual-networks-udr-overview.md) ve [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="test-routing"></a>Test yönlendirme

Yönlendirme sınamak için önceki adımda oluşturduğunuz rota üzerinden yönlendiren ağ sanal gereç olarak hizmet veren bir sanal makine oluşturacaksınız. Ağ sanal gereç oluşturduktan sonra bir sanal makineye dağıtacaksınız *ortak* ve *özel* alt ağlar. Ardından gelen trafiği yönlendirmek *ortak* alt ağına *özel* alt ağ sanal gereç aracılığıyla.

### <a name="create-a-network-virtual-appliance"></a>Ağ sanal uygulaması oluşturma

Önceki adımda, bir ağ sanal gereç sonraki atlama türü belirtilmiş bir yol oluşturuldu. Bir ağ uygulaması çalıştıran bir sanal makine, genellikle bir ağ sanal gereç da adlandırılır. Üretim ortamlarında, dağıttığınız ağ sanal gereç önceden yapılandırılmış bir sanal makine görülür. Birçok ağ sanal Gereçleri edinilebilir [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). Bu makalede, temel bir sanal makine oluşturulur. 

Bir ağ sanal gereç oluşturma *DMZ* alt ağ ile [az vm oluşturma](/cli/azure/vm#az_vm_create). Bir sanal makine oluşturduğunuzda, Azure oluşturur ve varsayılan olarak sanal makine için bir ortak IP adresi atar. `--public-ip-address ""` Parametre değil oluşturmak ve sanal makine için Internet'ten bağlı olması gerektiğinden değil sanal makineyi bir ortak IP adresi atamak için Azure bildirir. SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse komutu bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

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

Sanal makine oluşturmak için birkaç dakika sürer. Azure sanal makine oluşturma tamamlandıktan ve sanal makine hakkında çıktıyı döndürür kadar sonraki adıma devam etmeyin. Üretim ortamlarında, dağıttığınız ağ sanal gereç önceden yapılandırılmış bir sanal makine görülür. Birçok ağ sanal Gereçleri edinilebilir [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1).

Her Azure için IP iletimini etkinleştirmeniz gerekir [ağ arabirimi](virtual-network-network-interface.md) ağ arabirimine atanmış olmayan herhangi bir IP adresi için hedeflenen trafiği ileten bir sanal makineye bağlı. İçindeki ağ sanal gerecin oluşturduğunuzda *DMZ* alt ağ, otomatik olarak oluşturulan Azure adlı ağ arabirimi *myVmNvaVMNic*, ağ arabirimi sanal makineye bağlı ve Özel IP adresi atanmış *10.0.2.4* ağ arabirimi. Ağ arabirimi için IP iletimini etkinleştirme [az ağ NIC güncelleştirmesi](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

Sanal makinede işletim sistemi ya da sanal makinede çalışan bir uygulama aynı zamanda ağ trafiğini iletebilir olması gerekir. Bir üretim ortamında Ağ sanal gereç dağıttığınızda, Gereci genellikle filtreleri, günlükleri veya trafiği iletmeden önce başka bir işlevi gerçekleştirir. Bu makalede ancak, işletim sisteminin yalnızca aldığı tüm trafiği iletir. Sanal makinenin işletim sistemi ile içinde IP iletimini etkinleştirmeniz [az vm uzantısı kümesi](/cli/azure/vm/extension#az_vm_extension_set), işletim sistemi içinde IP iletimini etkinleştiren bir komut çalıştırır.

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
Komutu yürütmek için bir dakika sürebilir.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu trafiğinden doğrulamak için iki sanal makine sanal ağ oluşturma *ortak* alt ağ için yönlendirilir *özel* sonraki adımda ağ sanal gereç aracılığıyla alt ağ. 

Bir sanal makine oluşturmak *ortak* alt ağ ile [az vm oluşturma](/cli/azure/vm#az_vm_create). `--no-wait` Parametresi bir sonraki komuta devam edebilmek için arka planda komutu çalıştırmak Azure sağlar. Bu makalede kolaylaştırmak için bir parola kullanılır. Anahtarlar genellikle üretim dağıtımında kullanılır. Anahtarları kullanıyorsanız, SSH aracı iletmeyi yapılandırmanız gerekir. Daha fazla bilgi için SSH istemcisi belgelerine bakın. Değiştir `<replace-with-your-password>` seçtiğiniz bir parola ile aşağıdaki komutu.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Bir sanal makine oluşturmak *özel* alt ağ.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Sanal makine oluşturmak için birkaç dakika sürer. Sanal makine oluşturulduktan sonra Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmMgmt",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Not edin **Publicıpaddress**. Bu adres sanal makine bir sonraki adımda Internet'ten erişmek için kullanılır.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Bir ağ sanal gereç yoluyla trafiği yönlendirme

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVmMgmt* sanal makine. Değiştir  *<publicIpAddress>*  sanal makinenizin ortak IP adresine sahip. Yukarıdaki örnekte, IP adresidir *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

İçin bir parola istendiğinde, seçtiğiniz parolayı girin [sanal makineler oluşturma](#create-virtual-machines).

İzleme yolu yüklemek için aşağıdaki komutu kullanın *myVmMgmt* sanal makine:

```bash 
sudo apt-get install traceroute
```

Ağ trafiği için yönlendirme test etmek için aşağıdaki komutu kullanın *myVmWeb* sanal makineden *myVmMgmt* sanal makine.

```bash
traceroute myvmweb
```

Yanıt aşağıdaki örneğe benzer:

```bash
traceroute to myvmweb (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Trafik, doğrudan yönlendirilir görebilirsiniz *myVmMgmt* sanal makineye *myVmWeb* sanal makine. Azure'nın varsayılan yollar, doğrudan alt ağlar arasında trafiği yönlendirme. 

SSH için aşağıdaki komutu kullanın *myVmWeb* sanal makineden *myVmMgmt* sanal makine:

```bash 
ssh azureuser@myVmWeb
```

İzleme yolu yüklemek için aşağıdaki komutu kullanın *myVmWeb* sanal makine:

```bash 
sudo apt-get install traceroute
```

Ağ trafiği için yönlendirme test etmek için aşağıdaki komutu kullanın *myVmMgmt* sanal makineden *myVmWeb* sanal makine.

```bash
traceroute myvmmgmt
```

Yanıt aşağıdaki örneğe benzer:

```bash
traceroute to myvmmgmt (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
İlk atlama ağ sanal gereç ait özel IP adresi 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama 10.0.1.4, özel IP adresi olan *myVmMgmt* sanal makine. Eklenen rota *myRouteTablePublic* rota tablosu ve ilişkili *ortak* alt neden NVA aracılığıyla yerine doğrudan trafiği yönlendirmek Azure *özel* alt ağ.

Her ikisi için SSH oturumları kapatmak *myVmWeb* ve *myVmMgmt* sanal makineler.

## <a name="troubleshoot-routing"></a>Yönlendirme sorunlarını giderme

Önceki adımlarda öğrenilen Azure, isteğe bağlı olarak, kendi yollar geçersiz kılabilir varsayılan yolların geçerlidir. Bazı durumlarda, olmasını beklediğiniz gibi trafik yönlendirilemeyebilir. Kullanım [az Ağ İzleyicisi Göster sonraki atlama](/cli/azure/network/watcher#az_network_watcher_show_next_hop) iki sanal makineler arasındaki trafik nasıl yönlendirildiğini belirlemek için. Örneğin, aşağıdaki komutu gelen trafik yönlendirme testleri *myVmWeb* (10.0.0.4) sanal makineye *myVmMgmt* (10.0.1.4) sanal makine:

```azurecli-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region.
az network watcher configure --locations eastus --resource-group myResourceGroup --enabled true

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 10.0.1.4 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVmWeb \
  --out table
```
Aşağıdaki çıkış sonra kısa bekleme verilir:

```azurecli
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

Çıktı sonraki atlama IP adresi gelen trafiği için size bildirir *myVmWeb* için *myVmMgmt* 10.0.2.4 olduğu ( *myVmNva* sanal makine), sonraki türü atlama olduğu *Değerinin VirtualAppliance*, ve yönlendirme neden rota tablosu olduğunu *myRouteTablePublic*.

Her ağ arabirimi için etkili rotaları, Azure'nın varsayılan yollar ve tanımladığınız yollar birleşimidir. Bir sanal makineyle bir ağ arabirimi için geçerli olan tüm yolları bkz [az ağ NIC Göster-etkin-yol-tablosu](/cli/azure/network/nic#az_network_nic_show_effective_route_table). Örneğin, etkin yollar için göstermek için *MyVmWebVMNic* ağ arabiriminde *myVmWeb* sanal makine, aşağıdaki komutu girin:

```azurecli-interactive
az network nic show-effective-route-table \
  --name MyVmWebVMNic \
  --resource-group myResourceGroup
```

Tüm varsayılan yollar ve bir önceki adımda eklediğiniz rota döndürülür.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yol tablosu oluşturulur ve bir alt ağa ilişkilendirilmiş. Ortak bir alt ağ trafiği için özel bir alt ağa yönlendirilmiş bir ağ sanal gereç oluşturuldu. Bir sanal ağ içinde birçok Azure kaynakları dağıtabilirsiniz, ancak bazı Azure PaaS hizmetler için kaynaklar sanal bir ağa dağıtılamıyor. Hala erişimi bazı Azure PaaS Hizmetleri'nden trafik için yalnızca bir sanal ağ alt kaynaklara yine de kısıtlayabilirsiniz. Ağ erişimi Azure PaaS kaynaklarına erişimi kısıtlayabilir öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Ağ erişimi PaaS kaynaklarına erişimi kısıtlayabilir](virtual-network-service-endpoints-configure.md#azure-cli)
